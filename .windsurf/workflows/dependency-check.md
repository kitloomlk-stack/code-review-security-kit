# dependency-check

Review dependencies for necessity, maintenance, known-risk signals and licences.

# dependency-check

When invoked, optionally with the name of a package or manifest:

1. Locate the manifests and lockfiles (package.json, requirements, pyproject.toml, go.mod, Cargo.toml and similar). List direct dependencies with their declared version ranges.
2. For each direct dependency, note whether it is actually imported, whether it duplicates another dependency or the standard library, and whether the version is pinned through a lockfile.
3. If the project already provides an audit command and it works offline or against the project's configured registry, run it and report the output as it is. Otherwise say that no audit was run.
4. Note licences from the manifests or the packages' own metadata that are already on disk, and flag any that look incompatible with the project's licence or that are missing.
5. Flag packages that look unmaintained, are deprecated in their own metadata, or are unusually broad for how they are used.
6. Do not install, update or remove anything.

Report a table of findings with package, issue, evidence and suggested action, ordered by importance, and list what you could not verify.
