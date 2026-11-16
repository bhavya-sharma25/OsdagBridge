# OsdagBridge Short-Span Steel Girder Testing

## Purpose
Record reproducible functional, regression, data-integrity, and presentation checks for the short-span steel plate-girder module. This log distinguishes observations made in the desktop application from headless/source-level checks. No test is marked PASS without recorded evidence.

## Test environment
- Repository under test: https://github.com/bhavya-sharma25/OsdagBridge
- Upstream project: https://github.com/Nidhikhare12/OsdagBridge
- Branch under test: `dev` (record exact commit SHA for each new run)
- Desktop launch: `python -m osdagbridge.desktop`
- Environment: record OS, Python, Conda environment, dependency versions, screen resolution, and display scaling per run.
- Evidence location: `testing/evidence/<test-id>/<run-id>/`

## Test dimensions

### A. Basic geometry
Vary span, carriageway width, number of girders, spacing, skew, median, footpaths, deck thickness, and project location. Include minimum, nominal, maximum, and boundary-invalid values. Record both accepted/rejected behavior and the final normalized values.

### B. Custom material propagation
Use non-default values for:
- Girder steel: grade, Fy, Fu, E, Poisson ratio, density, thermal expansion where exposed.
- Cross-bracing steel and end-diaphragm steel independently.
- Concrete deck: grade, fck, fctm, Ecm, density, and any additional exposed fields.

For each field compare: UI-entered value -> saved .osi -> Input Dictionary -> runtime DTO/material object -> Output Dictionary/report. Record unit conversions explicitly. A displayed or dictionary value alone does not prove that the solver used it.

### C. Consecutive design runs
Within one application session:
1. Run baseline design.
2. Unlock/clear results, change one geometry input, run again.
3. Unlock/clear results, change a material input, run a third time.
4. Compare input/output dictionaries, result docks, CAD model, plots, and generated reports after every run.
5. Repeat with a fresh application session to distinguish stale state from deterministic calculation behavior.

### D. Output and component coverage
Inspect girder(s), stiffeners, cross bracing, end diaphragm, deck, crash barriers, footpaths/railings where enabled, and any other component represented in the output. Compare CAD dimensions and plot labels/values with the output dictionary and report. Capture screenshots and raw dictionaries.

### E. Persistence
Save and reload .osi files in the same session and a fresh session. Confirm that custom material values and all geometry/additional inputs remain unchanged and are re-applied during Design.

## Evidence and status rules
- PASS: expected behavior is demonstrated and evidence is saved.
- FAIL: a reproducible mismatch, crash, incorrect output, or state defect is recorded with exact inputs and evidence.
- BLOCKED: execution could not be completed due to environment/setup/access.
- NOT RUN: planned but not executed.
- A source-code suspicion is not a confirmed runtime failure; label it SOURCE REVIEW until reproduced.
- Never infer engineering adequacy from a successful software run. Compare numerical results with an independently verified reference calculation or accepted benchmark.

## GitHub issue policy
Before filing an issue on upstream `Nidhikhare12/OsdagBridge`, search open and closed issues using the symptom, affected component, and source symbol. If an issue already covers the same root cause, add a comment with new evidence rather than creating a duplicate. The fork currently has Issues disabled; use upstream only where repository permissions permit.

## Known upstream issue overlap
The upstream tracker already contains reports covering several required test areas, including:
- #19 custom-material fields not restored from .osi before Design
- #20 custom girder steel density ignored in material construction
- #41 custom deck density not propagated into deck self-weight
- #43 cross-bracing/end-diaphragm custom steel grades not used
- #76 custom material dialog precision restriction
- #85 3D CAD missing after unlock and second design run
- #96 custom-material validation skipped when loading .osi
- #66 in-place unit conversion causing report dimensions to be labelled incorrectly
- #44 design/CAD completion despite fatigue utilization above 100%
- #65 report summary says component pass despite overall failure

These are existing reports, not newly discovered bugs from this test campaign. New evidence should be attached to the existing issue where applicable.

## Run log
See `test-cases.csv` for recorded executions. Add one row per run; for multi-run tests, use a shared scenario ID and distinct run IDs.
