# Decisions waiting on Mortaro

Only questions Mortaro still has to answer. An agent adds one here with a link to where the docs argue it; when he
answers (inline or in `mortaros_notes.md`), the answer goes into the page that teaches it and a row of
`design/decisions.md`, and the item is deleted. Item numbers never change. Each item: the question, the options,
the recommendation, what it blocks. "D244" marks something that can go wrong silently today; "D205" one Claude
could decide itself. The principles that answer most questions before they are asked: the moron (anyone who uses
Spite) never chooses, the language does; performance choices belong to the compiler; whatever can be written in
Spite is written in Spite (D350); reflection exposes everything and tree-shakes what is not read (D353); one way to do each
thing; prevent mistakes rather than offer options; storage owns its items and other references are `T?` (D354).

Every other item is answered: the 14 principle answers by D369, and every confirmation of what agents decided by
D370, 76 by D371, 235 by D372, 161 by D373 and 105 by
D374 (2026-10-01); 264 by D378 and 250 by D379, 252 by D380, 254 by D381, 256 by D383, 257 by D382, 258 by D384, 261 by D385, 253 by D386, 255 by D387, 259 by D388, 260 by D389, 262 by D390, 251 by D391, 263 by D394 (2026-10-02); 266 to 296 by D468 to D488, and 268, 269, 271 and 276 to 279 by D493 to D497, 298 by D498, 301 by D499, 291 by D500, 299 by D501 (2026-10-03); 275 by D503, 302 by D504, 304 by D505 (2026-10-04); 280, 281, 284, 297, 300 and 303 by D510, decided by Claude under D509 and listed under To confirm (2026-10-07); the rest of To confirm triaged by D1781 (2026-10-29). Earlier answers are listed in each row of `design/decisions.md`.

## To confirm
- D1813: a program's Build flags are given on the compiler's command line beside the compiler's Environment settings, validated against the program's declared Build attributes (replaces D1812's last sentence).
- D1819: an attribute a loop keeps in a register is stored before and loaded after a call on a cold path of the loop (a block not run on every pass and in no inner loop), so such a call no longer keeps it in memory.

Triaged by D1781 (2026-10-29): every other agent decision listed here was superseded by a later decision or settled
as the compiler's own under D509 and D1758. What is left changes what a program can write or what it means.

- D1789: a program's own file wins over every pinned commit of a repository, and the wording of two commits that disagree (spite_compiler#253).
- D1790: the bootstrap's stepping stones bridged inside `scripts/bootstrap.sh` (spite_compiler#267).
- D1795: every conversion called by name is refused (D1730's broad form), and the wording of the two errors (spite_compiler#234).
- D1796: a serializer's keys map that is mixed, changed, held or passed is read as the program runs instead of refused, and the serializers' walks are built last (spite_compiler#263).
- D1797: a walk over a type's attributes is refused until reflection lists a type's members, and an error in a function names it (spite_compiler#271, #272).
- D1798: reflection's dictionaries: an object's keys and every `.functions` key unchecked (null when missing), a declaration still indexing an instance's attributes, and a bound attribute's `.owner` being the object (spite_compiler#254, #255).
- D1799: `Test(namespace).run()` runs each test in a run of its own named by `SPITE_TEST`, walks nested namespaces, and its report's words (spite_standard_library#35).
- D1800: the benchmark runner's named settings, the endless-loop rule judging only locals, and refusing `$T.attributes` read as a list on a type (spite_benchmarks#1, spite_compiler#260, #273).
- D1801: an enum reflects through `Spite.Enum` and `Spite.EnumValue` (an enum named as a value and `value.class` answer it, `Part.values` stays the typed list), a symbol key such as `attributes['to']` is refused for text (D1798 superseded there), and the class declaring an enum may add conversions for it (spite_compiler#42).
- D1802: a class's `.enums`, `.unions` and `.types` as name-keyed dictionaries, a union's `.members` keyed by each member's name with namespaces, and `Spite.Type` answering only its names and owner (spite_compiler#237).
- D1803: a crash line names its site as `, site <id>` after the function, the map places each part as `name:Type@line:column`, and a `fallback` of several `and` parts is a site per part (spite_compiler#251, #250, #248).
- D1804: how `Number` goes: the forms typed "any number" are removed with their programs, the library's types are named `Component`, `Element` and `Part`, a whole number no longer fits `Matrix4` or `Quaternion`, and `Float` and `Double` gain `remainder` (spite_compiler#238).
- D1805: a class's functions as `Spite.FunctionDeclaration` with no `call_function()`, a bound function's `.owner` carried only when asked, and a class known at run time running a function through `Spite.Call` on an instance it makes, whose constructor runs (spite_compiler#275).
- D1806: a walk of `Namespace.classes` or `Enum.values` unrolls with a walker of another object, its copies made in that object's class and called on it, the object read once (spite_compiler#23).
- D1807: the first esoteric-number rules (a decimal agreeing with a decimal preset, whole numbers of three or more digits that are no power of ten or two, preset bodies exempt), and named numbers folded while building in the library (spite_compiler#262).
- D1808: what the x64 targets share (small blocks stay per target), the calling convention as a table, D651 superseded, and where the emitter's refusals went (spite_compiler#209, #16, #6).
- D1809: how a number class states its width in `_set_this` and how the compiler reads it, `Boolean` told apart by declaring no `sum`, and a program without the library holding no numbers (spite_compiler#252).
- D1814: a preset as an attribute default, a cast as the target's `set_this` check, `set_this` and `get_this` refused by name in programs and the library calling accessors by name, and a buffer started smaller only where its room is proven unused (spite_compiler#280, #281, #283).
- D1815: the compiler reads the program's Build flags through a private `_flags` of its own `Environment`; a word by position is refused by every program; `spite` runs the program with no flags; an enum setting (spite_compiler#256).
- D1816: `Parallel(...)` and `Concurrent(...)` are an error naming the compiler; programs whose output came from handles are retired, and a server and client in one program run in order, leaving `Http.Client` and `WebSocket.Client` against a same-program server untested until #219 (spite_compiler#224, #239).
- D1817: a `Process`'s `keeps_errors_apart` with `read_errors_as_string()`, its `time_limit` with `run()` answering `null` and `was_stopped`, and its Windows output kept as the bytes written (`\r\n` included) (spite_standard_library#38).
- D1818: back references followed through calls to a fixed point, with keys at most four parts deep, at most 64 rounds, and an object of a recursive reading dropped (spite_compiler#26).

## Open

Settled on 2026-10-29, kept below until removed: a `switch` case naming several values (D1762, D1770), the test runner (D1768), a `Table` (D1766), a generic constrained by a union (D1759), `assert` (D1765, keyword still open in spite_compiler#258), D578 (D1763, D1764), D809 (postponed by D1764 until constants are shaped), `Environment` on a malformed setting (D1760).

- **What a constant is** (raised 2026-10-29 for D809; settled by D1776: a special value of a class is a preset, `Float.Pi()`, and a number that only needs a meaning is a private attribute naming it; the magic-number rule of D809 is open again). Today a number class answers constants through getters read on the class (`Float.pi`), and a program's class has no constant form, so `date.spite` writes `146097` and `719468` bare. Mortaro: "im still not convinced we should have a constant in a class it looks more like glbal PI is a variable that holds a singleton not a static thing which we shouldnt have according to our metaprograming". Shapes offered: a getter on the class generalised, a `const` declaration, an unassigned attribute the compiler folds; none chosen. Open: whether a constant belongs to a class at all, or is a value a singleton holds. Blocks the magic-number rule (D809).

- **A `switch` case that names several values** (raised by D625). `buffer_number_width` as a switch repeats
  `return 1` for `"byte"` and `"tiny"`, one case per value, because a case names one value today. Options: (a) keep
  one value per case (one way, nothing new to learn); (b) a case lists several values, `"byte", "tiny": return 1`,
  which reads like the `or` it replaces but is a second shape of case. Recommendation: (b), since the rule now pushes
  every such `or` into a switch and (a) makes that switch longer than the `if`s were. Blocks only the message text
  of D625, which writes the cases out. D244: a union switch whose members share a body must write `_:` today
  (two equal bodies are refused), so a member added to the union later takes that body with nothing to say so;
  (b) would let the switch name `Circle, Square:` and stay exhaustive.
- **A test runner written in Spite, and an `Executable` class (Mortaro: "this needs bigger discussion", 2026-10-16).**
  Wanted: run a program by path from Spite (`Executable("conformance1.exe").execute("argues")`, shaped like
  `DynamicLibrary`), so the tests are a Spite program that loads the compiler and runs the conformance programs, and
  `check.sh` shrinks. Also wanted: a `test` folder, `spite tests` (which matches none of the command patterns)
  replaced by metaprogramming over specially named functions, `examples/` gone (each becomes a conformance program
  or a piece of documentation), and `conformance/` moved inside the test folder, which also shows a project with
  several entry points sharing code (a game's scene previews: small scenes without the whole engine). Options:
  (a) `Executable(path)` with `run(arguments): Executable.Result` (exit code, output, error output) and a
  `Process` underneath; the test program lives in `test/` as its own entry that `load`s the project and the
  compiler, tests are the functions named `test_...` found by reflection as now, and `spite tests` becomes
  `spite test`, one program run like any other; (b) keep `Process.run` (which exists) and add only a compile-and-run helper `Spite.Compiler` the tests call, so there is no new class;
  (c) as (a), plus entry points as a language feature: a folder may hold several entry files that share the
  project's classes, each named on the command line, which is what `test/`, scene previews and `examples/`
  conversions all need. Recommendation: (c) in two steps: (b) first, because `Process` already exists and the
  conformance runner can move to Spite at once; then `Executable` and the several-entry-points rule (decided on
  its own, since it changes `load`). Open: what the result of an `execute` carries, whether `examples/` programs
  that cache shared files become the test folder's cache example, and how a test folder is told apart from an
  application folder. Blocks: replacing `check.sh` (D580 to D582).
- **A `Table` in the standard library (Mortaro: "i might be completely wrong tho!", 2026-10-16).** The engine
  manages columns and rows by hand; a `Table` of rows and named columns, always used the way the library intends,
  would let the compiler choose its layout with confidence. Options: (a) add `Table` as a class over `Vector`
  columns (each column a typed list, a row a view); (b) add nothing: `List<Row>` already lets the compiler choose
  structure of arrays per use (D402, D485, D520), so a `Table` would be a second spelling; (c) a `Table` only as
  a reflection helper that reads a class's list of rows as columns. Recommendation: (b) first, measured by whether
  the engine's code can say what it means with plain lists, which `design/optimization_research.md` now tracks;
  build (a) only if a case shows the compiler cannot recover the columns.
- **A generic constrained by a union (D175).** Mortaro asks whether it is justified: "one of these classes" as a
  constraint looks like an obscure abstraction. What it would do: `generic $item_type: Cat or Dog` accepts the
  members of a union. What the repository uses: nothing; every constraint in `library/` and the corpus is a
  `type` (`Number`, `Printable`), and the specification already refuses a union there. Options: (a) remove the
  line from `TODO.md` and the idea (a `type` already says "anything with these functions", and a union is a
  closed list of classes, which a `switch` over the union handles); (b) keep the TODO for later; (c) allow
  `Number`-style named unions only. Recommendation: (a).
- **`assert` misleads models (found by the cheap-agent test, 2026-10-16).** Three separate Sonnet agents, working on
  different parts of the compiler, read Spite's `assert` as a check that should halt and reported it as "inert" or
  "does not fire"; in Spite it answers nothing and returns early, and `crash` is the check that halts. Every other
  language uses `assert` for what Spite calls `crash`, so a model's prior points the wrong way. Options: rename the
  guard (for example `require`, `expect` or `guard`), or keep the word and teach it harder (the error texts and the
  skill's first lines). Renaming is a large migration but removes the trap for every future writer.
  A fourth agent added: a guard `assert` that answers nothing prints a `spite.assert` trace line that looks the
  same as a failure, so it chased those lines first; whatever the keyword, the trace of a guard that returned should
  not read like an error.

- **D578, which files count as "scripting".** The shell scripts, the git hooks (git runs them as shell, so they must
  stay shell or call a Spite program) and `bin/spite.cmd`: do the hooks stay as one-line shell files that run a
  Spite program, or is the rule only about the repository's own tools?

- **D809, which bare numbers to refuse (the magic-number rule, suspended).** Mortaro: "a lot of misterious numbers
  happening around that i dont know where they come from". Suggestions, narrowest first:
  1. A literal equal to a standard constant (`3.14159...`, `6.28318...`, `2.71828...`, `9.81`, a type's largest or
     smallest value) is refused, naming the constant to write (`Float.pi`, `Integer.largest`).
  2. A number that appears more than once with the same meaning in one class (the same literal compared, assigned
     or passed in two places) is refused, naming a constant to declare once.
  3. A number in a condition or a loop bound other than 0, 1, -1 and 2 is refused unless it is a named attribute or
     constant; arithmetic factors (`* 2`, `/ 100`) stay allowed.
  4. `--optimization-report` (or a separate report) lists every bare number with its place, so they can be read
     without refusing anything.
  Which of these, if any, and whether it is an error or a fix (D800)?
- **Does `Environment` still halt on a malformed setting after D1721?** (raised by D1724). D1721 says the library
  never halts on what the outside world does, but `spite_spec/specs/programs.md` still has `Environment` crash when
  a program run directly is given a value that is not of the setting's type (`--workers=many`, a bare `--name` for
  a `String`), since a setting is read in the constructor the compiler fills and the program has no answer to
  test. Options: (a) keep the crash: the person started the program wrong and nothing sensible follows (today's
  rule, now also what a bare `--name` for a `String` does); (b) stop with a message naming the setting and the
  form it takes, exit 1, like the snake-case refusal does; (c) keep the default and say so on the error stream.
  Recommendation: (b), a prose stop is what the snake-case case already does and it names the cause to the person
  who typed it. Blocks nothing; D1724 kept (a).
