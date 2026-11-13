# 0007. Statistical methods run offline: generators, search and analysis, never in a run

- Status: Proposed
- Date: 2026-11-12
- Deciders: maintainer
- Plan reference: none

## Context

[ADR 0005](0005-scope-instrument-first.md) rules out any model at run time, and
[ADR 0006](0006-core-shape.md) makes the engine pure and byte-for-byte repeatable: a run reads
a fixed scenario pack and draws randomness only through keyed `draw`. Packs are written by
hand so far.

Several established methods match the problem the packs describe, which is how facts move
through a group of people with different rights to know them:

- **Information diffusion and influence maximization.** The Independent Cascade and Linear
  Threshold models describe how information spreads over a social graph, and influence
  maximization selects the seed nodes that maximise that spread (Kempe, Kleinberg and Tardos,
  "Maximizing the Spread of Influence through a Social Network", KDD 2003).
- **Markov models** of activity: an actor moving between states such as active, quiet and away.
  A Markov-chain view of repeated agent runs has also been proposed for reliability
  estimates (arXiv 2604.24579).
- **Adversarial search** over inputs: mutating scenarios and keeping those that change a
  verdict, and attacking graders to find ones that can be gamed (arXiv 2606.08960).
- **Entity resolution** from information integration: the same person or fact appearing
  under different forms in different sources.
- **Markov logic networks** attach weights to first-order formulas, so hard rules and soft
  tendencies over relations can be stated together and sampled from (Richardson and
  Domingos, "Markov Logic Networks", Machine Learning 62, 2006). A team is relational:
  people, channels, membership, roles, authority over facts, who usually messages whom.

Any of these placed inside a run would break the determinism contract or put a learned
component in the grading path. Placed before or after a run, they can produce packs and
reports that the engine treats like any other.

## Decision

- Statistical and learned methods may be used only **offline**: to generate packs, to search
  over packs, and to analyse run records. Their output is a pack (with the generator, its
  version, its parameters and its seed recorded in the pack) or a report. The engine and the
  checks never call them.
- Generators use keyed `draw` for every random choice, and express probabilities and rates as
  integers (for example parts per million), so the same parameters and seed give the same
  pack on every operating system. Floating-point library functions are not used for
  decisions, because their last-bit results may differ between platforms.
- Generators follow ADR 0004: standard library only, unless a later ADR adds a dependency.
- Generated packs pass the same validity gates as hand-written ones (controls, reference and
  faulty references, repeat stability, key-seeker).
- Order of adoption:
  1. **Diffusion-based scenario generation** (Independent Cascade and Linear Threshold over the
     team graph), and **leak amplification**: the number of people outside a fact's allowed
     audience who can see it at the end of a run because of the subject's actions, computed
     from the knowledge ledger.
  2. **Markov actor timing**: per-actor state machines for activity and absence, with rates
     taken from aggregate patterns of real teams (numbers only, never text or identities).
  3. **Team structure from a Markov logic network**: hard formulas for what every team must
     satisfy (for example, every fact has exactly one owner) and weighted formulas for
     tendencies (for example, decisions are usually announced where the owner's whole
     subteam is present). Sampling produces the actors, surfaces, memberships and authority
     of a pack; the diffusion generator then places facts on that structure. Weights are
     integer fixed-point log-weights, and sampling uses keyed `draw`, so a sample is
     repeatable on every operating system. The network describes plausible teams only: it
     never decides who knows what in a run (the ledger does) and never grades.
     It must beat a simpler structural baseline (a stochastic block model of subteams) as
     well as hand-written structure, or it is dropped.
  4. **Adversarial scenario search** after the first benchmark ships. Packs it finds are
     frozen and checked against at least one subject other than the one searched against.
  5. **Cross-source entity traps** (aliases, conflicting copies of a fact) once a second
     surface beyond chat exists.
- Not adopted: data stream mining (possible later as concept-drift scenarios). Probabilistic
  relational models are adopted only as the offline structure generator in item 3; the ledger
  and the checks stay exact.
- **Bar for each method:** it ships only after a measured comparison with hand-written packs
  on a stated measure, decided before the comparison is run (for example: failures found that
  hand-written packs miss, at equal pack count; or timing statistics within a stated tolerance
  of the aggregate patterns). If it does not beat hand-written packs, it stays out.
- Prior-art research on each method, as applied to agent testing, comes before its code.

## Alternatives considered

- **Hand-written packs only.** Simplest and fully reviewable. Scenario structure and timing
  stay limited to what authors think of, and the "who knows what" structure stays small.
- **Methods inside the run** (for example, actors sampling their next move live). Richer
  behaviour, but a run would no longer be a pure function of pack and seed, and reduction
  would lose its meaning.
- **Machine-learning libraries** (NumPy, NetworkX and similar) for generators. Faster to
  write, but they add third-party code, and numeric results can vary across versions and
  platforms. The models adopted here are small enough to write with the standard library.
- **An existing Markov logic engine** (Alchemy, Tuffy, pracmln). Mature inference and weight
  learning, but each is a third-party dependency, some are no longer maintained, and none
  promises repeatable samples across platforms. Team-sized networks (tens of people and
  channels) are small enough for a hand-written Gibbs or MC-SAT sampler; if weight learning
  turns out to be needed, a dependency is proposed in its own ADR.

## Consequences

### Good

- Larger and more varied scenarios without giving up exact replay: every generated pack is
  data, hashed and replayable.
- Leak amplification is a count read from the ledger, so it needs no judge.
- Each method carries its own measured case for inclusion, or it stays out.

### Bad

- More code outside the engine to test and maintain, and generator versions to track.
- Generated packs are harder to review by eye than hand-written ones.
- Adversarial search can overfit to the subject it searched against.
- A Markov logic sampler is the largest piece here; writing one by hand is real work, and its
  weights need either expert setting or data to learn from.
- Rates for Markov timing need aggregate data from real teams, which must be collected
  without keeping any real text, names or identifiers.

### Follow-ups

- Prior-art note in `docs/research/` on diffusion-based scenario generation, information-flow
  metrics for agents, Markov activity models for simulated users, Markov logic networks (and
  simpler relational graph models) for generating synthetic organisations, and adversarial
  scenario search for agent tests.
- Define leak amplification precisely in the check catalogue, with golden tests.
- A small diffusion generator and its comparison against hand-written packs, after the engine
  and ledger exist.
