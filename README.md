
# RIGOR
### Research Integrity & Governance for Quantitative AI Research

**A research-credibility layer for quantitative AI research.**

RIGOR explores a central question:

> Given the results of a quantitative research workflow, how much confidence should we place in its conclusions—and what evidence supports that judgment?

Quantitative research can produce impressive historical results without establishing that those results are reliable. Data leakage, selection bias, weak validation, unrealistic execution assumptions, and irreproducible experiments can all make a strategy or model appear more convincing than it is.

RIGOR aims to assess the credibility of research results, explain detected weaknesses, and make the evidence behind an assessment inspectable.

**The goal is not another trading engine or ML dashboard. It is a layer for evaluating the credibility of the research those systems produce.**

---

## The Problem

A strong backtest does not automatically establish a strong research result.

Historical performance may be distorted by:

- **Look-ahead bias and information leakage:** Future information enters features, labels, preprocessing, or decisions.
- **Survivorship and selection bias:** The dataset or reported strategy omits failures or reflects choices made after observing results.
- **Weak temporal validation:** Evaluation does not reflect the information available when a prediction or trade would have occurred.
- **Repeated experimentation:** Many variants are tested, but only the strongest result is reported.
- **Backtest overfitting:** A strategy fits historical noise rather than a durable pattern.
- **Unrealistic execution assumptions:** Fees, spread, slippage, latency, liquidity, or market impact are ignored or understated.
- **Regime dependence:** Performance relies on a narrow set of market conditions.
- **Poor reproducibility:** The data, code, parameters, assumptions, and artifacts behind a result cannot be reconstructed.

These are distinct problems. Each requires appropriate evidence and methods, and no single metric can establish that a research result is trustworthy.

## The Proposed Solution

RIGOR's first intended capability is a **Research Credibility Report**.

A researcher provides a backtest or model result in a supported format, together with the available dataset manifest, experiment configuration, assumptions, and relevant artifacts.

RIGOR evaluates the supplied evidence and produces a report describing what appears credible, what appears fragile, what potential validity problems were found, and what cannot yet be concluded.

### Proposed Assessment Areas

| Assessment area | Questions RIGOR should investigate |
|---|---|
| Data integrity | Are timestamps, data provenance, transformations, and feature construction consistent with the information available at the time? |
| Reproducibility | Are the data, code, dependencies, configuration, parameters, and artifacts sufficiently documented to reconstruct the experiment? |
| Temporal validation | Does the evaluation respect time ordering, prediction horizons, and the experiment's intended use? |
| Research selection | How many trials or strategy variants were explored, and how might selection bias affect the reported result? |
| Overfitting risk | What evidence is available about the likelihood that historical performance reflects noise or research overfitting? |
| Execution realism | Does performance remain plausible under alternative fees, spread, slippage, latency, liquidity, and market-impact assumptions? |
| Regime robustness | Does the result depend heavily on particular market periods or conditions? |
| Model behavior | Which inputs or variables appear to influence predictions, and what are the limits of that interpretation? |
| Stress testing | Does the result survive relevant perturbations, alternative assumptions, and out-of-sample evaluation? |

These are intended capabilities, not claims that the checks already exist. Their methods, input requirements, and limitations must be specified and tested before they can support a meaningful report.

## Evidence Before Scores

RIGOR should not reduce a complex research assessment to an unexplained "AI score."

Every finding should, where applicable, identify:

- The check performed and the reason for performing it
- The evidence and inputs used
- The method, assumptions, and relevant thresholds
- The result and its severity
- Limitations, uncertainty, and missing evidence
- The implications for interpreting the original research claim

An aggregate grade may eventually be useful, but only if its construction is transparent and the underlying evidence remains available.

A missing check must not silently count as a passed check.

## Intended Verdicts

The initial design may use four broad outcome categories:

- **Robust:** The available evidence supports the result under the conditions tested.
- **Fragile:** The result deteriorates materially under plausible changes or stress tests.
- **Biased:** A material validity problem is detected.
- **Inconclusive:** The available evidence is insufficient to support a defensible assessment.

These categories are provisional. They are not validated guarantees, and a result may exhibit more than one issue. The reporting design must preserve the specific findings rather than hide them behind a single label.

## Initial Scope

The first version is deliberately narrow:

> A researcher submits a supported backtest result and its available research evidence. RIGOR produces a structured, evidence-backed assessment of the result's credibility.

The initial focus is on defining the inputs, checks, report format, and evidence needed to justify each finding.

Integration with particular research engines or experiment-tracking systems should follow from that work, rather than dictate it.

### Non-goals

RIGOR is not intended to:

- Generate buy/sell signals or investment recommendations
- Execute trades or operate a live-trading system
- Rebuild a full quantitative backtesting engine
- Replace general-purpose experiment tracking
- Guarantee that a strategy will be profitable in the future
- Certify a strategy as safe merely because it passes a finite set of tests

A credibility assessment is conditional on the supplied data, assumptions, artifacts, and methods. It cannot establish future profitability or eliminate every possible source of research error.

## Why Another Layer?

Substantial infrastructure already exists for quantitative research, backtesting, and experiment tracking:

- [QuantConnect LEAN](https://www.quantconnect.com/docs/v2/writing-algorithms/key-concepts/algorithm-engine) supports algorithm research, backtesting, and live-trading workflows.
- [Microsoft Qlib](https://github.com/microsoft/qlib) supports quantitative investment research and machine-learning workflows.
- [MLflow Tracking](https://mlflow.org/docs/latest/tracking/) records experiment parameters, metrics, code versions, and artifacts.

RIGOR's proposed focus is different: evaluating the credibility of the conclusions produced by research workflows.

That distinction is a hypothesis to validate, not a claim that existing tools lack all relevant capabilities. Researching their existing checks and extension points is part of the project.

---

## Research Agenda

RIGOR is currently in the **study and research-design stage**.

The reading list below establishes the subjects to investigate before committing to specific evaluation methods.

For each source, document:

1. What problem or failure mode does it address?
2. What assumptions does its method make?
3. What data and evidence does it require?
4. What can it detect, and what can it not establish?
5. How might its ideas translate into a RIGOR check?
6. How could that check be tested against counterexamples?

### 1. Financial Time Series

**Start here:** Rama Cont, ["Empirical properties of asset returns"](https://doi.org/10.1080/713665670).

Investigate returns, volatility, heavy tails, dependence, non-stationarity, and changing market conditions. These properties affect the assumptions behind validation procedures and performance statistics.

### 2. Research Protocol and Backtesting Discipline

Arnott, Harvey, and Markowitz, ["A Backtesting Protocol in the Era of Machine Learning"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3275654).

Study how hypotheses are formed, how many experiments are conducted, how model choices are made, and whether the final evaluation remains independent of those choices.

### 3. Leakage and Data Snooping

- David, ["Information leakage in financial machine learning research"](https://doi.org/10.3233/AF-190900)
- Halbert White, ["A Reality Check for Data Snooping"](https://doi.org/10.1111/1468-0262.00152)
- Sullivan, Timmermann, and White, ["Data-Snooping, Technical Trading Rule Performance, and the Bootstrap"](https://www.fmg.ac.uk/publications/discussion-papers/data-snooping-technical-trading-rule-performance-and-bootstrap)

Distinguish direct information leakage from the broader problem of selecting the best result after testing many alternatives. Study the evidence each method needs and the situations in which it may be inappropriate.

### 4. Multiple Testing and Backtest Overfitting

- Harvey, Liu, and Zhu, ["...and the Cross-Section of Expected Returns"](https://www.nber.org/papers/w20592)
- Bailey et al., ["The Probability of Backtest Overfitting"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2326253)
- Bailey and López de Prado, ["The Deflated Sharpe Ratio"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2460551)

Investigate how multiple trials affect the interpretation of performance, what assumptions underlie overfitting diagnostics, and why a reported probability or adjusted metric is not a universal certificate of validity.

### 5. Time-Series Validation

- Bergmeir, Hyndman, and Koo, ["A note on the validity of cross-validation for evaluating autoregressive time series prediction"](https://robjhyndman.com/papers/cv-wp.pdf)
- scikit-learn, [TimeSeriesSplit documentation](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)

Study chronological validation, walk-forward evaluation, untouched holdouts, prediction horizons, overlapping labels, and when purging or embargoing may be appropriate.

The objective is not to declare one splitter universally correct. Validation must match the data, target, feature construction, and intended research use.

### 6. Existing Research Infrastructure

- [Qlib paper](https://arxiv.org/abs/2009.11189)
- [Microsoft Qlib repository](https://github.com/microsoft/qlib)
- [QuantConnect LEAN Algorithm Engine documentation](https://www.quantconnect.com/docs/v2/writing-algorithms/key-concepts/algorithm-engine)
- [MLflow Tracking documentation](https://mlflow.org/docs/latest/tracking/)
- Jane Street, [Quantitative Research](https://www.janestreet.com/quantitative-research/)

Compare their capabilities, assumptions, outputs, and extension points. Identify what they already handle well before deciding where an additional credibility layer could provide value.

Jane Street's description is an example of one firm's research practice, not a universal blueprint.

## Research Notes

As the literature review grows, findings should be recorded in a consistent format:

- **Source:** Paper, documentation, or other reference
- **Problem:** The failure mode being studied
- **Method:** The proposed statistical or engineering approach
- **Assumptions:** Conditions needed for the method to be valid
- **Limitations:** Cases where the method may fail or mislead
- **RIGOR relevance:** A possible check, report finding, or design decision
- **Open questions:** What still needs investigation

The purpose of these notes is to preserve reasoning and evidence behind design decisions—not to imply that a method has been implemented merely because it has been studied.

---

## Project Principles

1. **Evidence over opaque scores.** Findings must be traceable to inputs and methods.
2. **Explicit assumptions.** Unsupported assumptions and missing evidence must be visible.
3. **Reproducibility.** Research conclusions should be connected to the artifacts and configuration that produced them.
4. **Appropriate validation.** Checks must reflect time ordering, data construction, and the research question.
5. **Adversarial testing.** Methods should be tested against both known failure cases and valid controls.
6. **Honest uncertainty.** Not detecting a problem is not the same as proving that no problem exists.
7. **Complement existing infrastructure.** Focus on research credibility instead of recreating mature engines and trackers.

## Current Status

**Stage:** Study, literature review, and research design.

The problem definition and research agenda are being established. The input contract, report specification, evaluation methods, supported formats, integrations, and implementation remain to be developed and validated.

Features described as planned or intended in this README should not be interpreted as implemented functionality.

## Disclaimer

RIGOR is a research and software project. It does not provide investment advice, generate trading recommendations, or guarantee the reliability or future profitability of any strategy. Its assessments are limited by the evidence supplied, the assumptions made, and the validity of the methods applied.
