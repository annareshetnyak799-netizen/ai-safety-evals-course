# Week 2 Notes

This folder contains the Week 2 notebook on evaluating LLMs on MMLU with `inspect_ai` and basic statistical analysis.

## Files

- `inspect_ai_tutorial_week_2.ipynb` — main notebook

## Environment

- Created a project-local virtual environment in `.venv`
- Installed the notebook dependencies there and switched Jupyter to that kernel
- This avoided package conflicts from the global Anaconda environment

## Current setup

- `MODEL_A = "ollama/llama2:latest"`
- `MODEL_B = "ollama/qwen2:7b-instruct-q4_K_M"`
- Working subset: `high_school_biology`

## What the notebook is doing

1. Loads MMLU and converts raw records into `inspect_ai` `Sample` objects
2. Creates a manageable benchmark subset for experiments
3. Runs evaluation tasks with `multiple_choice()` and `choice()`
4. Converts `EvalLog` into a flat DataFrame with per-question scores
5. Computes confidence intervals for accuracy
6. Visualizes how confidence intervals change with repeated runs `K` and number of questions `n`
7. Compares two models with a paired t-test
8. Estimates the uncertainty of the model gap and plans evaluation size
9. Compares baseline prompting vs chain-of-thought
10. Bonus: checks clustered standard errors on a reading comprehension benchmark

## Progress so far

- `log_to_df` works and produces rows with `id`, `epoch`, `score`, and `subject`
- Confidence interval functions passed the notebook tests
- The `K` experiment showed that more repeated runs mostly improve confidence, not the point estimate
- The `n` experiment showed that accuracy stabilizes much more clearly as the number of questions grows
- Paired comparison result:
  - `p-value = 1.0019382801225955e-24`
  - `mean difference (MODEL_A - MODEL_B) = -0.3741935483870968`
  - conclusion: `MODEL_B` strongly outperforms `MODEL_A` on this subset
- 95% CI for the model gap:
  - `MODEL_A - MODEL_B = [-0.456, -0.318]`
  - conclusion: the interval does not contain zero and agrees with the paired t-test
- Power analysis:
  - `omega2 = 0.2452`
  - `sigma2_A = 0.0333`
  - `sigma2_B = 0.0333`
  - `MDE with n=310 = 8.9%`
  - required questions:
    - `5% gap -> 980`
    - `10% gap -> 245`
- Baseline vs chain-of-thought on the same model:
  - `p-value = 0.6554508983185972`
  - `mean difference (baseline - CoT) = -0.006`
  - `95% CI = [-0.035, 0.022]`
  - conclusion: no meaningful or significant CoT gain on this subset
- Bonus clustered-SE experiment on `RACE`:
  - setup: `120` questions, `30` passage-level clusters
  - single-model accuracy on `MODEL_B`:
    - naive CI: `[0.806, 0.928]`
    - clustered CI: `[0.815, 0.918]`
  - model comparison on `RACE`:
    - naive diff CI: `[-0.334, -0.149]`
    - clustered diff CI: `[-0.332, -0.151]`
    - mean difference: `-0.242`
  - conclusion: `MODEL_B` still clearly outperforms `MODEL_A`, and the result is robust to clustered uncertainty

## Key takeaways

- Accuracy alone is not enough; we also need uncertainty estimates
- More epochs `K` help reduce response noise on the same questions
- More questions `n` usually help more because they make the benchmark estimate more stable
- Paired testing is the right way to compare models on the same questions
- Confidence intervals for differences are more informative than p-values alone because they show effect size and uncertainty together
- Power analysis helps decide whether a benchmark is suitable for detecting small versus moderate model gaps
- Chain-of-thought should be tested empirically; it did not show a reliable benefit here
- Clustered standard errors are worth checking on passage-based benchmarks because they test whether conclusions depend on a naive independence assumption

## Status

- Core Week 2 tasks completed
- Bonus clustered standard error analysis completed on `RACE`
- Remaining optional work: rerun the bonus on a larger `RACE` slice or on `RACE/high` to see whether clustered and naive uncertainty diverge more clearly

## How to keep this updated

- After each major notebook section, add:
  - the code cell you completed
  - one sentence on what it does
  - one sentence on the result or takeaway
- If you change models or subset, update the `Current setup` section first
