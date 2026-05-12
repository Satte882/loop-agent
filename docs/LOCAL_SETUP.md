# Local Setup

## Ziel

Lokale Voraussetzungen für 2W mit lokaler Codex CLI.

## Voraussetzungen

- Lokales Repository: `C:\Users\user\Documents\GitHub\loop-agent`
- GitHub Actions self-hosted Runner
- Lokal installierte Codex CLI
- `codex exec` läuft im Runner-Kontext ohne Rückfrage
- GitHub Actions ist im Zielrepo aktiviert

## Kein API-Key im Standardpfad

2W v0 nutzt im Standardpfad keinen `OPENAI_API_KEY` und nicht `openai/codex-action@v1`.

Der Workflow ruft lokal `codex exec` auf.

## Runner

Der Runner wird pro Zielrepo über GitHub registriert:

`Settings -> Actions -> Runners -> New self-hosted runner`

Der Runner muss mit dem Label `self-hosted` verfügbar sein.

## Start

Ein Issue startet den Lauf, wenn eine Bedingung gilt:

- Titel beginnt mit `2W_READY:`
- Label `2w:ready` ist gesetzt

## Nutzungsmodell

ChatGPT erstellt ein Issue. Der Runner startet. Codex bearbeitet das Repo. Der Workflow committet und kommentiert. ChatGPT prüft GitHub.
