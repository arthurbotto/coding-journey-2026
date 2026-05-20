# Python Learning Journey

Career changer rebuilding fundamentals and working toward a junior developer role.
Started May 2026.

---

## 2026-05-22 — Exercism: Currency Exchange + Ghost Gobble Arcade Game (Numbers, Bools concepts)

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

## 2026-05-21 — Exercism: Guido's Gorgeous Lasagna (Basics concept)

- **Time:** 30 minutes
- **What I did:** First Basics concept exercise. Defined a few simple functions and constants for a lasagna recipe.
- **What clicked:** Concepts were already familiar from Makers, so the Python itself flowed. Noticed Exercism wants very specific docstring format — different from how I usually write them.
- **What blocked me:** Instructions took longer to parse than the code took to write. Writing the docstrings felt tedious. Worth checking if my "annoyed by docstrings" reaction is laziness or a real signal that I don't fully see their value yet.
- **Next session:** Move to Numbers concept exercise.

---

## 2026-05-20 — Exercism Hello World + plan setup

- **Time:** 1.5 hours
- **What I did:** Set up Exercism CLI, configured pytest, finished hello_world exercise. Got Anki working after a macOS launcher bug.
- **What clicked:** Why test files don't auto-run — needed `python -m unittest` or `pytest`. The launcher script had a missing `/` in the path, spotted it from the error message.
- **What blocked me:** Anki wouldn't open from the dock icon. Resolved by running launcher path directly from Terminal.
- **Next session:** Start Exercism Basics concept exercises.

---