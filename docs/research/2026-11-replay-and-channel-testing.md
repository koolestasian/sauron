# Replay, channel testing and privacy evaluation: a focused check

Checked 2026-11-13. This supplements the earlier prior-art and literature notes; it does
not change an ADR. Scope: existing channel tests, replay boundaries, grader validation and
information-flow checks. This is a targeted search, not a systematic review or a novelty proof.

**Evidence labels:** source = primary documentation, code or metadata opened; reported =
authors' experimental result; inference = interpretation. None of the external systems below
was installed or executed during this check. A README is evidence of a documented capability,
not proof that it works. Missing documentation is not proof of a missing feature.

## Channel scenarios already exist

**S01 — OpenClaw, source.** Its QA overview describes synthetic channels, scenario runners and
transport adapters. Two concrete files go beyond generic unit tests: one sends blocked and
allowed actors' messages in sequence; another checks that a memory-backed answer remains in
its thread. These are project-specific tests, not evidence of a portable benchmark or outside
adoption. Their existence rules out a general claim that multi-person channel tests do not
exist. The assertions were read, not run.

- [QA overview](https://github.com/openclaw/openclaw/blob/main/docs/concepts/qa-e2e-automation.md).
- [Multi-actor ordering scenario](https://github.com/openclaw/openclaw/blob/adb3fb2967cfe6497b68e838f7eb26b88651c845/qa/scenarios/channels/channel-multi-actor-ordering.yaml), file blob `6b15fa2f547aaf4bf72a268743fd7ded4c5a0ca4`.
- [Thread memory isolation scenario](https://github.com/openclaw/openclaw/blob/18b0130a21b9b9aa6856b419ca0d22897f960c25/qa/scenarios/memory/thread-memory-isolation.yaml), file blob `b7bae7aa4221c8224afdac6e19c3570b67ef33cd`.

## Replay has several different meanings

| ID | Source and inspected scope | Supported claim | Limit |
|---|---|---|---|
| S02 | [havoc README](https://github.com/bernardobbl/havoc/blob/main/README.md), blob `0c05462ad3ecafb8cb6e976cd3a9b13dd9fe8863` | Documents deterministic tool-fault schedules, recorded model responses, structural state grading and replay divergence detection | The described subject is an agent in a fault-injected tool world; this is not evidence of scripted teammate reduction |
| S03 | [ReplayGate README](https://github.com/bessavagner/replaygate/blob/main/README.md) | Documents conversation invariants, recorded calls, pinned replay that reports divergence, and optional live fallback | Only the direct channel adapter is described as shipped; broader messaging adapters are roadmap items. Replaying cached model outputs does not test a newly sampled model response |
| S04 | [Chronicle paper](https://arxiv.org/html/2609.20625v1), sections 3–5 | Selective replay runs changed functions while stubbing other boundaries; reports six simulated incidents, faulty/fixed controls and mutation experiments | Simulated model boundaries; not a general live-agent reproduction rate. Matching by boundary name and occurrence does not catch every order change |
| S05 | [Chronicle current README](https://github.com/theagentplane/chronicle/blob/main/README.md), blob `556c084c5144bd4910fc41b09db893105811f8cb` | Documents a 1.0 redesign that requires a reachable control plane for replay; explicitly removes local stores and the old CLI | The paper and current product describe different versions. Do not use the paper as installation documentation for current main |
| S06 | [Antithesis debugging documentation](https://antithesis.com/docs/product/debugging/), Debugging/Causality analysis | Describes exact replay and interventions within its controlled simulation | Does not establish that an external hosted model becomes deterministic merely because its surrounding environment is seeded |
| S07 | [TigerBeetle safety documentation](https://docs.tigerbeetle.com/concepts/safety/), Engineering/VOPR | Describes running real database code under simulated network, storage and process faults | An engineering precedent, not measured transfer to team-facing agents |

**Inference:** distinguish four contracts when comparing tools:

1. Reset the workspace to a known initial state.
2. Replay an engine's recorded inputs to reproduce its outputs.
3. Re-execute changed subject code against selected recorded boundaries.
4. Run a live subject again and measure whether a failure recurs.

Success at one does not establish the others. In a reactive environment, emitted events depend
on the subject's observed actions. Fixed seed and pack alone cannot imply an identical complete
conversation across subjects that take different actions. This follows from the input/output
definition of a reactive state machine, not from an external benchmark result.

## Reduction and grader checking are also prior art

**S08 — Delta debugging for LLM-integrated systems, source.** The
[Amazon Science publication page](https://www.amazon.science/publications/delta-debugging-for-llm-integrated-systems)
describes semantic markers and reducing problematic input fragments, evaluated in development
and production settings; it lists ICSE 2026. This check opened the publication page, not a new
full-paper replication. It supports prior art in LLM-input reduction, not the specific claim
that a reducer preserves multi-person scenario dependencies.

Chronicle's S04 mutation experiment is also prior art for testing whether regression assertions
distinguish faulty and corrected behavior. It does not establish validation of every possible
grader fault. Together, these sources make broad claims such as “first replay tool for agents”
or “nobody validates agent tests” untenable.

**Inference:** reduction needs a declared removal unit, validity constraints, a fixed failure
predicate, a stopping rule and a record of unresolved trials. A smaller failing case is not
automatically a globally smallest case or a causal explanation. Removing an event may preserve
other random draws while changing their preconditions or the subject's response.

## Privacy evaluation and enforcement are different neighbors

**S09 — MuPPET, source/reported.** The [paper](https://arxiv.org/html/2606.23217v1), sections
3.1–3.2 and appendix A.6, describes generated group conversations, private memories, and
privacy/utility evaluation. It uses a semantic judge and reports human validation on sampled
outputs. Thus multi-party privacy and evaluator checking are already studied. It tests meanings
that exact canary matching would miss; conversely a semantic judge introduces its own errors.
This check does not replicate the results or equate those methods with executable state checks.

**S10 — FlowSeal, source.** [Section V and Theorem 1](https://arxiv.org/html/2609.14003v1)
describe provenance labels, recipient authorization and mediation at tool calls. The stated
confidentiality guarantee is conditional on the declassification function not returning an
incorrect safe verdict; that function uses an isolated model invocation. It is not an
unconditional mathematical guarantee of zero semantic leakage. This is an enforcement system,
not evidence that an evaluator's exposure ledger measures what an agent believes.

**Inference:** separate permission, opportunity to read, recorded delivery and actual use.
Membership alone cannot demonstrate that a subject understood a fact. Exact planted markers
support exact disclosure tests, not a comprehensive claim about paraphrase or inference leaks.

## What the evidence does and does not establish

Existing work covers meaningful parts of the proposed instrument: synthetic channel tests,
state oracles, replay, selective re-execution, privacy evaluation and validation controls.
This search did not establish a maintained implementation combining the entire proposed pack
contract, temporal disclosure ledger, control expectations and dependency-aware reduction.
That bounded observation is not a claim of uniqueness, demand or defensibility.

No feature, dependency or architecture choice follows automatically from the comparison.
The relevant empirical question is whether a portable scenario and its checks add useful
coverage, or reduce reproduction work, relative to an existing project's own tests.

## Search scope and limitations

Searches on 2026-11-13 included:

- `deterministic simulation testing AI agents replay shrinking multi user Antithesis`
- `information flow control agents multi user knowledge privacy benchmark MuPPET`
- `LLM agents information flow control arxiv privacy 2026`
- `Delta Debugging for LLM-integrated Systems Amazon science`
- GitHub primary documentation and the two scenario files linked above.
- Reddit and X queries about Slack bots, multi-user permissions and replay testing.

A [Reddit discussion](https://www.reddit.com/r/LLMDevs/comments/1woha8m/your_agent_has_more_permissions_than_your_users/)
was opened. It promotes an authorization tool and discusses requester permissions; it is not
an independently verified purchasing signal for a testing product. Its linked project now
redirects to [toolpass](https://github.com/roee-hersh/toolpass), whose README describes permission
checks and a local demo. This verifies the existence of the substitute, not its effectiveness.
An X replay-project lead appeared in search, but direct retrieval returned HTTP 403. No
technical conclusion here relies on X or on anonymous social claims.

No external test suite, paid model run, fresh customer interview, installation timing,
complete code audit, or exhaustive competitor search was performed. Product versions and
main-branch README claims may change. File blob identifiers above identify the inspected
content; they are not repository commit identifiers.
