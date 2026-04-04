# Week 3 Notes

This folder contains the Week 3 notebook on designing and auditing a custom classifier–judge evaluation pipeline with `inspect_ai`.

## Files

- `inspect_ai_tutorial_week_3.ipynb` — main notebook
- `logs/` — evaluation logs produced during model runs

## Environment

- Reused the project-local `.venv`
- Ran all experiments locally with Ollama models

## Main task

Build a two-stage evaluation pipeline on the Jigsaw Toxic Comment dataset:

1. A **classifier** labels a comment as `TOXIC` or `NON_TOXIC`
2. A **judge** decides whether that label is acceptable

The notebook studies both roles as objects of evaluation.

## Models used

- `ollama/llama2:latest`
- `ollama/llama3.2:latest`
- `ollama/mistral:latest`
- `ollama/gemma2:2b`
- `ollama/qwen2:1.5b-instruct-q4_K_M`
- `ollama/qwen2:7b-instruct-q4_K_M`
- `ollama/qwen2.5:7b`

## What was implemented

- A blinded `model_graded_qa` setup so the judge does not see the ground-truth label
- `compute_error_rates` to separate:
  - classifier FP / FN / failure
  - judge FP / FN / failure
- A comparison grid across multiple classifier–judge pairs
- Prompt engineering for both classifier and judge
- A larger-sample judge evaluation run
- A domain-specific scoring function
- A bonus transfer test to a second dataset

## Core findings

- Many weak configurations failed because of refusals or broken output format, not because of semantic misclassification alone
- Classifier failures propagated directly to the judge, so pipeline reliability depended heavily on upstream compliance
- Prompt engineering greatly reduced refusal and format failures, but sometimes increased false positives or changed judge strictness
- The `qwen` family appeared most practically reliable overall in this local setup

## Assignment 5 result

Configuration:

- classifier: `ollama/qwen2:7b-instruct-q4_K_M`
- judge: `ollama/qwen2:7b-instruct-q4_K_M`
- sample size: `100`

Observed rates:

- `clf_fp_rate = 0.31`
- `clf_fn_rate = 0.00`
- `clf_failure_rate = 0.01`
- `judge_fp_rate = 0.14`
- `judge_fn_rate = 0.17`
- `judge_failure_rate = 0.01`

Interpretation:

- The classifier was conservative and over-flagged benign content
- The judge was usable, but still too noisy to serve as a final evaluator without human oversight

## Assignment 6 result

Scenario:

- safety-oriented moderation setting where missed harmful content is more costly than over-flagging benign content

Weights:

- `FP = 1`
- `FN = 4`
- `failure = 3`

Best-ranked configuration on the selected comparison set:

- `ollama/qwen2:7b-instruct-q4_K_M -> ollama/qwen2:7b-instruct-q4_K_M`

## Bonus transfer test

Dataset:

- `ucirvine/sms_spam`

Configuration:

- classifier: `ollama/qwen2:7b-instruct-q4_K_M`
- judge: `ollama/qwen2:7b-instruct-q4_K_M`
- sample size: `50`

Observed rates:

- `clf_fp_rate = 0.38`
- `clf_fn_rate = 0.00`
- `clf_failure_rate = 0.02`
- `judge_fp_rate = 0.08`
- `judge_fn_rate = 0.18`
- `judge_failure_rate = 0.02`
- judge accuracy: `0.70`

Interpretation:

- The pipeline transferred successfully to an external dataset
- The judge remained usable, but its calibration shifted by task
- This supports the view that LLM judges can generalize across related tasks, but remain task-dependent and should still be validated against ground truth

## Key takeaways

- LLM judges must be audited, not assumed reliable
- Format failures and refusals can dominate early experiments
- Prompt engineering can improve compliance while worsening calibration
- A good evaluator depends on deployment priorities, not only on average accuracy
- Cross-domain transfer is possible, but evaluator behavior still shifts by task
