---
applyTo: "**"
description: "Always-on rule: keep credentials out of code, logs, commits and chat."
---

# Secrets hygiene

Always apply this.

- Never place API keys, tokens, passwords, private keys or connection strings with credentials in source files, tests, fixtures, logs, URLs or commit messages.
- Do not open, print or copy `.env` files, key files or credential stores. Work from the code that reads the configuration.
- Read configuration from the environment. Document required variable names in an example file that contains names only.
- Use obviously fake values in tests and documentation, such as `test-token-not-real`.
- If you notice something that looks like a real secret in the repository, stop and tell the user. Say that it must be revoked and rotated; do not quote the value.
- Do not send secrets to external services, paste them into issues or pull requests, or include them in your summary.
