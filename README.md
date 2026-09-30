# Aziz Bohra

**I build systems that make decisions on messy numbers, and I measure when they're wrong.**

AI agents on real business data · evals with confidence intervals · forecasting and quant research · Dubai, open to remote and relocation

By day I'm a Data Scientist & AI Solutions Engineer at TCS, working on a distribution and logistics business in the UAE: messy operational data, real stakeholders, decisions with money attached. On GitHub I build public versions of that kind of work, end to end. Coding agents do a lot of the typing; specs, tests and evals decide whether the result is right.

---

## Stockroom: an ops agent for a distributor's messy data

<a href="https://github.com/azb27/stockroom"><img src="https://github.com/azb27/stockroom/raw/main/docs/images/demo.gif" width="720" alt="Stockroom demo: a stock-out question answered with a live tool trace, then a reorder drafted and approved by a named human"></a>

Same model, same 120 ground-truth questions:

| | Accuracy [95% CI] | $ / question |
|---|---|---|
| Claude Sonnet 5 with my data-cleaning layer | **100%** [100, 100] | $0.021 |
| Claude Sonnet 5 on the raw data | **63%** [55, 72] | $0.029 |

- **Run as a forward-deployed engagement:** discovery memo → data-quality pass → six tools, each traced to a customer pain point → eval → runbook → week-2 plan.
- **Human in the loop by construction:** the agent drafts purchase orders; approving one takes a named human, on a path the agent can't reach.
- **Evaluation I'd trust:** deterministic scoring (no LLM judge), bootstrap CIs, McNemar tests, and a written analysis of where it fails. The eval also caught a real product bug, now fixed.
- **Portable:** the same tools ship as an MCP server. Claude Code scored 8/8 through it with its own file and shell tools switched off.
- **Forecasting:** a LightGBM demand model beats the customer's method by +3.6 pp WAPE (95% CI [+3.3, +4.0]) on a leak-proof backtest, and the report names the segment where it loses.

[Repo](https://github.com/azb27/stockroom) · [Eval report](https://github.com/azb27/stockroom/blob/main/docs/results/eval.md) · [Where it fails](https://github.com/azb27/stockroom/blob/main/docs/results/eval_failure_analysis.md) · [Build log](https://github.com/azb27/stockroom/blob/main/docs/build-log.md)

---

## Quant Research Lab

Risk and derivatives engines, unit-tested and CI-validated. [Repo](https://github.com/azb27/quant-research-lab)

- **VaR / ES engine:** historical, correlated Monte Carlo, Student-t copula and EVT tails, backtested with Kupiec and Christoffersen tests
- **Heston:** characteristic-function pricing, Andersen QE Monte Carlo, calibration
- **Local vol:** SVI slices → arbitrage-aware interpolation → Dupire surface
- **Stat-arb:** Kalman-filter hedge ratios, transaction costs, walk-forward validation

---

## Next: Skeptic

A research agent pointed at my own XAUUSD intraday mean-reversion pipeline. The pipeline looks strong on directional accuracy under walk-forward validation, which is exactly why I don't trust it yet. Skeptic's job is to find out whether it survives transaction costs, leakage checks and multiple-testing correction, and to say so plainly if it doesn't.

---

## Hiring for a specific role? Start here

| Role | Where to look |
|---|---|
| Forward Deployed Engineer | Stockroom's [discovery memo](https://github.com/azb27/stockroom/blob/main/docs/engagement/discovery-memo.md), [runbook](https://github.com/azb27/stockroom/blob/main/docs/engagement/runbook.md) and [week-2 plan](https://github.com/azb27/stockroom/blob/main/docs/engagement/week-2-plan.md) |
| AI / agent engineer | The [agent loop](https://github.com/azb27/stockroom/blob/main/src/stockroom/agent/loop.py), [design decisions](https://github.com/azb27/stockroom/tree/main/docs/adr) and [eval report](https://github.com/azb27/stockroom/blob/main/docs/results/eval.md) |
| AI-native / agentic engineering | How I direct coding agents: the [spec](https://github.com/azb27/stockroom/blob/main/SPEC.md), [`CLAUDE.md`](https://github.com/azb27/stockroom/blob/main/CLAUDE.md) and [phase-by-phase build log](https://github.com/azb27/stockroom/blob/main/docs/build-log.md) |
| Quant research / quant dev | [Quant Research Lab](https://github.com/azb27/quant-research-lab) and Stockroom's [forecast backtest](https://github.com/azb27/stockroom/blob/main/docs/results/forecast_backtest.md) |
| Data science / ML | The [forecast backtest](https://github.com/azb27/stockroom/blob/main/docs/results/forecast_backtest.md) and the eval statistics |

---

## How I work

- **Numbers come from generated reports,** never typed by hand.
- **Every project says what it won't do,** and where it still fails.
- **Evals before polish.** An eval table with no UI beats a UI with no numbers.
- **Markets are noisy and so is ERP data.** The discipline is the same: hold out honestly, quantify uncertainty, name the failure cases.

---

## Background

- **Data Scientist & AI Solutions Engineer, TCS (Choithrams Group)** (Dubai, 2026–): sole owner of the data-process redesign for a distribution and logistics business; replaced a multi-day, multi-person ERP-to-deck reporting cycle with an automated pipeline
- **ML Engineer, EasyBits** (Berlin, remote): cut inference from 120 ms to 42 ms with quantization, distillation and ONNX/TensorRT; built the evaluation thresholds that became the production-readiness standard across 3 product lines
- **Data Scientist intern, Sony PlayStation:** churn and lifetime-value modelling on 5M+ users
- **Software Quality Engineer, Writer:** test harnesses for an enterprise generative-AI platform
- **M.S. Applied Data Science, Carnegie Mellon** · B.S. Data Science, University of Pittsburgh

**Stack:** Python, SQL, DuckDB, LightGBM · Anthropic API, MCP, Claude Code · FastAPI, Next.js, Docker, GitHub Actions · NumPy, SciPy, statistical testing and Monte Carlo

---

[LinkedIn](https://www.linkedin.com/in/azizbohra/) · [aziz1605@outlook.com](mailto:aziz1605@outlook.com)
