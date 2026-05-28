# Guide: So schreibst du deinen Input für den Prompt Optimizer

## Das Wichtigste zuerst

Du brauchst **kein festes Format**. Der Optimizer ist darauf ausgelegt, mit Rohtext zu arbeiten. Aber: Je mehr Kontext du lieferst, desto präziser das Ergebnis.

---

## Der `prompt:`-Trigger (empfohlen)

Wenn du **garantiert** eine Optimierung bekommen willst — auch dann, wenn dein Input wie eine Frage aussieht oder auf angehängte Dokumente verweist — beginne deine Nachricht mit dem Präfix `prompt:`.

**Beispiel:**

```
prompt: Erklär mir anhand des angehängten PDFs, welche DSGVO-Risiken
bei unserem CRM bestehen und wie wir sie mindern.
```

Ohne Präfix könnte der Optimizer in Einzelfällen versuchen, die Frage inhaltlich zu beantworten oder das PDF direkt zu analysieren. Mit dem Präfix bekommst du **immer** einen optimierten Prompt zurück — inklusive Platzhalter für das Dokument.

Das Präfix ist case-insensitive: `prompt:`, `Prompt:`, `PROMPT:` funktionieren alle.

**Faustregel:**
- Inhalt ist eine klare Optimierungs-Aufgabe? → Präfix optional.
- Inhalt klingt wie eine Frage, enthält Dokumentverweise oder direkte Anweisungen? → **Präfix verwenden.**

---

## Minimal-Version (funktioniert immer)

Schreib einfach, was du willst:

```
Schreib mir ein Python-Skript, das CSV-Dateien zusammenführt
```

Der Optimizer erkennt automatisch Intent, Domäne und Komplexität und ergänzt das Fehlende.

---

## Bessere Version: Die Vier-Sätze-Methode

Wenn du bessere Ergebnisse willst, beantworte diese vier Fragen in jeweils ein bis zwei Sätzen:

**1. WAS soll Claude tun?** — die konkrete Aufgabe
**2. FÜR WEN / WARUM?** — Zielgruppe und Zweck
**3. WIE soll das Ergebnis aussehen?** — Format, Länge, Stil, Sprache
**4. WAS SOLL VERMIEDEN WERDEN?** (optional) — explizite No-Gos

**Beispiel:**

```
Schreib einen Blogartikel zu KI im Recruiting.
Zielgruppe: HR-Leitung im Mittelstand.
Rund 1.500 Wörter, praxisnah mit konkreten Tool-Empfehlungen.
Kein Marketing-Sprech, keine überzogenen Versprechen.
```

---

## Pro-Version: Kontext-Block

Für komplexe Aufgaben kannst du zusätzlichen Kontext strukturiert geben. Du musst **keine** XML-Tags schreiben — die ergänzt der Optimizer automatisch. Formuliere natürlich:

```
Aufgabe: Entwickle eine Migrationsstrategie von unserem On-Premise-Exchange auf Microsoft 365.

Kontext: Mittelständisches Unternehmen mit 200 Mitarbeitenden,
3 Standorte in Deutschland, aktuell Exchange 2016.
Budget rund 50.000 Euro. IT-Team: 4 Personen.

Ergebnis: Strukturiertes Dokument mit Phasenplan,
Risikobewertung, Kostenaufstellung und Checkliste.

Wichtig: DSGVO muss berücksichtigt werden.
Betriebsrat existiert — Change Management ist relevant.
```

---

## Spezialfälle

### Coding-Prompts
Sprache, Einsatzzweck und Qualitätsmerkmale nennen:

```
Python-Funktion, die mehrere PDFs zusammenführt.
Soll große Dateien handhaben (500+ Seiten), mit Fehlerbehandlung,
als CLI nutzbar.
```

### Kreativ-Prompts
Ton, Stimmung, Zielgruppe und Länge angeben:

```
Kurzgeschichte im Sci-Fi-Noir-Stil.
Protagonist: KI-Forensikerin in Neo-Berlin 2087.
Rund 3.000 Wörter, düsterer Ton, Plot-Twist am Ende.
```

### Analyse-Prompts
Datenquelle, Frage und gewünschtes Ergebnis benennen:

```
Analysiere die angehängten Quartalszahlen und finde
die drei größten Kostentreiber. Ergebnis als Executive Summary
für die Geschäftsleitung, maximal eine Seite.
```

### System-Prompt-Optimierung
Wenn du einen eigenen System-Prompt optimieren willst, sag das explizit:

```
Optimiere folgenden System-Prompt für mein Support-Projekt:
[dein System-Prompt hier]
Das System soll Tickets klassifizieren und Antwortvorschläge liefern.
```

---

## Was du NICHT tun musst

- Du musst keine XML-Tags schreiben — der Optimizer setzt sie.
- Du musst keine Rolle definieren — der Optimizer wählt die passende.
- Du musst keine Prompt-Engineering-Techniken kennen — genau dafür ist dieses Tool da.
- Du musst keinen polierten Text schreiben — Stichpunkte und Rohnotizen genügen.

---

## Was passiert nach der Optimierung?

1. Du bekommst den optimierten Prompt in einem Codeblock — **ein Klick zum Kopieren**.
2. Füge ihn in eine neue Claude-Konversation, ein Projekt oder deinen API-Call ein.
3. Wenn etwas nicht passt: Sag es dem Optimizer. Er überarbeitet gezielt.

**Tipps für iteratives Feedback:**
- "Mach den Prompt kürzer."
- "Füge Few-Shot-Beispiele hinzu."
- "Ändere die Rolle auf [XYZ]."
- "Kalibriere die Ausführlichkeit: Das soll eine tiefe Analyse werden."
- "Empfiehl mir einen passenden Effort-Level für die API-Nutzung."

**Gut zu wissen für API-Nutzer:**
Opus 4.8 hat auf der API standardmäßig kein Thinking aktiviert. Wenn der optimierte Prompt mehrstufiges Reasoning verlangt, aktiviere in deinem API-Request `thinking: {"type": "adaptive"}`. Der Effort-Default ist `high` und gilt auf allen Oberflächen (Claude API und Claude Code) — für schwierige oder lang laufende, asynchrone Aufgaben auf `xhigh` hochstufen (in claude.ai heißt diese Stufe „extra"), für einfache oder eng umrissene Aufgaben auf `low`/`medium` heruntergehen. Für sehr lange Agentic-Loops ist das Beta-Feature `task_budget` einen Blick wert; in langen, mehrstufigen Konversationen lassen sich Instruktionen zudem per Mid-Conversation-System-Message nachschieben, ohne den Prompt-Cache zu brechen.