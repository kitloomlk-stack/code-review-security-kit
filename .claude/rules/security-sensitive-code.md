---
paths:
  - "**/auth/**"
  - "**/api/**"
  - "**/middleware/**"
  - "**/*auth*.*"
  - "**/*session*.*"
  - "**/*crypto*.*"
---

# Security-sensitive code

Applies when you read or change authentication, session, API, middleware or cryptography code.

- Enforce access control on the server for every request. Check ownership of any object id taken from input.
- Validate input at the boundary against a schema. Reject invalid data instead of trying to clean it.
- Use parameterised queries and argument arrays. Never build SQL or shell commands by string concatenation with user data.
- Encode output for its context. Do not insert untrusted data as raw HTML.
- Hash passwords with a modern, slow algorithm. Use secure random generation for tokens. Do not write your own cryptography.
- Set secure cookie flags and sensible session lifetimes. Rate-limit login, reset and other abuse-prone endpoints.
- Return generic error messages to clients. Do not log secrets, tokens or personal data.
- If a change weakens a check, widens permissions or disables validation, call it out explicitly in your summary.
