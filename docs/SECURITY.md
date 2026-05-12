# Security

## Grundsatz

2W v0 ist ein minimaler Relay. Die Sicherheitsgrenze liegt in kleinen Arbeitsblöcken, klaren erlaubten Pfaden, GitHub-Historie und sichtbaren Workflow-Logs.

## Nicht erlaubt

- Keine Secrets in Issues, Kommentaren oder Logs.
- Keine Tokens in Repository-Dateien.
- Keine Branches und keine Pull Requests durch 2W v0.
- Keine Änderung außerhalb des Repository-Checkouts.
- Kein Zugriff auf andere lokale Projekte.
- Kein Remote-Server-Setup.
- Keine dauerhafte lokale Eigen-Engine.

## Erlaubt

- Kleine Änderungen im Repository.
- Workflow-Ausführung auf self-hosted Runner.
- Commit und Push durch GitHub Actions nach erfolgreichen Checks.
- Issue-Kommentar mit kompaktem Ergebnis.

## Kritische Voraussetzung

Der self-hosted Runner arbeitet mit Rechten des lokalen Nutzers. Deshalb dürfen 2W-Issues nur kleine, überprüfbare Arbeitsblöcke enthalten.

## Empfohlene Prüfpunkte

- Ist das Issue klein genug?
- Sind erlaubte und verbotene Pfade eindeutig?
- Ist ein Test oder Check angegeben?
- Wird kein Secret benötigt?
- Ist der erwartete Diff prüfbar?

## Rückkanal zu ChatGPT

Der optionale Chat-Ping darf nur Metadaten enthalten:

- Repository
- Issue-Nummer
- Commit-SHA
- Teststatus
- Bitte um Prüfung

Keine vollständigen Logs und keine sensiblen Inhalte.
