# Local Setup

## Ziel

Lokale Voraussetzungen für 2W mit lokaler Codex CLI.

## Voraussetzungen

- Lokales Repository-Checkout des Zielrepos, beliebiger Pfad
- GitHub Actions self-hosted Runner, registriert für das Zielrepo
- Lokal installierte Codex CLI
- `codex exec` läuft im Runner-Kontext ohne Rückfrage
- GitHub Actions ist im Zielrepo aktiviert

## Kein API-Key im Standardpfad

2W v0 nutzt im Standardpfad keinen `OPENAI_API_KEY` und nicht `openai/codex-action@v1`.

Der Workflow ruft lokal `codex exec` auf.

## Runner registrieren

Der Runner wird pro Zielrepo über GitHub registriert:

`Settings -> Actions -> Runners -> New self-hosted runner`

Der Runner muss mit dem Label `self-hosted` verfügbar sein.

## Wichtiger Hinweis: prepare-Job

Der `prepare`-Job läuft auf `ubuntu-latest`. Dieser Job liest nur das Issue und prüft Trigger-Bedingungen. Der eigentliche Codex-Job läuft auf dem self-hosted Runner. Beide Runner müssen für das Zielrepo erreichbar sein.

## Start

Ein Issue startet den Lauf, wenn eine Bedingung gilt:

- Titel beginnt mit `2W_READY:`
- Label `2w:ready` ist gesetzt

## Nutzungsmodell

ChatGPT erstellt ein Issue. Der Runner startet. Codex bearbeitet das Repo. Der Workflow committet und kommentiert. ChatGPT prüft GitHub.
