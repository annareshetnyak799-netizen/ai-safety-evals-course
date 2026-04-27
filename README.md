# AI Safety Evals Course

Weekly notebook assignments and research notes from a 5-week course on **AI safety evaluation methodology** with **Inspect AI**, local LLMs (via Ollama), and hands-on evaluation experiments.

The repo covers the full pipeline of building reliable evaluations - from task construction and statistical rigor to LLM-as-judge auditing, agent evaluation, and the methodological choices that make safety evals actually trustworthy.

---

## What's inside

Each week is a self-contained module with a completed Jupyter notebook and a README summarizing goals, experiments, and findings.

### [Week 1 - Inspect AI fundamentals and position bias](./week1)

Hands-on intro to Inspect AI's core abstractions: `Task`, `Sample`, `solver`, `scorer`. Built a sentiment classification benchmark, compared local models, and ran a **position bias experiment** with chi-squared significance testing.

**Key findings:** `qwen2:7b-instruct-q4_K_M` showed measurable position bias (preferring answer A). Adding `multiple_choice(cot=True)` improved accuracy and reduced the position effect.

### [Week 2 - MMLU evaluation with proper statistics](./week2)

Moved beyond raw accuracy to **uncertainty-aware evaluation**: confidence intervals for accuracy, paired t-tests for model comparison, power analysis for benchmark sizing, and clustered standard errors for passage-based benchmarks.

**Key findings:**
- On MMLU `high_school_biology`, `qwen2:7b` strongly outperformed `llama2`: paired difference 95% CI `[-0.456, -0.318]`, p < 1e-24.
- Power analysis: detecting a 5% gap requires ~980 questions; a 10% gap requires ~245.
- Chain-of-thought showed no statistically significant gain on this subset (p = 0.66, CI `[-0.035, 0.022]`).
- Clustered SE on RACE confirmed the model gap is robust to passage-level dependence (clustered diff CI `[-0.332, -0.151]`).

### [Week 3 - Auditing LLM-as-judge pipelines](./week3)

Built a **two-stage classifier–judge pipeline** on the Jigsaw Toxic Comment dataset and decomposed errors into classifier FP/FN/failure vs. judge FP/FN/failure. Ran a comparison grid across 7 local models and tested cross-domain transfer to SMS spam.

**Key findings:**
- Many "weak" configurations failed because of **refusals or broken output format**, not semantic misclassification - pipeline reliability depended heavily on upstream compliance.
- For the best `qwen2:7b → qwen2:7b` setup: classifier FP rate `0.31` / FN rate `0.00`; judge FP rate `0.14` / FN rate `0.17`.
- Cost-weighted ranking (FP=1, FN=4, failure=3) for safety-oriented moderation selected the same configuration.
- Cross-domain transfer to SMS spam preserved utility but shifted judge calibration - supports the view that **LLM judges generalize but remain task-dependent and need ground-truth validation**.

### [Week 4 - Agent evaluation on mathematical reasoning](./week4)

Evaluated a **ReAct agent** with custom tools (`modular_arithmetic`, `sympy_solve`) on the MATH-500 benchmark, restricted to tool-friendly subjects. Compared solver architectures, built a math-aware model-graded scorer, and iterated on dev/held-out splits.

**Key findings:**
- Solver comparison on toy math tasks: `generate()` alone → 42%; naive `use_tools() + generate()` loop → 8%; `react()` → **100%**.
- Naive tool access without proper loop structure **made performance worse**.
- Dev iteration: structured ReAct prompt 73% > baseline 70% > over-engineered prompt 60% - diminishing returns from prompt-only iteration.
- Held-out test (n=20): accuracy 70%, 95% Wilson CI `[48.1%, 85.5%]`. Subject breakdown showed Number Theory 100%, Intermediate Algebra 40% - most failures concentrated in inherent task difficulty rather than tool errors.

### [Week 5 - What makes a safety evaluation trustworthy](./week5)

Shifted from building evaluations to **research methodology**: why safety benchmarks differ from capability benchmarks, why measurement validity, coverage, and statistical practice matter for high-stakes decisions, and why research taste matters when easy-to-measure questions can crowd out important safety questions. Materials include a research mindmap and a goal statement on evaluation rigor.

---

## Tools and stack

- **Framework:** [Inspect AI](https://inspect.aisi.org.uk/) - UK AISI's evaluation framework, also used by Apollo Research, METR, and Redwood Research
- **Local model runtime:** Ollama (all experiments run locally without external API dependencies)
- **Models tested:** `llama2`, `llama3.2`, `mistral`, `gemma2:2b`, `qwen2:1.5b`/`7b`, `qwen2.5:3b`/`7b`
- **Datasets:** MMLU, RACE, Jigsaw Toxic Comments, SMS Spam (UCI), MATH-500
- **Evaluation primitives:** `Task`, `Sample`, `multiple_choice`, `model_graded_qa`, `react()`, custom solvers, custom scorers
- **Statistics:** confidence intervals, paired t-tests, power analysis, clustered standard errors, Wilson CI

---

## Course takeaways

Across the course, I learned to treat evaluation not as a single benchmark score, but as a **design problem** involving task construction, validity, error analysis, and careful interpretation.

The main themes I took away:

- **Accuracy without uncertainty is misleading.** Confidence intervals, paired tests, and power analysis change which conclusions are actually supported by the data.
- **Format failures dominate early experiments.** Many "model can't do this" results turn out to be "model can't follow the output format" - and that's a separate failure mode that needs separate fixes.
- **LLM judges must be audited, not assumed.** Decomposing errors into classifier vs. judge layers, and comparing across model pairs, makes weak components visible.
- **Agent evaluation is about the scaffold, not just the base model.** ReAct vs. naive tool loops gave order-of-magnitude differences on the same model with the same tools.
- **Reliable safety evaluation is a methodological problem, not just a benchmarking problem.** Choosing the right thing to measure is at least as important as measuring it well.

---

## Connection to other projects in this profile

This course provided the methodological foundation for two applied projects in the same direction:

- **[agentops-triage-poc](https://github.com/annareshetnyak799-netizen/agentops-triage-poc)** - applies the eval discipline from Weeks 3 and 4 (rubric-based scoring, decomposed metrics, LLM-judge auditing) to a bounded agentic system with explicit safety gates.
- **[agenthub-platform](https://github.com/annareshetnyak799-netizen/agenthub-platform)** - extends agent evaluation to **infrastructure-level safety** (gateway guardrails, multi-provider routing, observability for inter-agent traffic) - naturally building on the agent-evaluation work in Week 4.

---

## Author

Anna Reshetnyak => MLE transitioning into AI safety research. Currently in the project track of the Monoid AI Safety course.

