---
title: "DICOM — Digital Imaging and Communications in Medicine"
type: concept
tags: [medical, dicom, imaging, privacy, lgpd]
sources: []
last_updated: 2026-05-07
---

# DICOM — Digital Imaging and Communications in Medicine

## O que é

**DICOM** (Digital Imaging and Communications in Medicine) é o padrão internacional para comunicação e gerenciamento de imagens médicas e dados relacionados. Desenvolvido pelo NEMA (National Electrical Manufacturers Association) e o ACR (American College of Radiology), é o formato universal utilizado por praticamente todos os equipamentos de imagem médica: tomógrafos, aparelhos de ressonância magnética, ultrassom, radiografia digital, etc.

## Por que importa para o TILA

O TILA precisa processar imagens médicas para gerar pré-laudos. O DICOM é o formato padrão dessas imagens. Entender o DICOM é crucial para:

1. **Ingerir imagens** — receber e armazenar corretamente
2. **Extrair metadata** — tipo de exame, modalidade, parâmetros de aquisição
3. **Scrubbing de privacidade** — remover dados de paciente (LGPD)
4. **Alimentar o pipeline de IA** — imagem limpa → CNN → achados

## Estrutura de um Arquivo DICOM

Um arquivo DICOM contém:
- **Header** — metadata organizado em tags padronizadas
- **Pixel Data** — a imagem em si (pode ser comprimida)

### Tags Mais Relevantes para TILA

| Tag | Nome | Conteúdo | Uso no TILA |
|---|---|---|---|
| (0008,0060) | Modality | CR, CT, MR, US, etc. | Identificar tipo de exame |
| (0008,0070) | Manufacturer | Nome do fabricante | Contexto técnico |
| (0008,1030) | Study Description | Descrição do estudo | Input para LLM |
| (0010,0010) | Patient Name | Nome do paciente | ⚠️ REMOVER (LGPD) |
| (0010,0020) | Patient ID | ID/CPF do paciente | ⚠️ REMOVER (LGPD) |
| (0010,0030) | Birth Date | Data de nascimento | ⚠️ REMOVER (LGPD) |
| (0010,0040) | Patient Sex | Sexo do paciente | MANTER (relevante clinicamente) |
| (0018,0015) | Body Part Examined | Região anatômica | Input para CNN |
| (0018,0060) | kVp | Tensão do tubo (raio-X) | Parâmetros técnicos |
| (0028,0010) | Rows | Altura da imagem | Dimensões |
| (0028,0011) | Columns | Largura da imagem | Dimensões |
| (7FE0,0010) | Pixel Data | A imagem | Input para CNN |

## Scrubbing de Metadata (Requisito LGPD)

Antes de qualquer processamento, os dados pessoais do paciente devem ser removidos do arquivo DICOM. Isto é um requisito **legal** da LGPD (dados de saúde = dados sensíveis).

### Dados a REMOVER Obrigatoriamente
- `(0010,0010)` — Patient Name
- `(0010,0020)` — Patient ID (pode ser CPF)
- `(0010,0030)` — Patient Birth Date
- `(0010,1000)` — Other Patient IDs
- `(0010,1001)` — Other Patient Names
- `(0008,0050)` — Accession Number (pode ser rastreável)
- `(0008,0080)` — Institution Name (se necessário para anonimização completa)
- `(0008,0090)` — Referring Physician Name
- `(0008,1050)` — Performing Physician Name

### Dados a MANTER
- Modalidade, parâmetros técnicos, região anatômica — relevantes para o laudo
- Pixel data — a imagem em si (sem dados pessoais se scrubbing feito corretamente)

### Ferramentas de Scrubbing
- **dcm4che** — biblioteca Java para manipulação DICOM (compatível com stack TILA)
- **dcm4chee** — servidor PACS completo (inclui scrubbing)
- **DICOM Anonymizer** — ferramentas standalone

## Gap Atual no TILA

> ⚠️ **O TILA atualmente NÃO possui servidor DICOM.** O frontend faz upload de imagens genéricas (JPG/PNG), não DICOM.

### Planejamento
- **Phase 3** do roadmap prevê integração com servidor DICOM
- Decisão em aberto: usar dcm4chee (PACS completo) ou dcm4che (biblioteca) — ver [[context/roadmap]]
- O pipeline de scrubbing deve ser implementado ANTES do pipeline de IA

## Referências
- [[wiki/concepts/laudo-medico]] — O laudo que o DICOM alimenta
- [[context/ai-pipeline]] — Pipeline completo de processamento
- [[context/security-lgpd]] — Requisitos LGPD para dados médicos
- [[context/roadmap]] — Planejamento de implementação DICOM

## Backlinks
- [[wiki/overview]]
- [[context/ai-pipeline]]
