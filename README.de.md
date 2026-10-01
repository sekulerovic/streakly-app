# Streakly App

**Sprachen:** [English](README.md) | [Türkçe](README.tr.md) | Deutsch

[Projekt-Checkliste und Architektur (Türkisch)](TODO.md)

Streakly App ist eine Produktivitäts- und Gewohnheiten-Tracking-App. Sie soll Menschen dabei helfen, Beständigkeit aufzubauen, indem sie tägliche Aktivitäten, Serien und Fortschritte im Zeitverlauf erfasst.

## Projektziel

Die App soll Nutzerinnen und Nutzern helfen:

- Wiederkehrende Gewohnheiten anzulegen und zu verwalten
- Tägliche Check-ins und Erledigungen zu erfassen
- Serien, Fortschritte und den Verlauf einzusehen
- Dezente Motivation und Impulse zur Selbstverantwortung zu erhalten
- Sich auf nachhaltige Routinen statt auf Perfektion zu konzentrieren

## Technologie

Streakly ist eine plattformübergreifende mobile App für iOS und Android. Sie basiert auf:

- C# und .NET
- .NET MAUI für die gemeinsame mobile Benutzeroberfläche und native Plattformintegrationen
- SQLite für die lokale Datenspeicherung auf dem Gerät, sofern benötigt

Ein ASP.NET-Core-Dienst kann später ergänzt werden, falls Funktionen ein gemeinsames Backend, Benutzerkonten oder Synchronisierung erfordern. Sofern die Produktanforderungen nichts anderes vorgeben, soll die grundlegende Gewohnheitserfassung nicht von einem Backend abhängen.

## Repository-Status

Dieses Repository befindet sich derzeit im Aufbau. Als erste Schritte werden die Projektdokumentation und Richtlinien für die Zusammenarbeit erstellt, damit das Team von Anfang an einheitlich entwickeln kann.

## Geplante Struktur

- `src/` — .NET-MAUI-Anwendung und gemeinsam genutzter Code
- `tests/` — automatisierte Tests
- `docs/` — Design- und Produktdokumentation
- `.github/` — Repository-Automatisierung und Copilot-Anweisungen

## Lokale Einrichtung

Installiere das unterstützte .NET SDK, die .NET-MAUI-Workload und Git. Verwende unter Windows Visual Studio mit der .NET-MAUI-Workload oder unter macOS die .NET-Tools und Xcode.

So entwickelst und startest du die App:

1. Klone das Repository.
2. Stelle die .NET-Abhängigkeiten wieder her.
3. Starte die App auf einem Android-Emulator oder -Gerät.
4. Für das Erstellen, Ausführen oder Signieren für iOS benötigst du einen Mac mit Xcode. Visual Studio unter Windows kann für die iOS-Entwicklung mit einem gekoppelten Mac verbunden werden.

## Entwicklungsgrundsätze

- Bevorzuge einfachen, gut lesbaren Code gegenüber cleveren Abstraktionen
- Halte Funktionen klein und testbar
- Dokumentiere Annahmen und Entscheidungen im Code oder in der Dokumentation
- Überprüfe das Verhalten vor dem Zusammenführen von Änderungen
- Lege keine Geheimnisse oder umgebungsspezifischen Werte in der Versionsverwaltung ab

## Roadmap

- Produktanforderungen und Nutzerabläufe definieren
- Die .NET-MAUI-App erstellen und Android- sowie iOS-Ziele konfigurieren
- Domänenmodelle für Gewohnheiten und Check-ins definieren
- Das Anlegen und Erfassen von Gewohnheiten implementieren
- Serienberechnungen und Fortschrittsansichten ergänzen
- Daten lokal speichern und anschließend den Bedarf an Cloud-Synchronisierung bewerten

## Mitwirken

Verwende kurze, fokussierte Branch-Namen und halte Pull Requests leicht überprüfbar. Achte auf aussagekräftige Commit-Nachrichten und ergänze Tests für Verhaltensänderungen.

## Lizenz

Dieses Projekt ist derzeit nicht lizenziert. Eine endgültige Entscheidung zur Verbreitung und kommerziellen Nutzung steht noch aus.
