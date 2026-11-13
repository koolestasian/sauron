# Inherited from teamsim

sauron was split from teamsim (a sandbox of simulated teammates) on 2026-11-13. The decision
records and designs here are copied unchanged as background. They describe the earlier project;
read "teamsim" in them as the parent project. Sauron's own decisions go in `docs/adr/`.

- `adr/0006-core-shape.md` (event engine, keyed randomness, packs as data, ledger, verdicts,
  run records, reduction) and `adr/0004-python-standard-library-core.md` apply here as written.
- `adr/0005-scope-instrument-first.md` applies, except for the order of work, which sauron
  changes (exposure tests on real agents first).
- `design/benchmark.md` and `design/control-agent-audit.md` are the disclosure-layer designs,
  kept for the later phase.
