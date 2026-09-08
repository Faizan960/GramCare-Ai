# GramCare — Review 2 Reconciliation Report

> **Project:** GramCare — AI-Powered Offline Disease Outbreak Monitoring System
> **Document:** Reconciliation of the Review 2 project documentation against the Review 1 baseline
> **Version:** 0.1 (Review 2 — Reconciliation)
> **Status:** Findings only — no baseline decision has been changed by this document
> **Last updated:** 2026-09-08
> **Team:** M. Faizan Patel · Yasa Ghanchi · Niranjan Mohite · Yash S. Waghela
> **Guide / College:** *To be finalized*

---

## 1. Purpose and method

`GramCare_Full_Project_Documentation.docx` (the "Review 2 document") sets out the committed technology stack, DFD/UML specifications, a 16-week schedule, and an implementation plan. This report compares it against the twelve Review 1 markdown documents in this repository and records three things: whether the eleven protected decisions survive, which of the open decisions O-01…O-11 the document now closes, and where the two sources genuinely contradict each other.

Per the project's working rule, **this document changes nothing**. Where a contradiction exists it proposes the *smallest* correction and leaves the choice to the team. No decision record has been rewritten, and no open decision has been closed by this report.

## 2. Headline finding

The Review 2 document is **substantially consistent with the baseline**. It preserves all eleven protected decisions, reuses the same eight verified IEEE papers with matching DOIs, and — importantly — the stack it selects is the one the baseline's own analysis predicted. `AI_METHODOLOGY.md` §7 leaned toward "explainable classical ML" for symptom classification and "lightweight threshold / moving-average / clustering" for detection; the Review 2 document chooses a Decision Tree or small Random Forest and explainable SQL/statistical detection. The evidence chain from literature to design therefore holds rather than breaking.

Four genuine contradictions and one gap need resolving. None require reversing a protected decision.

## 3. Protected decisions — all eleven preserved

| # | Protected decision | Status in the Review 2 document |
|---|--------------------|--------------------------------|
| 1 | Architecture technology-neutral *at the idea/review stage* | **Deliberately superseded, as planned.** D-001 scoped neutrality to the idea stage and O-01…O-11 existed precisely to be closed later. Review 2 is that stage. Not a violation — but it must be recorded (see §4). |
| 2 | Health-worker mobile app is primary | Preserved — Flutter field app is the centre of the design. |
| 3 | Offline-first is core, not optional | Preserved — §10 states it explicitly; `sync_status = PENDING/SYNCED` model. |
| 4 | AI is decision support, not diagnosis | Preserved — §23 and §31; required language "possible condition" enforced. |
| 5 | Alerts indicate *potential* patterns | Preserved — "Potential Outbreak Alert", never confirmed. |
| 6 | Graph / Neo4j not mandatory for MVP | Preserved — deferred in §7, §25, §31; "do not force Neo4j merely because it appears advanced" (§30). |
| 7 | Prefer simple explainable detection | Preserved — SQL/statistical logic, "no black-box ML required". |
| 8 | Synthetic / demo data for the prototype | Preserved for demonstration (§22) — but the training-data stance changed; see §5.2. |
| 9 | Do not fabricate literature | Preserved — the same 8 papers; all 8 DOIs match the verified survey exactly. |
| 10 | Novelty is integration, not invention | Preserved — §24 states it in the same terms. |
| 11 | Simple → Working → Testable → Explainable → Extendable | Preserved — §27 and §30. |

## 4. Open decisions now resolved

The Review 2 document closes or narrows every open item. Recording these in `DECISIONS.md` as new dated entries (rather than editing D-001) is the change the baseline expects.

| Open item | Resolution in the Review 2 document | Fully closed? |
|-----------|-------------------------------------|---------------|
| O-01 language / framework | Flutter + Dart | Yes |
| O-02 database | Local Hive **or** sqflite; central AWS RDS PostgreSQL | Partly — Hive vs sqflite still open |
| O-03 cloud provider | AWS (RDS, S3); training on SageMaker Studio Lab or laptop | Yes |
| O-04 AI model | Decision Tree **or** small Random Forest (~10–20 trees), scikit-learn → m2cgen → Dart | Partly — final pick between the two still open |
| O-05 dataset | Real public disease/symptom dataset for training; synthetic for demo | **No** — no specific dataset named; see §5.2 |
| O-06 GIS / mapping | Leaflet | Yes |
| O-07 sync technology | Node.js + Express sync API | Partly — conflict-resolution rule still open |
| O-08 graph DB / algorithm | Deferred; PostgreSQL relations sufficient | Yes (as deferral, consistent with D-005) |
| O-09 detection algorithm | Three-dimension intersection (condition + village + rise) via `case_count > avg_baseline * threshold_multiplier` | Partly — baseline and multiplier values still open |
| O-10 final UI | React + Recharts + Leaflet | Yes |
| O-11 deployment architecture | AWS RDS + S3, Node/Express backend, React dashboard | Largely |

Residual open items after Review 2: Hive vs sqflite, Decision Tree vs Random Forest, the specific public dataset, sync conflict resolution, baseline/threshold values, final schema, and alert severity rules. The Review 2 document itself acknowledges this list in §25 and §31, which is consistent with the baseline's habit of stating what is *not* yet decided.

## 5. Contradictions requiring a decision

### 5.1 Demo figures — Village C / 18 versus Village B / 11

**The conflict.** The baseline fixes one canonical demo pattern, used in five documents: a weekly trend of 5 → 7 → 15 and a location snapshot of Village A 3, B 4, C 18 (sudden increase), D similar symptoms. It is also the test oracle in `TESTING_STRATEGY.md` (§5.2: "should flag Village C's rise … while leaving Villages A and B unflagged"). The Review 2 document instead offers, in §12, "Village B with 11 similar cases in a short window."

**Why it matters.** If Village B is both a *normal* village (baseline) and the *outbreak* village (Review 2 document), the demonstration contradicts its own expected result, and the testing oracle inverts.

**Smallest correction.** Change the single sentence in §12 of the Review 2 document to reference Village C with 18 cases, matching the five baseline documents. Reword the demo scenario in §19 step 22 the same way. This touches two sentences in one document rather than renumbering five. *(If the team prefers the 11-case figure, the alternative is to keep it purely as an illustrative threshold-tuning example, clearly labelled as not the demo scenario — but the canonical scenario should stay Village C.)*

### 5.2 Training data — synthetic-first versus real public dataset

**The conflict.** `DATASET_STRATEGY.md` is synthetic-first: public datasets are optional, provenance-gated inputs, and it explicitly warns that the common Kaggle-style "132 symptoms / 41 diseases" set is **not verified by any of the eight papers** and must be checked independently before use. The Review 2 document (§11, §22, §31) instead requires a real public disease/symptom dataset for training and evaluation, so that genuine metrics can be reported.

**Assessment.** The change is defensible and arguably better for the report — real evaluation metrics are stronger evidence than metrics computed on data the team generated itself, and separating training data from demo data (§30) is good practice. But the document names no dataset, and the most likely candidate is precisely the one the baseline flagged as unverified.

**Smallest correction.** Adopt the Review 2 split (real public data trains the model; synthetic data drives the demo) and carry the baseline's provenance gate forward unchanged: name the dataset, confirm its licence and source before training, and do not cite it as evidence until verified. Keep O-05 open until a specific dataset is named and checked. Concretely, that means updating `DATASET_STRATEGY.md` §3 and §7 rather than deleting its integrity rules.

### 5.3 Patient name field — an internal contradiction in the Review 2 document

**The conflict.** The Review 2 document contradicts itself. Its §13 data model lists `patients` as `id, age, gender, village_id, created_at` — no name, correctly matching `DATA_MODEL.md` and NFR-05 (pseudonymous identifiers, no real personal health information). But its §16 class diagram gives `Patient` the attributes `patientId, name, age, gender, villageId, createdAt`, and `HealthWorker` a `name` and `contact`.

**Why it matters.** A `name` attribute on a patient class shown in a Review 2 presentation invites exactly the privacy question the baseline was written to pre-empt, and it conflicts with the document's own schema two sections earlier.

**Smallest correction.** Drop `name` from the `Patient` class in §16, or render it as a pseudonymous display label, so §16 matches §13 and NFR-05. Optionally align `HealthWorker.name` with the baseline's `display_name` / pseudonym wording.

### 5.4 "Model not yet finalized" statements in the baseline

**The conflict.** `AI_METHODOLOGY.md` §8 states plainly: "No classifier and no detection algorithm is selected in this document. O-04 … and O-09 … remain open." `DATASET_STRATEGY.md` §7 and `DECISIONS.md` §2 make equivalent statements. These are now out of date.

**Smallest correction.** Do not delete the candidate comparisons — they are the justification for the choice and are worth keeping for the report. Add a short Review 2 note to `AI_METHODOLOGY.md` §8 recording that the comparison concluded in favour of tree-based classification and explainable statistical detection, with the decision recorded in `DECISIONS.md`. Update the `DECISIONS.md` status snapshot so the "deliberately not finalized" list reflects only the residual items in §4 above.

## 6. Gap — no security or authentication section

`REQUIREMENTS.md` NFR-04 requires access control, encryption appropriate to a prototype, and no clear-text secrets. The Review 2 document specifies a sync API and a dashboard but contains no authentication, authorization, or transport-security section, and §21 lists no auth mechanism. For a Review 2 "how it will be built" document this is a visible omission: the sync endpoint and the officer dashboard both need at least a stated approach.

**Suggested addition (not a change to any decision):** a short section covering how health workers and officers authenticate, that sync traffic uses HTTPS/TLS, and how credentials and the database connection string are kept out of the repository. Keeping it brief is consistent with prototype scope.

A smaller related gap: FR-AU-05 (annotate or acknowledge an alert) is a *Could*-priority requirement and is not mentioned in the Review 2 dashboard or alert specifications. Acceptable to drop, but worth an explicit note so the omission reads as deliberate.

## 7. Conceptual model to physical schema — mapping, not conflict

The baseline's twelve conceptual entities and the Review 2 document's eight tables differ in shape, which is expected: one is database-neutral, the other is a PostgreSQL schema. The mapping is coherent and should be documented so the conceptual model does not appear contradicted.

| Baseline entity (`DATA_MODEL.md`) | Review 2 realisation (§13) |
|-----------------------------------|----------------------------|
| Patient | `patients` |
| HealthRecord | `visits` / `health_records` |
| Symptom | `symptoms`, plus `symptoms[]` on visits |
| PossibleCondition | `condition` / `predicted_condition` column |
| AnalysisResult | `predicted_condition` + `confidence` columns on visits |
| Case + CaseCluster | `case_aggregates` (village_id, week, condition, case_count) |
| Alert | `alerts` (with village, condition, week, count, severity, reasoning) |
| Report | Generated on demand; no table |
| HealthWorker | `health_workers` |
| Location | `villages` |
| SynchronizationRecord | `sync_logs` |

**One open question this surfaces.** `alerts` carries a `reasoning` field, but `visits` carries only `predicted_condition` and `confidence` — there is no column for the patient-level explanation required by FR-AI-03 and NFR-07. If the reasoning is regenerated on-device from the tree at display time, that satisfies the requirement and should be stated; if it needs to survive synchronization for the officer to see it, the schema needs a field. Worth settling before the schema is frozen.

## 8. Internal defects in the Word document

These are formatting faults in the `.docx` itself, not consistency problems, and are worth fixing before submission:

The **list numbering runs on across sections**. The Demo Scenario steps in §19 are numbered 14–26 instead of restarting at 1, because the list continues from the end-to-end workflow list in §9. The Implementation Plan steps in §27 are likewise numbered 27–41 instead of 1–15. The effect is that §27 opens with an item numbered "27" directly under a heading numbered 27, which reads as a duplicate.

The **Executive Summary heading is not a real heading** — it appears as literal text ("1. Executive Summary") rather than a Word heading style, unlike sections 2–31. It will be missing from any generated table of contents.

There is also **no table of contents**, which a 31-section submission document would benefit from, and the ASCII flow diagrams (§6, §8, §10, §17, and the Gantt in §20) are rendered as line-broken paragraph text. They read acceptably but would present better as real diagrams — the baseline documents already contain Mermaid equivalents for several of these flows that could be exported as images.

## 9. Recommended sequence

If the team accepts the reconciliation, the smallest coherent order of work is: first add new dated decision records to `DECISIONS.md` closing O-01, O-03, O-06, O-08, O-10, O-11 and narrowing O-02, O-04, O-07, O-09, and update its status snapshot; then apply the four corrections in §5 (two sentences in the Review 2 document, the `Patient.name` attribute, the dataset stance in `DATASET_STRATEGY.md`, and the "not finalized" notes in `AI_METHODOLOGY.md`); then add the security section from §6 and the schema note from §7; then bump the twelve baseline documents from "Version 0.1 (Review 1)" to Review 2 with a current date, naming the committed stack where each document currently says a choice is deferred; and finally fix the numbering defects in §8.

Nothing in that sequence reverses a protected decision, and the success chain from problem through requirements, research, gap, contribution, architecture, data model, AI methodology, MVP, demo, and testing remains intact — now with an implementation plan attached to its end.

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
- [Testing Strategy](./TESTING_STRATEGY.md)
- [Decisions Log](./DECISIONS.md)
- [Literature Review](./LITERATURE_REVIEW.md)
