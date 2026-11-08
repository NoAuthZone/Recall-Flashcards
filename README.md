# Recall-Flashcards

A complete flashcard tool in **one single HTML file** – no server, no installation, no account. Open the file and study. All data stays local in your browser (IndexedDB).

Version 4.6 · UI in German/English · light/dark theme

## Quick start

1. Open `recall-flashcards.html` in a browser (Chrome, Edge, Firefox, Safari).
2. **Overview → + Deck** creates a deck (use `::` for sub-decks, e.g. `Crypto::AES`).
3. **Manage → + New card**, or **Import / Export** (JSON from an AI, see below).
4. Start **Study** – the app schedules the reviews for you.

> **Important:** Cards live in the browser storage of *this* file/location. Opening the file from another place shows an empty app. Create a **backup** regularly (Import / Export → Backup); the app reminds you.

## Areas

| Area | Purpose |
|---|---|
| **Overview** | Deck list with New/Learning/Due, forecast, study plan, create decks, reorder by drag & drop |
| **Study** | Spaced repetition (SM-2, optional FSRS), undo, suspend, bury, mark, flag |
| **Manage** | Deck tree on the left, card list in the middle, **live preview** on the right (toggle, 👁 per row), search, bulk actions, find & replace, duplicates, print, card templates |
| **Statistics** | Study history, distribution, weak spots; export as JSON |
| **Mock exam** | Dedicated exam mode (see below) |
| **Import / Export** | JSON, ZIP, CSV, backup/restore, packages, optional folder sync |

## Card types

| Type | Description |
|---|---|
| `basic` | Front/back, optional extra and source |
| `reversed` | Two cards (front→back, back→front) |
| `cloze` | Cloze deletion `{{c1::answer::hint}}`, each number = one card |
| `typein` | Typed answer compared with a diff |
| `mc` | Multiple choice, correct options start with `*`, multiple answers allowed, order is shuffled |
| `calc` | Calculation tasks with random numbers and a computed solution |
| `occlusion` | Image occlusion: masks (fractions 0–1) over an image |
| `m:<id>` | Custom note types with their own fields and templates |

All text fields support Markdown (**bold**, lists, code) and LaTeX (`$a^2 \bmod n$`). Attachments (images, audio) are supported.

### Calculation cards (`calc`)

```
vars:    p = prime 5..23        (random prime)
         a = 2..20              (random number)
         n = p*q                (computed)
answer:  modinv(e, phi_n)
```

Operators `+ - * / % ^`, parentheses; functions `modpow, modinv, gcd, lcm, phi, mod, xor, isprime`. Use `{a}` in the question and `{=n}` in the extra field. The task must be solvable for **every** random assignment (e.g. `modinv` only for coprime numbers).

## Mock exam

A separate menu item with three steps:

1. **Setup** – deck/tag, number of questions, time limit, question types, partial credit, shuffle answers; presets *Quick test* (10 / 10 min), *Standard* (20 / 30), *Full exam* (40 / 60); history of recent exams.
2. **Exam mode** – distraction-free (navigation hidden), timer, question overview, flagging. Keys: `←` `→` navigate, `M` flag, `1`–`9` choose an answer. Submitted automatically when time runs out.
3. **Grading → result** – multiple choice, typed and calculation questions are graded automatically. **You grade free-text questions yourself after submitting** (correct / partly / wrong) against the model answer. Optional: a suggestion based on key terms, or copy a prompt for AI grading. Only then the total result is shown (pass mark 50 %), with a breakdown by topic and the option to practise wrong cards.

The exam does not change the learning state of your cards.

## Study plan

For decks with an exam date the plan estimates your readiness on exam day and how many new cards per day you need (buffer adjustable, default 7 days). One click sets the deck's daily limit. The estimate is rough and does not replace a mock exam.

## Import and export

- **JSON is the default** for import and export. Format `karteikarten`, version 2: `notes`, optional `models`, `decks`, `package`.
- **ZIP** is an option ("compact with attachments") for packages with many images/audio and for larger backups; media are stored as files inside the ZIP.
- **CSV** can be imported.
- With a stable `id` per note, re-importing **updates instead of duplicating**; conflicts are shown.
- The import dialog validates first (errors are deselected, hints shown) and shows new/updated/unchanged.
- Backups can optionally be encrypted.

Minimal example:

```json
{
  "format": "karteikarten",
  "version": 2,
  "package": { "course": "Crypto", "version": "1" },
  "notes": [
    { "id": "crypto-hash-001", "type": "mc", "deck": "Crypto::Hash",
      "fields": { "front": "Which hash function is considered **broken**?",
                  "options": "*MD5\nSHA-256\nSHA-3\nBLAKE2" },
      "tags": ["hash"] }
  ]
}
```

## Creating cards with AI

`ki-vorlagen/` contains ready-made prompts and examples (German); `ki-examples-en/` contains English examples on encryption.

1. Copy the prompt from `ki-vorlagen/KI-PROMPT.md` and fill in your topic.
2. Optionally attach an example file ("follow exactly this structure").
3. The AI returns a `.json`; import it under **Import / Export**.
4. Spot-check in **Manage**; run calculation cards once in Study.

AI cannot create images: embed attachments as Base64 in `atts[].data`, or give only the file name and select the image files **together with the JSON** when importing.

## Study keyboard shortcuts

| Key | Action |
|---|---|
| `Space` / `Enter` | Show answer |
| `1`–`4` | Rate (Again, Hard, Good, Easy) |
| `Z` | Undo |
| `S` / `B` | Suspend / Bury |
| `M` / `F` | Mark / Flag |
| `R` | Read aloud |

## More features

- Study plan, forecast, daily goal, break reminder, study-time limit
- Custom sessions (by tag, failed cards, studying ahead), cramming without affecting the schedule
- Deck options (new cards/day, learning steps, intervals, leech handling, SM-2/FSRS), exam and due dates
- Automatic safety snapshots before risky actions (e.g. deleting)
- Folder sync via a shared file (**experimental**, so far tested only with a simulated folder)
- Accessibility: keyboard operation, screen-reader announcements, checked with axe

## Files in this project

| Path | Content |
|---|---|
| `recall-flashcards.html` | the app (single file) |
| `src/` | sources (`00_head.html`, `10_core.js` … `60_i18n.js`) |
| `build.sh` | assembles `src/*` into the final HTML file |
| `ki-vorlagen/` | AI prompt and German example files |
| `ki-examples-en/` | English example cards (all types) |
| `beispiele/` | example packages, update and learning-state examples |

Build: `bash build.sh` creates `recall-flashcards.html` (the shipped app).

## Privacy

There is no server communication and no tracking; cards, learning state and settings leave the browser only when you export or use folder sync.
