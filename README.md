# Conyso Recall

Instant lookup over a question-and-answer bank you supply yourself.

**[Open it →](https://krishnachagti-sudo.github.io/conyso-recall/)**

Paste your bank once, then type a few words of any question and its answer is on
screen. One self-contained HTML page: no account, no server, no build step, and
nothing stored or transmitted anywhere.

Recall is a reference and authoring tool **for the person who owns the answer
key** — auditing a bank for defects, and reading from a bank while running a quiz
for other people. It does not capture the screen, run OCR, read another
application's window, or deliver answers to a second device. Questions reach it
by being typed.

## The bank

Paste it in whichever shape your source already has; the parser works out which
convention dominates:

| Format | Example |
|---|---|
| `Q:` / `A:` blocks | `Q: What is kaizen?` then `A: Continuous improvement` |
| Pipe | `What is kaizen? \| Continuous improvement` |
| Tab | `What is kaizen?<TAB>Continuous improvement` |
| Alternating lines | question on one line, answer on the next |

Multiple-choice questions keep their options, so you can find a question by
typing **any of its choices**, distractors included:

```
Q: Which of the following is a lean waste?
A) Motion
B) Kaizen
C) Gemba
Answer: A
```

Options can be `A)`, `(a)`, `A.` or `1)`, one per line or inline, and the answer
can be the letter or the text. In a spreadsheet: `Question | A | B | C | D | Answer`.
An answer that is not one of its own options is flagged. Because MCQ banks reuse
boilerplate stems, the same stem with different options counts as a different
question, not a contradiction.

Import is two-phase. **Preview writes nothing**: it shows what was parsed, lists
unreadable lines by line number, and marks each record *new*, *duplicate*, or
*contradiction* — the same question carrying a different answer, which in an
answer key is a defect rather than a repeat. Contradictions are left unticked, so
adding one is deliberate. Each import can be rolled back as a unit.

The bank is **session-scoped**: it lives in the tab, survives a reload, and is
gone when you close it. *Download bank* saves it as a `.txt` you can import next
time, options and subjects included. Nothing leaves the page.

You can also import a `.txt`, `.csv` or `.tsv` file, or drop one on the paste
box. A line `Subject: ops` tags every question after it. Under the Bank tab,
*Everything in the bank* lists every question; edit or delete any one of them.

## Lookup

Matching is tiered per word — exact, then stem (`wastes` finds `waste`), then
prefix, then typo (`kiazen` and `kaizne` both find *kaizen*). A word you typed
correctly is never reinterpreted as a typo of something else, so typing well
keeps the precision of exact matching.

- Function words are optional, so `what is takt time` finds *"Define takt time"*.
- Matched words are weighted by rarity, so a distinctive term beats the
  boilerplate every stem shares.
- Options and answers are searchable too, at lower weight than the question.
- Typos are forgiven, including in short words (`tkat` → *takt*), as are words
  run together (`microsoftfoundations`) and initials (`ffm` → *Five Factor
  Model*). A word typed exactly right is always matched exactly first; looser
  readings are listed below it, never mixed in.
- A word at the start or end of a stem outscores the same word mid-sentence, and
  typing the **first word and the last word** is treated as near-certain
  identification.

One word usually cannot identify a question by itself. Measured on a real
447-card deck, the first word ranks the right answer first 47% of the time and
first+last 83%. So when results share opening words, the shared run is printed
once and each result shows only the part that differs, numbered, with every
answer already visible.

| Key | Does |
|---|---|
| Esc | Clears the box for the next question, wherever focus is |
| Enter | Keeps the answer on screen; your next keystroke starts a fresh question |
| typing anywhere | Starts a new question even if the cursor is not in the box |
| ↑ ↓ | Move through results |
| Alt+1–9 | Jump to a numbered result (Option on a Mac) |

On a phone, the × in the search box clears it.

## Building

`index.html` is generated, not hand-written. The source lives with the project's
logic modules, which carry the test suite, and a build step inlines them so the
page cannot drift into a second, untested copy of the parser and the ranker.

## Licence

AGPL-3.0-or-later.
