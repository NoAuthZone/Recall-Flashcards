# Recall-Flashcards

Ein vollständiges Lernkarten-Tool in **einer einzigen HTML-Datei** – ohne Server, ohne Installation, ohne Konto. Datei öffnen, lernen. Alle Daten bleiben lokal im Browser (IndexedDB).

Version 4.6 · Oberfläche Deutsch/Englisch · Hell/Dunkel

## Schnellstart

1. `recall-flashcards.html` im Browser öffnen (Chrome, Edge, Firefox, Safari).
2. **Übersicht → + Stapel** legt einen Stapel an (mit `::` entstehen Unterstapel, z. B. `Krypto::AES`).
3. **Verwalten → + Neue Karte** oder **Import / Export** (JSON von einer KI, siehe unten).
4. **Lernen** starten – die App plant die Wiederholungen selbst.

> **Wichtig:** Die Karten liegen im Browser-Speicher *dieser* Datei/dieses Ortes. Wer die Datei von einem anderen Ort öffnet, sieht zunächst nichts. Deshalb regelmäßig ein **Backup** erstellen (Import / Export → Backup). Die App erinnert daran.

## Bereiche

| Bereich | Zweck |
|---|---|
| **Übersicht** | Stapelliste mit Neu/Lernt/Fällig, Vorhersage, Lernplan, Stapel anlegen, per Drag & Drop umsortieren |
| **Lernen** | Wiederholen mit Spaced Repetition (SM-2, optional FSRS), Rückgängig, Aussetzen, Verbergen, Markieren, Flaggen |
| **Verwalten** | Stapelbaum links, Kartenliste in der Mitte, **Live-Vorschau** rechts (ein-/ausschaltbar, 👁 pro Zeile), Suche, Sammelaktionen, Suchen & Ersetzen, Dubletten, Drucken, Kartenvorlagen |
| **Statistik** | Lernverlauf, Verteilung, Schwächen; Export als JSON |
| **Probeklausur** | Eigener Prüfungsmodus (siehe unten) |
| **Import / Export** | JSON, ZIP, CSV, Backup/Wiederherstellen, Pakete, optional Ordner-Sync |

## Kartentypen

| Typ | Beschreibung |
|---|---|
| `basic` | Einfach: Vorderseite/Rückseite, optional Zusatz und Quelle |
| `reversed` | Beidseitig: erzeugt zwei Karten (vor→zurück, zurück→vor) |
| `cloze` | Lückentext `{{c1::Antwort::Hinweis}}`, jede Nummer = eine Karte |
| `typein` | Antwort wird eingetippt und mit Diff verglichen |
| `mc` | Multiple Choice, richtige Antworten mit `*`, Mehrfachauswahl möglich, Reihenfolge wird gemischt |
| `calc` | Rechenaufgaben mit Zufallszahlen und berechneter Lösung |
| `occlusion` | Bild-Verdeckung: Masken (Anteile 0–1) über einem Bild |
| `m:<id>` | Eigene Notiztypen mit eigenen Feldern und Vorlagen |

Alle Textfelder unterstützen Markdown (**fett**, Listen, Code) und LaTeX (`$a^2 \bmod n$`). Anhänge (Bilder, Audio) sind möglich.

### Rechenaufgaben (`calc`)

```
vars:    p = prime 5..23        (Zufallsprimzahl)
         a = 2..20              (Zufallszahl)
         n = p*q                (berechnet)
answer:  modinv(e, phi_n)
```

Operatoren `+ - * / % ^`, Klammern; Funktionen `modpow, modinv, gcd, lcm, phi, mod, xor, isprime`. In der Frage `{a}`, im Zusatz `{=n}`. Die Aufgabe muss für **jede** Zufallsbelegung lösbar sein (z. B. `modinv` nur bei teilerfremden Zahlen).

## Probeklausur

Eigener Menüpunkt mit drei Schritten:

1. **Einrichtung** – Stapel/Tag, Anzahl Fragen, Zeitlimit, Fragearten, Teilpunkte, Antworten mischen; Voreinstellungen *Schnelltest* (10/10 Min), *Standard* (20/30), *Vollklausur* (40/60); Verlauf der letzten Klausuren.
2. **Prüfungsmodus** – ohne Ablenkung (Leiste ausgeblendet), Timer, Fragenübersicht, Markieren. Tasten: `←` `→` blättern, `M` markieren, `1`–`9` Antwort wählen. Bei Zeitablauf wird automatisch abgegeben.
3. **Bewertung → Ergebnis** – MC, Eintippen und Rechnen werden automatisch bewertet. **Freitext-Fragen bewertest du nach der Abgabe selbst** (richtig / teilweise / falsch) anhand der Musterlösung. Optional: Vorschlag über Schlüsselbegriffe oder Prompt für eine KI-Bewertung kopieren. Erst danach erscheint das Gesamtergebnis (Bestehensgrenze 50 %), mit Auswertung nach Thema und der Möglichkeit, falsche Karten zu üben.

Die Klausur verändert den Lernstatus der Karten nicht.

## Lernplan

Für Stapel mit Prüfungstermin schätzt der Lernplan die Bereitschaft am Prüfungstag und berechnet, wie viele neue Karten pro Tag nötig sind (Puffer einstellbar, Standard 7 Tage). Ein Klick setzt das Tageslimit des Stapels. Die Schätzung ist grob und ersetzt keine Probeklausur.

## Import und Export

- **JSON ist der Standard** für Import und Export. Format `karteikarten`, Version 2: `notes`, optional `models`, `decks`, `package`.
- **ZIP** ist eine Option („kompakt mit Anhängen“) für Pakete mit vielen Bildern/Audio und größere Backups; die Medien liegen dann als Dateien im ZIP.
- **CSV** kann importiert werden.
- Mit einer stabilen `id` pro Notiz wird beim erneuten Import **aktualisiert statt dupliziert**; Konflikte werden angezeigt.
- Der Importdialog prüft vorab (Fehler werden abgewählt, Hinweise angezeigt) und zeigt neu/aktualisiert/gleich.
- Backups sind optional verschlüsselt.

Minimalbeispiel:

```json
{
  "format": "karteikarten",
  "version": 2,
  "package": { "course": "Krypto", "version": "1" },
  "notes": [
    { "id": "krypto-hash-001", "type": "mc", "deck": "Krypto::Hash",
      "fields": { "front": "Welche Hashfunktion gilt als **nicht mehr sicher**?",
                  "options": "*MD5\nSHA-256\nSHA-3\nBLAKE2" },
      "tags": ["hash"] }
  ]
}
```

## Karten mit KI erstellen

Im Ordner `ki-vorlagen/` liegen fertige Prompts und Beispiele (deutsch), im Ordner `ki-examples-en/` englische Beispiele zum Thema Verschlüsselung.

1. Prompt aus `ki-vorlagen/KI-PROMPT.md` kopieren, Thema eintragen.
2. Optional `alle-kartentypen.json` als Beispiel anhängen („halte dich exakt an diese Struktur“).
3. Die KI liefert eine `.json`; sie unter **Import / Export** importieren.
4. In **Verwalten** stichprobenartig prüfen, Rechenaufgaben einmal im Lernen kontrollieren.

Bilder kann die KI nicht erzeugen: Anhänge entweder als Base64 in `atts[].data` einbetten oder nur den Dateinamen angeben und die Bilddateien beim Import **zusammen mit der JSON** auswählen.

## Tastaturkürzel beim Lernen

| Taste | Aktion |
|---|---|
| `Leertaste` / `Enter` | Antwort aufdecken |
| `1`–`4` | Bewerten (Nochmal, Schwer, Gut, Leicht) |
| `Z` | Rückgängig |
| `S` / `B` | Aussetzen / Verbergen |
| `M` / `F` | Markieren / Flagge |
| `R` | Vorlesen |

## Weitere Funktionen

- Lernplan, Vorhersage, Tagesziel, Pause-Hinweis, Lernzeit-Limit
- Eigene Sitzungen (nach Tag, vergessene Karten, Vorlernen), Pauken ohne Einfluss auf den Lernplan
- Stapeloptionen (neue Karten/Tag, Lernschritte, Intervalle, Leech-Behandlung, SM-2/FSRS), Prüfungs- und Fristtermine
- Automatische Schutz-Snapshots vor riskanten Aktionen (Löschen, Import)
- Ordner-Sync über eine gemeinsame Datei (**experimentell**, bisher nur mit simuliertem Ordner getestet)
- Barrierefreiheit: Tastaturbedienung, Screenreader-Ansagen, axe-geprüft

## Dateien in diesem Projekt

| Pfad | Inhalt |
|---|---|
| `recall-flashcards.html` | die App (eine Datei) |
| `src/` | Quellen (`00_head.html`, `10_core.js` … `60_i18n.js`) |
| `build.sh` | setzt `src/*` zur fertigen HTML-Datei zusammen |
| `ki-vorlagen/` | KI-Prompt und deutsche Beispieldateien |
| `ki-examples-en/` | englische Beispielkarten (alle Typen) |
| `beispiele/` | Beispielpakete, Update- und Lernstatus-Beispiele |

Bauen: `bash build.sh` erzeugt `recall-flashcards.html` (die ausgelieferte App).

## Datenschutz

Es gibt keine Server-Kommunikation und kein Tracking; Karten, Lernstatus und Einstellungen verlassen den Browser nur, wenn du exportierst oder den Ordner-Sync nutzt.
