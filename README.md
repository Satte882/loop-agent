# loop-agent

`loop-agent` ist die minimale 2W-Schicht für eine einfache ChatGPT ↔ Codex-CLI-Rückkopplung.

## Ziel

Nach der Zieldefinition soll der Nutzer möglichst aus dem operativen Hin-und-Her herausfallen:

1. ChatGPT formuliert einen kleinen Arbeitsblock als GitHub Issue.
2. Das Issue wird durch das Label `2w:ready` oder den Titelpräfix `2W_READY:` startfähig gemacht.
3. Ein GitHub Actions Workflow läuft auf einem lokalen self-hosted Runner.
4. Der Runner startet Codex CLI im Repository-Checkout.
5. Codex setzt den Arbeitsblock um.
6. Der Workflow committet und pusht die Änderungen nach erfolgreichen Minimalchecks.
7. Der Workflow kommentiert das Issue mit Status, Commit-SHA, Tests und geänderten Dateien.
8. Ein optionaler Mini-Ping ins offene ChatGPT-Fenster enthält nur Repo, Issue, Commit und Status.
9. ChatGPT prüft GitHub und erstellt den nächsten Arbeitsblock oder einen Fix-Block.

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
| `.github/workflows/2w-codex.yml` | Startet Codex CLI auf einem self-hosted GitHub Actions Runner |
| `.github/ISSUE_TEMPLATE/2w-workblock.yml` | Vorlage für 2W-Arbeitsblöcke |
| `docs/2W_PROTOCOL.md` | Minimaler Kommunikationsvertrag |
| `docs/LOCAL_SETUP.md` | Lokale Einrichtung des self-hosted Runners |
| `docs/VALIDATION.md` | Cline-Validierung nach lokalem `fetch` |
| `docs/SECURITY.md` | Sicherheitsgrenzen für 2W v0 |

## Betriebsmodell

Die GitHub-Historie ist die Wahrheitsquelle. Das ChatGPT-Fenster ist nur ein Trigger- und Prüfkanal. Lange Logs, vollständige Diffs und Secrets gehören nicht in den Chat.

## 2W v0 in einem Satz

ChatGPT erstellt ein startfähiges GitHub Issue, der self-hosted Runner führt Codex CLI aus, der Workflow committet und kommentiert, ChatGPT prüft GitHub und startet den nächsten Block.

## Harte Grenzen

Dieses Repo legt die GitHub-Struktur an. Es kann nicht automatisch Ihren lokalen GitHub Actions Runner registrieren und kein `OPENAI_API_KEY` Secret in Ihrem GitHub Account setzen. Diese zwei Punkte sind bewusst außerhalb des Repository-Inhalts.
