# The Brenner Worksheet (Executable)

A domain-independent, fill-in-the-blanks protocol distilled from the Brenner
Method, plus a worked example from options trading.

This is the "Brenner Loop" from
[`specs/operator_library_v0.1.md`](../specs/operator_library_v0.1.md) turned
into a runnable worksheet. It is gated by
[`specs/evaluation_rubric_v0.1.md`](../specs/evaluation_rubric_v0.1.md) and emits
the same artifact sections the `brenner` CLI tracks, so you can run it by hand on
paper **or** drive it through the CLI (see
[Running it through the CLI](#running-it-through-the-cli)).

The two axioms it rests on (from the distillations):

1. **Reality has a generative grammar** — phenomena are *produced* by causal
   machinery with discoverable rules, not just correlations.
2. **To understand is to be able to reconstruct** — you have not explained
   something until you could, in principle, rebuild it from primitives.

Neither mentions biology. That is why the method ports to any system produced by
hidden rules: code, markets, legal regimes, trained models, organizations.

> Scoring philosophy (from the rubric): **discriminative power over
> thoroughness.** A single hypothesis that kills an alternative is worth more
> than ten interesting observations.

---

## The artifact you are filling (7 sections)

```
research_thread        the question, in one decidable sentence
hypothesis_slate       H1, H2, H3 (third alternative), each level-typed
assumption_ledger      load-bearing assumptions; flag scale_check ones
predictions_table      per hypothesis: "if true, I'd see ___"
discriminative_tests   killer tests + potency check, ranked by likelihood ratio
anomaly_register       quarantined exceptions (do they cluster, or scatter?)
adversarial_critique   the strongest attack on your own front-runner
```

---

## The passes

Run in order. Each pass cites the operator that drives it and the artifact
section it writes.

| #  | Operator | What you write down |
|----|----------|---------------------|
| 0  | ◊ **Paradox-Hunt** | State two things you believe that *cannot both be true*. That is your entry point. → `research_thread` |
| 1  | ⊘ **Level-Split** | Name the level confusion. Separate **signal** (the real structural effect) from **implementation** (your execution of it); separate **won't** (no effect) from **can't** (your tooling failed). → 2–3 typed hypotheses in `hypothesis_slate`, including a genuine **third alternative** (orthogonal, not a blend) |
| 2  | 𝓛 **Recode** | Change coordinates so the rivals *disagree*. Reduce dimensionality — collapse the high-D path into the few primitives the system computes in ("machine language"). → sharpen `hypothesis_slate` mechanisms |
| 3  | ⊞ **Scale-Check** | Put real numbers + units on it *before* theorizing. What order-of-magnitude fact rules a hypothesis out? → `assumption_ledger` (mark `scale_check: true`) |
| 4  | ≡ **Invariant-Extract** | What must hold regardless of mechanism? Use it to delete whole model families. → constraints in `predictions_table` |
| 5  | ⌂ **Materialize** | For each H: "if this were true, what would I *see*?" Shortest path to a discriminating observation. → `predictions_table` |
| 6  | ✂ **Exclusion-Test** | For each H write its **forbidden pattern** (what must NOT appear if it is true). Design the cheapest test that probes a forbidden pattern. → `discriminative_tests` |
| 7  | ⟂ **Object-Transpose** | Do not accept the inherited test system. Find the substrate where the decisive observation is cheap/fast/clean (the "discount organism"). → re-target `discriminative_tests` |
| 8  | ↑ **Amplify** + 👁 **HAL** | Pick a regime where the signal is *loud* and the readout is **digital** (survive/blow-up). Before fancy inference — just *look* at the raw distribution. |
| 9  | 🔧 **DIY** + ⚡ **Quickie** | Build the crude instrument that is *good enough to decide*; run the cheap pilot that could kill the thesis in an afternoon before you commit months. |
| 10 | 🎭 **Potency-Check** | A positive control: something that *should* show the effect. If even that fails, your tool is broken (impotence), not the hypothesis (chastity). → attach to each test |
| 11 | ΔE **Quarantine** / † **Kill** / ∿ **Dephase** | Anomalies that cluster → the core is wrong. Pre-commit the **kill condition**. If the field is crowded, move out of phase. → `anomaly_register`, KILL rows |

---

## Pass/Fail gates

A contribution that fails these is rejected (from the rubric):

- **Discriminative power ≥ 2/3** — outcomes must differ per hypothesis, ideally
  binary/large. A test that gives the same result under every H scores 0.
- **Potency check ≥ 2/3** — you can tell "no effect" from "assay failed."
  *No potency check = automatic test rejection.*
- **Third alternative present** — a slate with only H1/H2 is "incomplete."
  "Both could be wrong" alone is a placeholder (scores 1).
- **Scale check on every mechanism claim** — hand-waved "should work" scores 0.
- **Kill justified and pre-committed** — "this seems unlikely / it's getting
  complicated" is an unjustified kill.

---

## Worked examples

The same passes, gates, and artifact sections run unchanged across domains; only
the substrate changes. That portability is the whole claim of the method. Two
runs follow — one financial, one in distributed systems.

## Worked example 1: a 0DTE SPX option-selling strategy

### Pass 0 — ◊ Paradox-Hunt → `research_thread`

> "My backtest of selling 0DTE SPX put credit spreads shows a Sharpe of ~2.5 and
> wins ~95% of days — **yet** it is a known fact that systematic
> short-gamma/short-vol books have fat left tails and most retail
> premium-sellers eventually blow up. Both cannot be true."

The paradox **is** the entry point. Decidable question: *Is there a real,
harvestable edge here, or is the equity curve a tail-risk premium being
mis-booked as alpha?*

### Pass 1–2 — ⊘ Level-Split + 𝓛 Recode → `hypothesis_slate`

The level confusion: conflating **"the variance risk premium is real"** (signal
— structurally true; IV prints above subsequent RV on average) with **"my
strategy harvests it"** (implementation — fills, exits, sizing). And conflating
*won't* with *can't*.

**Recode (dimensional reduction):** stop looking at the aggregate equity curve
(high-D, path-dependent). Decompose each day's P&L into its **machine language —
the Greeks**:

```
P&L = theta(time) + vega*dSigma + 0.5*Gamma*(dS)^2 + delta*dS + slippage
```

That is the 1-D representation where the rivals disagree.

| H | Level type | Claim |
|---|-----------|-------|
| **H1 — Real edge** | signal | A genuine VRP: you are paid theta faster than gamma losses accrue. Survives realistic fills. |
| **H2 — Backtest artifact** | implementation | Edge lives in mid-price fills and a benign sample (no vol spike in window). Execution fiction + lucky path. |
| **H3 — Tail premium mis-booked (third alternative)** | *orthogonal* | The edge is *real but not alpha* — it is an **insurance premium for the left tail you have not sampled yet**. Booked as profit; should be reserved against catastrophe. (Imported via ⊕ Cross-Domain from insurance: insurers reserve premium, they do not book it.) |

H3 is genuinely orthogonal: it agrees the cash flow is real (vs H2) but denies
it is free money (vs H1).

### Pass 3 — ⊞ Scale-Check → `assumption_ledger` (the move that kills theories for free)

- SPX VRP ≈ IV − subsequent RV ≈ **~3–4 vol points** on average → translate to
  **expected $ credit per spread per day**.
- Against that: **bid/ask half-spread + fees per round trip**. `scale_check`: *if
  per-trade edge ≈ transaction cost, no backtest Sharpe survives live — full
  stop.*
- Tail arithmetic: a 0DTE put spread is short `X:1` (max loss : credit). One
  max-loss day erases ~`X` winning days. With a 95% win rate, a 5%-probability
  max-loss day has expectancy `0.95*(+1) − 0.05*(X)`. **If X > 19, expectancy is
  negative** — the win rate is cosmetic. This single calculation is the
  imprisoned-imagination filter.

### Pass 4–5 — ≡ Invariant + ⌂ Materialize → `predictions_table`

**Invariant (holds regardless of model):** under no-arbitrage the risk-neutral
expected return of any option position is the risk-free rate. *Positive expected
return can only come from bearing a risk someone pays to shed.* So the question
is not "does it make money" — it is **"which risk am I being paid for, and when
does the bill come due?"**

| H | Materialized observable |
|---|-------------------------|
| H1 | P&L dominated by **theta capture**; gamma losses small and bounded; **edge persists at bid/ask fills** |
| H2 | Edge **collapses to ~0 when re-marked at bid/ask** instead of mid; concentrated in one calm sub-period |
| H3 | Long stretches of small theta gains **punctuated by rare gamma-driven losses that wipe out months**; strongly negative skew |

### Pass 6–7 — ✂ Exclusion-Test + ⟂ Object-Transpose → `discriminative_tests`

**Forbidden patterns:**

- H1 is **forbidden** if the edge vanishes under realistic fills, or if one tail
  day > several months of gains.
- H2 is **forbidden** if the edge *persists* out-of-sample through a real vol
  spike.
- H3 is **forbidden** if, across the worst historical 0DTE days, no single day
  causes catastrophic drawdown.

**⟂ Transpose the test object** — do not pay for a year of live trading
(expensive, slow, ambiguous) to learn this. The cheap discriminating substrate
is your *existing backtest, re-marked*:

> **Killer test:** Re-run the exact backtest with two changes: (1) fills at
> **bid/ask minus a slippage haircut**, and (2) force-include the known tail days
> — **5 Feb 2018, 16 Mar 2020, 5 Aug 2024**. Read the left tail.

This one test discriminates all three: kills H2 if edge survives fills; kills H1
if a tail day blows the curve; confirms H3 if the shape is "theta drip + rare
gamma bomb."

### Pass 8–10 — ↑ Amplify / 👁 HAL / 🔧 DIY / ⚡ Quickie / 🎭 Potency

- **↑ Amplify + digital readout:** do not measure subtle daily edge — run the
  regime where the signal is *loud* (the 5 worst days) and make the readout
  binary: **survive vs. blow up.** Seven-cycle-log-paper logic: if it is real you
  will not need statistics to see it.
- **👁 HAL:** before computing any Sharpe, *just plot* the 10 worst days and the
  P&L histogram. Look at the left tail directly.
- **🔧 DIY + ⚡ Quickie:** you do not need a tick-data vendor — a crude
  `bid/ask + haircut` fill model is *good enough to decide*. The whole re-mark is
  an afternoon's work and can kill the thesis before you risk a dollar.
- **🎭 Potency check (chastity vs impotence):** if it bleeds, is there no edge
  (chastity) or is your fill model just punishing (impotence)? **Positive
  control:** simulate a clean, liquid, long-dated theta position you *know*
  should capture VRP. If even that shows no edge under your fill model, your
  *instrument* is broken — fix it before concluding anything about 0DTE.

### Pass 11 — ΔE / † / ∿ → `anomaly_register`, kill condition, `adversarial_critique`

- **ΔE Quarantine:** the big-loss days — do they **cluster** on vol spikes (→ the
  core thesis is a short-vol bomb, H3) or **scatter** idiosyncratically (→
  noise)? Clustering is the tell.
- **† Pre-committed kill condition (write it *before* running):**
  > "Kill H1 ('harvestable alpha') and reclassify as H3 ('tail premium') if
  > EITHER re-marking at realistic fills drops annualized edge below transaction
  > costs, OR any single day's loss exceeds 3 months of accumulated gains."
- **∿ Dephase:** 0DTE SPX selling is now extremely crowded — peak fashion. The
  honest prior is that easy VRP is competed away precisely where everyone is
  piled in; that *raises* the odds of H2/H3.
- **adversarial_critique:** strongest attack on H1 → "Your 95% win rate and 2.5
  Sharpe are exactly the signature insurers show right up until the catastrophe
  year; you have sampled the calm and called it skill."

### Verdict

Not "is it good?" but a **decisive reclassification**: with high prior
probability this is **H3 — a real but non-alpha tail-risk premium**. The
actionable output is not "trade it / don't" — it is *"if you trade it, book it
like an insurer: reserve against the tail and size so one Aug-5 day cannot end
the book,"* a completely different decision than "I found a Sharpe-2.5 edge."
That reframing — from **alpha** to **priced insurance** — is the Brenner payoff:
you changed the representation until the truth became cheap to see.

---

## Worked example 2: "we added servers and it got *slower*"

A non-financial run, to show the loop is substrate-independent. It leans on the
operators the trading example barely used — Scale-Check, Object-Transpose, HAL.

### Pass 0 — ◊ Paradox-Hunt → `research_thread`

> "We doubled the worker fleet to handle rising load — **yet** p99 latency got
> *worse* and total throughput *dropped*. More capacity cannot make a system
> slower. Both cannot be true."

Decidable question: *Is the slowdown caused by real contention on a shared
resource, a measurement/routing artifact, or a coordination feedback loop that
more workers amplify?*

### Pass 1–2 — ⊘ Level-Split + 𝓛 Recode → `hypothesis_slate`

The level confusion: conflating **capacity** (signal — "more workers = more
compute") with **coordination** (implementation — what those workers contend
on). And conflating *can't* (a structural ceiling) with *won't* (a config knob).

**Recode to machine language:** stop staring at the aggregate latency graph
(high-D, path-dependent). Decompose each request into the primitives the system
actually computes in — a **span timeline**:

```
total = queue_wait + service_time + downstream_wait + retry_overhead
```

per hop. That is the 1-D representation where the rivals disagree.

| H | Level type | Claim |
|---|-----------|-------|
| **H1 — Shared-resource contention** | signal/structural | A downstream bottleneck (DB connection pool, lock, single partition) saturates; extra workers just pile up waiting. More workers → more contention. |
| **H2 — Routing/measurement artifact** | implementation | Load balancer is not spreading evenly (sticky sessions, stale DNS), or the p99 metric is mis-aggregated across heterogeneous fleets. Nothing is actually slower. |
| **H3 — Metastable feedback loop (third alternative)** | *orthogonal* | Not a capacity problem at all. A retry/timeout storm: slow responses trigger client retries, which add load, which slows responses further. More workers *accept* more doomed work and **amplify** the loop. (Imported via ⊕ Cross-Domain from control theory: positive feedback past a stability threshold.) |

H3 is orthogonal: it agrees the slowdown is real (vs H2) but denies adding
capacity could ever fix it (vs H1, where a bigger downstream would).

### Pass 3 — ⊞ Scale-Check → `assumption_ledger` (kills theories for free)

- DB `max_connections = 500`. Each worker holds a pool of `20`. At 25 workers
  that is `500` — exactly tapped. **Double to 50 workers → 1000 requested against
  a 500 ceiling.** `scale_check: true` — you have *starved the database*, and the
  symptom is workers blocking on connection acquisition. This alone makes H1 the
  front-runner before any experiment.
- **Little's Law** invariant: `L = lambda * W`. If concurrency-in-flight `L` is
  pinned by the pool ceiling while arrival rate `lambda` rises, wait time `W` must
  blow up. Pure arithmetic.

### Pass 4–5 — ≡ Invariant + ⌂ Materialize → `predictions_table`

**Invariant (model-independent):** the **Universal Scalability Law** — throughput
does not just plateau under contention/coherency cost, it can *retrograde*
(decrease) as you add nodes. So "more workers → less throughput" is not
paradoxical; it is the *expected* signature of a coherency-bound system. The
paradox dissolves once you are in the right coordinates.

| H | Materialized observable |
|---|-------------------------|
| H1 | `downstream_wait` (connection-acquire time) dominates slow spans; DB active-connection metric flat at its ceiling |
| H2 | Per-worker request counts are wildly uneven; slow spans concentrate on a subset of nodes; a correctly-aggregated metric shows no regression |
| H3 | `retry_overhead` and request *rate* spike together; queue depths **oscillate**; goodput (successful work) collapses while total work rises |

### Pass 6–7 — ✂ Exclusion-Test + ⟂ Object-Transpose → `discriminative_tests`

**Forbidden patterns:**

- H1 forbidden if slow spans show negligible `downstream_wait`.
- H2 forbidden if load is provably even *and* the regression persists per-node.
- H3 forbidden if retry counts are flat while latency climbs.

**⟂ Transpose the test object** — do not diagnose in prod under live traffic
(slow, risky, ambiguous). The cheap discriminating substrate is a **load-test
harness replaying captured traffic at controlled concurrency** against one
representative endpoint.

> **Killer test:** pull a distributed trace on 50 of the slowest requests and
> read where the time goes. One look classifies all three: `downstream_wait` →
> H1; node-skew → H2; `retry_overhead` + oscillation → H3.

### Pass 8–10 — ↑ Amplify / 👁 HAL / 🔧 DIY / ⚡ Quickie / 🎭 Potency

- **↑ Amplify + digital readout:** sweep worker count and make the readout binary
  — *does adding one worker raise or lower throughput?* If it **lowers** it, you
  are past the USL peak (H1/H3), not under-provisioned. Seven-cycle-log-paper
  logic: you will not need statistics to see it.
- **👁 HAL:** before any fancy analysis, *just open one slow trace.* The flame
  graph usually ends the investigation.
- **🔧 DIY + ⚡ Quickie:** do not re-architect yet. Cheapest pilot — **halve the
  fleet** and watch p99. If p99 *improves* when you remove workers, that is
  near-proof of contention/feedback (H1/H3) and instantly **kills H2**. Five
  minutes, reversible.
- **🎭 Potency check (chastity vs impotence):** is your load generator actually
  saturating the system, or is *it* the bottleneck? Positive control: hit a
  static `/healthz` on the same fleet. If even *that* degrades as you add workers,
  the problem is infra (LB/network), not your app — you would be debugging the
  wrong subsystem.

### Pass 11 — ΔE / † / ∿ → `anomaly_register`, kill condition, `adversarial_critique`

- **ΔE Quarantine:** do the slow spans **cluster** on one dependency (→ H1, a
  specific pool) or **scatter** with retries (→ H3)? Clustering localizes the
  bottleneck.
- **† Pre-committed kill condition:** "Kill H2 ('artifact') if halving the fleet
  improves p99 *and* per-node request counts are within 10% of even. Kill H1
  ('pure capacity') if raising the DB connection ceiling does **not** restore
  throughput — that promotes H3."
- **∿ Dephase:** the reflexive fix everyone reaches for is "add more workers /
  autoscale." That is exactly the move that *feeds* H1 and H3. The neglected angle
  is **reducing** concurrency (smaller pools, load-shedding, retry budgets with
  jitter).
- **adversarial_critique:** "You assumed the fleet is the independent variable.
  But you doubled workers right as organic traffic rose — you may be attributing a
  load-driven regression to the deploy. Separate the two before celebrating any
  fix."

### Verdict

The worksheet drives a **reclassification**, not a "buy more servers": with the
connection-pool arithmetic and a single trace, this is overwhelmingly **H1
contention, at risk of tipping into H3** if retries are not budgeted. The
actionable output is the *opposite* of the instinctive one — **cap concurrency
and add a retry budget**, do not add workers. Same Brenner payoff as example 1:
you changed the representation (aggregate graph → span machine language;
"capacity" → USL coherency cost) until the truth became cheap to see, and the
paradox evaporated.

---

## Running it through the CLI

There is no single `loop` command; the loop is the artifact, and each section has
its own command group. A typical run:

```sh
# 0. Open the thread with the paradox as the research question (◊).
brenner session start \
  --thread-id <id> --sender <you> --to Codex,Opus,Gemini \
  --excerpt-file <corpus-excerpt.md> \
  --question "Is the 0DTE put-spread edge real alpha, a backtest artifact, or mis-booked tail premium?"

# 1-2. Hypothesis slate, including the third alternative (⊘, 𝓛).
brenner hypothesis create --name "Real VRP edge" ...
brenner hypothesis create --name "Backtest artifact" ...
brenner hypothesis create --name "Tail premium mis-booked" --third-alternative ...

# 3. Scale-check assumptions (⊞).
brenner assumption create --scale-check "per-trade edge vs bid/ask + fees" ...

# 6-7. Discriminative tests with potency checks (✂, ⟂, 🎭).
brenner test create --name "Re-mark at bid/ask + force tail days" ...
brenner test execute <test-id> ...
brenner test suggest-kills        # which hypotheses the results refute

# 11. Quarantine anomalies, kill, critique (ΔE, †, ◊).
brenner anomaly create ...
brenner hypothesis kill <id> --reason "..."
brenner critique create ...

# Score the session against the rubric (7-dimension scorecard).
brenner score <session-id>
```

`brenner session nudge --thread-id <id>` inspects the artifact's lint gaps and
recommends the next operator to apply — e.g. "missing third alternative" routes
you to ⊕ Cross-Domain, "no potency check" routes you to 🎭. That is the loop's
`WHILE (understanding incomplete)` step, automated.

The seven scorecard dimensions (`brenner score`) are exactly the worksheet's
gates: paradox grounding, hypothesis kill rate, test discriminability, assumption
tracking, third-alternative discovery, experimental feasibility, and adversarial
pressure.

---

*Derived from `specs/operator_library_v0.1.md`,
`specs/evaluation_rubric_v0.1.md`, and the three method distillations in the repo
root.*
