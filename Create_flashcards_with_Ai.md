# Create flashcards with AI (English)

How to use: copy the **prompt** below into ChatGPT, Claude or a local model, optionally attach `all-card-types-en.json` as a few-shot example, and add your topic. The AI returns one `.json` file. Import it in Recall-Flashcards under **Import / Export → select files**. In the import dialog you can choose the **target deck** (as in the file, an existing deck, or a new one).

## Files in this folder

| File | Content |
|---|---|
| `all-card-types-en.json` | all card types in one file (24 notes / 36 cards) – best few-shot example |
| `json/01-basic.json` … `json/09-basic-with-image-attachment.json` | one file per card type |
| `aes-round-en.png` | image used by the occlusion/attachment examples |

---

## Prompt to copy

```
You create flashcards for my flashcard app. Output exactly ONE valid JSON file
(in a single code block, no explanatory text before or after).

TOPIC / SOURCE MATERIAL:
<paste topic, lecture notes or bullet points here>

GOAL: <e.g. 30 cards, exam preparation, bachelor level>
LANGUAGE: English
DECK ("deck"): "<Course>::<Topic>"   (use :: to create sub-decks)

FORMAT:
{
  "format": "karteikarten",
  "version": 2,
  "package": { "course": "<course name>", "version": "1" },
  "notes": [ { "id": "...", "type": "...", "deck": "...", "fields": { ... }, "tags": ["..."] } ]
}

RULES FOR EVERY NOTE:
- "id": unique and stable, e.g. "course-topic-001". The same id in a later update updates the card instead of duplicating it.
- "type": use exactly one of: basic, reversed, cloze, typein, mc, calc (or "m:<id>" for a custom template I supply).
- "deck": deck name, "::" for hierarchy.
- "tags": short lowercase keywords without spaces.
- All text fields may contain Markdown (**bold**, lists, ```code blocks```) and LaTeX ($a^2 \bmod n$).
  In JSON, write line breaks as \n and backslashes as \\.
- Optional fields: "extra" (shown after revealing, e.g. a mnemonic) and "source" (reference).
- One idea per card, precise wording, no yes/no questions without a reason.
- Do not invent sources or numbers. If you are unsure, leave the card out.

CARD TYPES (use a sensible mix):

1) "basic"    – fields: front, back
2) "reversed" – fields: front, back (term <-> definition; creates two cards)
3) "cloze"    – fields: text
   Blanks as {{c1::answer}} or {{c1::answer::hint}}. Same number = same blank,
   different numbers = different cards.
4) "typein"   – fields: front, back (back = short answer, one word/abbreviation that the learner types)
5) "mc"       – fields: front, options
   options = answer choices, one per line (\n), correct ones prefixed with *.
   Several correct answers are allowed. Use 4 choices with plausible distractors; order does not matter (it is shuffled).
6) "calc"     – fields: front, vars, answer (optional extra)
   vars: one line per variable: "a = 2..20" (random integer), "p = prime 5..23" (random prime),
   "n = p*q" (computed). front uses {a}, {p} ...; extra may contain {=n} (computed expression).
   answer: integer expression with + - * / % ^ and parentheses; functions modpow(a,e,n), modinv(a,n), gcd, lcm, phi, mod, xor, isprime.
   IMPORTANT: the task must be solvable for EVERY possible random assignment
   (e.g. modinv only if the numbers are coprime – example: fixed e = 17 with primes up to 23).
7) Custom template "m:<id>" – only if I provide a template.
   Add at the top level of the JSON: "models": [ { "id": "model-cipher", "name": "Cipher profile",
   "fields": ["Cipher","Type","Key size","Weakness"],
   "tpls": [ { "front": "{{Cipher}}: type and key size?", "back": "{{Type}}\n\n{{Key size}}" },
             { "front": "Known weakness of {{Cipher}}", "back": "{{Weakness}}" } ] } ]
   The note then has "type": "m:model-cipher" and its fields are named as in "fields".

DO NOT generate: image occlusion (needs an image – I add that myself) and attachments.

EXAMPLE (excerpt):
{
  "id": "crypto-hash-001", "type": "mc", "deck": "Crypto::Hash",
  "fields": { "front": "Which hash function is considered **broken** for collision resistance?",
              "options": "*MD5\nSHA-256\nSHA-3\nBLAKE2",
              "extra": "Practical MD5 collisions have existed since 2004." },
  "tags": ["hash"]
}

Before answering, check: valid JSON (no comments, no trailing commas), all ids unique,
every mc card has at least one correct option, every cloze card has at least one blank.
```

### Tip: few-shot

Attach `all-card-types-en.json` and add: "Follow the structure of this file exactly." Smaller local models then make fewer format mistakes.

### Attachments in JSON

Images/audio go into `atts`: `{"name":"image.png","type":"image/png","side":"back","data":"<Base64>"}`. AI cannot create images – add attachments in the editor, or leave out `data`, give only `"name":"image.png"` and select the image files **together with the JSON** when importing; they are matched by file name.

### Card type names

The importer is tolerant: names such as "Multiple Choice", "Cloze", "Type in", "Basic" are recognised, and a missing or unknown type is inferred from the fields (with a warning in the import preview). The exact names above are still the safest.

### Check after importing

1. The import preview shows new / updated / unchanged – for new files everything should be "new". Check the **Type** column.
2. Open 5 random cards in **Manage** (blanks, MC ticks, a calculation).
3. For calculation cards, start **Study** once and compare the solution with your own calculation.
4. Fix faulty cards in the editor; if the file is imported again later, the app detects conflicts.
