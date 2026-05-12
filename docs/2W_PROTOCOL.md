# 2W Protocol

## Purpose

2W is the minimal communication path between ChatGPT, GitHub Issues, GitHub Actions and local Codex CLI.

## Flow

1. ChatGPT creates a small GitHub Issue.
2. The Issue is made runnable with the label `2w:ready` or the title prefix `2W_READY:`.
3. GitHub Actions starts.
4. The prepare job reads the Issue and checks the trigger.
5. The Codex job runs on a self-hosted runner.
6. Local Codex CLI receives the Issue body as workblock input.
7. The workflow runs available checks.
8. The workflow commits and pushes successful changes.
9. The workflow comments on the Issue with result, commit and checks.
10. ChatGPT reviews GitHub and decides the next workblock.

## Scope

This repository is not an operator engine. It only provides the minimal relay structure.

## Required Issue fields

- **Ziel** (required): What should Codex do concretely?
- **Kontext und Grenzen** (optional): What Codex should know or avoid.
- **Fertig wenn** (optional): How ChatGPT recognises completion.

These fields match the Issue Template. Do not add more required fields for 2W v0.

## Status labels

| Label | Meaning |
|---|---|
| `2w:ready` | Workblock can start |
| `2w:running` | Workblock is running |
| `2w:done` | Workblock completed |
| `2w:failed` | Workblock failed |

## Result format

The Issue comment contains:

- `2W_DONE` or `2W_FAILED` marker
- repository and issue number
- commit SHA
- test status
- changed files list
- ChatGPT next-step instruction
