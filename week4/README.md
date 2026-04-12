# Week 4 Notes

This folder contains the Week 4 notebook on evaluating LLM agents for mathematical reasoning with `inspect_ai`.

## Files

- `inspect_ai_tutorial_week_4.ipynb` — main notebook
- `logs/` — agent evaluation logs

## Environment

- Reused the project-local `.venv`
- Ran all experiments locally with Ollama models
- Switched to `ollama/qwen2.5:3b` to match the tutorial setup and get working tool-calling behavior

## Main task

Evaluate a simple ReAct agent on mathematical reasoning tasks:

1. Build custom tools
2. Compare solver architectures
3. Create a math-aware scorer
4. Iterate on a dev set
5. Run a held-out test evaluation

## Tools implemented

- `modular_arithmetic(a, b)` — compute `a mod b`
- `sympy_solve(equation)` — solve equations for `x` using SymPy

## Solver comparison on toy tasks

Using `ollama/qwen2.5:3b`:

- `generate()` only: `42%`
- `use_tools() + generate()` naive loop: `8%`
- `react()` with simple prompt: `100%`

Interpretation:

- Plain generation could sometimes reason correctly but made arithmetic mistakes
- Naive tool access alone did not help and often made things worse
- ReAct scaffolding made tool use reliable and dramatically improved performance

## Math scorer

- Used `ollama/qwen2.5:7b` as the grading model
- Built a `model_graded_qa` scorer that checks mathematical equivalence rather than exact string match
- Sanity check passed on equivalent answers such as `10/4` vs `5/2`

## Dev/test setup

- Loaded `HuggingFaceH4/MATH-500`
- Restricted to tool-friendly subjects:
  - Algebra
  - Intermediate Algebra
  - Number Theory
  - Prealgebra
- Split into:
  - `DEV_SET = 36`
  - `TEST_SET = 329`

## Dev iteration results

- Attempt 1 baseline ReAct prompt: `70%`
- Attempt 2 more structured ReAct prompt: `73%`
- Attempt 3 decision-oriented prompt: `60%`

Interpretation:

- Moderate prompt engineering improved the baseline
- More complex prompt structure hurt performance
- This suggests diminishing returns from prompt-only iteration

## Held-out test result

Due to local compute limits, the final test was run on a reduced held-out subset of `20` samples.

Best configuration:

- `react()` with `MY_REACT_PROMPT_V2`
- tools: `ALL_TOOLS`
- scorer: `MATH_SCORER`

Observed result:

- accuracy: `70.0%`
- 95% Wilson CI: `[48.1%, 85.5%]`
- `n = 20`

Interpretation:

- The best dev configuration transferred reasonably well to held-out data
- The confidence interval is still wide, so the final estimate remains uncertain

## Breakdown by subject

| Subject | Correct | Total | Acc |
|---|---:|---:|---:|
| Algebra | 6 | 8 | 75% |
| Intermediate Algebra | 2 | 5 | 40% |
| Number Theory | 5 | 5 | 100% |
| Prealgebra | 1 | 2 | 50% |

## Breakdown by level

| Level | Correct | Total | Acc |
|---|---:|---:|---:|
| 1 | 1 | 2 | 50% |
| 3 | 4 | 4 | 100% |
| 4 | 9 | 12 | 75% |
| 5 | 0 | 2 | 0% |

## Bonus error analysis

Main failure modes:

- multi-step reasoning error
- tool misuse or incorrect tool choice
- symbolic tasks that exceeded the current tool set
- some harder Intermediate Algebra failures that were better explained by inherent difficulty than by simple arithmetic mistakes

## Key takeaways

- Agent quality depends on the whole scaffold, not just the base model
- Tools without the right loop structure can hurt performance
- ReAct-style scaffolding can unlock large gains, especially on tool-friendly tasks
- Prompt engineering helps, but only up to a point
- Agent evaluation is much more expensive than passive QA evaluation
- Local compute limits are part of experimental design in practice
