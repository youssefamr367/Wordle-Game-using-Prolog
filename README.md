# Pro-Wordle — Wordle in Prolog

A console version of Wordle written in Prolog. You build the word database yourself at startup, choose a category and a word length, and then get six guesses at a randomly chosen word, with feedback on which letters are right and which are in the right position.

Written as a logic programming course project.

## How it works

The game runs in three phases:

1. **Build the knowledge base.** `build_kb` reads a word and its category, one per line, asserting each pair as a `word(Word, Category)` fact. Entering `done` ends this phase.
2. **Pick a word.** `play` lists the categories it found, asks for one and for a word length, then picks a random word of that length from that category. Bad input is handled rather than fatal: an unknown category, a length with no matching words, and a non-numeric length each re-prompt.
3. **Guess.** Six guesses. Each one must be the right length and must exist in the database. After a wrong guess the game reports the letters that appear in the target word and, separately, the letters that are in the correct position, then the number of guesses left.

## Predicates worth knowing

| Predicate | Purpose |
| --- | --- |
| `build_kb` | Reads words and categories into the database until `done` |
| `categories(L)` | All distinct categories, via `setof` |
| `is_category(C)` | True if any word has that category |
| `list_of_random(C, L)` | All words in category `C` |
| `pick_word(W, L, C)` | Random word of length `L` from category `C` |
| `available_length(L)` | True if a word of that length exists |
| `correct_letters(L1, L2, CL)` | Letters of the guess that appear in the target, no duplicates |
| `correct_positions(L1, L2, CP)` | Letters of the guess that sit in the right position |
| `play` / `play_helper/4` | Game loop and guess counter |
| `main` | Entry point — prints the banner, builds the database, starts the game |

The helper predicates are written in the accumulator style, with a two-argument wrapper delegating to a four-argument version that threads the result through `Acc`.

## Running it

Requires SWI-Prolog (`random_member/2` and `atom_chars/2` are used).

```bash
swipl Wordle.pl
```

Then at the prompt:

```prolog
?- main.
```

Words and categories are read as Prolog terms, so each entry needs a trailing full stop:

```
Please enter a word and its category on separate lines:
|: apple.
|: fruit.
|: melon.
|: fruit.
|: done.
```

Guesses are entered the same way — `apple.` rather than `apple`.
