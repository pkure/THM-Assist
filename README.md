# THM Assist

**Reverse-search TryHackMe task bodies by answer shape.**

When TryHackMe shows you a grayed-out hint like `______ ________`, THM Assist turns that shape into a search pattern and finds every substring in the task text that fits — so you can stop staring at the wall of text and start clicking candidates.

---

## The Problem

TryHackMe answer fields show you the *structure* of the expected answer — character counts, spaces, periods, slashes — but not the answer itself. The answer is sometimes in the task body (depending on what type of room it is...), but hunting through dense technical text for a phrase of exactly the right shape is slow and error-prone, especially when answers contain dashes, colons, or other technical punctuation that don't "look" like a word. And if you've been learning with TryHackMe for any length of time, you've almost definitely had that frustration of trying to guess the exact phrasing or 'way' they want you to answer, maybe it's slightly different than the way the thing is mentioned in the paragraph, it can feel like they want you to 'read their mind' in a way that is not a good use of your time and energy.

THM Assist solves this with a single HTML file and zero dependencies.

---

## Features

- **Shape-to-regex engine** — type the answer hint using underscores for blank characters; everything else (spaces, periods, slashes, revealed letters) is matched literally as an anchor
- **Smart character matching** — the default mode matches anything THM doesn't explicitly reveal, so dashes, colons, brackets, and other technical chars fill underscore slots correctly
- **Word boundary enforcement** — won't match a 6-char slot against the middle of a longer word
- **Ranked candidates** — results sorted by frequency in the text, with surrounding context highlighted
- **Click to copy** — one click copies the candidate straight to clipboard for pasting into THM
- **Live regex preview** — shows exactly what pattern was built so you can reason about it
- **Three match modes** — Smart (default), Strict alnum, Any char

---

## Usage

1. **Download** `thm-assist.html` and open it in any browser (double-click — no server needed)
2. On TryHackMe, scroll through the task, select all the text, and copy it
3. Paste the text into the **Task Text** box
4. In the **Answer Shape** field, type the hint exactly as THM shows it:
   - Use `_` (underscore) for each blank/unknown character
   - Type spaces, periods, slashes, and any revealed letters literally
   - Example: `______ ________` for a 6-char word, space, 8-char word
5. Hit **Search** — candidates appear ranked below with context
6. Click any result to copy it to your clipboard

### Tips

| Situation | What to do |
|---|---|
| Too many results | Enable **whole tokens only** to drop partial-word matches |
| Answer has dashes, colons, etc. | Default **smart** mode handles this automatically |
| Nothing matches at all | Switch to **any char** mode, or recount your underscores |
| Mask has a period or slash | Type it literally — it acts as an anchor in the search |

---

## How It Works

Each run of underscores is converted to a regex quantifier for the selected character class:

```
______ ________
→ [^ ./\\]{6} [^ ./\\]{8}     (smart mode, default)
→ [A-Za-z0-9]{6} [A-Za-z0-9]{8}  (strict alnum)
```

The **smart** default class is `[^ ./\\]` — anything that isn't a space, period, or slash — because those are the only characters TryHackMe explicitly reveals in its answer hints. This means dashes, colons, underscores, and other technical punctuation are all valid matches without any extra configuration.

With **whole tokens only** enabled, lookahead/lookbehind assertions are added around the pattern so partial-word matches are excluded.

---

## Limitations

- You paste the task text manually (copy-paste from THM)
- Short masks with no anchors (e.g. `____`) will match many words — longer masks with spaces or punctuation work best
- Not every answer is in the task body — VM/attack-box answers and external research questions won't be found here

A browser extension that reads the THM tab directly and highlights answers in-page is the natural next step for this project.

---

## Roadmap

- [ ] Browser extension — auto-read task text and answer fields, highlight candidates in-page
- [ ] Auto-detect answer hint fields from the DOM (no manual mask entry)
- [ ] Screenshot/OCR input for hint fields that aren't selectable text

---

## Credits

**Concept & product direction** — Peter Kure
**Engineering** — [Claude Sonnet](https://www.anthropic.com/claude) by Anthropic  

This project was AI-engineered: the interface, matching logic, regex engine, and iterative bug fixes were all built through a conversation with Claude. Peter described the problem, drove the design decisions, and tested against real TryHackMe rooms to catch edge cases (like `embedding-level poisoning` revealing the need for smart character matching).

---

## License

MIT — do whatever you want with it.
