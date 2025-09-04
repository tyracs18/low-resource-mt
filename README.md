# In-Context Learning vs. Pivot-Based Translation for Low-Resource MT

#### Overview
This project compares three strategies for low-resource machine translation (MT) on the Nepali ↔ Sinhala pair:
1. Direct multilingual MT using NLLB-200 (distilled 600M)
2. Pivot-based translation via English (with and without fine-tuning)
3. In-Context Learning (ICL) using an instruction-tuned LLM (few-shot prompting)

We evaluate with BLEURT, SacreBLEU, and ChrF on FLoRes-200 dev/devtest splits (plus WMT21 WikiTitles v3 for pivot fine-tuning experiments in the paper).
#### Key finding:
Direct NLLB performs reasonably well for this true low-resource pair; pivoting + light fine-tuning improves scores further. ICL (zero/3-shot) shows gains with examples but lags supervised approaches for Nepali–Sinhala in our setup.

#### Repository Contents
- CS7650_final_report_v1.pdf        # Full write-up (methods, experiments, results)
- CS7650_proposal.pdf               # Initial proposal (motivation, plan, datasets)
- cs7650_NLLB.ipynb                 # Reproducible notebook: direct NLLB + evaluation
- cs7650_directtranslation.ipynb    # Reproducible notebook: direct translation + evaluation 
- processing.ipynb                  # Reproducible notebook: processing the FLoRes-200 dataset for desired language pairs

#### Datasets
1. FLoRes-200 (dev, devtest): benchmark for 200+ languages. We use Nepali ↔ Sinhala splits.
2. WMT21 WikiTitles v3: used in the paper for pivot fine-tuning experiments (notebook here demonstrates direct NLLB + eval).

#### Results
##### Direct vs. Pivot (NLLB-200 distilled 600M)
- Direct NLLB: moderate BLEURT/ChrF on Nepali–Sinhala
- Pivot via English (no FT): slightly worse than direct (error propagation)
- Pivot via English (with FT): consistent gains across metrics
##### ICL (LLaMA-2-7B-Chat, zero vs. 3-shot)
- Zero-shot: weak (tokenization & no translation supervision)
- 3-shot ICL: noticeable improvements (esp. ChrF), but still below supervised NLLB
- See CS7650_final_report_v1.pdf for full tables, discussion, and implications.

#### Reproducing Our Pipeline
1. Prepare data: Convert FLoRes-200 dev/devtest to JSONL (src/tgt).
2. Direct NLLB: Run cs7650_NLLB.ipynb to generate predictions + metrics.
3. Pivoting (paper): Chain NLLB (ne→en, en→sin), compare w/ and w/o fine-tuning.
4. ICL (paper): Prompt an LLM (zero/3-shot) and evaluate with the same metrics.
