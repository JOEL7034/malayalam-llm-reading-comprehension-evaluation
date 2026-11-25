# Malayalam LLM Reading Comprehension Evaluation

## Overview

This repository presents a comparative evaluation of three open-source language models on Malayalam reading comprehension using the Malayalam subset of the **Belebele multilingual reading comprehension benchmark**. The study examines the effect of language choice in prompting and the use of few-shot demonstrations on multiple-choice reading comprehension performance.

The evaluation focuses on English zero-shot, Malayalam zero-shot, and English few-shot prompting conditions, alongside accuracy-based analysis, statistical uncertainty estimation, tokenizer efficiency, and inspection of model errors.

## Research Objectives

- Evaluate the reading comprehension performance of small language models on Malayalam benchmark questions.
- Investigate the effect of prompt language on model performance.
- Compare zero-shot and few-shot prompting conditions.
- Estimate uncertainty in accuracy using bootstrap confidence intervals.
- Examine tokenizer efficiency for Malayalam and English text.
- Inspect incorrect predictions and answer-choice distributions.

## Models

The study considers the following instruction-tuned models:

| Model | Model Identifier |
|---|---|
| Qwen 2.5 3B Instruct | `Qwen/Qwen2.5-3B-Instruct` |
| Gemma 2 2B IT | `google/gemma-2-2b-it` |
| Llama 3.2 3B Instruct | `meta-llama/Llama-3.2-3B-Instruct` |

## Dataset

- **Benchmark:** Belebele
- **Dataset repository:** `facebook/belebele`
- **Language configuration:** `mal_Mlym` (Malayalam)
- **Split:** Test
- **Evaluation sample:** 200 questions
- **Task:** Multiple-choice reading comprehension

A fixed random seed is used for sample selection and reproducibility of the sampling procedure.

## Experimental Conditions

The models are evaluated using three prompting conditions:

| Condition | Description |
|---|---|
| English Zero-Shot | English instruction without demonstrations |
| Malayalam Zero-Shot | Malayalam instruction without demonstrations |
| English Few-Shot | English instruction with 3 demonstrations |

The few-shot demonstrations are separated from the evaluation sample during sample selection.

## Evaluation Methodology

The evaluation pipeline includes:

1. Loading the Malayalam Belebele test split.
2. Selecting evaluation questions and few-shot examples using a fixed random seed.
3. Loading model tokenizers and instruction-tuned language models.
4. Constructing prompts for the experimental conditions.
5. Generating multiple-choice predictions.
6. Recording predictions, gold answers, correctness, format compliance, and prompt token counts.
7. Combining prediction files generated across model evaluation sessions.
8. Computing evaluation metrics and conducting comparative analysis.

## Evaluation Metrics and Analysis

### Accuracy

The proportion of questions answered correctly under each model and prompting condition.

### Bootstrap Confidence Intervals

Bootstrap resampling is used to estimate 95% confidence intervals for accuracy.

### Answer-Format Compliance

Measures whether the generated response follows the required multiple-choice answer format.

### Prompt Token Analysis

Records prompt token counts for evaluation prompts.

### Tokenizer Efficiency

Compares tokenization efficiency across Malayalam and English text using average tokens per whitespace-separated word (token fertility).

### Statistical Comparisons

Paired bootstrap comparisons are conducted to estimate accuracy differences between:

- English Few-Shot and English Zero-Shot
- English Zero-Shot and Malayalam Zero-Shot
- English Few-Shot and Malayalam Zero-Shot

Model comparisons are also performed under selected prompting conditions using paired question-level predictions.

### Error Analysis

Incorrect predictions are extracted for inspection alongside the original passages, questions, and answer options. The notebook also examines answer-choice distributions to explore potential response-position patterns.

## Reproducibility

The experiment is developed in Google Colab and uses GPU-accelerated inference where available.

The notebook includes environment setup, dataset loading, model inference, prediction collection, statistical analysis, and export steps.

A fixed random seed (`42`) is used for sample selection. Prediction result files from model evaluation sessions are combined for the comparative analysis.

## Limitations

- The evaluation uses a 200-question sample rather than the full Malayalam benchmark.
- The study includes three language models.
- The task is limited to multiple-choice reading comprehension.
- Performance may be influenced by prompting, model generation behavior, and tokenization.
- The findings should not be interpreted as a comprehensive measure of Malayalam language understanding.

## Repository Contents

| File | Description |
|---|---|
| `malayalam_llm_eval.ipynb` | Research notebook containing evaluation and analysis workflow |

Additional result files, tables, and visualizations may be added as the experiment is finalized.

## Status

This repository contains the experimental workflow. Final quantitative results and figures will be added after the evaluation outputs are consolidated and verified.

## Citation and Dataset

Belebele is the multilingual reading comprehension benchmark used in this study. Please refer to the original dataset publication and the official dataset repository for benchmark details.

- Dataset: [facebook/belebele on Hugging Face](https://huggingface.co/datasets/facebook/belebele)

---

*This repository is intended for research, experimentation, and reproducible analysis of multilingual language-model evaluation.*
