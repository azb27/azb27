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

## Skeptic: an LLM auditor whose only job is to kill trading strategies

```text
$ skeptic audit examples/fade_leaky
REJECT  [centered_window]  (strategy.py:9)
The "typical move" baseline is computed with `rolling(params["span"], center=True, ...)`, a centered window
that uses future bars to judge whether the current move is a "shock" — a clear look-ahead leak. [...]
```

It reads a strategy's code and research notes, runs deterministic checks (look-ahead, costs, random entries, deflated Sharpe), and returns REJECT or SURVIVES CHECKS with file:line evidence. It never says "profitable". Measured on 90 strategies with planted flaws and planted real edges:

| | Verdict correct [95% CI] | Right reason | Leak line found | Real edges rejected |
|---|---|---|---:|---:|
| Fixed rules, no LLM | 94% [89, 99] | 49% | 0% | 0 / 30 |
| Claude Sonnet 5 + the checks | **97%** [92, 100] | **88%** | **89%** | 3 / 30 |
| Claude Sonnet 5 reading code, no checks | 71% [67, 77] | 52% | 91% | **25 / 30** |

- **The honest headline:** on verdicts alone the LLM doesn't beat fixed rules (p = 0.73). Its value is *why* and *where*. And without the statistics it rejects almost every real edge, which is why the model never produces a number.
- **Audited my own gold system:** I pre-registered tests for my XAUUSD intraday system, and they failed. Re-run on an independent price feed it lost **−0.118R per trade** at a $0.50 spread (my original run: −0.123R), and Skeptic's checks found no edge: the spread is the whole loss. The write-up also lists three gaps in my own research process that the auditor missed.
- **Ships three ways:** a CLI, an MCP server, and a `/skeptic` command for Claude Code, which went 20/20 on bench cases fixed in advance.
- **Checked its own claims:** reran its errors (7 of 9 come back right: borderline calls, not a blind spot), removed a hint I had added after seeing them, and had the README fact-checked before shipping.

[Repo](https://github.com/azb27/skeptic) · [Bench report](https://github.com/azb27/skeptic/blob/main/docs/results/bench.md) · [Where it fails](https://github.com/azb27/skeptic/blob/main/docs/results/failure-analysis.md) · [Case study](https://github.com/azb27/skeptic/blob/main/docs/case-study-gold-sniper.md)

---

## Quant Research Lab

Risk and derivatives engines, unit-tested and CI-validated. [Repo](https://github.com/azb27/quant-research-lab)

- **VaR / ES engine:** historical, correlated Monte Carlo, Student-t copula and EVT tails, backtested with Kupiec and Christoffersen tests
- **Heston:** characteristic-function pricing, Andersen QE Monte Carlo, calibration
- **Local vol:** SVI slices → arbitrage-aware interpolation → Dupire surface
- **Stat-arb:** Kalman-filter hedge ratios, transaction costs, walk-forward validation

---

## Hiring for a specific role? Start here

| Role | Where to look |
|---|---|
| Forward Deployed Engineer | Stockroom's [discovery memo](https://github.com/azb27/stockroom/blob/main/docs/engagement/discovery-memo.md), [runbook](https://github.com/azb27/stockroom/blob/main/docs/engagement/runbook.md) and [week-2 plan](https://github.com/azb27/stockroom/blob/main/docs/engagement/week-2-plan.md) |
| AI / agent engineer | Stockroom's [agent loop](https://github.com/azb27/stockroom/blob/main/src/stockroom/agent/loop.py), [design decisions](https://github.com/azb27/stockroom/tree/main/docs/adr) and [eval report](https://github.com/azb27/stockroom/blob/main/docs/results/eval.md); Skeptic's [ablations](https://github.com/azb27/skeptic/blob/main/docs/results/bench.md) (what the code, the checks and the model each add) |
| AI-native / agentic engineering | How I direct coding agents: the [spec](https://github.com/azb27/stockroom/blob/main/SPEC.md), [`CLAUDE.md`](https://github.com/azb27/stockroom/blob/main/CLAUDE.md) and [phase-by-phase build log](https://github.com/azb27/stockroom/blob/main/docs/build-log.md); Skeptic's [MCP server and Claude Code skill](https://github.com/azb27/skeptic/blob/main/docs/adr/0003-ship-as-folder-contract-plus-mcp-checks.md) |
| Quant research / quant dev | Skeptic's [case study](https://github.com/azb27/skeptic/blob/main/docs/case-study-gold-sniper.md) and [checks](https://github.com/azb27/skeptic/blob/main/src/skeptic/checks.py) (truncation leak test, deflated Sharpe, PBO), [Quant Research Lab](https://github.com/azb27/quant-research-lab), and Stockroom's [forecast backtest](https://github.com/azb27/stockroom/blob/main/docs/results/forecast_backtest.md) |
| Data science / ML | The [forecast backtest](https://github.com/azb27/stockroom/blob/main/docs/results/forecast_backtest.md) and the eval statistics |

---

## How I work

- **Numbers come from generated reports,** never typed by hand.
- **Every project says what it won't do,** and where it still fails.
- **Evals before polish.** An eval table with no UI beats a UI with no numbers.
- **Pre-register, then try to break it.** My own trading system failed its pre-registered tests, so the repo says so.
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
