# Inherited from teamsim

sauron was split from teamsim (a sandbox of simulated teammates) on 2026-11-13. The decision
records and designs here are copies, as of teamsim commit `d1e5b2d`, kept as background. They
describe the earlier project; read "teamsim" in them as the parent project. teamsim no longer
follows ADR 0005 or these designs; sauron owns them from here. Sauron's own decisions go in
`docs/adr/`.

- `adr/0006-core-shape.md` (event engine, keyed randomness, packs as data, ledger, verdicts,
  run records, reduction) and `adr/0004-python-standard-library-core.md` apply here as written.
- `adr/0005-scope-instrument-first.md` applies, except for the order of work, which sauron
  changes (exposure tests on real agents first).
- `adr/0007-offline-statistical-methods.md` was a proposal that teamsim never accepted. It is
  open for sauron to accept, change or drop in its own `docs/adr/`.
- `design/benchmark.md` and `design/control-agent-audit.md` are the disclosure-layer designs,
  kept for the later phase.
