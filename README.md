# loop-agent

`loop-agent` ist ein portables 2W-Starterkit für eine einfache ChatGPT ↔ GitHub Issues ↔ lokale Codex-CLI-Rückkopplung.

## Verbindlicher Prozessvertrag

Der verbindliche Zielprozess ist nicht: Nutzer prüft nach jedem Lauf manuell GitHub oder startet Cline.

Der verbindliche Zielprozess ist:

1. Nutzer und ChatGPT stimmen ein Ziel ab.
2. ChatGPT erstellt das erste `2W_READY:`-Issue.
3. GitHub Actions startet den self-hosted Runner.
4. Der Runner führt lokal `codex exec` aus.
5. Codex setzt den Arbeitsblock um.
6. Der Workflow committet und pusht auf `main`.
7. Der Workflow kommentiert `2W_DONE` mit Commit-SHA, Teststatus und geänderten Dateien.
8. Der lokale Watcher erkennt `2w:done` automatisch.
9. Der Watcher erzeugt automatisch den Prüfauftrag mit Commit-SHA, Issue-Nummer und Repository-Kontext.
10. Die Reviewer-Rolle liest Commit, Diff und Issue-Kommentar selbst über GitHub.
11. Die Reviewer-Rolle entscheidet ohne Nutzerinteraktion: `COMPLETE`, `NEXT_ISSUE`, `FIX_ISSUE` oder `BLOCKED`.
12. Der Watcher kommentiert `2W_REVIEWED`, setzt `2w:reviewed` und erstellt bei `NEXT_ISSUE` oder `FIX_ISSUE` automatisch das nächste `2W_READY:`-Issue.
13. Der Kreislauf läuft weiter, bis `COMPLETE` oder `BLOCKED` erreicht ist.

Nach der initialen Zieldefinition darf der Nutzer nicht gebraucht werden, um Commit-SHAs zu kopieren, Issues prüfen zu lassen, Cline zu starten oder den nächsten Arbeitsblock manuell anzustoßen. Wenn das nötig ist, ist der Zielprozess nicht erfüllt.

Siehe dazu: `docs/2W_PROCESS_CONTRACT.md`.

## Aktueller Validierungsstand

Stand: 2W-Ausführungskern und Dedupe-Gate sind bewiesen. Der lokale Watcher ist implementiert, aber der autonome Watcher-Betrieb ohne Nutzerinteraktion ist noch zu beweisen.

Der bisher bewiesene Kern umfasst: Issue-Trigger, self-hosted Windows Runner, lokale Codex CLI, Commit/Push durch Workflow, `2W_DONE`-Kommentar und Dedupe gegen doppelte produktive Läufe.

Offen ist: Der Watcher muss im Zielbetrieb automatisch `2w:done` erkennen, Review ausführen, `2W_REVIEWED` schreiben und den nächsten Block oder Abschluss ohne Nutzerinteraktion erzeugen.

## Zielbild

Der Nutzer definiert mit ChatGPT ein Ziel und einen kleinen Arbeitsblock. ChatGPT schreibt diesen Arbeitsblock als GitHub Issue in das jeweilige Zielrepo. Das Issue wird durch `2W_READY:` im Titel oder das Label `2w:ready` startfähig. Ein lokaler self-hosted GitHub Actions Runner erkennt das startfähige Issue, ruft im Zielrepo die lokal installierte Codex CLI mit `codex exec` auf und übergibt den Issue-Inhalt als Arbeitsauftrag.

Codex bearbeitet den Arbeitsblock im Repository. Nach der Bearbeitung führt der Workflow verfügbare Checks aus. Wenn der Lauf erfolgreich ist, committet und pusht der Workflow die Änderung direkt in das Zielrepo. Danach kommentiert der Workflow das Issue mit Commit-SHA, Teststatus und geänderten Dateien.

Der lokale Watcher übernimmt danach den Callback. Er erkennt `2w:done`, lädt Commit und Diff, ruft lokal die Reviewer-Rolle auf und erstellt automatisch den nächsten `2W_READY:`-Block oder markiert das Ziel als abgeschlossen. Das ChatGPT-Browserfenster ist nicht der technische Trigger. Der verbindliche Zielmaßstab bleibt trotzdem: Der Nutzer soll nach der Zieldefinition nicht mehr als manueller Bote zwischen ChatGPT, GitHub und Codex CLI gebraucht werden.

## Ablauf

1. Nutzer und ChatGPT definieren Ziel und ersten Arbeitsblock.
2. ChatGPT erstellt ein GitHub Issue im Zielrepo.
3. Das Issue ist startfähig durch Titelpräfix `2W_READY:` oder Label `2w:ready`.
4. Ein self-hosted GitHub Actions Runner startet den Workflow.
5. Der Workflow ruft lokal `codex exec` auf.
6. Codex setzt den Arbeitsblock im Zielrepo um.
7. Der Workflow führt verfügbare Checks aus.
8. Bei erfolgreichem Lauf committet und pusht der Workflow.
9. Der Workflow kommentiert das Issue mit `2W_DONE` und Ergebnisdaten.
10. Der lokale Watcher erkennt das `2w:done`-Issue automatisch.
11. Der Watcher ruft die lokale Reviewer-Rolle auf.
12. Die Reviewer-Rolle entscheidet `COMPLETE`, `NEXT_ISSUE`, `FIX_ISSUE` oder `BLOCKED`.
13. Der Watcher kommentiert `2W_REVIEWED` und setzt `2w:reviewed`.
14. Bei `NEXT_ISSUE` oder `FIX_ISSUE` erstellt der Watcher automatisch das nächste `2W_READY:`-Issue.
15. Der Loop läuft weiter, bis `COMPLETE` oder `BLOCKED` erreicht ist.

## Aktuelle Nähe zum Zielbild

Der GitHub-seitige Ausführungskern ist vorbereitet und bewiesen: Workflow, Issue-Template, portable Templates, Smoke-CI, self-hosted Runner, lokale Codex CLI und Dedupe-Gate funktionieren.

Der Standardpfad nutzt lokale Codex CLI und verlangt keinen `OPENAI_API_KEY`.

Noch nicht bewiesen ist der vollständig autonome Watcher-Betrieb: Der Watcher muss ohne manuelles Anstoßen pro Issue den Rückfluss übernehmen und den nächsten Arbeitsblock oder Abschluss erzeugen.

## Wichtige Entscheidung

Der Standardpfad verwendet keinen `OPENAI_API_KEY` und nicht `openai/codex-action@v1`.

Stattdessen muss Codex CLI lokal auf dem self-hosted Runner und für den Watcher verfügbar und bereits mit dem gewünschten Nutzerkonto angemeldet sein. Ob das konkret über ein ChatGPT-Plus-Konto funktioniert, wird lokal validiert. Das Repository erzwingt keinen API-Key.

## Nicht-Ziele

- Kein OperatorLoop.
- Keine eigene Engine im Sinne von Datenbank, Web-App, Server oder DecisionEngine.
- Keine Stage-Logik.
- Keine PR-Orchestrierung.
- Kein Remote-Server.
- Keine Browser-Automation des ChatGPT-Fensters.
- Keine 0Admin-Produktlogik.

## Kern-Dateien

| Datei | Zweck |
|---|---|
| `.github/workflows/2w-codex.yml` | Lokaler 2W-Workflow für dieses Repo |
| `.github/workflows/ci.yml` | Smoke-CI für Repo-Struktur und Runtime-Invarianten |
| `.github/ISSUE_TEMPLATE/2w-workblock.yml` | Issue-Vorlage für 2W-Arbeitsblöcke |
| `templates/2w-local-codex.yml` | Portabler Workflow für andere Repos |
| `templates/ISSUE_TEMPLATE/2w-workblock.yml` | Portables Issue-Template für andere Repos |
| `scripts/2w-watcher.ps1` | Lokaler Callback-Loop nach `2W_DONE` |
| `docs/2W_PROCESS_CONTRACT.md` | Verbindlicher Zielprozess für Autonomie nach Zieldefinition |
| `docs/2W_WATCHER.md` | Watcher-Aufgaben, Grenzen und Betriebsmodell |
| `docs/INSTALL_IN_TARGET_REPO.md` | Installation in beliebigen Zielrepos |
| `docs/AUTH_WITH_CHATGPT_PLUS.md` | Authentifizierungsmodell ohne API-Key im Standardpfad |
| `docs/VALIDATE_CODEX_CLI_LOGIN.md` | Lokale Login-/CLI-Validierung |
| `docs/2W_PROTOCOL.md` | Minimaler Kommunikationsvertrag |
| `docs/LOCAL_SETUP.md` | Lokale Einrichtung des self-hosted Runners |
| `docs/VALIDATION.md` | Cline-Validierung nach lokalem `fetch` |
| `docs/SECURITY.md` | Sicherheitsgrenzen für 2W v0 |

## Betriebsmodell

Die GitHub-Historie ist die Wahrheitsquelle. Lange Logs, vollständige Diffs und sensible Inhalte gehören nicht in den Chat.

`scripts/2w-watcher.ps1` ist der lokale Callback-Loop nach `2W_DONE`. One-shot ist nur ein Testmodus. Für den Zielprozess muss der Watcher im Poll-Modus oder über einen lokalen Scheduler laufen.

## 2W v0 in einem Satz

ChatGPT erzeugt ein startfähiges GitHub Issue, der lokale self-hosted Runner führt `codex exec` aus, der Workflow committet und kommentiert `2W_DONE`, der lokale Watcher prüft Commit und Diff und erzeugt automatisch den nächsten Block oder Abschluss.

## Harte Grenzen

Dieses Repo kann keine lokale Codex-Anmeldung durchführen, keinen self-hosted Runner registrieren und nicht garantieren, dass ein ChatGPT-Plus-Login mit der installierten Codex CLI auf Ihrem Rechner funktioniert. Genau dafür gibt es `docs/VALIDATE_CODEX_CLI_LOGIN.md`.

Dieses Repo garantiert außerdem nicht, dass dieses konkrete ChatGPT-Browserfenster automatisch geweckt wird. Der Zielprozess wird über den lokalen Watcher geschlossen, nicht über Browser-Automation.