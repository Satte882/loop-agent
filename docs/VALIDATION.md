# Validation

## Ziel

Diese Datei beschreibt, was nach dem lokalen Fetch mit Cline geprüft werden soll.

## Repo-Pfad lokal

`C:\Users\user\Documents\GitHub\loop-agent`

## Pflichtprüfungen

1. Repository lokal aktualisieren.
2. Prüfen, ob alle erwarteten Dateien vorhanden sind.
3. YAML-Syntax der Workflow-Datei prüfen.
4. Prüfen, ob der Codex-Job auf `self-hosted` läuft.
5. Prüfen, ob der Workflow nur startfähige 2W-Issues verarbeitet.
6. Prüfen, ob Erfolg und Fehler ins Issue kommentiert werden.
7. Prüfen, ob keine Secrets oder Tokens in Dateien enthalten sind.
8. Optional: Test-Issue mit Titelpräfix `2W_READY:` auslösen, sobald Runner und Secret eingerichtet sind.

## Erwartete Dateien

- `README.md`
- `.github/workflows/2w-codex.yml`
- `.github/ISSUE_TEMPLATE/2w-workblock.yml`
- `docs/2W_PROTOCOL.md`
- `docs/LOCAL_SETUP.md`
- `docs/VALIDATION.md`
- `docs/SECURITY.md`
- `.gitignore`

## Ergebnisformat für Cline

Cline soll berichten:

- Status: GREEN, YELLOW oder RED
- geprüfte Dateien
- gefundene Risiken
- Syntax-/Strukturfehler
- konkrete Fix-Empfehlung, falls nötig
