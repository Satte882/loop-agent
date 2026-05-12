# Local Setup

## Ziel

Dieses Dokument beschreibt die lokalen Schritte, die nicht im Repository vorab erledigt werden können.

## Voraussetzungen

- Lokales Repository: `C:\Users\user\Documents\GitHub\loop-agent`
- GitHub Actions self-hosted Runner auf dem Rechner des Nutzers
- Repository Secret `OPENAI_API_KEY`
- GitHub Actions im Repository aktiviert

## Lokaler Runner

Der Runner muss für `Satte882/loop-agent` registriert werden und mit dem Label `self-hosted` verfügbar sein.

GitHub-Pfad:

`Settings -> Actions -> Runners -> New self-hosted runner`

Danach den von GitHub angezeigten Installationsbefehlen folgen. Der Token ist kurzlebig und darf nicht committed werden.

## Repository Secret

GitHub-Pfad:

`Settings -> Secrets and variables -> Actions -> New repository secret`

Name:

`OPENAI_API_KEY`

Wert:

Persönlicher OpenAI API Key für Codex Action.

## Start eines 2W-Laufs

Ein Issue startet den Workflow, wenn eine der Bedingungen erfüllt ist:

- Label `2w:ready`
- Titel beginnt mit `2W_READY:`

Der Titelpräfix ist der robuste Fallback, falls das Label noch nicht existiert.

## Lokale Validierung

Nach dem Fetch soll Cline prüfen:

- Workflow-Datei ist syntaktisch gültiges YAML.
- Workflow nutzt `runs-on: [self-hosted]` für Codex.
- Workflow hat keine Cloud-Ausführung für Codex.
- Workflow startet nur bei startfähigem Issue.
- Workflow kommentiert Erfolg und Fehler ins Issue.
- Dokumentation passt zum Workflow.
