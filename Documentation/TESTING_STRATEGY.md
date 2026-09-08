# GramCare — Testing Strategy

> **Project:** GramCare — AI-Powered Offline Disease Outbreak Monitoring System
> **Document:** Testing & Verification Strategy
> **Version:** 0.1 (Review 1 — Research & System Design)
> **Status:** Baseline — plan only; no results yet
> **Last updated:** 2026-08-22
> **Team:** M. Faizan Patel · Yasa Ghanchi · Niranjan Mohite · Yash S. Waghela
> **Guide / College:** *To be finalized*

---

> **Integrity note.** This document defines *how* GramCare will be verified. It contains **no results**. Every metric table is a placeholder to be filled **after an actual run on a stated dataset** — figures must never be pre-filled or invented (extends D-008/D-009 to evaluation). All test data is synthetic and labelled **DEMO / SIMULATED DATA**.

## 1. Testing philosophy

"Testable" is the middle link of the development philosophy in D-011 (Simple → Working → **Testable** → Explainable → Extendable), so verification is planned alongside the requirements rather than bolted on at the end. The strategy is proportionate to a 3–4 month prototype on synthetic data: it favours a small set of meaningful, repeatable checks — each traceable to a requirement in `REQUIREMENTS.md` — over exhaustive test suites. The demonstration in `DEMO_SCENARIO.md` doubles as the top-level acceptance test.

## 2. Levels of testing

Testing runs at four levels. **Unit** checks isolate individual functions (encoding symptoms, computing a weekly count, applying a threshold). **Feature/integration** checks verify a requirement end to end (capture → store → sync). **System** checks run the full demo flow across both actors. **Model evaluation** measures the AI components quantitatively on a labelled split. The first three answer "does it do what the requirement says"; the fourth answers "how well does the AI perform."

## 3. Functional verification (traceability)

Each functional requirement gets at least one check with an observable pass criterion. The table maps them so nothing is untested and no test is orphaned.

| Requirement | Verification approach | Pass criterion |
|---|---|---|
| FR-HW-01…03 | Capture patient + encounter, inspect local store | Records saved with the entered fields |
| FR-HW-04, NFR-01 | Perform capture with device offline (airplane mode) | All field functions work with zero connectivity |
| FR-HW-05 | Browse/search local records offline | Records retrievable without network |
| FR-HW-06, NFR-02/03 | Sync pending records; interrupt mid-sync and resume | Status pending → synced; no loss, no duplication; resumes cleanly |
| FR-AI-01…03, NFR-07 | Submit known symptom sets | Ranked possible condition(s) + confidence + contributing-symptom reason returned |
| FR-AI-04 | Inspect every AI output label | Always "possible condition," never "diagnosis" |
| FR-AI-05, NFR-01 | Run inference offline | Result produced on-device with no network |
| FR-CA-01…03 | Feed the synthetic dataset | Cases aggregated; weekly trend 5 → 7 → 15; per-village counts A 3 / B 4 / C 18 |
| FR-CA-04 | Run clustering on the demo data | Village C and D cases grouped by symptom similarity + proximity |
| FR-CA-05/06 | Run detection on the demo data | Village C flagged as *potential*; A and B not; flag carries evidence |
| FR-DB-01…06 | Load dashboard on demo data | Overview, trend, location view, hotspot (C), alert list render correctly |
| FR-AU-01…05 | Officer reviews an alert | Evidence viewable; human decision recorded; alert can be acknowledged |

## 4. Non-functional verification

Non-functional qualities are checked with targeted tests rather than assumed. Offline availability (NFR-01) is the headline test: the full field flow is exercised in airplane mode. Reliability (NFR-02) is checked by force-closing the app and cutting sync mid-transfer, then confirming no record is lost or duplicated. Consistency (NFR-03) compares a synced record's central copy against its local source. Privacy/security (NFR-04/05) confirms identifiers are pseudonymous, data is synthetic, and no secret is stored in clear text. Usability (NFR-06) is a light observed walkthrough — can a first-time user complete register → record → view result with minimal guidance. Performance (NFR-10) times on-device inference and dashboard rendering against the **provisional** demo targets (a couple of seconds for inference), with the figures confirmed here, not asserted upstream.

## 5. AI model evaluation

### 5.1 Patient-level classifier
Evaluated as multi-class classification on a labelled train/test split of the synthetic dataset (optionally sanity-checked on a verification-gated public dataset per `DATASET_STRATEGY.md`). Reported metrics: accuracy, per-class precision/recall/F1, and a confusion matrix. Because `AI_METHODOLOGY.md` leaves the model open (O-04), several candidates (e.g. Decision Tree, Naive Bayes, Random Forest, SVM, KNN) are compared under the **same** split so the choice is evidence-based. Explainability is assessed qualitatively: does each prediction expose the symptoms that drove it (NFR-07)?

*Results placeholder — complete only after real runs; do not pre-fill.*

| Candidate model | Accuracy | Precision | Recall | F1 | Notes on explainability |
|---|---|---|---|---|---|
| *(to be completed after run)* | | | | | |

### 5.2 Community-level outbreak detection
The demo scenario is the test oracle: the method **should** flag Village C's rise (and the C–D similarity cluster) while leaving Villages A and B unflagged. Evaluation varies the sensitivity/threshold to observe true flags versus false alarms, and records at what point in the 5 → 7 → 15 trend the flag first fires. As O-09 is open, candidate methods (threshold, moving average, statistical anomaly, lightweight clustering) are compared on the same synthetic data. Detection quality is described in terms of correct flags and false alarms on the labelled demo pattern rather than borrowed epidemiological accuracy claims.

*Results placeholder — complete only after real runs; do not pre-fill.*

| Detection method | Flags C? | Flags A/B (false alarm)? | Week first flagged | Notes |
|---|---|---|---|---|
| *(to be completed after run)* | | | | |

## 6. Acceptance criteria for the demo

The prototype is considered to meet its Review-stage goal when the `DEMO_SCENARIO.md` walkthrough runs start to finish on synthetic data: capture and AI suggestion work offline; sync completes without loss; the community layer reproduces the trend and per-village counts, clusters C with D, and raises a *potential* alert with evidence for Village C but not A/B; the dashboard shows it; and the officer can review and decide. Meeting these is the observable proof of the success chain from problem to testing.

## 7. Scope and honesty of testing

Testing is scoped to demo scale (NFR-09): correctness and explainability of the end-to-end flow on synthetic data, not load, penetration, or large-population validation, which are future scope. Two honesty rules bind the results: every figure comes from an actual run on a named dataset, and any limitation found (a miscalibrated confidence, a threshold that over-fires) is reported rather than hidden. A negative or partial result is a valid outcome and is recorded as such.

---

### Related documents

- [Project Overview](./PROJECT_OVERVIEW.md)
- [MVP Scope](./MVP_SCOPE.md)
- [Architecture](./ARCHITECTURE.md)
- [AI Requirements](./AI_REQUIREMENTS.md)
- [Requirements Specification](./REQUIREMENTS.md)
- [Data Model](./DATA_MODEL.md)
- [AI Methodology](./AI_METHODOLOGY.md)
- [Dataset Strategy](./DATASET_STRATEGY.md)
- [Demo Scenario](./DEMO_SCENARIO.md)
- [Testing Strategy](./TESTING_STRATEGY.md) *(this document)*
- [Decisions Log](./DECISIONS.md)
- [Literature Review](./LITERATURE_REVIEW.md)
