# Boundary-testing design

Status: proposed method, November 2026. No engine or agent integration has been
implemented in this repository. This note describes the reasoning and intended
observations; it does not define a public API or scenario format.

## Question and threat model

An agent may serve several people while sharing a process, memory store, retrieval
index or conversation cache. A private fact can cross a boundary before the model
produces an answer: a stale session can be reused, retrieval can ignore the active
user, or an outbound reply can be routed to the wrong conversation.

The first experiment targets those integration faults. All actors, conversations
and facts are synthetic. Target agents run in an isolated local sandbox, with
loopback networking, no real credentials and shell tools disabled. Scenario text
is untrusted data. The permissions and expected results used for grading stay
outside the target's reachable environment.

The instrument observes **delivery and exposure**, not what an agent knows or
believes. A permitted recipient is not necessarily a recipient who received the
fact; membership alone is insufficient evidence of delivery.

## Why replace the model?

A real model might suppress, paraphrase or ignore a private fact that was wrongly
included in its request. An output-only check could miss the context error. A
deterministic tracer instead emits every planted marker it receives, making that
path visible without sampling a language model.

Keep two observations separate:

| Observation | Supported conclusion |
|---|---|
| An unauthorized marker occurs in a captured model request | The context assembly path exposed that marker during this turn |
| The tracer's marker is delivered to an unauthorized audience | The observed output path disclosed that marker to that audience |

An echoed marker alone does not locate the first faulty component. Capture the
relevant request and delivery path before attributing a failure to memory,
retrieval, session reuse or routing.

### Worked example

This is an illustrative trace, not an executed test or a published fixture.

| Step | Event | Expected boundary |
|---|---|---|
| 1 | Actor A sends `CANARY_A7` in a private conversation | The fact is restricted to A's conversation |
| 2 | Actor B starts a separate private turn | A's marker must be absent from B's model request |
| 3 | The tracer receives the assembled request and echoes its markers | Captured input identifies which markers reached the model boundary |
| 4 | The agent routes the tracer response | A's marker must not be delivered to B |

The check needs a positive control showing that the tracer sees and returns the
marker on A's authorized path. Otherwise, a clean result might mean the marker
never reached the tracer at all.

## Design choices and tradeoffs

| Choice | Reason | Cost or limit |
|---|---|---|
| Scripted actors | Hold the test condition fixed and avoid a second model in the test loop | Cannot answer arbitrary questions or validate human realism |
| Synthetic markers | Make exact exposure and delivery observable | Miss semantic, paraphrased and inferred disclosures |
| Permissions derived from scenario events | Keep audience expectations consistent with the timeline | Depends on correct event and membership semantics |
| Random choices keyed by event identity | Avoid shifting unrelated draws when removing an event | Stable draws do not preserve the removed event's preconditions or downstream behavior |
| Checks outside the target | Prevent the target from reading or altering the answer key | Isolation must itself be tested |

Replacing the model also changes the target's behavior. If the tracer does not
exercise a tool path that a real model would choose, a clean run says nothing
about that path. Coverage must name the interactions actually exercised.

## Replay: state what is controlled

There are four different claims commonly called replay:

1. Reset a workspace to the same initial state.
2. Feed recorded observations into the engine and reproduce its outputs.
3. Re-execute target code against recorded external responses.
4. Run the live target again and observe whether the failure recurs.

Success at one does not establish the others. In a reactive scenario, different
target actions can trigger different events. Identical seeds and scenarios do
not imply identical complete conversations across different targets.

A useful failure record must identify the scenario, seed, engine and target
versions, initial state, delivered events, target observations, captured model
requests and verdicts. Report reset failures, unavailable observations and replay
divergence explicitly. These are requirements to validate, not implemented
guarantees.

## Reduction: smaller is a bounded claim

Reduction should remove events only while preserving scenario validity and a
declared failure predicate. Removing a disclosure, join or triggering action can
invalidate a later event even when its random choices remain unchanged.

Before evaluating a reducer, declare its removal unit, dependency rules, repeat
policy, stopping rule and treatment of unresolved trials. Record the trials that
could not be classified. Report a smaller reproducing case; claim minimality only
under the specific removal rules that were actually checked. A reduced case is
not automatically a causal explanation or a globally shortest sequence.

## What would count as a useful first result?

One local integration with an authorized path, a known faulty variant and a
corrected variant. The faulty variant must expose the planted marker; the corrected
one must preserve the authorized behavior while rejecting the forbidden path.
Replaying and reducing that case would test whether the instrument produces a
useful debugging artifact.

Only then compare it with the target project's own tests: does it find an omitted
boundary case or reduce reproduction work? That comparison, rather than a broad
novelty claim, is the initial research question.

See [prior art](prior-art.md) for the neighboring methods and
[evidence and validation](evidence-standards.md) for the proposed controls.
