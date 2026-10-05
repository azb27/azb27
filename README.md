# Aziz Bohra

**I build systems that make decisions on messy numbers, and I measure when they're wrong.**

AI products on real business data, end to end · evals with confidence intervals · forecasting and quant research · Dubai, open to remote and relocation

By day I'm a Data Scientist & AI Solutions Engineer at TCS, working on a distribution and logistics business in the UAE: messy operational data, real stakeholders, decisions with money attached. On GitHub I build public versions of that kind of work, end to end. Coding agents do a lot of the typing; specs, tests and evals decide whether the result is right.

---

## Orderdesk: WhatsApp orders in four languages, drafted for a person to confirm

<a href="https://github.com/azb27/orderdesk"><img src="https://github.com/azb27/orderdesk/raw/main/docs/images/demo.gif" width="720" alt="Orderdesk demo: an Arabizi WhatsApp order becomes a draft, an out-of-stock line is swapped, the order is confirmed and posted to the ERP with a reply in Arabizi; then a photo of a handwritten list becomes a six-line draft"></a>

Gulf retailers order stock over WhatsApp in English, Arabic, Arabizi and Roman Urdu, and sometimes send a photo of a handwritten list. Orderdesk drafts the sales order with evidence for every line, and an order-taker confirms it. On 300 test conversations (1,902 lines), scored line by line:

| | Lines found exactly [95% CI] | Orders exactly right | $ / conversation |
|---|---|---|---|
| Fuzzy matching, no model | 56.4% [52.0, 60.8] | 20.7% | $0 |
| Claude Haiku 4.5 pipeline | 88.9% [86.7, 90.9] | 57.0% | $0.012 |
| Claude Sonnet 5 pipeline | **93.5%** [92.0, 94.8] | **72.7%** | $0.021 |

- **A full product, not a notebook:**
  - a React 19 + TypeScript console: hand-written CSS, keyboard-first, Arabic right to left;
  - FastAPI on Postgres: a `SKIP LOCKED` job queue, and live updates over `LISTEN/NOTIFY`;
  - a WhatsApp Cloud API webhook with signature checks;
  - a mock ERP whose idempotency key survives fault injection;
  - Playwright tests against the same Docker image that deploys.
- **The model never produces an id, a price or a total.** It reads the message and chooses from candidates it was given; code does every number.
- **An honest eval:**
  - A hand-written set the generator never saw scores lower: 80.5% of lines found.
  - The customer's order history matters more than the model: 56.2% of lines without it.
  - The failure analysis traced every error to the step that lost it. Three code fixes followed, re-measured from the response cache.
- **Run as an engagement:**
  - discovery memo and process map;
  - a rollout plan with measured gates (shadow, then assist, then narrow auto-confirm);
  - a runbook, a data-handling note and a week-2 plan.

[Repo](https://github.com/azb27/orderdesk) · [Eval report](https://github.com/azb27/orderdesk/blob/main/docs/results/eval.md) · [Failure analysis](https://github.com/azb27/orderdesk/blob/main/docs/engagement/failure-analysis.md) · [Rollout plan](https://github.com/azb27/orderdesk/blob/main/docs/engagement/rollout-plan.md)

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
| Forward Deployed Engineer | Orderdesk's [engagement pack](https://github.com/azb27/orderdesk/tree/main/docs/engagement) (process map, [rollout plan](https://github.com/azb27/orderdesk/blob/main/docs/engagement/rollout-plan.md), runbook, data-handling note) and Stockroom's [discovery memo](https://github.com/azb27/stockroom/blob/main/docs/engagement/discovery-memo.md) and [week-2 plan](https://github.com/azb27/stockroom/blob/main/docs/engagement/week-2-plan.md) |
| Full-stack / product engineer | Orderdesk's [React console](https://github.com/azb27/orderdesk/tree/main/web/src), [Postgres job queue and live updates](https://github.com/azb27/orderdesk/blob/main/docs/adr/0002-postgres-is-the-only-state.md), [webhook contract](https://github.com/azb27/orderdesk/blob/main/docs/adr/0003-a-simulator-that-speaks-the-cloud-api.md) and [CI with end-to-end tests](https://github.com/azb27/orderdesk/blob/main/.github/workflows/ci.yml) |
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

**Stack:** Python, SQL, Postgres, DuckDB, LightGBM · Anthropic API, MCP, Claude Code · FastAPI, React, TypeScript, CSS, Next.js · Docker, GitHub Actions, Playwright · NumPy, SciPy, statistical testing and Monte Carlo

---

[LinkedIn](https://www.linkedin.com/in/azizbohra/) · [aziz1605@outlook.com](mailto:aziz1605@outlook.com)
