# Selected prior art

Channel isolation, agent evaluation, privacy and deterministic debugging provide
the foundations for Sauron's test design. Sources reviewed: primary documentation,
a pinned scenario file and paper abstracts, 2026-11-20.

## Existing capabilities and their relevance

| Work and inspected evidence | What the source supports | Implication for Sauron |
|---|---|---|
| [OpenClaw thread-memory isolation scenario](https://github.com/openclaw/openclaw/blob/18b0130a21b9b9aa6856b419ca0d22897f960c25/qa/scenarios/memory/thread-memory-isolation.yaml), pinned source file | Checks memory-tool use, consumption of the matching tool result, delivery inside the thread and absence of a root-channel reply | Provides a concrete comparison for portable channel-boundary tests |
| [AgentDojo](https://arxiv.org/abs/2406.13352v3), paper abstract | Evaluates tool-using agents over untrusted data with tasks, security cases, attacks and defenses | Security evaluation must observe task execution and the attacked path. Sauron's first question is context and routing isolation with a substituted model |
| [tau-bench](https://arxiv.org/abs/2406.12045v1), paper abstract | Compares final database state with annotated goals and evaluates consistency across repeated trials | Observable state and repeated success are established evaluation ideas. An exact check still needs a justified expected outcome |
| [MuPPET](https://arxiv.org/abs/2606.23217v1), paper abstract | Studies contextual privacy in multi-party conversations and reports privacy failures and defense tradeoffs | Multi-party privacy is already a research area. Marker exposure tests cover a narrower integration question than contextual appropriateness |
| [FlowSeal](https://arxiv.org/abs/2609.14003v1), paper abstract | Describes confidentiality enforcement at tool calls using provenance and controlled declassification | Connects audience-aware testing with provenance and enforcement boundaries |
| [Antithesis debugging](https://antithesis.com/docs/product/debugging/), official documentation | Describes exact replay and interventions within its deterministic simulation | Makes control of the execution boundary central to the replay contract |
| [TigerBeetle safety](https://docs.tigerbeetle.com/concepts/safety/), official documentation, Engineering section | Describes testing real database code under simulated network, storage and process faults in VOPR | Provides an engineering precedent for testing real code inside a controlled simulation |

The 2026 arXiv entries are preprints. The table identifies the source scope for
each comparison.

## Sauron's research direction

Sauron combines scripted multi-person interactions, a substituted marker tracer,
explicit audience expectations and reducible failure records. The research
question is how much this combination improves detection and reproduction of
context and routing faults.

Test a concrete boundary case through both Sauron and the target's existing tests.
Measure coverage added, false findings and the work needed to reproduce the failure.

## Design implications

- **Context exposure and disclosure differ.** Record model input separately from
  recipient delivery. A privacy benchmark's semantic judgment and an exact marker
  check measure different things.
- **Replay and reproduction differ.** Replaying recorded inputs tests a controlled
  contract; rerunning a changed or external target adds sources of variation.
- **Reduction preserves a failure predicate and scenario validity.** Record the
  dependency rules, removal trials and final reproduction.
- **Controls validate the evaluator.** Known correct and faulty targets establish
  whether the checker detects the intended fault.

These distinctions motivate the [boundary-testing design](boundary-testing.md)
and its [validation plan](evidence-standards.md).
