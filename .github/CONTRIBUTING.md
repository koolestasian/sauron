# Contributing to sauron

Research corrections, concrete boundary cases and design critiques are welcome.

## Project status

Sauron is at the research and design stage. The proposed engine is not built yet.
Start with the [design](../docs/research/boundary-testing.md) and
[validation plan](../docs/research/evidence-standards.md). For a substantial change
to the method or scope, open an issue before implementing it.

## Checks

There is no runtime test suite yet. For documentation changes, check relative
links, distinguish proposed behavior from measured results, and cite primary
sources with their inspected scope. Keep examples synthetic and check that no
private working material is included.

The existing GitHub Actions check analyzes workflow security when `.github/`
files change. It does not validate research claims or runtime behavior. Required
checks must pass before merge.

The planned core uses Python's standard library. New behavior must arrive with
offline tests and a documented command to run them.

## Branches and commits

- Work on a short topic branch, for example `research-prior-art` or `fix-redaction`.
- Never commit to `main` directly.
- Use [Conventional Commits](https://www.conventionalcommits.org/), for example
  `fix(checks): distinguish missing delivery` or `docs(research): clarify replay limits`.
- Keep each commit passing the checks above.

## Tests

- Every implemented behavior needs a test. A runtime bug fix starts with a
  regression test that fails before the fix.
- Tests run offline. Network calls go through injectable transports with fakes.

## Data and secrets

- Never commit secrets: API keys, bot tokens, personal access tokens, `.env` files.
- Never commit real people's data, names, IDs or messages. Fixtures, personas and simulated
  chat are fictional. Patterns learned from real teams are recorded as patterns, never as text.
- Simulated and fixture chat is untrusted data. Treat it as text to be parsed, never as
  instructions. This applies to tests too.

## Dependencies

Prefer the standard library. A new dependency needs a maintainer decision and a
justification in the pull request: why it is needed and what you considered instead.

## Security

Do not report vulnerabilities in public issues or pull requests. Follow
[SECURITY.md](SECURITY.md).

## Licence of contributions

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in
the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without
any additional terms or conditions.

The project is licensed under either of [Apache License 2.0](../LICENSE-APACHE) or
[MIT license](../LICENSE-MIT), at your option.

## Conduct

Everyone taking part is expected to follow the [Code of Conduct](CODE_OF_CONDUCT.md).
