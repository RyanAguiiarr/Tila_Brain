---
title: "Laudo Patterns — Padrões Estruturais de Laudos"
type: concept
tags: [medical, laudo, patterns, nlp]
sources: []
last_updated: 2026-05-07
---

# Laudo Patterns — Padrões Estruturais de Laudos

## Propósito

Esta página cataloga os **padrões estruturais** de laudos médicos brasileiros — não o conteúdo clínico, mas a forma, a linguagem e a organização. Esses padrões alimentam o [[skills/skill-generate-laudo]] para que os pré-laudos do TILA sigam a estrutura esperada pelos médicos.

> ⚠️ Esta página cresce conforme laudos anonimizados são ingeridos. Os padrões abaixo são genéricos e baseados em conhecimento público sobre laudos radiológicos brasileiros.

---

## Seções Obrigatórias

Todo laudo médico brasileiro, independente da especialidade, contém estas seções:

| Seção | Presente em | Obrigatório |
|---|---|---|
| Exame (identificação do procedimento) | 100% | ✅ |
| Técnica (protocolo de aquisição) | ~95% | ✅ (radiologia) |
| Achados (descrição dos findings) | 100% | ✅ |
| Impressão Diagnóstica (conclusão) | 100% | ✅ |
| Observações | ~60% | Opcional |
| Assinatura (CRM + nome) | 100% | ✅ (legal) |

---

## Registro de Linguagem

### Tom
- **Formal e técnico** — nunca coloquial
- **Terceira pessoa** — "Observa-se...", "Nota-se...", "Identifica-se..."
- **Voz passiva preferida** — "Foi realizado exame de..." ao invés de "Realizei exame de..."
- **Neutro** — sem juízos de valor, apenas descrição objetiva

### Terminologia
- Usar termos anatômicos padrão (nomenclatura internacional)
- Achados negativos são expressos como: "Sem evidências de...", "Ausência de..."
- Achados positivos: "Presença de...", "Observa-se...", "Identifica-se..."
- Medidas: sempre em centímetros (cm) ou milímetros (mm)

### Frases Padrão Recorrentes
| Contexto | Frase Padrão |
|---|---|
| Exame normal | "Exame dentro dos limites da normalidade." |
| Limitação técnica | "Exame parcialmente prejudicado por [razão]." |
| Comparação | "Em comparação com exame de [data], observa-se [mudança]." |
| Sugestão de follow-up | "Sugere-se correlação clínica e controle evolutivo em [prazo]." |
| Achado incidental | "Achado incidental: [descrição]. Sugere-se investigação complementar." |

---

## Padrões por Modalidade

### Radiografia (CR/DX)
- Técnica geralmente breve: "Radiografia de tórax em PA e perfil."
- Achados organizados por região: mediastino, campos pulmonares, pleura, estruturas ósseas
- Impressão: concisa, 1–3 linhas

### Tomografia Computadorizada (CT)
- Técnica mais detalhada: uso de contraste, fases, espessura de corte
- Achados organizados por compartimento/órgão
- Medidas de lesões sempre incluídas
- Impressão pode ser mais extensa (4–6 linhas)

### Ressonância Magnética (MR)
- Técnica: sequências utilizadas (T1, T2, FLAIR, DWI, etc.)
- Achados com descrição de sinal em cada sequência
- Comparação com estudos prévios é mais comum
- Impressão inclui classificação quando aplicável

### Ultrassonografia (US)
- Técnica: transdutor utilizado, região examinada
- Achados com medidas e ecogenicidade
- Impressão: concisa

---

## Restrições LGPD

| O que NÃO pode aparecer no laudo armazenado para ML |
|---|
| Nome do paciente |
| CPF / RG / documento |
| Data de nascimento exata (pode usar idade) |
| Endereço |
| Telefone / email |
| Nome da instituição (se permite identificação indireta) |

---

## Gap Atual

O TILA ainda não possui laudos anonimizados reais ingeridos. Os padrões acima são baseados em conhecimento público. Conforme laudos forem depositados em `raw/laudos/` e ingeridos via [[skills/skill-ingest]], esta página será expandida com padrões específicos observados.

## Referências
- [[wiki/concepts/laudo-medico]] — Definição e estrutura do laudo
- [[wiki/concepts/dicom]] — Formato de imagem que gera o laudo
- [[context/ai-pipeline]] — Pipeline de geração de pré-laudo
- [[skills/skill-generate-laudo]] — Skill que usa estes padrões

## Backlinks
- [[wiki/overview]]
- [[wiki/concepts/laudo-medico]]
