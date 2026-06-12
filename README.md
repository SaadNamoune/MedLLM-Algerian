# MedLLM-Algerian

**Medical Large Language Model Fine-Tuning for the Algerian Healthcare Context** — Arabic/French bilingual LLM training pipeline covering PT → SFT → RLHF → DPO/ORPO stages, adapted for Algerian medical terminology and clinical workflows.

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/)
[![License: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-green)](LICENSE)
[![HuggingFace](https://img.shields.io/badge/🤗-Transformers-yellow)](https://huggingface.co/)
[![DeepSpeed](https://img.shields.io/badge/DeepSpeed-ZeRO--3-orange)](https://www.deepspeed.ai/)

---

## Overview

MedLLM-Algerian fine-tunes open-source LLMs on Arabic/French medical corpora, with specific adaptations for:

- **Algerian medical Arabic** (Darija + MSA clinical terminology)
- **French-Arabic code-switching** common in Algerian medical records
- **Local disease prevalence** (diabetes, hypertension, leishmaniasis, viral hepatitis)
- **SANTE platform integration** (Algeria's national electronic health record system)

The training pipeline supports four stages progressively:

```
Pre-training (PT) → Supervised Fine-Tuning (SFT) → RLHF → DPO/ORPO alignment
```

---

## Training Pipeline

```
data/pretrain/   Arabic medical articles + French clinical texts
      │
      ▼  training/pt_train.py
  Pre-trained base (continued pretraining on medical domain)
      │
      ▼  training/sft_train.py (LoRA / QLoRA)
  Instruction-tuned model (Q&A, diagnosis assistance)
      │
      ▼  training/rlhf_train.py
  RLHF reward model + PPO alignment
      │
      ▼  training/dpo_train.py  or  training/orpo_train.py
  Final aligned model (safe, factual, bilingual)
```

---

## Supported Base Models

| Model | Parameters | Language | Medical Adaption |
|---|---|---|---|
| Llama-3.1-8B | 8B | EN/AR | Via continued PT |
| Mistral-7B-v0.3 | 7B | EN/FR | Via SFT |
| Jais-13B | 13B | AR/EN | Natively Arabic |
| Qwen2.5-7B | 7B | ZH/EN/AR | Multilingual |

---

## Dataset Strategy

| Stage | Data | Size |
|---|---|---|
| PT | Arabic medical Wikipedia, PubMed abstracts (FR) | ~500MB |
| SFT | Medical QA pairs (Arabic + French), clinical notes | ~50K pairs |
| RLHF | Human-ranked response pairs from Algerian physicians | ~5K pairs |
| DPO | Preference pairs: safe/unsafe responses | ~10K pairs |

---

## Quick Start

```bash
git clone https://github.com/SaadNamoune/MedLLM-Algerian.git
cd MedLLM-Algerian
pip install -r requirements.txt

# SFT with LoRA on Mistral-7B
python training/sft_train.py \
  --model_name mistralai/Mistral-7B-v0.3 \
  --train_file data/sft/algerian_medical_qa.jsonl \
  --output_dir outputs/sft_mistral \
  --use_lora True \
  --lora_rank 16 \
  --num_train_epochs 3

# DPO alignment
python training/dpo_train.py \
  --model_name outputs/sft_mistral \
  --train_file data/dpo/algerian_preferences.jsonl \
  --output_dir outputs/dpo_final
```

---

## Research Context

Developed at **ESI Alger** as part of research in Arabic NLP and medical AI for the Maghreb region. The goal is to bridge the gap between general-purpose LLMs and the specific needs of Algerian healthcare practitioners who work in Arabic-French bilingual environments.

**Affiliated with:** Ministry of Health Algeria digitization initiative, SANTE platform research group.

---

## Disclaimer

This model is intended for research purposes only. It should not be used as a substitute for professional medical advice, diagnosis, or treatment.

---

## Author

**Saad Namoune** — ESI Alger | Cybersecurity & AI Research  
[GitHub](https://github.com/SaadNamoune) · [Email](mailto:saad.namoune28@gmail.com)
