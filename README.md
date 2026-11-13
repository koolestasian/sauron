# sauron (working name)

A clean, deterministic, zero-cost fuzzing and delta-debugging harness for multi-tenant AI agent isolation. 

You point it at an agent. A scripted, seeded team talks to the agent through a synthetic team sandbox , and every private planted fact carries a marker. The agent's model is replaced by a tracer that
repeats every marker it was shown. If a marker from one person's private chat comes out in
front of another person, the agent's plumbing showed one person's data in another's turn. The
result is one replay file and the shortest message sequence that still does it. No model, no
key, no bill; the same seed gives the same result.

## Status

Pre-alpha, private. Nothing is built yet. The research behind it is in
[docs/research/](docs/research/) and the decisions it inherits are in
[docs/inherited/](docs/inherited/).

## Licence

MIT OR Apache-2.0, at your option. See `LICENSE-MIT` and `LICENSE-APACHE`.
