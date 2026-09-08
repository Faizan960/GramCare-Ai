# GramCare — Dataset Strategy

> **Project:** GramCare — AI-Powered Offline Disease Outbreak Monitoring System
> **Document:** Dataset Strategy (data sources, synthetic-data plan, integrity)
> **Version:** 0.1 (Review 1 — Research & System Design)
> **Status:** Baseline — dataset not yet finalized
> **Last updated:** 2026-08-22
> **Team:** M. Faizan Patel · Yasa Ghanchi · Niranjan Mohite · Yash S. Waghela
> **Guide / College:** *To be finalized*

---

## 1. Purpose and stance

This document sets out what data GramCare needs, where it will come from, and how integrity is protected. The governing decision is D-008: the prototype is driven by **synthetic / demo data**, not real patient information. Two integrity rules follow and are applied throughout: every generated dataset is labelled **"DEMO / SIMULATED DATA,"** and no public dataset is treated as verified unless its provenance has been checked. The specific dataset(s) are **not finalized** here — this remains open item O-05 in `DECISIONS.md`.

## 2. What data the two AI layers need

The two problems in `AI_METHODOLOGY.md` have different data needs, so the strategy addresses them separately.

The **patient-level classifier** needs labelled records: a set of symptoms (and basic attributes such as age and gender) paired with a known "possible condition" label, so a supervised model can be trained and evaluated. The **community-level analysis** needs a stream of *cases* stamped with time and location, arranged so that normal background activity and an unusual increase are both present — this is what exercises the trend, clustering, and anomaly logic. The community layer does not need condition labels; it needs realistic spatio-temporal structure.

## 3. Primary source for the prototype — synthetic data

For the MVP the primary source is a synthetic generator the team controls, for three reasons: it avoids the privacy and ethics burden of real health data in a college prototype (NFR-05); it lets the demonstration reproduce a specific, explainable pattern on demand; and it sidesteps the dependency on obtaining and licensing a suitable real dataset within a 3–4 month window.

**Demo pattern (source of truth for the numbers).** The synthetic community dataset reproduces the pattern already fixed in `PROJECT_OVERVIEW.md` and `MVP_SCOPE.md`, so all documents agree:

| Time trend (one location) | Cases |
|---|---|
| Week 1 | 5 |
| Week 2 | 7 |
| Week 3 | 15 |

| Location snapshot | Cases | Role in demo |
|---|---|---|
| Village A | 3 | Normal |
| Village B | 4 | Normal |
| Village C | 18 | Sudden increase |
| Village D | *similar symptoms* | Symptom-similarity cluster |

The generator produces normal background activity for most villages and an engineered rise for Village C (with Village D contributing symptom similarity), so the potential-outbreak logic has something real to detect. The full narrative and expected system behaviour live in `DEMO_SCENARIO.md`; this document owns only the data-generation intent.

**Labelling rule.** Every synthetic file, screen, and report derived from it is marked **DEMO / SIMULATED DATA** so no output can be mistaken for a real-world claim. This restates the boundary in D-007/D-008 at the level of the data itself.

## 4. Public datasets — candidates and honest verification status

Real public datasets are useful for two limited purposes: to inform the structure of the synthetic data (realistic symptom sets, attribute ranges) and, optionally, to sanity-check a classifier on genuinely collected data. They are **candidates, not commitments**, and are recorded here with honest verification status so an unverified source is never presented as verified. The distinction that matters: some of these are cited by the verified papers in `LITERATURE_REVIEW.md`; others are common in practice but are **not** vouched for by any of the eight papers and must be checked independently before use.

| Candidate dataset | Possible use | Verification status |
|---|---|---|
| UCI Chronic Kidney Disease dataset (≈400 records, 25 attributes) | Reference for structured symptom/attribute → condition classification; classifier sanity-check | Used by a verified Tier-1 paper (paper 2 in `LITERATURE_REVIEW.md`); provenance is credible, but licence/terms to be confirmed before any use |
| Italian Civil Protection COVID-19 time-series | Reference for realistic spatio-temporal increase shapes | Used by a verified Tier-1 paper (paper 3); real, but real-world and not needed for the demo — synthetic data is preferred |
| Kaggle-style symptom–disease sets (e.g. ~132 symptoms / ~41 diseases) | Convenient symptom vocabulary and label set for the patient-level model | **NOT verified by any of the 8 papers.** Provenance, licence, and label quality must be independently checked before use; do not cite as evidence |

The recommendation is to **build the MVP on synthetic data** and treat public datasets as optional inputs whose provenance is checked first. Choosing any of them is O-05 and is deferred.

## 5. Data volume and scale

Volumes are deliberately small — demo scale, matching NFR-09: on the order of tens to low hundreds of records across roughly three to five villages and a few weeks of time. This is enough to demonstrate the full flow and to make trends and a sudden increase visible and explainable, without the effort of large-scale data engineering that the timeline does not allow. Scaling beyond demo size is explicitly future scope, not an MVP target.

## 6. Integrity and privacy rules

Four rules protect the project's defensibility and follow directly from the decisions log:

Synthetic data is always labelled **DEMO / SIMULATED DATA** (D-008). No real personal health information is collected or stored; identifiers are pseudonymous (NFR-05, and the pseudonymous `patient_id` in `DATA_MODEL.md`). No public dataset is used until its licence and provenance are confirmed, and unverified sources are never cited as evidence (extends D-009's honesty stance from literature to data). And no results are invented: any accuracy or detection figure must come from an actual run on a stated dataset, reported per `TESTING_STRATEGY.md` — this document plans data, it does not report outcomes.

## 7. Decision status

Open item **O-05 (dataset) remains open.** What is settled is the *approach*: synthetic-first, demo-scale, clearly labelled, with public datasets as verification-gated optional inputs. What is not settled is the exact generator design and whether any public dataset is adopted for classifier sanity-checking; both will be resolved alongside O-04 (model) once the classifier candidates in `AI_METHODOLOGY.md` are evaluated.

---

### Related documents

- [Project Overview](./PROJECT_OVERVIEW.md)
- [MVP Scope](./MVP_SCOPE.md)
- [Architecture](./ARCHITECTURE.md)
- [AI Requirements](./AI_REQUIREMENTS.md)
- [Requirements Specification](./REQUIREMENTS.md)
- [Data Model](./DATA_MODEL.md)
- [AI Methodology](./AI_METHODOLOGY.md)
- [Dataset Strategy](./DATASET_STRATEGY.md) *(this document)*
- [Demo Scenario](./DEMO_SCENARIO.md)
- [Testing Strategy](./TESTING_STRATEGY.md)
- [Decisions Log](./DECISIONS.md)
- [Literature Review](./LITERATURE_REVIEW.md)
