# Security Policy

## Supported versions

Only `main` is supported. There are no LTS branches.

## Reporting a vulnerability

**Do not file public issues for security problems.**

Email the maintainer directly (see repository owner contact on GitHub). Acknowledgment within 72 hours.

## Scope

In scope:

- Leaks of user API keys (e.g., `$OPENROUTER_API_KEY`) via script output, logs, or error messages
- Command injection in any script that accepts CLI args
- Path-traversal bugs in scripts that read/write user files
- Insecure defaults that would surprise a careful user

Out of scope:

- The Python packages or external CLIs we depend on (report to those projects)
- The fact that AI-generated content can be wrong (this is a known limitation; users are responsible for review)
- KDP, Amazon, or OpenRouter terms-of-service compliance (the user accepts responsibility for their account when they use these services)

## Disclosure

Once a fix is available, the maintainer will publish a security advisory on GitHub. Reporters are credited unless they request otherwise.
