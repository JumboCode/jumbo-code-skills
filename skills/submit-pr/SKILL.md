---
name: submit-pr
description: Prepare and submit a GitHub pull request from the current branch when the user asks to open, create, or submit a PR. Use for preflight validation and PR creation; do not use to review or merge someone else's PR.
---

# Submit PR

Create a focused, reviewable pull request only after checking that the branch is safe to submit. Preserve the user's work and follow the target repository's own rules over this general workflow.

## Establish repository context

1. Verify that the working directory is a Git repository with a GitHub remote. Determine the current branch and the repository's default base branch from Git or GitHub metadata; do not assume it is `main`.
2. Read applicable instructions before changing anything. Check `AGENTS.md`, `CONTRIBUTING.md`, the README, `.github/pull_request_template*`, CI workflows, and relevant package or build configuration.
3. Inspect the working tree, staged changes, branch commits, and complete diff against the base branch. Read relevant surrounding code, not only changed lines.
4. Stop and ask when the base branch is ambiguous, the current branch is the default branch, unrelated changes are mixed into the diff, or the intended PR scope cannot be inferred safely.

Never discard, reset, overwrite, or silently include unrelated user changes. Never force-push unless the user explicitly requests it and the exact branch is confirmed.

## Check merge readiness

Run the repository-required checks discovered from its documentation, scripts, and CI configuration. Prefer targeted checks first, then the broader required suite. Depending on the project, this commonly includes:

- formatting, linting, type checking, unit tests, integration tests, and a production build;
- new or updated tests for behavior changed by the branch;
- generated files, database migrations, seed data, and deployment configuration kept in sync;
- UI behavior, responsive layout, accessibility, loading/error states, and screenshots when the PR changes visible interfaces;
- API and schema compatibility, authorization, data ownership, failure handling, concurrency, and transaction boundaries;
- accidental secrets, credentials, personal data, debug output, large binaries, unsafe input handling, injection, XSS, CSRF, SSRF, and insecure file uploads.

Investigate failures rather than labeling them pre-existing without evidence. Fix only issues within the requested scope. If a required check cannot run, record the exact command and reason; do not claim it passed.

## Prepare the PR

Use the repository's naming and commit conventions when they exist. Otherwise:

- keep the branch and commits focused on one logical change;
- write a concise imperative title that describes the user-visible or engineering outcome;
- preserve meaningful existing commit history;
- do not amend published commits, rebase shared work, or rewrite history without explicit authorization.

Use the repository's PR template. If none exists, write a body containing only relevant sections from:

- Summary: why the change exists and what outcome it provides.
- Changes: the important implementation decisions, not a file-by-file transcript.
- Validation: commands actually run and their results.
- UI evidence: screenshots or recordings for visible changes.
- Risk and rollout: migrations, feature flags, compatibility, monitoring, or rollback concerns.
- Related work: the exact issue or project reference when known; never invent one.

Do not include secrets, internal tokens, unsupported claims, or boilerplate sections with no useful content.

## Submit safely

- If the user asked only to prepare, inspect, or validate the PR, stop before committing, pushing, or opening it.
- If the user explicitly asked to submit, open, or create the PR, that authorizes the ordinary non-destructive steps needed for this PR: stage only in-scope files, create a commit when necessary, push the current feature branch, and open the PR. Ask before any ambiguous inclusion or history rewrite.
- Open a draft PR when work is intentionally incomplete, required validation is blocked or failing, or the user requests a draft. Otherwise open it ready for review.
- Prefer the authenticated GitHub CLI or configured GitHub integration. If neither is available, provide the prepared title, body, and exact remaining step; never request or expose a personal access token in chat.
- Never merge the PR as part of this skill.

After submission, report the PR link, base and head branches, commit submitted, validation performed, and any remaining risk or blocked check.
