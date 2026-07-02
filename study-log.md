# Python Learning Journey

Career changer rebuilding fundamentals and working toward a junior developer role.

## Started May 2026

## 2026-07-02 — Exercism: RNA Transcription, Atbash Cipher, Wordy

- **Time:** (not tracked across the sessions, did it across 3 days.)
- **What I did:** Three exercises since the last log entry. RNA Transcription, a straightforward one-liner dict substitution. Atbash Cipher, a substitution cipher with groups-of-5 encoding and decoding. Wordy, a word problem parser evaluating math expressions left to right, with multiple error cases to handle.
- **What clicked:**
  - **RNA Transcription as a clean one-liner.** Fixed substitution dict plus a generator expression inside `join`, no loop needed. Same pattern as Atbash but simpler since there's no grouping or error handling involved.
  - **Atbash Cipher built on a previous string exercise insight.** The groups-of-5 encoding (`clean[i:i+5] for i in range(0, len(clean), 5)`) came directly from looking back at Rotational Cipher. `str.translate` with `str.maketrans` handled the substitution in one call for both encode and decode, cleaner than a dict lookup loop. `split()` with no arguments handles any whitespace in decode, including the multiple-spaces test case, since it discards empty strings automatically.
  - **Mapping operation words to functions via a dict.** `'plus': lambda a, b: a + b` etc. The operation word becomes the dict key, the function is the value, no `if/elif` chain for four operations. `operations[operator](result, number)` applies whichever one matched. Same "data, not branching" pattern as the tax bands.
  - **While loops for consuming a list as you go.** `list.pop(0)` removes and returns the first item, shrinking the list each pass until it's empty. Used to consume the word list left to right: pop the first number as the initial result, then repeatedly pop an operator and a number, apply the operation, update the result.
  - **`'multiplied'` and `'divided'` are two-word operations.** After popping `'multiplied'` or `'divided'`, pop and discard the next word (`'by'`) before popping the number. `'plus'` and `'minus'` go straight to the number.
  - **Two distinct `ValueError` messages, each triggered by different bad input shapes.** `"unknown operation"` only for words that aren't numbers and aren't in the operations dict (genuinely unsupported operations like `'cubed'`). `"syntax error"` for everything else: empty question, number where an operation was expected, operation where a number was expected, missing operand at the end. Getting the distinction right required running the debugger to watch exactly which line each failing test was hitting and why.
  - **`lstrip('-').isdigit()` to detect a number token.** Plain `.isdigit()` returns `False` for `'-2'` since the minus sign isn't a digit. Stripping the leading minus first makes it work for negative numbers too, without accidentally treating operation words as numbers.
  - **`try/except ValueError` around `int()` calls to catch bad syntax.** When `int()` receives a word like `'plus'` instead of a number string, it raises a `ValueError` with Python's own message. Wrapping those calls in `try/except` and re-raising as `ValueError("syntax error")` converts Python's internal error into the message the test expects.
  - **Using the debugger actively to trace which line each test failure hit.** Set breakpoints on each `raise` and stepped through the failing cases. Made the error flow concrete instead of trying to reason about it abstractly, and made the distinction between `"unknown operation"` and `"syntax error"` much clearer.
- **What blocked me:**
  - On Wordy, the error handling. The main calculation logic came together without too much trouble; the eight failing tests were all in the error cases. The debugger was what finally made them click, watching exactly which line each bad input reached and what it crashed on.
- **Reflection:** RNA Transcription and Atbash Cipher both came independently. Wordy was a different story: started out quite lost, initially trying to extract numbers separately from operations before understanding that keeping them together as a word list was the right approach. The overall structure (cleaning the string, `pop(0)` loop, dict of lambdas, handling two-word operations) came from heavy hinting throughout. The error handling section, which produced 8 failures, also needed help to sort out. Used the debugger once on the "plus plus" case to understand which line was being hit, which helped that specific case click. Honestly more of a "followed along and understood" session than an independent one.
- **Next session:** Continue Exercism cadence.

## 2026-06-25 — Exercism: Flower Field

- **Time:** 6h+, on and off
- **What I did:** Solved Flower Field (a Minesweeper style exercise: annotate each empty square in a 2D grid with the count of adjacent flowers, raise a `ValueError` on malformed input). Heavy reliance on AI help throughout. `count_adjacent_flowers`, the core neighbor counting function, was generated by AI, not figured out independently. The rest, the main `annotate` loop and the validation, was built by re reading and working through AI's explanation rather than arriving at it alone. Got stuck for a long time, frustrated repeatedly.
- **What clicked:**
  - **Thinking of the grid as rows and columns by index**, `garden[r]` is row `r`, `garden[r][c]` is the character at that row, column `c`. Took a long back and forth to actually picture this, but it did land.
  - **Offsets as row/column index differences, not "movement."** The 8 neighbor positions are just `r-1/r/r+1` paired with `c-1/c/c+1`, minus the cell itself. Adding an offset's two numbers to the current `r, c` gives a neighbor's coordinates.
  - **"Out of bounds" means the resulting row or column index doesn't exist in the grid**, not negative numbers being inherently invalid, just that there's no row or column at that index. Checking this explicitly matters because Python silently allows negative indexing (`garden[-1]` returns the last row instead of erroring), so you can't rely on a crash to catch an invalid index.
  - **Bounds checks for a neighbor must use the neighbor's own row index**, not the original cell's row index, when checking the neighbor's column length, since rows could in principle have different lengths.
  - **`and` versus `or` for two independent failure conditions.** First version of the validation used `and`, which only raised when both the length check and the character check failed simultaneously, instead of either one alone. Fixed by separating into two independent checks, each able to raise on its own.
  - **An average based length check doesn't test what "all rows the same length" actually means.** First version compared the average row length to the row count, two unrelated numbers. Fixed by comparing every row's length directly against the first row's length.
- **What blocked me:** Most of it. Could not work out `count_adjacent_flowers` independently, even after understanding the grid and offsets conceptually, translating that into working code didn't happen without it being written out directly. The validation logic also failed for a while, the `and`/`or` mistake meant boards with mismatched row lengths weren't being caught, despite passing some other tests.
- **Reflection:** Genuinely the hardest and most frustrating exercise so far. Real progress: actually understanding the grid as row/column indices and how the 8 offsets work, that part is solid now. The rest, particularly the counting function, is shaky, reproducing it from scratch right now isn't something I'm confident I could do. Being honest about that rather than overstating it.
- **Next session:** Continue Exercism cadence. Possibly worth revisiting Flower Field cold at some point to see if it sits better the second time.

## 2026-06-24 — Exercism: Tisbury Treasure (continued), Matching Brackets, Diamond Kata

- **Time:** I didnt track to be fair. This was over two days.
- **What I did:** Yesterday, continued Tisbury Treasure, fixed a bug in `convert_coordinate` and built `clean_up`, then solved Matching Brackets. Today, solved the Diamond Kata.
- **What clicked:**
  - **A test calling a function once per item can look, from a partial view of the test file, like it's calling the function on the whole list at once.** On `convert_coordinate`, printed the function's output, noticed it exactly matched the test's full expected list, and traced backward from that to realize I'd been calling the function on the entire `input_data` list rather than one coordinate. The test was actually looping and calling the function per item via subtests, which wasn't visible in the snippet I'd been looking at.
  - **Indexing one position gives a bare value; slicing a range gives back the same type (a tuple, here).** `clean_up` failed trying to add `data[0]` (a string) to `data[2:]` (a tuple), string plus tuple isn't valid. Fixed by slicing instead of indexing the first element, `data[:1]`, so both sides were tuples and could be concatenated.
  - **`str()` versus `repr()`, and why printed output looks different from a test's expected string.** Used `str()` to turn each cleaned tuple into a string before joining lines with `\n`. Separately, `print()` shows the *effect* of a newline character (an actual line break), while a test failure message or the REPL shows the *repr*, with `\n` spelled out literally. Same string, two different displays.
  - **A stack (list, `.append()`/`.pop()`) and a dict (fixed lookup of closer to opener) do two different jobs in Matching Brackets.** The list tracks what's currently open and unclosed, in order; the dict is a fixed reference table for "what does this closer need to match." Needed both together.
  - **Module level variables persist across multiple calls in the same process, which only shows up under pytest, not when running the script directly.** `stack` and `pairs` were defined outside the function. Running the script directly called the function once, so it looked correct. Running pytest called it many times in the same process, sharing the same `stack` between calls, so leftover state from one test call corrupted the next. Every failing test was one expecting `True`, the fingerprint of leftover junk dragging down otherwise valid input. Fixed by moving `stack` inside the function so each call starts fresh.
  - **Used the debugger properly for the first time on this kind of problem.** Breakpoints only pause at the exact line they sit on, moving one inside the loop (rather than on the first and last lines of the file) let me step through character by character and watch `stack` grow and shrink in the variables panel.
  - **Diamond Kata, breaking the problem into a row formula plus a mirroring step.** Needed the row spacing relationship explained (leading and trailing spaces for row `i` equal `n - i`; spaces between the two letters equal `2i - 1`, where `n` is the widest letter's distance from A), but worked out the implementation myself, including handling the single letter row separately, computing `n` from `ord(letter)`, and building the full diamond as the top half plus the top half reversed with the last row dropped so the middle row isn't duplicated. Verified it by hand tracing against the C and D examples in the instructions before trusting it.
  - **`"\n".join(list)` versus printing a list directly.** Printing the raw list of rows shows Python's repr, quotes and commas, all on one line, since there are no actual newline characters between list elements. Joining the rows with `"\n"` first produces the real multi line diamond shape.
- **What blocked me:**
  - Misreading what `convert_coordinate` and the test expected as input, until tracing the printed output against the test's expected list revealed the mismatch.
  - The Diamond Kata overall, this is a famously difficult exercise even for experienced developers; getting from the problem statement to the row formula didn't happen on my own, needed it explained, then did the implementation and verification myself.
- **Reflection:** Matching Brackets surfaced a real bug category, state living somewhere it shouldn't (module level instead of inside the function) producing different results depending on how many times the code runs, worth remembering outside this exercise. Diamond Kata took over an hour and needed real help on the core insight; being honest that the formula wasn't self derived, but the translation into working, verified code was.
- **Next session:** Continue Exercism cadence.

## 2026-06-21 — Exercism: Resistor Color Expert + Making the Grade

- **Time:** ~3h (most of it on Resistor Color Expert)
- **What I did:** Finished Resistor Color Expert, solved correctly on the first try. Then completed Making the Grade, a loop based exercise covering rounding scores to ints, counting non passing students, filtering scores above a threshold, generating evenly spaced letter grade thresholds, matching ranked names to scores, and finding a perfect score in a list of student records.
- **What clicked:**
  - **Trio and Expert have different prefix rules, even as sibling exercises.** Trio only promotes to kilo/mega/giga when the value divides evenly by 1000 (3300 stays "3300 ohms"). Expert allows fractional prefixes (7300 becomes "7.3 kiloohms"). Knew from the instructions this exercise needed different handling from Trio, and built it that way from the start.
  - **`correct_division`'s int vs float split does real work.** Clean multiples of 1000 return as `int` (so "2 kiloohms", not "2.0 kiloohms"); non clean multiples return as `float` ("6.89 kiloohms"). Both output shapes are required by Expert's tests, so the split isn't defensive code, it's load bearing.
- **What blocked me:** Working out the math and logic for the fractional prefix rule took a while, but got there.
- **Reflection:** Resistor Color Expert took most of the time; Making the Grade was a straightforward loop exercise by comparison.
- **Next session:** Continue Exercism cadence.

## 2026-06-19 — Exercism: Resistor Color Trio

- **Time:** ~1h30
- **What I did:** Resistor Color Trio on Exercism, decode three resistor colour bands into a resistance label like "33 ohms" or "33 kiloohms". My approach: look up each colour's index in a colours list, build the two-digit value from the first two bands, multiply by `10 ** (third band)` for the trailing zeros, then format with the right metric prefix.
- **What clicked:**
  - **The decode itself.** First two colours → a two-digit number; third colour → a power of ten (the number of zeros). Got this part on my own.
  - **The metric prefix depends on clean divisibility, not size.** You only step up to kilo/mega/giga when the value divides *evenly* by 1000 — 33000 → "33 kiloohms", but 3300 stays "3300 ohms". I'd assumed it was purely about magnitude (anything ≥ 1000 → kilo), which is wrong.
  - **`//` truncates.** `3300 // 1000` is `3`, silently dropping the 300. Integer division is only safe once you've checked the value divides cleanly (`% 1000 == 0`), then it's exact.
  - **Passing all tests ≠ correct.** Every test value was a clean multiple of its prefix unit, so my divisibility bug never showed. It only surfaced by tracing an input the suite skips, the spec's own orange-orange-red example, which my code returned as "3 kiloohms" instead of "3300 ohms".
- **What blocked me:** The metric-prefix formatting. First version chose the prefix by magnitude and used `//`, which gave wrong answers for non-clean multiples, and passed every test, so the bug was invisible until I traced a case the tests don't cover. Fix: check `% 1000 == 0` for each prefix, largest first, with plain ohms as the fallback. The maths was the demanding part, exponents, zeros-as-powers-of-ten, and the divisibility rule took the most thinking.
- **Reflection:** Took ~1h30, a maths-heavy exercise, exponents and the divisibility rule took the most thought. Got the core decode on my own; the single piece I missed was the prefix formatting, which clicked once I saw it hinges on clean divisibility by 1000 rather than size. Good reminder that green tests don't prove correctness, the bug only appeared by tracing an input the suite leaves out.
- **Next session:** Back to Exercism cadence.

## 2026-06-18 — CSV-to-JSON Mini-Project (Phase 1): argparse CLI + CLI tests — complete

- **Time:** ~2h
- **What I did:** Finished the mini-project. Added the argparse CLI layer (step 4): a required positional `input` argument and an optional `-o/--output` flag, so the tool prints JSON to the terminal by default or writes it to a file with `-o`. Refactored `main` to take `argv=None` so the CLI is testable. Added three CLI tests, writes-to-file, prints-to-stdout, and errors-on-no-args, bringing the suite to six, all green. It's a complete, typed CSV→JSON CLI converter now.
- **What clicked:**
  - **argparse is small, and positional vs optional is the core distinction.** A bare name (`input`) is a required positional; a dash-prefixed name (`-o`) is optional. `-o` takes a *value* (the filename), so it uses the default "store" behaviour with `default=None`. `action="store_true"` is for *valueless* on/off flags like `--verbose`, the wrong tool for `-o`. The CLI was the part I'd built up as big; it's the smallest piece of the project.
  - **CLI code has to live inside `main()`, under the guard, not at module level.** My first version had `parse_args()` at the top of the file. Module-level code runs on *import*, so when a test did `from main import converter`, argparse fired during import (reading pytest's argv), errored, and took the tests down with it. The `if __name__ == "__main__"` guard exists precisely to stop import-time execution; moving the argparse into `main()` put it back under that protection. Same guard lesson as before, now with a concrete consequence.
  - **`write_text` creates the file; `touch` doesn't take a filename.** I'd written `Path(".").touch(output)` assuming I had to create the file before writing. Traced it: `touch`'s first argument is a file *mode*, not a filename, and on an existing path it just bumps the timestamp and returns, the filename string was silently ignored. `write_text` had been creating the file all along (it creates if missing). Deleted the `touch` line.
  - **A Path is just a name; relative paths resolve from the current working directory.** `Path("output.json")` doesn't "look" anywhere by itself, the OS resolves it when you *act* (`write_text`, `open`). No leading `/` means relative, measured from wherever the terminal is when the program runs. So no need for `Path(".")`, the cwd is automatic. (Same cwd idea that made the test's CSV unfindable a couple of sessions back.)
  - **Testing CLI behaviour = hand argparse the arg list directly.** The command line is just a list of strings: `sample.csv -o out.json` is `["sample.csv", "-o", "out.json"]`. `parse_args()` reads that from `sys.argv` by default but will take a list too. Refactoring `main(argv=None)` + `parse_args(argv)` lets tests call `main([...])` with no real shell, while `None` falls back to real argv so the CLI is unaffected.
  - **`capsys` for testing printed output.** Built-in pytest fixture, inject-by-name like `tmp_path`; `capsys.readouterr().out` is whatever the code printed, ready to assert on. Used it for the default print branch. (Detail: `print` adds a trailing `\n`, but `json.loads` ignores surrounding whitespace, so it parses cleanly.) The suite is layered now, `converter` tests for the logic, `main` tests for the CLI wiring (file output, stdout, bad-args `SystemExit`).
- **What blocked me:**
  - The `touch` line, ran without any error but did nothing useful; only caught by tracing it rather than assuming it worked.
  - Briefly tangled on what the finished tool even does: clarified that you type a *command naming files*, not data, `sample.csv` is read, a new *JSON* file is written, the CSV stays untouched.
- **Reflection:** The argparse layer felt easy, smaller than I expected. The trickier parts weren't the CLI itself but the concepts around it: where CLI code has to live (inside `main`, under the guard), how pathlib resolves relative paths, and how to feed argparse a fake command line for tests. Satisfied with where it's at and leaving it here. Mini-project complete: typed CLI converter that prints or writes to a file, six tests across logic and CLI.
- **Next session:** Mini-project done, back to Exercism cadence and on toward Week 2 (data-structures fluency) per the plan.

## 2026-06-15 — CSV-to-JSON Mini-Project (Phase 1): converter + first hand-written pytest tests

- **Time:** ~3h
- **What I did:** Finally started the Week 1 mini-project that's been pushed back for weeks. Built the core of a CSV-to-JSON converter: a `converter(file)` function that reads a CSV with `csv.DictReader` and returns a JSON string via `json.dumps(..., indent=2)`. Structured it as a thin `main()` calling the worker function, with the `if __name__ == "__main__"` guard. Added a type hint and then widened it. Wrote three pytest tests for it, for now, happy path, missing file, and header-only edge case Did NOT reach argparse yet; that's tomorrow.
- **What clicked:**
  - **What a CLI actually is.** A command-line tool is the *simplest* kind of program, not a desktop or web app, no GUI, the terminal is the interface. I already use these daily (`uv`, `git`, `ruff`). I'd been intimidated by the word and assumed the project meant building something with a UI. It doesn't. The finished tool is just `uv run main.py somefile.csv`.
  - **Separate the logic from the I/O (worker + thin `main`).** Same shape as the salary calculator's "bands as data, logic as function": `converter` does the transform, `main` just wires it up (read → call → print). Keeping `converter` free of printing/CLI concerns is what makes it testable in isolation.
  - **Type hints are like TypeScript — not comments, not runtime rules.** Syntax is `file: str`. The interpreter *ignores* hints at runtime; a separate checker (mypy/ty) reads them, exactly like `tsc` checks TS and then strips it. Checked-ahead-of-time documentation, not enforcement.
  - **Make the hint match reality, not the other way round.** `tmp_path` hands tests a `Path`, but I'd annotated `converter(file: str)`. The test passed anyway (interpreter ignores hints), but the types were inconsistent. The fix wasn't to wrap calls in `str(...)` — it was to widen the hint to `str | Path`, since a file-opening function *should* accept both. The signature should tell the truth about what it takes.
  - **Test the meaning, not the formatting.** First version of the happy-path test asserted against the exact pretty-printed JSON string — brittle: changing `indent=2` to `4` breaks it even though the conversion is still correct. Better: `json.loads(result)` to parse back to Python data, then assert on the *data*. Whitespace becomes irrelevant; the test fails only when the conversion is truly wrong. (`loads` = load string → Python; the mirror of `dumps`.)
  - **`tmp_path` for self-contained tests.** pytest built-in fixture: name a test parameter `tmp_path` and pytest injects a fresh temp folder. Build a file with `tmp_path / "data.csv"` then `.write_text(...)`. The test creates its own input instead of depending on a committed `sample.csv` that could change underneath it. Input and expected output sit together.
  - **raise vs try/except vs pytest.raises are three different jobs, not competing options.** `raise` signals a problem; `try/except` handles one where you can do something useful; `pytest.raises` is a *test* tool asserting the code errors when it should. raise + except are two halves of one conversation; pytest.raises lives only in tests. For the missing file: `open()` already raises `FileNotFoundError`, so `converter` just lets it propagate (a utility function shouldn't decide whether to print/quit/404 — the caller decides), and the test uses `with pytest.raises(FileNotFoundError):` to verify it.
- **What blocked me:**
  - **Spent a long time just not understanding what the project *was*** — see the CLI point. The blocker was conceptual, not technical.
- **Next session:** Build the argparse layer (step 4) — read the filename and an optional `-o` output flag from the command line so the tool runs on any file without editing code. Then add the user-facing `try/except FileNotFoundError` at that CLI layer for a clean error message. After that the mini-project is complete; back to Exercism cadence / Week 2.

## 2026-06-11 — UK Net Salary Calculator (side project)

- **Time:** ~3h
- **What I did:** Built a UK net salary calculator from scratch — `main.py` that takes a gross salary and returns annual + monthly take-home after income tax, National Insurance, optional pension contributions, and optional student loan repayments. Started with a hardcoded straightforward version that worked, then refactored to DRY using a generic `calculate_tax(money, bands)` function. Added the personal allowance taper for incomes over £100,000 via a `get_income_tax_bands(salary)` function that dynamically adjusts the first band. Set up the project with uv + ruff and added a README and module docstring.
- **What clicked:**
  - **Band-based progressive taxation as a data-driven pattern.** Income tax and NI are the same shape of calculation — apply different rates to different income slices. By representing each tax as a list of `(threshold, rate)` tuples and writing one generic `calculate_tax(money, bands)` function, both calculations share the same code. Two completely different taxes, one function. The "bands as data, logic as function" split is a strong general pattern — anywhere you have parallel rule-based calculations, extract the rules into data and write one function that consumes them.
  - **The `prev_threshold` insight.** The break check in the loop is `if money <= prev_threshold`, not `if money <= threshold`. This tripped me up conceptually — I assumed the loop would stop when it hit the `(∞, 45%)` band because of the infinity comparison. The answer: the loop stops naturally when all bands are exhausted. The break fires only when your salary doesn't even *reach* the start of the current band — i.e. when `money <= prev_threshold`. For a £150k salary, that never happens so all four income tax bands run. For a £40k salary, the loop breaks at the higher rate band because `40,000 <= 50,270`. Once that clicked, the whole loop made sense.
  - **Calculation order matters in tax law and code.** Pension contributions reduce *taxable salary* before income tax is calculated, but NI and student loan use the gross salary. That distinction took a small amount of design thinking — `calculate()` passes `taxable_salary` to income tax but `salary` to NI and student loan. The order of operations in the real-world process maps directly to the order of operations in the code.
  - **`max(0, ...)` for clamping at zero.** Used repeatedly — pension qualifying earnings (`max(0, min(salary, upper) - lower)`), student loan repayment (`max(0, salary - threshold)`), and personal allowance taper. The pattern "calculate something that could go negative, clamp to zero" appears whenever real-world rules say you only pay above a threshold. Worth filing as a routine pattern.
  - **AI-assisted refactoring vs. AI-written code.** Wrote the initial version myself with correct marginal band logic. For the DRY refactor I asked for an explanation first, couldn't see the path, then asked to just be shown it. Studied it line by line after. This is still useful — the DRY version is genuinely cleaner and I can now defend it line by line — but the distinction matters: understanding shown code is not the same as writing it.
  - **First proper use of `uv` and `ruff`.** Set up the project from scratch using the modern toolchain rather than bare Python. `uv init`, `uv add --dev ruff`, ruff auto-fixes formatting. The Phase 0 setup is now actually being used, not just installed.
- **What blocked me:** Two bugs I didn't catch myself. First, the `first_t` formula in `get_income_tax_bands` — for salaries below £100,000, `(salary - 100000)` goes negative, which caused the personal allowance to *inflate* rather than stay fixed. The formula needed a second `max(0, ...)` wrap: `max(0, 12570 - max(0, (salary - 100000) / 2))`. Second, pension contributions not reducing taxable income — I was passing the full `salary` to `calculate_tax` even with pension enabled, which meant the income tax calculation was wrong. Both bugs produced plausible-looking output with no errors — just quietly wrong numbers. That's the harder category of bug.
- **Reflection:** This is the first time in Phase 1 I built something that wasn't a single-function exercise. Designing the overall shape of the program — which functions, what they return, what they accept — separating data from logic, deciding where to round, choosing what to print vs. return — these are different skills from solving Exercism puzzles. Felt good. Should do more of this kind of thing, not less.
- **Next session:** Continue Exercism. Possibly start the CSV-to-JSON Phase 1 mini-project — given today felt productive and project-shaped, may as well keep the momentum.

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
