# UniTest: Evidence-Driven Regression Intelligence for OsdagBridge

## Purpose

Add a practical, reviewable QA layer around the short-span steel-girder workflow. The goal is not to claim that an unexecuted GUI test passed; every result must be traceable to an input set, application run, and evidence artifact.

## What makes this approach distinctive

### 1. Input-to-output traceability matrix
For each material and geometry field, record the expected path:

`UI field -> saved .osi -> Input Dictionary -> runtime model/material object -> Output Dictionary -> report / CAD / plots`

A test is complete only when the relevant downstream representations agree. This helps detect stale values and hard-coded defaults, not just crashes.

### 2. Consecutive-run regression fingerprint
For each design run, capture a small run fingerprint:
- normalized input values (units made explicit);
- selected material grades and custom material properties;
- key output metrics and component status;
- report / plot / CAD artifact names;
- application version, branch, and timestamp.

Compare Run 1, Run 2, and Run 3 after one controlled input change at a time. Flag unexpected unchanged outputs, unexpected changed outputs, missing artifacts, and inconsistent units. Do not compare floating-point values without documented tolerances.

### 3. Risk-based test prioritization
Prioritize tests using three factors:
- impact: safety/design result, data loss, report integrity, or cosmetic;
- likelihood: known source-code risk, prior issue, or reproducible observation;
- detectability: whether the problem is visible in the UI/report or only in internal data.

Known upstream issues are linked rather than duplicated. A source-code suspicion is labelled SOURCE REVIEW until reproduced in the application.

### 4. Evidence-linked defect records
Every defect record should include:
- exact preconditions and input values;
- minimal reproduction steps;
- expected vs actual result;
- run fingerprint and relevant logs/dictionary snippets;
- screenshot only where visual state matters;
- related upstream issue number, if one exists;
- status: NOT RUN, BLOCKED, PASS, FAIL, or SOURCE REVIEW.

### 5. Honest quality dashboard
Summarize coverage by component (girder, cross-bracing, end diaphragm, deck) and by layer (UI, persistence, input dictionary, runtime, output dictionary, report, CAD/plots). Show NOT RUN and BLOCKED separately from PASS. Never turn missing evidence into a pass.

## Proposed implementation phases

### Phase A — Baseline and traceability (no application code changes)
1. Preserve the supplied baseline .osi and report.
2. Record application version, environment, geometry, material grades, and key outputs.
3. Map each assignment requirement to a test case and evidence location.
4. Link known upstream issues #19, #20, #41, #43, #65, #66, #76, #85, and #96 where relevant.

### Phase B — Repeatable test data
1. Keep test cases in CSV with stable IDs.
2. Add a run log with one row per execution and a unique run ID.
3. Store sanitized input/output snapshots as JSON where available.
4. Use explicit numeric tolerances and unit labels.

### Phase C — Lightweight regression helper
Build a small Python utility that reads run snapshots and produces:
- field-level input/output diffs;
- missing-key and unit-consistency warnings;
- a component coverage summary;
- a Markdown/HTML evidence report.

The helper must not make engineering adequacy claims. It checks consistency and regression signals only.

### Phase D — Verified application testing
Execute the minimum GUI actions needed to validate custom materials, persistence, consecutive design runs, and CAD/plot refresh. Mark each result only after observing it. Do not alter the original baseline; use copies for test variants.

## Acceptance criteria

- Every assignment requirement maps to at least one test case.
- Each executed test has a run ID, environment, inputs, expected result, actual result, and evidence.
- At least three consecutive runs are compared where the application workflow permits.
- Custom properties are checked across UI, saved file, dictionaries, runtime model, and report where those representations are accessible.
- Known upstream issues are cross-referenced; no duplicate issue is filed without checking.
- The final report separates verified runtime findings from source-review findings and unexecuted tests.

## Portfolio framing

Describe this as an evidence-driven QA and regression-testing project for an engineering desktop application. Emphasize test design, reproducibility, Python-based comparison tooling, defect triage, Git/GitHub workflow, and technical reporting. Do not claim automation, bug fixes, or successful runtime coverage until the corresponding work has actually been completed and evidenced.
