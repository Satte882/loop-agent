# 2W Protocol

## Purpose

2W is the minimal communication path between ChatGPT, GitHub Issues, GitHub Actions and Codex CLI.

## Flow

1. ChatGPT creates a small GitHub Issue.
2. The Issue is made runnable with the label `2w:ready` or the title prefix `2W_READY:`.
3. GitHub Actions starts on a self-hosted runner.
4. Codex CLI receives the Issue body as workblock input.
5. The workflow commits and pushes successful changes.
6. The workflow comments on the Issue with result, commit and checks.
7. ChatGPT reviews GitHub and decides the next workblock.

## Scope

This repository is not an operator engine. It only provides the minimal relay structure.

## Required Issue fields

- Goal
- Allowed paths
- Forbidden paths
- Expected checks
- Acceptance criteria
- Notes

## Status labels

| Label | Meaning |
|---|---|
| `2w:ready` | Workblock can start |
| `2w:running` | Workblock is running |
| `2w:done` | Workblock completed |
| `2w:failed` | Workblock failed |

## Result format

The Issue comment should contain:

- status
- repository
- issue number
- commit sha
- test status
- changed files
- compact final message
