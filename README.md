# Malayalam LLM Reading Comprehension Evaluation

## Overview

This research project presents a comparative evaluation of three instruction-tuned language models on Malayalam reading comprehension using the Malayalam subset of the **Belebele multilingual reading comprehension benchmark**.

The study investigates the effects of prompting language and few-shot demonstrations on model performance. Three prompting conditions are examined: English zero-shot, Malayalam zero-shot, and English few-shot prompting.

The evaluation uses a shared sample of 200 questions and analyzes accuracy, bootstrap confidence intervals, paired prompt-condition comparisons, and model prediction patterns.

---

## Research Objectives

- Evaluate small language models on Malayalam reading comprehension.
- Compare English zero-shot and Malayalam zero-shot prompting.
- Investigate the impact of English few-shot demonstrations.
- Compare model performance using a shared set of evaluation questions.
- Quantify accuracy uncertainty using bootstrap confidence intervals.
- Examine answer-selection patterns and incorrect predictions.
- Explore tokenization efficiency across Malayalam and English.

---

## Models Evaluated

| Model | Hugging Face Model ID | Parameters |
|---|---|---:|
| Qwen 2.5 3B Instruct | `Qwen/Qwen2.5-3B-Instruct` | 3B |
| Gemma 2 2B IT | `google/gemma-2-2b-it` | 2B |
| Llama 3.2 3B Instruct | `meta-llama/Llama-3.2-3B-Instruct` | 3B |

---

## Dataset

- **Benchmark:** Belebele
- **Dataset:** `facebook/belebele`
- **Language configuration:** `mal_Mlym` (Malayalam)
- **Split:** Test
- **Evaluation sample:** 200 questions
- **Task:** Four-option multiple-choice reading comprehension

The evaluation sample is selected using a fixed random seed (`42`). Three separate examples are selected for few-shot demonstrations. The demonstration examples are separated from the evaluation questions.

Dataset: [Belebele on Hugging Face](https://huggingface.co/datasets/facebook/belebele)

---

## Experimental Design

Each model is evaluated under three prompting conditions using the same 200 evaluation questions.

| Condition | Prompt language | Demonstrations |
|---|---|---:|
| English Zero-Shot (`en_zero`) | English | 0 |
| Malayalam Zero-Shot (`ml_zero`) | Malayalam | 0 |
| English Few-Shot (`en_few`) | English | 3 |

This design enables comparisons between prompting language and the use of demonstrations while controlling for question-level variation through shared evaluation items.

---

## Evaluation Method

### Constrained Multiple-Choice Prediction

Predictions are obtained using the models' next-token logits. The evaluation compares the scores associated with the answer labels A, B, C, and D, and selects the highest-scoring option from these four labels.

This is a **constrained-choice evaluation procedure** rather than free-form answer generation followed by textual parsing.

The recorded prediction is compared with the benchmark's gold answer to determine correctness.

### Metrics

- **Accuracy:** Proportion of questions for which the selected answer matches the gold answer.
- **Bootstrap 95% confidence interval:** Uncertainty around observed accuracy estimated using bootstrap resampling (2,000 iterations).
- **Letter rate:** Proportion of cases in which the model's unrestricted highest-scoring next token is one of the allowed answer-label tokens (A, B, C, or D). This is not free-form answer-format compliance.
- **Prompt token count:** Number of tokens in the input prompt.

### Statistical Comparisons

Paired bootstrap comparisons are used to estimate differences in accuracy across prompting conditions within each model. Comparisons include:

- English Few-Shot vs. English Zero-Shot
- English Zero-Shot vs. Malayalam Zero-Shot
- English Few-Shot vs. Malayalam Zero-Shot

The paired comparisons use matching question indices and 5,000 bootstrap iterations.

---

## Results

### Accuracy by Model and Prompting Condition

Accuracy is measured on 200 questions per condition.

| Model | English Zero-Shot | Malayalam Zero-Shot | English Few-Shot |
|---|---:|---:|---:|
| Gemma 2 2B IT | 47.5% | 21.0% | 49.5% |
| Llama 3.2 3B Instruct | 39.5% | 46.0% | **52.0%** |
| Qwen 2.5 3B Instruct | 40.0% | 33.5% | 46.5% |

The highest observed accuracy in this evaluation is achieved by Llama 3.2 3B Instruct under English Few-Shot prompting (52.0%).

### Bootstrap 95% Confidence Intervals

| Model | Condition | Accuracy | 95% CI |
|---|---|---:|---:|
| Gemma 2 2B IT | English Zero-Shot | 47.5% | 40.5–54.5% |
| Gemma 2 2B IT | Malayalam Zero-Shot | 21.0% | 15.5–26.5% |
| Gemma 2 2B IT | English Few-Shot | 49.5% | 43.0–56.5% |
| Llama 3.2 3B Instruct | English Zero-Shot | 39.5% | 33.0–46.5% |
| Llama 3.2 3B Instruct | Malayalam Zero-Shot | 46.0% | 39.5–53.0% |
| Llama 3.2 3B Instruct | English Few-Shot | 52.0% | 45.0–59.0% |
| Qwen 2.5 3B Instruct | English Zero-Shot | 40.0% | 33.5–46.5% |
| Qwen 2.5 3B Instruct | Malayalam Zero-Shot | 33.5% | 27.0–40.0% |
| Qwen 2.5 3B Instruct | English Few-Shot | 46.5% | 40.0–53.5% |

Confidence intervals are estimated by bootstrap resampling the question-level correctness indicators. They describe uncertainty around the observed sample accuracy.

### Letter Rate

The letter rate measures whether the unrestricted top-scoring next token belongs to the answer-label token set. Since the final prediction is constrained to the four choices, this metric should not be interpreted as free-form response-format compliance.

| Model | English Zero-Shot | Malayalam Zero-Shot | English Few-Shot |
|---|---:|---:|---:|
| Gemma 2 2B IT | 100.0% | 0.0% | 99.5% |
| Llama 3.2 3B Instruct | 100.0% | 100.0% | 100.0% |
| Qwen 2.5 3B Instruct | 100.0% | 12.5% | 100.0% |

The low letter rates observed for Gemma and Qwen under Malayalam Zero-Shot indicate that their unrestricted next-token preferences often fall outside the allowed answer-label token set. The constrained selection procedure nevertheless produces an answer choice for every evaluated question.

---

## Paired Prompting Comparisons

Accuracy differences are reported in percentage points (pp). Confidence intervals are based on paired bootstrap resampling of question-level correctness differences.

| Model | Comparison | Difference (pp) | 95% CI |
|---|---|---:|---:|
| Gemma | EN Few − EN Zero | +2.0 | −5.0 to +9.0 |
| Gemma | EN Zero − ML Zero | +26.5 | +17.5 to +35.5 |
| Gemma | EN Few − ML Zero | +28.5 | +19.0 to +37.5 |
| Llama | EN Few − EN Zero | +12.5 | +4.5 to +20.5 |
| Llama | EN Zero − ML Zero | −6.5 | −13.0 to −0.5 |
| Llama | EN Few − ML Zero | +6.0 | −0.5 to +12.5 |
| Qwen | EN Few − EN Zero | +6.5 | +1.0 to +12.0 |
| Qwen | EN Zero − ML Zero | +6.5 | −1.0 to +14.0 |
| Qwen | EN Few − ML Zero | +13.0 | +4.5 to +21.5 |

These comparisons are descriptive bootstrap estimates. They should not be interpreted as definitive hypothesis tests, and no multiple-comparison correction is applied here.

---

## Interpretation

The observed results show differences in performance across models and prompting conditions.

- English Few-Shot prompting yields the highest observed accuracy for all three models in this sample.
- Llama 3.2 3B Instruct achieves the highest observed accuracy overall (52.0%).
- Gemma exhibits a substantial observed difference between English and Malayalam Zero-Shot conditions.
- Llama performs better in Malayalam Zero-Shot than in English Zero-Shot in this sample.
- Qwen shows a higher observed accuracy under English Few-Shot than under either zero-shot condition.

These findings are specific to this benchmark sample and experimental setup. They do not establish general Malayalam language capability or causal effects beyond the tested conditions.

---

## Reproducibility

The experiments are implemented in a Google Colab notebook and use GPU-based inference where available.

The notebook contains:

- Dataset loading and sample selection
- Prompt construction
- Model and tokenizer loading
- Constrained answer-label prediction
- Prediction export in JSONL format
- Accuracy and bootstrap confidence interval calculations
- Paired prompting comparisons
- Answer-choice distribution analysis
- Incorrect prediction inspection
- Summary table export

The evaluation sample selection uses random seed `42`. The model prediction files are collected separately and combined for analysis.

---

## Limitations

- The evaluation uses 200 Malayalam questions rather than the full benchmark.
- Only three instruction-tuned models are evaluated.
- The task is limited to four-option reading comprehension.
- The constrained next-token answer selection differs from free-form response generation.
- The findings may be affected by model tokenization, prompting language, and answer-label tokenization.
- Bootstrap confidence intervals quantify sample uncertainty but do not eliminate dataset or methodological limitations.
- Results should not be generalized to all Malayalam language tasks or domains.

---

## Repository Contents

| File | Description |
|---|---|
| `malayalam_llm_eval.ipynb` | Notebook containing experimental setup, evaluation, and analysis |

Prediction files and generated result artifacts can be added to the repository when appropriate, after checking file contents and ensuring no sensitive credentials or unnecessary data are included.

---

## Future Work

- Extend evaluation to additional Malayalam benchmarks and larger samples.
- Evaluate more models and model sizes.
- Investigate prompting-language effects in greater depth.
- Conduct more detailed qualitative error analysis.
- Explore additional statistical comparisons and uncertainty reporting.
- Compare constrained-choice inference with free-form generation and answer parsing.
- Extend the study to other Indian languages.

---

## Citation

Benchmark: [Belebele multilingual reading comprehension dataset](https://huggingface.co/datasets/facebook/belebele)

This repository is intended for research, experimentation, and reproducible analysis of multilingual language-model performance on Malayalam reading comprehension.
