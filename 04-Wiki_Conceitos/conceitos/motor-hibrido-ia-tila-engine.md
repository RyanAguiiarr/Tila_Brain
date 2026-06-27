# Motor Híbrido de IA do TILA (TorchXRayVision + MedGemma INT4 + TILA Neural Engine)

Este documento detalha a arquitetura de inteligência artificial de triagem e geração de pré-laudos radiológicos do ecossistema TILA, projetada para máxima precisão clínica, conformidade com a LGPD/CFM e eficiência extrema de hardware local.

---

## 1. Visão Geral da Arquitetura em 3 Camadas

O serviço de IA do TILA (`tila-ai-service`) opera de forma autônoma e desacoplada da nuvem através de um pipeline de processamento em três etapas fundamentais:

```
[ Imagem DICOM / PNG ] + [ Indicação Clínica ]
              │
              ▼
┌──────────────────────────────────────────────────────────────┐
│ Camada 1: Visão Computacional Quantitativa (TorchXRayVision) │
│ - Modelo DenseNet121 pré-treinado em 18 patologias torácicas │
│ - Cálculo de probabilidades (%) e níveis de criticidade      │
└──────────────────────────────────────────────────────────────┘
              │
              ▼
┌──────────────────────────────────────────────────────────────┐
│ Camada 2: RAG Clínico (PgVector / Bio_ClinicalBERT)          │
│ - Busca semântica de diretrizes e protocolos médicos         │
└──────────────────────────────────────────────────────────────┘
              │
              ▼
┌──────────────────────────────────────────────────────────────┐
│ Camada 3: Redação e Validação Cruzada (Motor Híbrido Fluido) │
│ ├── Opção A: MedGemma 1.5 4B (GPU INT4 Live)                 │
│ └── Opção B: TILA Neural Engine (Fallback Determinístico)    │
└──────────────────────────────────────────────────────────────┘
```

---

## 2. Detalhes Técnicos das Camadas

### 2.1. Triagem Visual (`TorchXRayVision`)
O módulo `triagem_service.py` utiliza o modelo `densenet121-res224-all` da biblioteca TorchXRayVision. A imagem de entrada é redimensionada para 224x224 pixels e normalizada. O modelo avalia 18 patologias pulmonares (ex: *Opacidade Pulmonar, Derrame Pleural, Infiltração, Massa, Pneumotórax, Cardiomegalia*).
- **Classificação de Criticidade:**
  - `ALTO`: Probabilidade >= 70% (ou achados críticos urgentes)
  - `MODERADO`: Probabilidade entre 55% e 69%
  - `BAIXO`: Probabilidade < 55%

### 2.2. O Problema de VRAM e a Solução INT4 (`MedGemma 1.5 4B`)
O modelo **MedGemma 1.5 4B** (`google/medgemma-1.5-4b-it`) possui 4 bilhões de parâmetros.
- **Em 16-bit (`float16` / `bfloat16`):** O modelo consome ~8GB de VRAM. Em placas de vídeo comuns como a **NVIDIA RTX 4060 (8GB)**, o carregamento simultâneo dos modelos de visão e busca causa esgotamento da VRAM, ativando o *CPU Offload* (troca de memória via PCIe), o que congela o sistema operacional.
- **A Solução INT4 (`bitsandbytes`):** Através da configuração `BitsAndBytesConfig(load_in_4bit=True, bnb_4bit_quant_type="nf4", bnb_4bit_use_double_quant=True)`, os pesos são compactados para **~2.5GB de VRAM**. Isso elimina totalmente o engarrafamento de memória, garante fluidez no Windows e latência mínima na geração de tokens.

### 2.3. Motor Fluido de Fallback (`TILA Neural Engine`)
Caso o servidor não possua GPU dedicada ou o carregamento do LLM falhe (ex: falta de token HuggingFace ou ambiente de desenvolvimento ultraleve), o sistema aciona de forma transparente e instantânea (< 1 segundo) o **TILA Neural Engine**.
O motor monta uma redação clínica altamente estruturada, correlacionando os achados quantitativos da visão com a indicação clínica informada, inserindo notas de diagnóstico diferencial (ex: necessidade de investigar Tuberculose ou Pneumonia diante de infiltrados) e anexando as diretrizes RAG recuperadas.

---

## 3. Transparência na API e Alertas para Pacientes

No retorno do endpoint `POST /api/v1/laudos/gerar`, o objeto `PreLaudoResponse` entrega dois recursos diferenciais:

1. **`modelo_ia_utilizado`:** Permite auditoria imediata sobre qual motor processou o exame:
   - `MedGemma-1.5-4B (GPU INT4 Live) + TorchXRayVision`
   - `TILA Neural Engine (TorchXRayVision + RAG + Validador Fluido)`

2. **`resumo_para_leigo`:** Tradução humanizada e acessível da linguagem médica complexa para que o paciente ou leigo compreenda a gravidade e a necessidade de seguimento clínico sem pânico desnecessário (ex: explicita quando há indicação de buscar exames para descartar Tuberculose ou pneumonia).

---

## 4. Aviso Legal e Conformidade CFM/LGPD

Todo documento gerado carrega obrigatoriamente a advertência legal estabelecida pelo CRM/CFM:
> *"AVISO LEGAL (CFM / LGPD): Este documento constitui uma sugestão preliminar gerada por Inteligência Artificial (MedGemma 1.5 + TorchXRayVision) e NÃO possui validade médica legal. A revisão, validação e assinatura por um médico radiologista registrado no Conselho Regional de Medicina (CRM) são estritamente obrigatórias antes de qualquer liberação ao paciente ou conduta clínica."*
