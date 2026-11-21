# Validation plan

Validation design, November 2026.

Three checks make a marker result interpretable: the input arrived, the expected
audience is correct, and the checker distinguishes working behavior from known
faults.

## What a finding must establish

| Evidence | Observation | Validation check |
|---|---|---|
| Captured delivery of the planted event | The target encountered the intended test input | Match the delivered event to the scenario record |
| Scenario permissions at the relevant time | The allowed audience for this case | Derive permissions from the event timeline |
| Marker in a captured model request | Exposure at the model-input boundary | Match the marker, active turn and permitted audience |
| Marker in an observed outbound message | Exact disclosure on the output path | Match the marker and actual recipients |
| A successful recorded replay | Reproduction under the recorded conditions | Compare engine outputs from the same recorded inputs |
| A reduced case that still fails | A smaller valid reproducing case | Check dependencies and rerun the failure predicate |

## Test the checker with controls

Declare the expected result for each control before running it. The expectations
belong to the particular scenario: an inactive target may correctly pass a case
that requires abstention.

| Control | Purpose |
|---|---|
| Authorized reference path | Confirm the marker reaches the tracer and can return to its permitted audience |
| Correct isolated target | Confirm that legitimate behavior is preserved and forbidden delivery is absent |
| Target with one planted fault | Check sensitivity to a specific context, retrieval, session or routing defect |
| Inactive target | Expose checks that pass despite missing required behavior |
| Missing delivery or broken adapter | Verify the harness reports an invalid test condition rather than a clean result |
| Target probing its available interfaces | Confirm the grading key and expected values are unreachable through the target's sandbox |

Record pass, fail, not applicable and harness error separately. A missing
precondition must not silently count as a pass. A failed control invalidates the
interpretation of the affected check until the cause is resolved.

Report detection by fault type so the result maps directly to the coverage tested.

## Measurements for the first implementation

1. **Control validity:** observed versus expected outcomes, by case and control.
2. **Repeatability:** identical records for controlled engine inputs; live-target
   reproduction reported separately with attempts and failures.
3. **Reduction:** starting and final event counts, validity checks, trial outcomes,
   unresolved trials and whether the resulting case still reproduces the failure.
4. **Incremental value:** the same case through the target's existing tests, with
   added coverage and reproduction work reported directly.

Publish the scenario, relevant source versions, checker expectations and synthetic
failure record alongside any result. Report counts and denominators before rates.
Group repeated runs by scenario when reporting uncertainty. Separate fault
coverage, reproduction reliability and reduction results.

The [selected prior art](prior-art.md) connects these checks to the research behind
the design.
