# Selected prior art

Scope: channel isolation, agent evaluation, privacy and deterministic debugging.
Primary sources below were opened on 2026-11-20; the selection builds on a focused
check from 2026-11-13. This is not an exhaustive search or a novelty proof. No
external system was installed or executed for this comparison.

## Existing capabilities and their relevance

| Work and inspected evidence | What the source supports | Implication for Sauron |
|---|---|---|
| [OpenClaw thread-memory isolation scenario](https://github.com/openclaw/openclaw/blob/18b0130a21b9b9aa6856b419ca0d22897f960c25/qa/scenarios/memory/thread-memory-isolation.yaml), pinned source file | Checks memory-tool use, consumption of the matching tool result, delivery inside the thread and absence of a root-channel reply | Concrete channel-boundary tests already exist. A portable instrument must demonstrate value beyond a project's own assertions |
| [AgentDojo](https://arxiv.org/abs/2406.13352v3), paper abstract | Evaluates tool-using agents over untrusted data with tasks, security cases, attacks and defenses | Security evaluation must observe task execution and the attacked path. Sauron's first question is context and routing isolation with a substituted model |
| [tau-bench](https://arxiv.org/abs/2406.12045v1), paper abstract | Compares final database state with annotated goals and evaluates consistency across repeated trials | Observable state and repeated success are established evaluation ideas. An exact check still needs a justified expected outcome |
| [MuPPET](https://arxiv.org/abs/2606.23217v1), paper abstract | Studies contextual privacy in multi-party conversations and reports privacy failures and defense tradeoffs | Multi-party privacy is already a research area. Marker exposure tests cover a narrower integration question than contextual appropriateness |
| [FlowSeal](https://arxiv.org/abs/2609.14003v1), paper abstract | Describes confidentiality enforcement at tool calls using provenance and controlled declassification | Enforcement is a neighboring approach. Sauron proposes an instrument to detect tested boundary violations, not an enforcement guarantee |
| [Antithesis debugging](https://antithesis.com/docs/product/debugging/), official documentation | Describes exact replay and interventions within its deterministic simulation | Exact replay requires control of the relevant execution environment. A seeded wrapper cannot confer that guarantee on arbitrary external components |
| [TigerBeetle safety](https://docs.tigerbeetle.com/concepts/safety/), official documentation, Engineering section | Describes testing real database code under simulated network, storage and process faults in VOPR | Controlled simulation is an engineering precedent. Its effectiveness for databases is not measured evidence for this agent-testing design |

The paper rows summarize their abstracts, not independent replications or fresh
full-text reviews. The 2026 arXiv entries are preprints; the table makes no claim
about subsequent peer review. Documentation establishes what a system describes;
reading an assertion establishes what it checks, not whether it passes.

## The distinction worth evaluating

These sources already cover important parts of the proposal. Sauron's hypothesis
is that combining scripted multi-person interactions, a substituted marker tracer,
explicit audience expectations and reducible failure records can make integration
faults easier to detect and reproduce.

That is an engineering hypothesis. The useful comparison is a concrete boundary
case tested both through Sauron and through the target's existing tests. Measure
coverage added, false findings and the work needed to reproduce the failure.
This selection establishes neither uniqueness nor demand for a new tool.

## Limits that shape the design

- **Context exposure and disclosure differ.** Record model input separately from
  recipient delivery. A privacy benchmark's semantic judgment and an exact marker
  check measure different things.
- **Replay and reproduction differ.** Replaying recorded inputs tests a controlled
  contract; rerunning a changed or external target adds sources of variation.
- **Reduction needs its own evidence.** Preserving one failure after removing
  events does not establish global minimality, causality or coverage of other
  failures.
- **Evaluator quality is a separate question.** Known correct and faulty targets
  are needed before interpreting a result as evidence about an agent.

These distinctions motivate the [boundary-testing design](boundary-testing.md)
and its [validation plan](evidence-standards.md). No external result in this note
is a Sauron performance result.
