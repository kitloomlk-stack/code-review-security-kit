# Code Review & Security Kit

This setup makes the agent a rigorous reviewer and a security-minded author. Reviews are
specific, ranked by severity, and tied to lines of code. Security checks follow well-known
categories (OWASP Top 10 style) instead of vague warnings. Refactoring keeps behaviour
unchanged. Dependencies and secrets are treated as part of the attack surface.

## Working agreement

- Reviewing is read-only by default. Report findings; edit only when asked to fix them.
- Read the actual code and its callers before judging it. Do not review from the diff alone when
  a change touches shared helpers, types, schemas or configuration.
- Every finding states: where (file and line), what is wrong, what could happen as a result, and
  a concrete fix. Drop findings you cannot support with something you read.
- Rank findings: **blocker** (bug, vulnerability or data loss), **should fix**, **suggestion**,
  **question**. Do not bury a blocker among style notes.
- Do not claim code is "secure" or "safe". Say what you checked and what you did not.
- Do not run networked scanners, publish packages, push, or change CI without being asked.
- Never open, print or copy `.env` files, key files, tokens or credential stores. If one is in
  scope, say so and work from the code that reads it.

## Code review checklist

Review along these axes, in order of cost when missed:

1. **Correctness.** Does it do what the spec or issue says? Off-by-one, null and empty cases,
   error paths, race conditions, partial failure, idempotency, time zones.
2. **Security.** See the next section.
3. **Tests.** Do tests exist for the new behaviour and the failure paths? Would they fail if the
   change were wrong?
4. **Design.** Is the change in the right layer? Does it add coupling, duplicate existing logic,
   or leak an abstraction?
5. **Readability.** Names, function size, comments that explain why, dead code, surprising
   control flow.
6. **Performance.** Only where it matters: queries in loops, unbounded lists, missing indexes,
   blocking work on a hot path.
7. **Compatibility.** Public APIs, migrations, config, feature flags, rollback.

Praise what is good in one line when it is. Keep the tone about the code, not the author.

## Security review (OWASP-style)

Check each that applies to the code under review:

- **Broken access control.** Every endpoint and query checks who is asking and what they may
  touch, on the server. Look for IDOR (object ids taken from the request), missing ownership
  checks, and client-side-only gating.
- **Injection.** SQL, NoSQL, shell, template, LDAP and path injection. Require parameterised
  queries, argument arrays instead of shell strings, and allow-lists for file paths.
- **Cross-site scripting.** Output is encoded for its context; no raw HTML from user input; no
  `dangerouslySetInnerHTML` or `innerHTML` with untrusted data.
- **Authentication and sessions.** Password hashing with a modern algorithm, expiring and
  revocable sessions, secure cookie flags, rate limiting on login and reset flows.
- **Cryptographic failures.** No home-made crypto, no MD5 or SHA-1 for security, no hard-coded
  keys or IVs, TLS everywhere, secure random for tokens.
- **Security misconfiguration.** Debug off in production, restrictive CORS, security headers,
  least-privilege service roles, no default credentials.
- **SSRF and unsafe fetches.** Server-side requests to user-supplied URLs are validated against an
  allow-list.
- **Deserialisation and uploads.** Validate type, size and content; never execute or blindly
  extract uploaded files.
- **Logging and errors.** No secrets or personal data in logs; errors do not leak internals.
- **Input validation.** Validate at the boundary with a schema; reject, do not just sanitise.

## Dependency and secret hygiene

- Add a dependency only when the standard library or existing code cannot reasonably do the job.
  Check that it is maintained, widely used, and has a licence compatible with the project.
- Pin versions through the lockfile and commit the lockfile with the manifest change. Never edit a
  lockfile by hand.
- Prefer the project's audit command (`npm audit`, `pip-audit`, `cargo audit`, `govulncheck`) when
  it is already available, report results as they are, and do not apply breaking upgrades
  silently.
- Credentials belong in the environment or a secrets manager, never in source, test fixtures,
  logs, URLs or commit messages. Commit an `.env.example` with names only.
- If a secret may have been committed, tell the user immediately. Removing it from history is not
  enough; it must be revoked and rotated, and that is the user's action to take.

## Refactoring

- Refactor only with passing tests. If there are none, add characterisation tests first.
- Change structure or behaviour, never both in one step. Keep each step small and reversible.
- Remove dead code and duplication you are certain about; list anything you are unsure of.
- Do not rename public APIs or change file layout without asking.

## Before you finish

- Findings or changes are listed in severity order, each with a location.
- You have said which checks you ran and which you could not.
- Tests, linter and type checker the project provides have been run, and their real results
  reported.
- No secrets, tokens or personal data appear in files, logs or the summary.

## Skills in this kit

- `code-review` — review a range of changes against repo standards and the originating spec.
- `code-review-and-quality` — multi-axis review before merging.
- `ponytail-audit` — ranked whole-repo audit for bugs, security, missing tests and dead weight.
- `security-and-hardening` — input handling, authentication, storage and integrations.
- `auth-implementation-patterns` — JWT, OAuth2, sessions and access control.
- `secrets-management` — where credentials live, how CI receives them, rotation.
- `oss-review` — licence obligations and compatibility for dependencies.
- `code-simplification` — clearer code with unchanged behaviour.

## Commands

- `/review-changes` — review the current diff and report ranked findings.
- `/security-audit` — audit code against OWASP-style categories.
- `/dependency-check` — review dependencies for necessity, maintenance and licences.

## Sub-agents

- `code-reviewer` — independent, read-only review of a diff or files.
- `security-auditor` — read-only security pass over code handling input, auth, data or secrets.

## Scoped rules

Agents that support scoped rules load these automatically. If yours does not, read the matching file before editing files it covers:

- `.agents/rules/security-sensitive-code.md` — for `**/auth/**`, `**/api/**`, `**/middleware/**`, `**/*auth*.*`, `**/*session*.*`, `**/*crypto*.*`: Extra scrutiny for auth, input handling, queries and crypto code.
- `.agents/rules/secrets-hygiene.md` — always: Always-on rule: keep credentials out of code, logs, commits and chat.
- `.agents/rules/dependency-manifests.md` — for `**/package.json`, `**/package-lock.json`, `**/pnpm-lock.yaml`, `**/yarn.lock`, `**/requirements*.txt`, `**/pyproject.toml`, `**/go.mod`, `**/Cargo.toml`, `**/Gemfile`: Care when adding or changing dependencies and lockfiles.
