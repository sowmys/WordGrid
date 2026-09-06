# Grid Word

A single-file browser word game. Fill a 5×5 grid so that every row is a valid word; because the grid is symmetric, the columns read the same words.

```
C A D E T
A . . . .
D . . . .
E . . . .
T . . . .
```

Open `index.html` in any browser. No build step, no dependencies, works offline.

## How to play

- Each puzzle starts with a seed word across row 1 and down column 1 (gold tiles, locked).
- Type a letter and it fills the mirrored cell too. Arrow keys move, Backspace erases, Ctrl/⌘ Z undoes.
- The **possible squares** counter shows how many valid completions remain from the current position. If it reaches zero the board turns red; undo or change a letter.
- **Hint** places a letter consistent with at least one valid completion. Hints are counted.
- Each row shows a clue: the definition of the word from one valid solution. Once the row is a complete valid word, the clue becomes that word's own definition.
- Any valid word square is accepted. The game recognises a completed square automatically.

## Word list

The dictionary is the 2,309-word Wordle answer list (the curated set of words that can appear as a daily solution). Definitions are embedded in the file, drawn from the Wordset dictionary with a small number of hand-written entries for irregular forms the dictionary lacked.

## Implementation notes

Everything lives in `index.html`: markup, styles, the embedded dictionary (`DEFS`), and the game logic.

- `enumerate()` is a backtracking solver over the symmetric grid. Row *r* must match the letters already fixed by rows above it (column constraints) plus any letters the player has typed. Candidates are narrowed via a per-position letter index.
- `countSolutions()` runs the solver with a cap of 250 and a node budget, so the live counter stays fast even on an empty board.
- `findSolution()` returns one completion consistent with the current grid; Hint and seed selection both use it.
- Undo is a simple move stack; a hint move is flagged so undoing it also decrements the hint counter.

## Attribution

- Wordle answer list: extracted from the original game's source, widely mirrored (e.g. the `possible_words.txt` in 3b1b/videos).
- Definitions: [Wordset dictionary](https://github.com/wordset/wordset-dictionary), Open Data Commons Attribution License.
