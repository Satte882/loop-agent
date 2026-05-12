# loop-agent

`loop-agent` ist ein portables 2W-Starterkit für eine einfache ChatGPT ↔ GitHub Issues ↔ lokale Codex-CLI-Rückkopplung.

## Ziel

Nach der Zieldefinition soll der Nutzer möglichst aus dem operativen Hin-und-Her herausfallen:

1. ChatGPT formuliert einen kleinen Arbeitsblock als GitHub Issue.
2. Das Issue wird durch das Label `2w:ready` oder den Titelpräfix `2W_READY:` startfähig gemacht.
3. Ein GitHub Actions Workflow läuft auf einem lokalen self-hosted Runner.
4. Der Runner startet die lokal installierte und bereits eingeloggte Codex CLI mit `codex exec`.
5. Codex setzt den Arbeitsblock im jeweiligen Zielrepo um.
6. Der Workflow committet und pusht die Änderungen nach erfolgreichen Minimalchecks.
7. Der Workflow kommentiert das Issue mit Status, Commit-SHA, Tests und geänderten Dateien.
8. ChatGPT prüft GitHub und erstellt den nächsten Arbeitsblock oder einen Fix-Block.

## Wichtige Entscheidung

Der Standardpfad verwendet keinen `OPENAI_API_KEY` und nicht `openai/codex-action@v1`.

Stattdessen muss Codex CLI lokal auf dem self-hosted Runner verfügbar und bereits mit dem gewünschten Nutzerkonto angemeldet sein. Ob das konkret über ein ChatGPT-Plus-Konto funktioniert, wird lokal validiert. Das Repository erzwingt keinen API-Key.

## Nicht-Ziele

- Kein OperatorLoop.
- Keine eigene Engine.
- Keine Stage-Logik.
- Keine DecisionEngine.
- Kein PR-Workflow.
- Kein Remote-Server.
- Keine 0Admin-Produktlogik.

## Kern-Dateien

| Datei | Zweck |
|---|---|
| `.github/workflows/2w-codex.yml` | Lokaler 2W-Workflow für dieses Repo |
| `.github/ISSUE_TEMPLATE/2w-workblock.yml` | Issue-Vorlage für 2W-Arbeitsblöcke |
| `templates/2w-local-codex.yml` | Portabler Workflow für andere Repos |
| `templates/ISSUE_TEMPLATE/2w-workblock.yml` | Portables Issue-Template für andere Repos |
| `docs/INSTALL_IN_TARGET_REPO.md` | Installation in beliebigen Zielrepos |
| `docs/AUTH_WITH_CHATGPT_PLUS.md` | Authentifizierungsmodell ohne API-Key im Standardpfad |
| `docs/VALIDATE_CODEX_CLI_LOGIN.md` | Lokale Login-/CLI-Validierung |
| `docs/2W_PROTOCOL.md` | Minimaler Kommunikationsvertrag |
| `docs/LOCAL_SETUP.md` | Lokale Einrichtung des self-hosted Runners |
| `docs/VALIDATION.md` | Cline-Validierung nach lokalem `fetch` |
| `docs/SECURITY.md` | Sicherheitsgrenzen für 2W v0 |

## Betriebsmodell

Die GitHub-Historie ist die Wahrheitsquelle. Das ChatGPT-Fenster ist nur ein Trigger- und Prüfkanal. Lange Logs, vollständige Diffs und Secrets gehören nicht in den Chat.

## 2W v0 in einem Satz

ChatGPT erstellt ein startfähiges GitHub Issue, der self-hosted Runner führt lokal `codex exec` aus, der Workflow committet und kommentiert, ChatGPT prüft GitHub und startet den nächsten Block.

## Harte Grenzen

Dieses Repo kann keine lokale Codex-Anmeldung durchführen, keinen self-hosted Runner registrieren und nicht garantieren, dass ein ChatGPT-Plus-Login mit der installierten Codex CLI auf Ihrem Rechner funktioniert. Genau dafür gibt es `docs/VALIDATE_CODEX_CLI_LOGIN.md`.
