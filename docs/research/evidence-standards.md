# Evidence and validation

Status: proposed validation plan, November 2026. No controls, target-agent tests
or reduction experiments described here have been run in this repository.

The central risk is a test that appears precise while measuring the wrong thing.
A marker detector is useful only if the marker arrived, the expected audience is
correct, and the checker distinguishes working behavior from known faults.

## What a finding must establish

| Evidence | What it establishes | What it does not establish |
|---|---|---|
| Captured delivery of the planted event | The target encountered the intended test input | That it retrieved, remembered or understood the fact |
| Scenario permissions at the relevant time | The audience allowed by this synthetic case | A universal policy for real organizations |
| Marker in a captured model request | Exposure at that observed context boundary | That a real model would disclose the fact |
| Marker in an observed outbound message | Exact disclosure on that output path | Detection of paraphrase, inference or encoded leakage |
| A successful recorded replay | Reproduction under the recorded boundary conditions | Live reproduction after target or environment changes |
| A reduced case that still fails | A smaller valid case under the tested removal rules | A globally smallest case or causal explanation |

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

One planted fault establishes sensitivity to that fault. It does not estimate the
prevalence of real defects or prove general detection effectiveness.

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
Repeated runs of the same case are not independent scenarios; preserve those
groupings when reporting uncertainty. Do not convert a small convenience sample
into a general agent-safety score.

The [selected prior art](prior-art.md) identifies the sources and inspected scope
behind the design. Claims about detection, replay or reduction performance await
runnable artifacts and measured results.
