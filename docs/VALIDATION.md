# Validation

## Ziel

Diese Datei beschreibt die Validierung nach lokalem `git fetch/pull` — bevor ein echter 2W-Lauf gestartet wird.

## Kernprüfung

1. Sind alle erwarteten Dateien vorhanden?
2. Ist die Workflow-Datei gültiges YAML?
3. Nutzt der Codex-Job `runs-on: [self-hosted]`?
4. Wird lokal `codex exec` aufgerufen?
5. Gibt es keinen Standardpfad mit `OPENAI_API_KEY`?
6. Gibt es keinen Standardpfad mit `openai/codex-action@v1`?
7. Kommentiert der Workflow Erfolg und Fehler ins Issue?
8. Funktioniert `codex --version` im Runner-Kontext?
9. Funktioniert ein kleiner Testlauf mit Titelpräfix `2W_READY:`?

## Erwartete Dateien

- `README.md`
- `.github/workflows/2w-codex.yml`
- `.github/ISSUE_TEMPLATE/2w-workblock.yml`
- `templates/2w-local-codex.yml`
- `templates/ISSUE_TEMPLATE/2w-workblock.yml`
- `docs/INSTALL_IN_TARGET_REPO.md`
- `docs/AUTH_WITH_CHATGPT_PLUS.md`
- `docs/VALIDATE_CODEX_CLI_LOGIN.md`
- `docs/2W_PROTOCOL.md`
- `docs/LOCAL_SETUP.md`
- `docs/VALIDATION.md`
- `docs/SECURITY.md`
- `.gitignore`

## Ergebnisformat für Cline

- Status: GREEN, YELLOW oder RED
- geprüfte Dateien
- gefundene Risiken
- konkrete Fix-Empfehlung, falls nötig

## Bewertung

GREEN: Alle Dateien vorhanden, kein API-Key-Pfad, Workflow syntaktisch korrekt, lokaler Runner sieht Codex CLI, Test-Issue läuft bis `2W_DONE`.

YELLOW: GitHub-Struktur korrekt, aber lokaler Codex-Login oder Runner-Kontext noch nicht bewiesen.

RED: Workflow startet nicht, Codex CLI fehlt, Login hängt interaktiv oder Commit/Kommentar funktioniert nicht.

## Cline-Validierungsprompt

Kopiere diesen Block direkt in Cline nach `git pull`:

```text
Du bist ein kritischer Repo-Validator für das 2W-System (loop-agent).

Validiere das lokale Repository nach folgendem Raster:

1. STRUKTUR: Prüfe, ob alle Pflichtdateien aus docs/VALIDATION.md vorhanden sind.
2. API-KEY-FREIHEIT: Suche in .github/workflows/2w-codex.yml und templates/2w-local-codex.yml nach OPENAI_API_KEY oder openai/codex-action. Wenn gefunden: RED.
3. WORKFLOW: Prüfe, ob der Codex-Job auf runs-on: [self-hosted] läuft und codex exec aufruft.
4. COMMIT-PFAD: Prüfe, ob git add/commit/push nur nach den Checks ausgeführt wird.
5. KOMMENTAR: Prüfe, ob der Workflow 2W_DONE und 2W_FAILED ins Issue schreibt.
6. .2w/-AUSSCHLUSS: Prüfe, ob .2w/ in .gitignore steht.
7. ISSUE-TEMPLATE: Prüfe, ob das Template das Titelpräfix 2W_READY: vorausfüllt und nur minimale Pflichtfelder hat.
8. SHELL-SICHERHEIT: Prüfe im Build-Codex-Prompt-Step, ob Issue-Titel per printf statt echo interpoliert werden.
9. PORTABILITÄT: Prüfe, ob hardcodierte persönliche Pfade in Doku-Dateien stehen.

Ausgabe:
- Status: GREEN / YELLOW / RED
- Liste der gefundenen Probleme mit Dateipfad und Zeile
- Empfohlene Fix-Schritte, falls YELLOW oder RED
- Bestätigung, welche Schritte noch lokal auf dem Runner ausgeführt werden müssen
```
