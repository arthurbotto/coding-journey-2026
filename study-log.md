# Python Learning Journey

Career changer rebuilding fundamentals and working toward a junior developer role.

## Started May 2026

## 2026-05-26 — Exercism: Pig Latin, Little Sister's Vocabulary, Pangram

- **Time:** ~3h
- **What I did:** Three Exercism exercises. Pig Latin (translate English words/sentences using four rules around vowels and consonants) — the hard one. Little Sister's Vocabulary (four small string-transformation functions for prefixes and suffixes). Pangram (check if a sentence uses every letter of the alphabet).
- **What clicked:**
  - **The "find the split index" reframe on Pig Latin.** I was trying to handle the four rules as four separate conditional branches and the code kept getting messier. The reframe — every rule reduces to *where do I split the word*, then `word[i:] + word[:i] + "ay"` — collapsed four branches into one scan with two exceptions (qu and non-leading y). Big mental model shift. Worth filing: when "just one more if statement" keeps making things worse, the *approach* needs to change, not the conditionals.
  - **Sets eliminate duplicates automatically.** On Pangram, instead of a for-loop appending characters to a list and checking length/uniqueness manually, `set(char for char in sentence.lower() if char.isalpha())` collects only the unique letters in one expression. Then comparing the sorted set to `'abcdefghijklmnopqrstuvwxyz'` is the cleanest check. First version used a for loop with a list — refactored to the set comprehension and the code halved in size.
  - **Smart use of `join()` on Little Sister's `make_word_groups`.** The instructions explicitly said not to use a for loop. The trick was making the separator itself include the prefix: `f" :: {vocab_words[0]}"`, then `separator.join(vocab_words)`. Once seen, obvious; before seen, not.
  - **`isalpha()` for catching trailing punctuation.** On `adjective_to_verb`, my first version produced `"black.en"` because the punctuation stayed attached. Checking `verb[-1].isalpha()` and stripping the last char if False fixed it. Worth remembering — string methods like `isalpha`, `isdigit`, `isspace` are simple but underused.
- **What blocked me:** Pig Latin was a real wall — spent ~2 hours unable to extend my approach to Rules 3 and 4 because I was iterating and mutating a list instead of thinking in terms of slicing. Needed help to see the split-index reframe. The other two went smoothly.
- **Next session:** Continue Exercism. Push through the remaining practice exercises in the current concept area.

## 2026-05-25 — Exercism: Bob, Raindrops + 10 Codewars 8 kyu katas

- **Time:** ~2h
- **What I did:** Two Exercism exercises (Bob — pattern-match a reply based on input shape; Raindrops — FizzBuzz variant using divisibility by 3/5/7) and ten 8 kyu Codewars katas on fundamentals.
- **What clicked:**
  - **`if`/`elif`/`else` stops at the first match — multiple independent `if` statements don't.** On Raindrops I initially used `elif` chains and only one Pling/Plang/Plong got appended even when multiple conditions applied. Switching to separate `if` statements fixed it. Different tools for different jobs: `elif` for mutually exclusive branches, separate `if`s for independent checks that should all run.
  - **The order of conditionals matters when there's a default case.** My initial bug was that I appended to the result, then unconditionally returned `n_string` at the end — so the result got overridden. Wrapped the return in `if len(result) > 0` to check first. Could also have built `result` as a string with `+=`, but the list + `''.join()` approach is fine and actually slightly more efficient for multiple appends.
  - **Bob was straightforward** — recognising it's a pattern-match on input shape (question? all caps? both? empty?) makes the structure obvious. The four conditions plus a default catch-all.
- **What blocked me:** Raindrops — forgot how `if/elif` flow control actually works under the hood. Specifically that `elif` short-circuits once a branch matches. Two-week-old me would have spotted this faster — clearly a rusty spot to drill.
- **Next session:** Continue Exercism.

## 2026-05-24 — Exercism: Armstrong Numbers, Collatz Conjecture, Meltdown Mitigation

- **Time:** ~3h
- **What I did:** Three Exercism exercises — Armstrong Numbers (check if digits raised to N equal the number), Collatz Conjecture (iterate until a number reaches 1), and Meltdown Mitigation (conditional logic for reactor states). Also reviewed old notes from Makers bootcamp.
- **What clicked:**
  - **Practice exercises ≠ concept exercises.** I'd been holding back from using for-loops, list comprehensions, `sum()` and whatever other methods because they hadn't appeared in Exercism's syllabus yet. That caution made sense on concept exercises (where the point is to compose within the taught feature), but it's the wrong rule on practice exercises — which are open to anything I genuinely know. Community solutions confirmed it: everyone uses whatever Python they have. Going forward: on practice exercises, write idiomatic Python freely.
  - **Generator expressions vs list comprehensions inside `sum()`.** `sum([x for x in stuff])` builds a full list in memory then throws it away. `sum(x for x in stuff)` — same code minus the brackets — generates and sums one value at a time. Same result, less memory. Applies to `any()`, `all()`, `max()`, `min()` too. Small but very idiomatic.
  - **Caching repeated computation.** On Armstrong, I computed `str(number)` once and stored it. Looking at community solutions, several recomputed `len(str(number))` inside the loop for every digit. Small inefficiency but a real one. The instinct to compute something once and reuse it is a habit worth keeping.
  - **`while` loops are the natural fit for "keep going until a condition is met."** Collatz needs to iterate an unknown number of times until reaching 1 — exactly what `while` is for, vs `for` which is for known iteration counts. Worth filing this distinction.
  - **Conditional logic on Meltdown felt easy.** No real friction — that's the signal that the basics are starting to settle. Compare with last week where the Triangle precedence bug took me a while to spot.
- **What blocked me:** Nothing significant today. Had to look up how to convert a number to a string and back to int (`str()` and `int()`) — still not at fingertip recall.
- **Next session:** Continue Exercism (whatever unlocks after Meltdown). Daily kata.

## 2026-05-23 — Codewars: four 8 kyu katas

- **Time:** 1h
- **What I did:** Four 8 kyu katas — "Grasshopper Terminal Game Combat" (subtract damage from health, floor at 0), "ASCII Total" (sum of character ord values in a string), "Square(n) Sum" (sum of squared numbers), and "L1: Bartender, drinks!" (return drink based on customer name).
- **What clicked:**
  - `ord()` returns a character's ASCII/Unicode code point. Counterpart is `chr()` which goes the other way. Worth knowing — comes up in string-processing problems.
  - List comprehensions plus `sum()` are the natural fit for "transform each, then total": `sum(n ** 2 for n in nums)`. A generator expression inside `sum()` is even more memory-efficient than a list comprehension since it doesn't build the intermediate list — same syntax, just no brackets.
  - `match`/`case` works cleanly for "if input == X return Y" mapping problems. Cleaner than chained `if/elif/else` once there are more than 2-3 cases. Worth remembering as an alternative — it's a Python 3.10+ feature so doesn't appear in older tutorials.
- **What blocked me:** Nothing significant. Had to look up `ord()` since I didn't remember the function name, but the concept was familiar. Speed is starting to come — four katas in an hour feels like a real shift from last week.
- **Next session:** Continue Exercism + Codewars katas. Likely ready to mix in 7 kyu within a few days.

## 2026-05-22 — Codewars: two katas

- **Time:** 1h
- **What I did:** Two 8 kyu Codewars katas — "Remove all exclamation marks from the end of sentence" and "Enumerable Magic #30 - Split that Array!"
- **What clicked:**
  - `rstrip()` is the cleanest way to strip trailing characters — `s.rstrip("!")` removes all trailing `!`s in one call. Worth remembering it also takes a string of characters to strip, not just whitespace.

  - A manual `while` loop also works and is worth understanding even if not idiomatic:

    ```python
    while s and s[-1] == "!":
        s = s[:-1]
    return s
    ```

    The `s and ...` short-circuit prevents an IndexError on empty strings — neat pattern.

  - On the second kata I reached for `map()` when I needed `filter()`. Mixed them up. The distinction: `map` *transforms* each element (same count out), `filter` *keeps or drops* each element (fewer count out). Filing this — they get confused easily.
- **What blocked me:** Had to look up string methods. Don't have them in muscle memory yet.
- **Next session:** Continue Exercism + one Codewars kata.

## 2026-05-21 — Exercism: Leap, Triangle, Grains And some codewars Katas

- **Time:** 4h
- **What I did:** Three practice exercises — Leap (year-checking logic), Triangle (classify as equilateral/isosceles/scalene with a validity check), and Grains (compute grains on a chessboard square + total).
- **What clicked:**
  - **Tuple unpacking works on lists too** — `a, b, c = [1, 2, 3]` is valid. Missed this from the Basics material and it would have saved me time on Triangle if I'd known it earlier.
  - **Parentheses when mixing `and` and `or`** — `and` binds tighter than `or` in Python, same as `*` binds tighter than `+` in math. Without parens, `is_triangle(sides) and a == c or a == b or b == c` gets read as `(is_triangle AND first pair) OR second pair OR third pair`, meaning is_triangle only gates the first comparison. Two tests failed because of this. The fix was wrapping the `or` chain: `is_triangle(sides) and (a == c or a == b or b == c)`. Rule going forward: whenever I mix `and` with `or`, parens around the `or` group, even if not strictly needed — for readability.
  - **Simulation vs formula** — Grains. My solution used a loop and a dict to compute every square's value, then look up the answer. It worked, but the elegant version is just `2 ** (n - 1)` for one square and `2 ** 64 - 1` for the total. I had actually spotted the doubling pattern, but didn't have the math vocabulary (`2 ** n`) to express it directly, so I reached for a loop. Worth knowing for next time: doubling N times = `2 ** N`; halving N times = `value / (2 ** N)`; sum of doubling from 1 = `2 ** N - 1`.
- **What blocked me:**
  - Leap year instructions were confusing — the divisibility-by-100/400 exception is genuinely awkward English. Slowed me down even though the logic itself is simple.
  - Triangle isosceles bug — couldn't see the precedence issue from staring at the code. Only became obvious when I traced `[1, 1, 3]` step by step. Reinforces the lesson: when code looks right but tests fail, substitute the failing input and walk through manually.
  - Grains — felt stuck because my instinct was to loop, but the exercise hinted that loops/dicts weren't expected yet. Spent time over-engineering with a global dict before solving it. The real gap was a missing piece of math notation, not a thinking problem.
- **Next session:** Continue with whatever Exercism unlocks next.

---

## 2026-05-20 — Exercism: Currency Exchange + Ghost Gobble Arcade Game (Numbers, Bools concepts)

- **Time:** 2 hours
- **What I did:** Two concept exercises. Ghost Gobble was straightforward booleans. Currency Exchange was the real work — six functions involving currency conversion, bill denominations, and remainders.
- **What clicked:**
  - The "physical booth" framing: denomination matters because the booth only hands out whole bills. `(money // denomination) * denomination` strips the fractional bill — equivalent to `money - (money % denomination)`. Understanding the *why* made the math obvious.
  - Float + `round()` is genuinely enough for currency here. I didn't need Decimal at all. Bigger lesson: don't reach for sophisticated tools when basic ones meet the requirements.
  - Tests are the source of truth, not example output in prose.
- **What blocked me:**
  - Over-engineered with Decimal and made three mistakes in that detour:
    - Thought `getcontext().prec = 5` meant decimal places — it's actually total significant digits (caused `DivisionImpossible` on large numbers like 470000 / 1.17e-07).
    - Used `Decimal(float)` instead of `Decimal(str(value))`, which captures float's imprecision and defeats the point of using Decimal.
    - Returned `Decimal` where tests expected `float` (TypeError).
  - Mentally locked on the example output `181.82` as a strict rule, assumed float division wouldn't produce it. Should have just tried the simple version and let pytest tell me what's right.
  - Same as last session: parsing instructions took longer than writing the code.
- **Next session:** Continue Exercism — whatever unlocks next.

---

## 2026-05-19 — Exercism: Guido's Gorgeous Lasagna (Basics concept)

- **Time:** 30 minutes
- **What I did:** First Basics concept exercise. Defined a few simple functions and constants for a lasagna recipe.
- **What clicked:** Concepts were already familiar from Makers, so the Python itself flowed. Noticed Exercism wants very specific docstring format — different from how I usually write them.
- **What blocked me:** Instructions took longer to parse than the code took to write. Writing the docstrings felt tedious. Worth checking if my "annoyed by docstrings" reaction is laziness or a real signal that I don't fully see their value yet.
- **Next session:** Move to Numbers concept exercise.

---

## 2026-05-19 — Exercism Hello World + plan setup

- **Time:** 1.5 hours
- **What I did:** Set up Exercism CLI, configured pytest, finished hello_world exercise. Got Anki working after a macOS launcher bug.
- **What clicked:** Why test files don't auto-run — needed `python -m unittest` or `pytest`. The launcher script had a missing `/` in the path, spotted it from the error message.
- **What blocked me:** Anki wouldn't open from the dock icon. Resolved by running launcher path directly from Terminal.
- **Next session:** Start Exercism Basics concept exercises.

---
