# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## Project

**BuzzFam Advertising — official website.**

This is the source repository for BuzzFam Advertising's official website.

## Current state (read this first)

This repository is **greenfield**. As of this writing it contains only:

```
.
├── README.md     # one-line project description
└── CLAUDE.md     # this file
```

There is **no application code, build system, package manifest, framework, or
tooling yet**. Do not assume a tech stack exists — none has been chosen. When a
task implies one (e.g. "add a contact page"), either:

1. Use the stack already present in the repo if code has been added since this
   file was written (check first — see "Keeping this file current"), or
2. If still greenfield, confirm the intended stack with the user before
   scaffolding, then update this file to reflect what was chosen.

Do not invent a framework, directory layout, or conventions and present them as
established. Treat anything not visible in the working tree as undecided.

## Git workflow & conventions

These are the conventions actually in evidence in this repo's history and
branch setup — follow them.

- **Default branch:** `main`.
- **Feature branches:** use the `claude/<short-kebab-description>` naming
  pattern (e.g. `claude/claude-md-docs-2014kj`). Develop on a feature branch,
  never commit directly to `main`.
- **Commits:** write clear, descriptive, imperative-mood messages
  (e.g. "Add contact form to homepage"). Keep commits focused.
- **Pushing:** push with `git push -u origin <branch-name>`. Do **not** open a
  pull request unless the user explicitly asks for one.
- **Merged PRs are final:** if a feature branch's PR has already merged, start
  follow-up work fresh from the latest `main` rather than stacking new commits
  on already-merged history.

## Working agreements for assistants

- **Be honest about state.** This is a near-empty repo. Don't describe
  structure, scripts, or workflows that don't exist. If asked "how do I run the
  tests," the correct answer today is "there is no test setup yet."
- **Scaffold deliberately.** When introducing the first real code, prefer a
  conventional, well-supported layout for the chosen stack and document it here
  in the same change.
- **Keep secrets out of git.** No API keys, tokens, or `.env` files committed.
  Add a `.gitignore` as soon as a stack is chosen.
- **Verify before claiming done.** Once tooling exists, run the project's build
  / lint / tests and report real results.

## Keeping this file current

This file describes a project that will grow. When you add or change anything
structural — a framework, a build tool, a directory layout, scripts, deploy
config, test setup — **update the relevant section of this file in the same
change** so it always reflects the real state of the codebase. Sections to add
once they apply:

- **Tech stack** — language(s), framework, package manager.
- **Project structure** — what lives where.
- **Development** — install, run-locally, build, and test commands.
- **Deployment** — how the site is built and published, and where it's hosted.
- **Conventions** — formatting, linting, naming, component/style patterns.
