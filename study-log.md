# Python Learning Journey

Career changer rebuilding fundamentals and working toward a junior developer role.

## Started May 2026

## 2026-06-10 — Exercism: All Your Base + List Methods

- **Time:** ~2h (mostly fighting All Your Base)
- **What I did:** Two Exercism exercises. **All Your Base** — convert a number from one base to another, taking digits in input_base and returning digits in output_base. **List Methods** — practice exercise drilling common list operations.
- **What clicked:**
  - **"Convert a number to base X" is two operations, not one.** Direction 1: given digits in some base, compute the underlying value (multiplication formula — sum of digit × base^position). Direction 2: given a value, produce digits in some base (repeated divmod, collect remainders, reverse at end). These are *inverse* operations — easy to confuse them when both involve the same base and similar-looking math. The function pivots through a plain integer in the middle, which is the only thing both bases can agree on.
  - **"X in base Y" is ambiguous in English.** It can mean either "the digit-pattern X interpreted in base Y" (direction 1) or "the value X rewritten using base Y notation" (direction 2). Same words, opposite operations. The exercise wanted the second meaning; my instinct went to the first. Worth noting because the ambiguity in plain language is real, not a comprehension failure.
  - **Integer division vs true division.** `2 / 16 = 0.125` (true division, gives float). `2 // 16 = 0` (floor division, gives the quotient as a whole number). `2 % 16 = 2` (remainder). The base-conversion algorithm uses floor division and remainder, not true division. `divmod(a, b)` returns both at once as a tuple `(quotient, remainder)`. The `divmod` function is the natural fit for this kind of algorithm.
  - **Multiple ways to unpack sequences.** `a, b = some_tuple`, `a, _, c = some_tuple` (underscore to ignore values), `first, *rest = some_list` (star to absorb the rest), `for i, n in enumerate(...)` to get both index and value. All work on any sequence — tuples, lists, strings, divmod output.
  - **`return` is silent at the call site unless you wrap with `print()`.** Calling `rebase(...)` alone doesn't display anything in a script; the return value evaporates if nothing catches it. Either `print(rebase(...))` or assign to a variable. `print` inside a function is a side effect; `return` is for handing the value to the caller. They're independent and different things.
  - **`raise ValueError("msg")` for invalid input.** Used to signal bad input — stops execution immediately and the caller can `try/except` to catch it. Tests use `assertRaises(ValueError)` to verify the function raises when expected. My function needed three validation checks at the top: input_base >= 2, output_base >= 2, and every digit must satisfy `0 <= d < input_base` (using `any()` over the digits list).
- **What blocked me:**
  - **The whole exercise was a wall.** Conceptually got tangled between the two directions of conversion for a long time. Even after understanding the concept, made multiple bugs implementing it: used `divmod(num, ...)` instead of `divmod(q, ...)` causing an infinite loop (my laptop cried, froze for a while lmao), missed the empty-digits-list edge case. Each bug was small in isolation but stacking them while still shaky on the concept made debugging slow.
  - **Binary still feels shaky.** I got the algorithm working but couldn't have explained the formula from scratch at the start of the day. Worth more reps with manual conversions on paper to make it stick. Probably worth circling back with deliberate practice on number bases as a topic, separate from the Exercism cadence.
- **Reflection on plan progress:** Haven't started the Week 1 mini-project (CSV-to-JSON converter) yet, two weeks in. Worth being honest about that rather than glossing over it — the Exercism + Codewars rhythm has been building real fluency, but the project portion of Phase 1 is where I'd build muscle for *applying* the patterns to multi-file, real-world code rather than single-function exercises. Should make space for it soon.
- **Next session:** Either start the CSV-to-JSON mini-project (the big rock that's been pushed back), or continue Exercism cadence if time is tight.

## 2026-06-09 — Codewars: ~6 lvl-8 list katas

- **Time:** ~1h
- **What I did:** Worked the full bar shift until 9pm — limited time at home. Did 5–6 level-8 Codewars katas focused on lists. Light maintenance day to keep the muscle memory going.
- **What clicked:**
  - **The sliding-window slice pattern came back automatically.** On the `each_cons` kata (take cascading subsets of size n from a list), wrote `[lst[i:i+n] for i in range(len(lst) - n + 1)]` first try, no fumbling. Same shape as the `is_contained` / sublist helper from yesterday — that pattern is genuinely settling in. The `range(len - n + 1)` boundary that took me ages to internalise on Sublist now feels obvious.
- **What blocked me:** Nothing. Quick session, easy katas, no friction.
- **Next session:** Number bases (still pending from yesterday). Continue Exercism. Daily kata.

## 2026-06-08 — Exercism: Darts, Perfect Numbers, Sublist, Resistor Colors (x2), Little Sister's Essay, Cards Game, Blackjack

- **Time:** ~9h (2h afternoon + 7h evening into early morning)
- **What I did:** Eight Exercism exercises across the day:
  - **Darts** — score a dart based on where it landed using Cartesian coordinates and circle radius.
  - **Perfect Numbers** — classify a number as perfect/abundant/deficient based on its aliquot sum (sum of factors excluding itself).
  - **Sublist** — given two lists, determine if one is equal to, a sublist of, a superlist of, or unequal to the other.
  - **Resistor Color** + **Resistor Color Duo** — look up resistor band values; combine two bands into a two-digit number.
  - **Little Sister's Essay** — string manipulation: title-case, sentence-ending check, whitespace trimming, word replacement.
  - **Cards Game** (7 functions) — list manipulation: get rounds, concatenate, contains, averages of various kinds, conditional doubling.
  - **Blackjack** (6 functions) — card value calculation, blackjack-hand detection, split/double-down rules.
- **What clicked:**
  - **Pythagorean theorem applied to coordinates.** Distance from origin to point `(x, y)` is `√(x² + y²)`. Same formula whether it's a dart on a target, a player in a game, or two cities on a map. The math I "didn't have" was just school geometry applied to a real situation — file it as a permanent tool, not a one-off.
  - **Order of `if` checks matters when ranges nest.** On Darts, checking `distance <= 10` first would catch every dart inside the target as 1 point, including bullseyes — because the smaller circles are inside the bigger one. Always check the narrowest condition first when ranges nest inside each other.
  - **Early return vs `elif`/`else`.** When every branch returns, both styles produce identical behaviour. `if/return/if/return/return` is the "early return" pattern — slightly less nesting, common in real code. Worth recognising both shapes.
  - **The "find factors" loop.** `[i for i in range(1, n) if n % i == 0]` collects every divisor of `n` below `n` itself. The `% == 0` test ("divides evenly") is the core mechanism. Comes up anywhere you need to enumerate factors.
  - **Sublist required a new mental model.** "Contiguous sub-sequence" means the values appear *adjacent* in the bigger list, in order — like searching for a word inside a sentence. The implementation: a helper function that slides the smaller list across the bigger one, checking each position with slicing. The argument order of `is_sublist(small, big)` ("is small a sublist of big?") reads more naturally than `contains(big, small)` ("does big contain small?") — both work, but choosing names that match the question makes the call site self-documenting.
  - **Compute once, use many.** On Perfect Numbers I had `sum(r)` called twice. Storing it in a variable makes the code clearer AND avoids walking the list twice. Cheap performance win, cleaner reading.
  - **String slicing tricks.** `[::-1]` reverses, `[::-2]` reverses with step 2, `[:-1]` is everything-but-last. The third argument of a slice is the step, and it can be negative. Will reach for this often.
  - **The "compute one thing, then use it" shape.** On Darts and Perfect Numbers, the pattern is: do a single calculation (distance, factor sum), then a simple decision tree on the result. Recognising this shape early speeds things up — instead of trying to do everything in one expression, separate "calculate the key value" from "decide based on the value."
- **What blocked me:**
  - **Forgot the distance formula on Darts.** Two coordinates inside a 10-unit grid doesn't mean inside a 10-radius circle — the diagonal distance is what matters. Geometry rust. Refreshed and noted that this comes up everywhere in programming.
  - **Sublist was the hardest of the day.** Took multiple passes to internalise three things at once: what "contiguous sub-sequence" means, how the helper function's loop iterates over positions, and which argument-order convention to follow. Felt confusing partly because the helper parameter names (`big`/`small`) weren't enforced — when the function got called with the arguments effectively flipped, the math returned False naturally (because of an empty `range(-1)`).
  - **Number bases (next exercise) — paused.** Started reading the positional notation explainer and decided to leave it for tomorrow.
- **Next session:** Number bases (the paused one). Then continue Exercism cadence.

## 2026-06-01 — Exercism: Rotational Cipher + CV/LinkedIn rewrite

- **Time:** ~3h (cipher + profile work)
- **What I did:** Implemented `rotate(text, key)` — a Caesar cipher that shifts each letter by a given key while preserving case, spaces, and punctuation. Separately, did a substantial rewrite of my CV and LinkedIn profile based on the feedback from the Google engineer at codebar.
- **What clicked:**
  - **Modulo for wraparound is the core mechanic of cyclic problems.** `(n - base + key) % 26 + base` is the canonical "shift within a cyclic range" formula. The four operations: shift to a 0-indexed range, add the offset, modulo by the cycle size, shift back. Same shape applies to anything cyclic — clock arithmetic, calendar wraparound, ring buffers, any rotation problem.
  - **Two parallel systems with different bases.** Uppercase letters live at ASCII 65–90; lowercase at 97–122. There's a 6-character gap of non-letter symbols (`[ \ ] ^ _ \``) between them, which is why you can't treat A–z as one continuous alphabet. The fix: detect case per character and use the right base (65 or 97) in the same formula. General pattern: when two systems do the same thing with different reference points, parameterize the offset rather than write two versions of the math.
  - **Don't destroy information you'll need later.** My first attempt called `.lower()` on the input upfront and then couldn't recover the original case. Cleaning data is often necessary, but doing it too early throws away information later steps need. The fix was to preserve the input and decide per character whether to treat it as upper or lower.
  - **Read hints, stop before the answer, work it out, then compare.** Coach gave a conceptual nudge ("two alphabets with different bases"). I stopped reading there, wrote the working solution myself, then came back and saw the only difference was a small refactor (using a `base` variable instead of duplicating the math in two branches). That's the rhythm — partial hint, productive struggle, then compare. Much better retention than reading the full answer up front.
- **What blocked me:** Got tangled trying to handle uppercase by treating A–z as one 58-character alphabet. The ASCII gap between Z and 'a' was the missing piece — once I saw they were two separate 26-character systems with the same math but different bases, it fell into place.
- **CV / LinkedIn rewrite:** Acted on feedback from the codebar conversation. Trimmed CV to one page, removed redundant tech stack listings, cut padding so every line earns its space. Updated LinkedIn About to match the new positioning. Next step is to actually start sending applications with the new version.
- **Next session:** Continue Exercism. Daily kata.

## 2026-05-28 — Exercism: ISBN Verifier

- **Time:** ~2h (solving + reviewing community solutions + rewriting from memory)
- **What I did:** ISBN-10 Verifier. Strip hyphens from input, validate it's 10 characters with digits-only except the last (which can be a digit or 'x'), then apply the ISBN checksum formula `(d₁ * 10 + d₂ * 9 + ... + d₁₀ * 1) mod 11 == 0` to confirm validity.
- **What clicked:**
  - **Handle special cases at the conversion step, not as workarounds.** My first solution converted the string to digits, then realised `x` would break the conversion, then patched it by replacing `x` with `'1'` before the conversion AND hardcoding `10 * 1` at the end of the formula to compensate. Two layers of workaround for one problem. The cleaner pattern handles `x → 10` *inside* the list comprehension: `[10 if char == 'x' else int(char) for char in s]`. The special case lives in the moment it matters; the rest of the code stays clean. Same shape applies to anything "convert each item, but one is special" — grades, currency symbols, sentinel values.
  - **`zip` + `range(10, 0, -1)` for paired iteration with weights.** My first solution wrote out the checksum by hand: `n[0]*10 + n[1]*9 + n[2]*8 + ... + n[9]*1`. Long, error-prone, and duplicated once for the x-case and once for the no-x case. The clean version pairs each digit with its weight and sums the products in one expression: `sum(d * w for d, w in zip(digits, range(10, 0, -1)))`. `range(10, 0, -1)` generates `[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]`. Much shorter, no duplication, easier to change later.
  - **The validation cascade pattern.** Three sequential `if` checks each handling one rule (length wrong → reject, non-digit before last → reject, last char wrong → reject). Top-to-bottom reading order, each rule isolated. Same "one job per line" idea from Pig Latin's function split.
  - **`char.isdigit()` is more readable than `char in '0123456789'`.** Both work; the first reads as English.

### First solution (works, but awkward)

```python
def is_valid(isbn):
    s = isbn.lower().replace('-', '')

    if not s or len(s) != 10:
        return False
    if not all(char in '0123456789' for char in s[:-1]):
        return False
    if s[-1] not in '0123456789x':
        return False

    if s[-1] == 'x':
        new_s = s.replace('x', '1')   # workaround: pretend x is 1 so int() doesn't break
        n = [int(digit) for digit in new_s]
        return (n[0]*10 + n[1]*9 + n[2]*8 + n[3]*7 + n[4]*6 +
                n[5]*5 + n[6]*4 + n[7]*3 + n[8]*2 + 10*1) % 11 == 0   # then hardcode 10 here

    n = [int(digit) for digit in s]
    return (n[0]*10 + n[1]*9 + n[2]*8 + n[3]*7 + n[4]*6 +
            n[5]*5 + n[6]*4 + n[7]*3 + n[8]*2 + n[9]*1) % 11 == 0
```

Two issues: the `x → 1` replacement plus the hardcoded `10 * 1` is a double-workaround for one problem. And the checksum is written out twice, once per branch.

### Cleaner version (rewritten from memory after reading community solutions)

```python
def is_valid(isbn):
    s = isbn.lower().replace('-', '')

    if len(s) != 10:
        return False
    if not all(char.isdigit() for char in s[:-1]):
        return False
    if not (s[-1].isdigit() or s[-1] == 'x'):
        return False

    digits = [10 if char == 'x' else int(char) for char in s]
    return sum(digit * weight for digit, weight in zip(digits, range(10, 0, -1))) % 11 == 0
```

The `x` is handled once, at the conversion step. The checksum is a single expression. No duplication.

- **What blocked me:** Spent extra time getting the validation logic right (mixed up `and`/`or` in the original condition, same shape of bug as in Triangle). Got there with guidance, then rewrote the cleaner version from memory after looking at community solutions — that felt like a small but real milestone: not just copying a better solution, but understanding the *pattern* behind it well enough to reconstruct it on my own.
- **Next session:** Continue Exercism. Daily kata.

## 2026-05-27 — Exercism: Isogram + codebar London workshop

- **Time:** ~3h (30min coding + 2.5h workshop)
- **What I did:** Solved the Isogram exercise on Exercism (check if a word has no repeated letters). Attended the codebar London student workshop at Google's London office in Pancras Square — work-through-tutorials format with volunteer developer coaches, plus a one-to-one career conversation with a Google software engineer.
- **What clicked:**
  - **The frequency-counter pattern:** `char_count[char] = char_count.get(char, 0) + 1` builds counts in a dict in one line per item. The `.get(key, default)` method returns the default if the key isn't present yet — much cleaner than checking `if key in dict` first. Worth filing as a real-world pattern; it comes up constantly.
  - **Choosing the right tool by question shape.** My dict approach worked but was more powerful than needed. The cleaner version uses a set: `len(word) == len(set(word))` after cleaning. Rule of thumb: "any duplicates?" → set. "How many of each?" → dict (or `collections.Counter`). "What's most common?" → `Counter`.
  - **`collections.Counter` exists** — it's a Python built-in that does exactly what my hand-rolled dict counter does, in one line: `Counter("eleven")` → `{'e': 3, 'l': 1, 'v': 1, 'n': 1}`.
- **What blocked me:** First instinct with a set was failing. Switched to the dict approach and it worked.
- **Career conversation at codebar (key takeaways):**
  - **CV needs heavy rework.** Should be **one page**. Too much redundant content, and tech stacks listed in too many places — the same stack repeated under skills, projects, and elsewhere reads as padding rather than depth. She suggested ruthless trimming.
  - **Two strategic paths to break into tech:** (1) **Freelance** as a faster route in — get real paid experience, build a track record, then move to permanent roles. (2) **Drill DSA hard** if I want to target bigger companies (the Phase 2 plan already covers this).
  - Worth deciding which path fits better before applying broadly. Freelance is faster but lower-paid and less structured early; DSA-heavy prep is slower but opens larger doors.
- **Next session:** Catch up on regular Exercism cadence. Start thinking seriously about the CV rewrite — that's free, high-leverage, and an actionable take from a real working engineer.

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
