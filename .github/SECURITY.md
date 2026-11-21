# Security Policy

## Reporting a vulnerability

Report vulnerabilities privately through GitHub private vulnerability reporting:

<https://github.com/koolestasian/sauron/security/advisories/new>

Do not open a public issue, pull request or discussion for a vulnerability.

Include the affected commit, the steps to reproduce and the impact as you see it. Do not send
real API keys, tokens, chat messages or personal data. Describe what you found and we will agree
how to share the rest.

## Supported versions

No release has been made yet, so no version is supported.

## What to expect

Reports are handled by the maintainer on a best-effort basis. We aim to acknowledge
a report within 7 days. There is no fixed timeline for a fix or for disclosure.

## Scope

The repository currently contains research and design documents. For future
implementations, relevant issues include:

- Exposure of API keys or tokens in logs, reports, the live view or error messages.
- Scenario content interpreted as instructions by the harness.
- The target reaching the grading key or escaping its intended sandbox.
- Unexpected network access outside the local test environment.

Out of scope: vulnerabilities in third-party services and in the products being tested. Report
those to their maintainers.
