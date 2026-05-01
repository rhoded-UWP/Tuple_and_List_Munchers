# Tuple_and_List_Munchers
2nd try at Tuple and List Munchers



# Python Munchers: Lists vs Tuples

A short, retro-styled exit-ticket game inspired by the classic *Number Munchers* (MECC, 1986). Move the green muncher around a 5×4 grid and chomp only the cells that match the round's prompt — **List** or **Tuple** — while dodging the ladybug Syntax Bugs.

This README is written for students in an Intro to Python course who are also learning to use AI tools like Claude as part of their coding workflow. The game itself is the Python content; this document is about **how the game was built**.

---

## How to play

1. Open `python-munchers.html` in any modern browser. No install, no server, no dependencies.
2. Press **Enter** or **Space** to start.
3. **Arrow keys** to move, **Space** or **Enter** to munch the current cell, **R** to restart.
4. Two rounds: munch the Lists, then munch the Tuples.
5. Finish both rounds to get the Canvas Quiz passcode.

Scoring: +100 for a correct munch, −50 for a wrong one (and a wrong munch spawns an extra bug, capped at 3). You start with 3 lives. Lives are only lost on bug contact, never on a wrong answer.

---

## How this game was built with AI

The entire game — HTML, CSS, and JavaScript in a single file — was built in a back-and-forth conversation with Claude. The interesting part for you, as a student learning both Python and AI, isn't the final code. It's the **process**.

### The original prompt

The very first message to the AI was a long, detailed specification. Read it carefully — it's a model of how to ask an AI to build something non-trivial. Notice that it specifies *what* the game should do, *how it should look*, *what the rules are*, and even *what the data should look like*, but it leaves the actual implementation to the AI.

> **Project Description: Python Munchers, a Number Munchers-Inspired Exit Ticket Game**
>
> Create a single-file HTML, CSS, and JavaScript browser game inspired by the classic Number Munchers by MECC. The game should use the same general logic: a player moves around a grid, "munching" correct answers while avoiding roaming enemies. However, instead of math prompts like "Multiples of 15," this game is for an Intro to Programming course in Python and focuses only on Lists and Tuples.
>
> The game should feel retro and arcade-like, with a black background, bright neon grid, chunky pixel-inspired visuals, and simple keyboard controls. The fonts for the actual Python content inside grid cells must be modern, high contrast, and ADA-accessible. Prioritize readability over pure retro accuracy.
>
> *(...followed by detailed sections on game theme, core game logic, grid layout, player controls, munching rules, scoring, lives, enemies, round flow, sample items, visual design, accessibility, and a suggested data structure.)*

The full prompt covered roughly:

- Theme and visual design (colors, fonts, retro feel)
- Exact game rules (4 rounds, scoring, lives, enemy behavior)
- Grid dimensions and cell content guidelines
- Sample List items, Tuple items, and shared items
- A suggested JavaScript data structure for the answer bank

**Lesson 1:** A vague prompt gets vague output. The more specific your prompt — without prescribing the *implementation* — the closer the first draft will be to what you actually want.

### The iterative refinement

The first version worked but wasn't right. Real software is never one-shot, and AI-built software is no exception. Here's the actual sequence of follow-up requests, with what each one taught:

| Round | What was asked | What it taught |
|---|---|---|
| 1 | "The Muncher is not visible. Add more items. Generate items in patterns like `a = (x, y, z)`." | Always **test what the AI gives you**. The first draft had a CSS bug (`width: 70%` on the SVG sprite) that made the muncher render at 70% of the *whole grid* instead of 70% of one cell. The AI didn't catch this — the human player did, by trying to play. |
| 2 | "Make the muncher partially see-through so I can read the cell. Make enemies look like ladybugs. Slow them down. Move start position." | Visual and gameplay polish that's hard to anticipate from a spec. **You won't know what feels wrong until you actually use it.** |
| 3 | "Make the mouth opening clear, not green. Cap enemies at 3. Cut from 4 rounds to 2." | Tuning. The original spec said 4 rounds, but in practice 2 was enough for a 5–10 minute exit ticket. **Specifications are guesses; testing is truth.** |
| 4 | (Image attached) "The mouth still has a line through it." | The first attempt drew the mouth as a closed polygon, so its back edge was visible. The fix required redrawing the muncher as a single Pac-Man-style path with the wedge cut out. **A picture in the conversation was clearer than 100 words of description.** |
| 5 | "Remove the for-loop generator. 'Faster to create' should be 'executes fast' and 'uses less memory'." | Editorial refinements to the question bank. |
| 6 | "Wait — `executes fast` should apply to **Tuples**, not Lists. And double-check that `x[3] = 99` is List-only because tuples can't be edited." | **The most important lesson.** The teacher caught a Python correctness error that the AI made and the AI did not flag. Tuples are in fact faster to create and use less memory than lists. The AI followed the original instruction without questioning it. **You are the domain expert. The AI is not.** |

### Lessons for working with AI as a Python student

1. **Be specific.** "Make a game" gives you junk. The original prompt above is closer to a design document than a prompt — that's why it worked.
2. **Run the code.** AI confidence ≠ AI correctness. The CSS bug, the mouth line, and the pedagogical errors were all things the AI was sure about and got wrong.
3. **Trust your domain knowledge.** When the AI told you tuples were faster but you said "wait, isn't that a list thing?" — your hesitation was the signal. If something feels off, it probably is. Verify with the docs, an interpreter, or your instructor.
4. **Iterate in small steps.** Each round above changed only a few things. Big rewrites are hard to verify; small changes are easy to spot-check.
5. **Show, don't just tell.** The screenshot of the muncher's mouth was worth far more than another paragraph of text.
6. **Read the diff.** Before accepting AI changes, look at what actually changed. The AI is fast, which means it can introduce mistakes fast too.

---

## What you can learn from the code itself

Even though this game is written in JavaScript, the data design echoes patterns you'll see in Python:

```js
const ITEM_BANK = [
  { text: "x = [1, 2, 3]", code: true,  categories: ["List"] },
  { text: "y = (1, 2, 3)", code: true,  categories: ["Tuple"] },
  { text: "ordered",       code: false, categories: ["List", "Tuple"] },
  // ...
];
```

That's an array of objects — almost identical to a Python `list` of `dict`s. The `categories` field is itself a list, and the rule for whether an item is "correct" in a given round is just:

```js
const isCorrect = item.categories.includes(currentCategory);
```

In Python, that same idea would be:

```python
is_correct = current_category in item["categories"]
```

The point: the **data shape** is the same idea across languages. Once you understand lists and dicts in Python, you understand arrays and objects in JavaScript.

---

## Files

- `python-munchers.html` — The game. Single file, no dependencies.
- `index.html` — (Existing file in the directory.)
- `README.md` — This file.

---

*Built collaboratively by a human teacher and Claude, an AI assistant from Anthropic. The interesting parts of the build process — the bugs, the back-and-forth, the moment the human caught the AI being wrong — are documented above so future students can see what real AI-assisted programming actually looks like.*

