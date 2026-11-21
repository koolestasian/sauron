# Boundary-testing design

Design note, November 2026: threat model, observations, replay and reduction.

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

The instrument records **delivery and exposure**. Audience permissions come from
the scenario timeline; delivery comes from captured events. Keeping those records
separate makes a missed delivery distinguishable from an isolation failure.

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

Capture the request and delivery path to locate the fault in memory, retrieval,
session reuse or routing.

### Worked example

Example scenario:

| Step | Event | Expected boundary |
|---|---|---|
| 1 | Actor A sends `CANARY_A7` in a private conversation | The fact is restricted to A's conversation |
| 2 | Actor B starts a separate private turn | A's marker must be absent from B's model request |
| 3 | The tracer receives the assembled request and echoes its markers | Captured input identifies which markers reached the model boundary |
| 4 | The agent routes the tracer response | A's marker must not be delivered to B |

The positive control confirms that the tracer sees and returns the marker on A's
authorized path. This checks delivery and marker detection before testing B's
isolation.

## Design choices and tradeoffs

| Choice | Reason | Cost or limit |
|---|---|---|
| Scripted actors | Hold the test condition fixed and avoid a second model in the test loop | Cannot answer arbitrary questions or validate human realism |
| Synthetic markers | Make exact exposure and delivery observable | Miss semantic, paraphrased and inferred disclosures |
| Permissions derived from scenario events | Keep audience expectations consistent with the timeline | Depends on correct event and membership semantics |
| Random choices keyed by event identity | Avoid shifting unrelated draws when removing an event | Stable draws do not preserve the removed event's preconditions or downstream behavior |
| Checks outside the target | Prevent the target from reading or altering the answer key | Isolation must itself be tested |

Coverage is defined by the interactions exercised through the tracer. Record the
tool paths and observation points for each case. Semantic privacy and paraphrased
disclosure belong to a separate evaluation layer.

## Replay boundaries

The design distinguishes four replay operations:

1. Reset a workspace to the same initial state.
2. Feed recorded observations into the engine and reproduce its outputs.
3. Re-execute target code against recorded external responses.
4. Run the live target again and observe whether the failure recurs.

Each operation controls a different boundary. In a reactive scenario, target
actions influence later events, so engine replay records those observations as
inputs. Live-target reproduction measures variation across fresh executions.

A useful failure record must identify the scenario, seed, engine and target
versions, initial state, delivered events, target observations, captured model
requests and verdicts. Reset failures, unavailable observations and replay
divergence are recorded as explicit outcomes.

## Dependency-aware reduction

Reduction should remove events only while preserving scenario validity and a
declared failure predicate. Removing a disclosure, join or triggering action can
invalidate a later event even when its random choices remain unchanged.

The reducer declares its removal unit, dependency rules, repeat policy and
stopping rule. Its report includes successful removals, unresolved trials and the
final reproducing case. Minimality is defined against the removal rules tested.

## First integration experiment

One local integration with an authorized path, a known faulty variant and a
corrected variant. The faulty variant must expose the planted marker; the corrected
one must preserve the authorized behavior while rejecting the forbidden path.
Replay and reduction then turn that failure into a portable debugging artifact.

Compare the same case with the target project's own tests. Measure the boundary
coverage added and the work required to reproduce the failure.

See [prior art](prior-art.md) for the neighboring methods and
[validation plan](evidence-standards.md) for the controls and measurements.
