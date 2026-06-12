# Research Notes — MedLLM-Algerian

## Motivation

Algeria has ~44 million people with healthcare documentation primarily in French (institutional) and Arabic (patient-facing), with frequent code-switching. Existing medical LLMs (MedPaLM, BioMedGPT) are English-centric and lack:

1. Algerian Arabic dialect understanding
2. French-Arabic bilingual instruction following
3. Knowledge of locally endemic diseases (leishmaniasis, hepatitis B/C prevalence)
4. Alignment with INSP (Institut National de Santé Publique) clinical guidelines

## Baseline Evaluation

Models evaluated on 200 Arabic/French medical Q&A pairs curated from Algerian physicians:

| Model | Arabic ROUGE-L | French ROUGE-L | Safety Score |
|---|---|---|---|
| GPT-4 (baseline) | 0.41 | 0.58 | 0.92 |
| Mistral-7B (base) | 0.22 | 0.49 | 0.71 |
| Jais-13B (base) | 0.38 | 0.31 | 0.78 |
| **MedLLM-Algerian SFT** | **0.51** | **0.61** | **0.89** |

## Key Findings

- SFT on 5K Algerian medical QA pairs improves Arabic ROUGE-L by +133% over base Mistral-7B
- DPO alignment significantly improves safety score (no unsolicited drug dosing advice)
- French-Arabic code-switching in prompts does not degrade performance post-SFT
- Leishmaniasis and tuberculosis protocols correctly referenced with Algerian-specific context

## Future Work

- Expand to 50K QA pairs with Algerian physician validation
- Fine-tune on SANTE EHR de-identified records (pending MoH approval)
- Evaluate on USMLE-style Arabic MCQ benchmark
- Deploy as API for pilot in CHU Mustapha Pacha telemedicine service
