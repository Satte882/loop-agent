# 2W Process Contract

## Zweck

Dieses Dokument fixiert den verbindlichen Zielprozess für 2W.

Der Nutzer soll nach der Zieldefinition nicht mehr als manueller Bote zwischen ChatGPT, GitHub, Codex CLI und dem nächsten Arbeitsblock gebraucht werden.

Der bisherige manuelle Pull-Schritt `Prüfe Issue #N` ist ausdrücklich nicht der Zielprozess. Er war nur eine Zwischenstufe zum Testen.

## Fixiertes Zielbild

Der Zielprozess lautet:

1. Nutzer und ChatGPT stimmen ein Ziel ab.
2. ChatGPT erstellt das erste `2W_READY:`-Issue.
3. GitHub Actions startet den self-hosted Runner.
4. Der Runner führt lokal `codex exec` aus.
5. Codex setzt den Arbeitsblock um.
6. Der Workflow committet und pusht auf `main`.
7. Der Workflow kommentiert `2W_DONE` mit Commit-SHA, Teststatus und geänderten Dateien.
8. Der lokale Watcher erkennt `2w:done` automatisch.
9. Der Watcher erzeugt automatisch einen Prüfauftrag mit Commit-SHA, Issue-Nummer und Repository-Kontext.
10. Die Reviewer-Rolle liest Commit, Diff und Issue-Kommentar selbst über GitHub.
11. Die Reviewer-Rolle entscheidet ohne Nutzerinteraktion:
    - `COMPLETE`: Ziel ist abgeschlossen.
    - `NEXT_ISSUE`: nächster Arbeitsblock wird erzeugt.
    - `FIX_ISSUE`: gezielter Fix-Block wird erzeugt.
    - `BLOCKED`: automatische Fortsetzung wird gestoppt.
12. Der Watcher schreibt `2W_REVIEWED` ins geprüfte Issue.
13. Der Watcher setzt `2w:reviewed`.
14. Bei `NEXT_ISSUE` oder `FIX_ISSUE` erstellt der Watcher automatisch das nächste `2W_READY:`-Issue.
15. Der Kreislauf läuft weiter, bis `COMPLETE` oder `BLOCKED` erreicht ist.

## Harte Prozessregel

Nach der initialen Zieldefinition darf der Nutzer nicht benötigt werden, um:

- den Abschluss eines 2W-Laufs zu melden
- eine Commit-SHA in den Chat zu kopieren
- ChatGPT manuell mit `Prüfe Issue #N` anzustoßen
- den Watcher pro Issue manuell zu starten
- den nächsten Arbeitsblock manuell anzustoßen

Wenn einer dieser Schritte nötig ist, ist der Zielprozess nicht erfüllt.

## Rolle von ChatGPT

ChatGPT ist im Zielprozess die Planungs- und Reviewer-Rolle.

Der technische Trigger in dieses konkrete ChatGPT-Browserfenster ist nicht garantiert. Deshalb muss die Implementierung den Prüfauftrag lokal über den Watcher erzeugen und mit lokal verfügbaren Mitteln ausführen.

Für 2W v1 bedeutet das:

- Der Watcher übernimmt den automatischen Callback.
- Der Watcher ruft eine lokale Reviewer-Ausführung auf.
- Die Reviewer-Ausführung muss Commit und Diff selbst aus GitHub lesen können.
- Der nächste Issue-Block entsteht automatisch aus der Reviewer-Entscheidung.

Der Maßstab bleibt trotzdem der ursprüngliche Nutzerprozess: Der Nutzer soll nach Zieldefinition aus der Schleife herausfallen.

## Rolle des Watchers

Der Watcher ist nicht ein optionales Prüfwerkzeug.

Der Watcher ist der verbindliche Callback-Loop nach `2W_DONE`.

Er muss:

- dauerhaft oder regelmäßig laufen
- `2w:done`-Issues ohne `2w:reviewed` finden
- Commit-SHA aus `2W_DONE` extrahieren
- Commit und Diff über GitHub laden
- einen Reviewer-Auftrag erzeugen
- die Reviewer-Entscheidung auswerten
- `2w:reviewed` setzen
- `2W_REVIEWED` kommentieren
- bei Bedarf das nächste `2W_READY:`-Issue erstellen

## Was nicht als erfüllt gilt

Folgende Zustände gelten nicht als Zielerfüllung:

- Der Nutzer muss nach jedem `2W_DONE` manuell etwas in den Chat schreiben.
- Cline muss nach jedem `2W_DONE` manuell gestartet werden.
- Der Watcher läuft nur als einmaliger Test, aber nicht als fortlaufender Callback.
- Der Watcher liest zwar GitHub, schreibt aber keine Reviewer-Entscheidung zurück.
- Der Watcher setzt kein `2w:reviewed`.
- Der Watcher erzeugt kein Folge-Issue bei `NEXT_ISSUE` oder `FIX_ISSUE`.
- Das System stoppt nach jedem Commit und wartet auf menschliche Vermittlung.

## Zulässige Minimalimplementierung

Eine zulässige Minimalimplementierung darf klein bleiben.

Erlaubt ist:

- ein lokales PowerShell-Skript
- `gh` als GitHub-Zugriff
- lokale Codex CLI als Reviewer-Ausführung
- Polling statt Webhook
- `2w:reviewed` als Dedupe-Marker
- `2w:complete` als Abschlussmarker
- `2W_REVIEWED` als Audit-Kommentar

Nicht erforderlich für v1:

- Web-App
- Datenbank
- Remote-Service
- Browser-Automation
- OpenAI API-Key
- OperatorLoop
- DecisionEngine
- PR-Workflow

## Betriebsmodell

Der Watcher muss im Zielbetrieb im Poll-Modus oder über einen lokalen Scheduler laufen.

One-shot ist nur ein Testmodus.

Für den autonomen Zielprozess ist One-shot nicht ausreichend.

## Bewertungskriterien

Ein 2W-Lauf gilt erst dann als vollständiger Callback-Loop, wenn:

- das ursprüngliche Issue `2W_DONE` erhält
- der Watcher das Issue ohne Nutzerinteraktion erkennt
- der Watcher Commit und Diff lädt
- der Reviewer eine Entscheidung liefert
- der Watcher `2W_REVIEWED` kommentiert
- der Watcher `2w:reviewed` setzt
- der Watcher bei Bedarf das nächste `2W_READY:`-Issue erzeugt

Ein Test gilt nur dann als autonom, wenn der Nutzer zwischen `2W_DONE` und dem Folge-Issue keine Aktion ausführen musste.

## Aktueller offener Punkt

Der 2W-Ausführungskern ist bewiesen.

Der Dedupe-Gate-Fix ist bewiesen.

Der Watcher ist implementiert.

Noch zu beweisen ist der automatische Watcher-Betrieb ohne manuelles Anstoßen pro Issue.