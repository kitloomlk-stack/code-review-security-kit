# security-audit

Audit the code or a given path against OWASP-style categories and report evidence-backed findings.

# security-audit

When invoked, optionally with a path or feature name:

1. Define scope: the given path, or the code that handles input, authentication, sessions, data access, file handling, outbound requests and configuration. State what is in and out of scope.
2. Map entry points (routes, handlers, queue consumers, CLI arguments) and trace untrusted data from each to where it is used.
3. Check against these categories: access control, injection, cross-site scripting, authentication and sessions, cryptography, misconfiguration, server-side request forgery, unsafe deserialisation and uploads, logging and error handling, input validation.
4. Look for hard-coded credentials by searching for patterns in code only. Do not open `.env` files, key files or credential stores, and never print a suspected secret; report its location only.
5. Do not edit files, run exploit code or contact external services.

Report findings ranked by severity. Each gives the location, the category, a short attack scenario, the evidence you read, and a fix. Finish with the areas you did not cover. Do not describe the code as secure; describe what was checked.
