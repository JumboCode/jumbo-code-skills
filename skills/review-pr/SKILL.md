---
name: review-pr
description: Review a GitHub pull request for correctness, security, and merge readiness when the user asks for a PR review. Produce an evidence-backed draft first; do not edit code, post comments, approve, request changes, or merge without explicit authorization.
---

# Review PR

Find defects and meaningful risks before merge. Prioritize correctness over summarizing the implementation, and follow the target repository's own review rules over this general workflow.

## Establish review context

1. Identify the exact repository, PR, base commit, and head commit. If the target is ambiguous, stop and ask rather than reviewing a guessed PR.
2. Read applicable `AGENTS.md`, contribution guidance, PR templates, CI workflows, and project documentation. Read the PR description, linked issue or specification, checks, and existing review discussion.
3. Inspect the complete diff against the actual base and the relevant surrounding implementation. Trace changed interfaces to callers, consumers, tests, schemas, and configuration when needed.
4. Treat the PR description and existing comments as context, not proof. Verify claims against the code and avoid duplicating already-resolved feedback.

Do not modify the branch or working tree during a review. If the user later asks for fixes, treat that as a separate implementation task.

## Review priorities

Review in this order, spending attention according to the change's risk:

1. Correctness: edge cases, state transitions, error paths, invariants, concurrency, transactions, and data integrity.
2. Security and privacy: authentication, authorization, tenant or user ownership, input validation, injection, XSS, CSRF, SSRF, secrets, logs, file uploads, and sensitive data exposure.
3. Compatibility and operations: API contracts, schema migrations, rollback safety, deployment order, configuration, feature flags, and observability.
4. Reliability and performance: retries, idempotency, resource usage, query patterns, timeouts, partial failure, and realistic scale.
5. Product quality: requirements coverage, UI states, accessibility, responsive behavior, and user-facing error handling.
6. Verification and maintainability: meaningful tests, understandable design, duplication, and consistency with established project patterns.

Do not spend review attention on formatting already enforced by tools. Avoid subjective rewrites and speculative concerns without a concrete failure mode.

## Validate findings

Run safe, relevant checks when the environment supports them. Start with tests closest to the changed behavior, then consult broader CI results. Do not claim a failure belongs to the PR until the evidence supports that conclusion.

For every finding:

- cite the most specific changed file and line available;
- state the triggering input, state, or execution path;
- explain the observable impact;
- give a concise fix direction without taking ownership of the implementation;
- distinguish a required correction from an optional suggestion.

Use these severities consistently:

- **Blocker:** credible security exploit, data loss, outage, or a core path that is definitely incorrect.
- **Major:** a reproducible bug, significant regression, broken requirement, or unsafe migration that should be fixed before merge.
- **Minor:** a limited-scope defect or maintainability problem worth fixing but not normally merge-blocking.
- **Nit:** optional polish; use sparingly and never present it as a defect.

## Present the draft review

Lead with findings ordered by severity, then file location. For each one, include severity, location, impact, evidence, and fix direction. Follow with:

- open questions or assumptions that materially affect the verdict;
- checks run and checks that could not be run;
- a recommendation: request changes, comment, or approve.

Recommend **request changes** for any supported Blocker or Major finding. Recommend **comment** when only non-blocking feedback or unresolved questions remain. Recommend **approve** only when no blocking issue remains and the available validation is sufficient.

If no actionable defects are found, say `No blocking findings` and identify any meaningful untested area or residual risk. Do not invent feedback to make the review look substantial.

## Keep posting separate

Always show the complete review draft to the user before sending anything to GitHub. Ask for explicit confirmation of the intended action: post comments, approve, or request changes. Until confirmed, do not post inline comments or submit a review.

After confirmation, post only the reviewed content and report what was submitted. Never push commits, merge the PR, or change repository settings as part of this skill.
