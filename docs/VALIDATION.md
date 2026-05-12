# Validation

## Ziel

Diese Datei beschreibt die einfache Validierung nach lokalem Fetch.

## Lokaler Pfad

`C:\Users\user\Documents\GitHub\loop-agent`

## Kernprüfung

1. Sind die erwarteten Dateien vorhanden?
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

GREEN bedeutet: lokaler Runner sieht Codex CLI, ein Test-Issue läuft bis `2W_DONE`, und der Workflow erzeugt einen nachvollziehbaren Issue-Kommentar.

YELLOW bedeutet: GitHub-Struktur ist korrekt, aber lokaler Codex-Login oder Runner-Kontext ist noch nicht bewiesen.

RED bedeutet: Workflow startet nicht, Codex CLI fehlt, Login hängt interaktiv oder Commit/Kommentar funktioniert nicht.