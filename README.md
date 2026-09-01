# JumboCode Skills

This repository contains reusable agent skills for JumboCode. It starts with two skills that provide a consistent pull-request workflow across club projects while respecting each project's own instructions and CI configuration, and it can grow as the club adds other shared workflows.

## Skills

- `submit-pr` validates the current branch, runs repository-specific checks, prepares the PR, and submits it when authorized. It never merges.
- `review-pr` performs a correctness- and security-focused review, presents a draft locally, and asks before posting anything to GitHub.

## Repository layout

```text
skills/
├── submit-pr/
│   ├── SKILL.md
│   └── agents/openai.yaml
└── review-pr/
    ├── SKILL.md
    └── agents/openai.yaml
```

Publish this repository in the club's shared GitHub organization. Members can install each directory through their Codex skill installer or copy the skill directories into their Codex skills directory.

## Example prompts

```text
Use $submit-pr to validate and submit my current branch as a pull request.
Use $submit-pr to prepare this branch, but stop before pushing.
Use $review-pr to review PR #123 and draft actionable findings.
```

The review skill never posts, approves, or requests changes until the user confirms the completed draft.
