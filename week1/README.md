# Week 1 — Inspect AI Tutorial

## Goal

The goal of this assignment was to get familiar with the basic workflow of Inspect AI:
- creating `Task` objects,
- defining `Sample`s,
- using `solver`s and `scorer`s,
- running local evaluations with Ollama,
- analyzing logs in `inspect view`.

## What I completed

In this notebook I:
- ran a first simple evaluation with a local model,
- explored logs in `inspect view`,
- tested system prompts, prompt templates, and chain-of-thought,
- built a simple sentiment classification benchmark,
- compared different local models,
- implemented a position bias experiment,
- explored several bonus extensions.

## Main experiments and findings

### Sentiment classification
- `llama2` performed worse on neutral examples.
- `qwen2:7b-instruct-q4_K_M` achieved better results on the same classification task.

### Position bias
- `llama2` showed weak overall performance and no clear preference for position A.
- `qwen2:7b-instruct-q4_K_M` performed better when the correct answer was placed in position A, suggesting a likely position bias.

### Chain-of-Thought
- Adding `multiple_choice(cot=True)` greatly improved performance.
- It also reduced the gap between biased and unbiased settings.

### More answer choices
- The position effect persisted with 5 and 6 choices.
- However, the effect did not increase cleanly in a monotonic way.

### Statistical test
- A chi-squared test showed a visible trend, but not enough evidence for statistical significance at the 0.05 level.

## Files

- `inspect_ai_tutorial_week_1.ipynb` — completed notebook for Week 1.
