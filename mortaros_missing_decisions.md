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
D374 (2026-10-01); 264 by D378 and 250 by D379, 252 by D380, 254 by D381, 256 by D383, 257 by D382, 258 by D384, 261 by D385, 253 by D386, 255 by D387, 259 by D388, 260 by D389, 262 by D390, 251 by D391, 263 by D394 (2026-10-02); 266 to 296 by D468 to D488, and 268, 269, 271 and 276 to 279 by D493 to D497, 298 by D498, 301 by D499, 291 by D500, 299 by D501 (2026-10-03); 275 by D503, 302 by D504, 304 by D505 (2026-10-04); 280, 281, 284, 297, 300 and 303 by D510, decided by Claude under D509 and listed under To confirm (2026-10-07). Earlier answers are listed in each row of `design/decisions.md`.

## To confirm

- **D1090 to D1097 (restructure worker):** every stage has one entry class named for its folder whose `run` does the stage, and `spite_compiler.spite` only calls them in order; the outputs stage writes the texts, then runs the generator (D1090); the pass types `Proof`, `HighOptimizationPass` and `LowOptimizationPass`, per-function low passes run over every selected function before the next pass (D1091); a class named for its own folder does not count toward a shared name prefix, a sentence for `specs/classes_and_files.md` (D1092); the proofs judged on the built program run in `proofs/` in a written order, with every lent call recorded in the context (D1093); the proofs that read the builder's scope stay in the builder until it records each site (D1094); one vector-fit test (D1095); `untouched_attributes` is the first high pass, the optimisations chosen while building or sized by the lowering stay until a high pass can build functions and layouts are sized outside the lowering (D1096); a list its setup fills with objects it makes holds them, and a loop calling through its `type` on each is direct calls, which makes each stage's `run` direct calls in order (D1097).
- **D1072 to D1081 (proofs worker):** every proof is its own pass in `proofs/`, acceptance proofs on the built high IR before any optimisation (D1072); a switch over a `T?` covers its value and `Null` or ends with `_:` (D1073); a narrowed attribute read past a call that may assign it is refused, followed through everything the call reaches (D1074); a local narrowed before a loop and assigned a maybe-null inside it is refused unless the body narrows it again (D1075); an attribute assigned by its bare name, a setter store and an index store undo the proofs through them (D1076); `argument_count` of a pattern whose functions disagree is refused (D1077); `fits_vector()` is false for a class using `this` as a value (D1078); `count() > index` is a bound and a bound's left side is any call-free expression (D1079); later `or` sides of a leaving `if` see earlier ones narrowed (D1080); a copy narrowed by a `switch` is refused (D1081).
- **D1051 to D1059 (frames worker):** a call in a loop is inlined up to 96 instructions (400 for a function's only call) when the callee has no loop of its own, and inlining runs again after the first cleanup (D1051); a function value whose address is one known function is called directly (D1052); the class word of an object made in the function is a constant (D1053); copies are read through across the function for registers written once, and values overwritten before any read go (D1054); a 32-bit mask of a `Float` value is a copy (D1055); after vectorising, straight blocks merge, comparisons tested against zero invert and steady constants fold (D1056); `xorps` before turning a number into a decimal (D1057); a decimal constant gives its register to a value that has none (D1058); checked arithmetic whose operands' bounds fit is unchecked after inlining (D1059).
- **D1061 to D1065 (borrows worker):** a call whose whole body writes back an attribute into its list slot is left out after a call on the same receiver that ended reading that slot into it, when nothing between may change any of them (D1061); a singleton's attribute only ever given items of a list it holds, whose items nothing but `append` and that write-back writes, is not counted (D1062), and its default is not made when every read of it is proven to follow a write (D1063); an `append` no longer counts as a write that could free an item a name holds without counting it, except in a list whose objects sit inside its storage (D1064); a list whose items hold rooms of numbers keeps them in one array per number, never whole in its storage (D1065).
- **D1021 to D1024 (vectors worker):** counted loops over numbers run in AVX2 lanes in front of the loop as written, which runs the rest and every halt, with `cpuid` asked once at the start (D1021); whole-number totals kept in lanes, a checked one only when the sum of its terms' sizes proves no partial total overflows, its work thrown away otherwise (D1022); decimals in lanes element-wise always, a decimal total reordered only where every partial total is proven exact (D1023); a pick in an `if` without `else` is one conditional move (D1024).
- **D1034 (owners worker):** an attribute whose object only its holder can ever hold (a class of plain numbers, written only with new objects by the defaults or a constructor, read only for its parts) is laid inside the holder.
- **D1041 to D1047 (folding worker):** after inlining each function is evaluated over all its paths, knowing the objects it makes until something it cannot follow could reach them (D1041); short loops it can count are unrolled, up to 16 passes and 1 024 instructions (D1042); a call passing a known closure gets its own copy of the callee (D1043); a read proven inside a known object cannot fault and goes when unread (D1044); arithmetic by 0 and 1 is a constant or a copy (D1045); a known operand is written as a constant before its instruction (D1046); the evaluation's limits (D1047).
- **D1031 to D1033 (owners worker):** who holds each object is decided while compiling: a function that keeps a parameter at its last read is handed its count by the caller, and a local is handed to what keeps it at its last read (D1031); a slot known to hold nothing (a constructor's trimmed default, the slot past a list's end) is written without reading it, and defaults copies make what they hold with trimmed defaults (D1032); `list[i] = name` after `var name = list[i]` in the same block is left out (D1033).
- **D1011 and D1012 (ranges worker):** a value the kept registers cannot hold lives in a scratch register everywhere but the blocks where it crosses a call, with a store or a load on each edge between the two (D1011); `r9` and `r8` hold values like `r10`, and a checked arithmetic no longer counts as a call, its report copying its numbers to their slots first (D1012).
- **D966 to D969 (optC2 worker):** attributes carry regions that never share memory, so a loop keeps the attributes it updates in registers (D966); a quiet singleton is tested once before its loop (D967); in a threaded program only never-changing loads leave loops (D968); a fresh `List()` over a list nothing else holds clears it in place (D969, not built).
- **D971 to D980 (optD worker):** decimals live in decimal registers (D971); a bound test a signed loop test already proved is dropped (D972); attributes and number items are two kinds of memory, so attribute loads leave loops that write only items (D973); a loop over one object's attributes keeps them in registers (D974); `each_` on a list of a program class runs in bands when every pass writes only its own element, weighed while compiling and checked for shared elements when it runs (D975); unchecked 32-bit sums are one instruction (D976); the loop passes ignore unreachable blocks (D977); a counted loop of one banded pass runs its rounds inside each band (D978); a copy read only later in its own block is its source (D979); D973 and D974 become D966's regions, D977 becomes D964, and a load with a region leaves a loop in a threaded program when no other thread touches its class (D980).
- **D988 and D989 (optE worker):** a counted loop of reads shares its singleton's lock through a count of readers, and changing functions wait for the readers (D988); loop heads on 32-byte boundaries (D989).
- **D981 to D987 (optE worker):** a `clear()` of a list of lists followed by a loop appending new empty lists keeps the lists only it holds, emptied, with their room (D981); a counted loop holding a singleton's lock may take objects of number-only classes without `drop()` from it (D982); a call in a loop passed an object the caller made is inlined up to 128 instructions (D983); an object let go before anything reads it is never made (D984); what a never-changing load answers is not counted where it is read (D985); register allocation by live stretches per block, spilling the least weight per instruction covered, loop uses weighing 32 per depth (D986); common subexpressions shared again after hoisting (D987).
- **D991 to D993 (optF worker):** a local list literal of whole numbers only counted and indexed lives in the frame (D991); a proven `var item = list[index]` is held without counting when its block cannot write the slot (D992); a counted loop holding a singleton's lock may call helpers that call nothing, make nothing and have no loop (D993).
- **D994 and D995 (optF2 worker):** a list filled only with fresh objects of numbers and read back holds the objects inside its storage (D994); a compiler-written copy makes its object without the defaults (D995).
- **D996 and D997 (optF3 worker):** an `if` that only adds or picks a value runs without a branch (D996); an object appended right after it is made is made in its list's slot (D997).
- **D998 (optF4 worker):** a list of fresh objects of whole numbers read only for attributes holds one array per attribute, with no header.
- **D961 to D965 (optC worker):** a singleton the entry class makes is not tested again where the defaults cannot be running (D961); loads that never change move out of loops and survive writes (D962); a store stands for the load that reads it back (D963); unreachable blocks lead nowhere for the passes and the register allocator (D964); a counted loop may call lock-free singletons, and an atomic singleton such a loop holds takes its lock instead (D965).
- **D928, D929 (selfhost worker):** the bootstrap seed is commit `47767ec` and `scripts/bootstrap.sh` builds the compiler from the old one to a fixed point; after the switch the compiler is built only by a self-built compiler, and a new one is kept only when it passes the suite and rebuilds itself byte for byte.
- **D957 to D959 (optB worker):** a proven list item passed as a call's receiver is not counted when the call cannot write that list class's items (D957); a call in a loop tests its callee's early return in place (D958); functions and loop heads are 16-byte aligned (D959).
- **D926, D927 (selfhost worker):** every build writes `<executable>.functions` (each function's start and end), and a native fault in the program's code names its function and callers from it (`spite.function`, `spite.caller`); the seed for the bootstrap is now the `selfhost_fix` branch, and the compiler it writes rebuilds itself to the same bytes.
- **D949 (optA3):** a function answering a singleton class does not count it, nor do its callers let it go.
- **D948 (optA merge):** D943 withdrawn for D951; D942 folded into D953 (now also atomic); D946 folded into D956 and master's skipped-side zeroing.
- **D941 to D947 (optimizer optA):** the lowered program's evaluation runs before inlining too and follows straight-line blocks (D941); object placement reads directly-releasing free functions (D942); a retain and release in one block with nothing between that can let go are dropped (D943); a release counts down in place and only calls `_free_by_class` at zero (D944); a `map_` in the middle of a fused chain (D945); a condition's counted temporaries are let go before its branch, fixing a leak in `crash list[index]` (D946); `list[index] is present` is a bounds test with no read (D947).
- **D951 to D956 (optB worker):** a retain and the release after it with nothing between that can let an object go are both removed (D951); a branch its predecessor decides goes straight to its side (D952); an object placed in registers follows its count while compiling, and a row whose `_free_` releases in place is placed (D953); an attribute holding a quiet singleton is the singleton wherever read, in a program with no `drop()` (D954); making a singleton is a function of its own, the inliner copies addresses, skips stopping blocks and takes calling callees of 24 instructions (D955); a statement's temporaries are let go at its end, a condition's right after it (D956, fixing the D871 leak).
- **D921 to D925 (selfhost worker):** the compiler's repeated-prefix classes move into folders (`Found.Block`, `Assign.Attribute`, `Decimal.Compare`... and `LoweredProgram`); a `return` or bare `crash` ends at its line; the compiler is built from a seed commit the old compiler builds (stack raised to 256 MB), then by itself; a `while` condition's hoisted reads are redone at the end of each pass; the rest of a union after `_:` fits the union.
- **D936 to D938 (nointernal worker):** `internal/` is gone; the fault report, the assert ring, the copy table (`Spite.CopyTable`, renamed in the library) and the REPL prompt are Spite source the compiler holds as text and adds only when needed (D936 to D938); `conformance/internal_load` is deleted, since nothing is left to load. The fault report and the assert ring are not low functions yet.
- **D935 (nointernal worker):** a failed crash's report is moved into a cold function before inlining, so a function holding a `crash` inlines again and the passing path is unchanged; the report and its frame lines read the same.
- **D931 to D934 (nointernal worker):** `Concurrent`, `Parallel`, the scheduler, the handles, the calls-in-a-row table, the `to_debug` cycle guard and the null stand-in are written by the compiler as functions, with no `Spite.Internal` class (D931, D934); the scheduler's halts say what happened and show the program's frames, so three SpiteSpec tests expect new texts (D932); the calls-in-a-row table is sized while compiling (D933). Still Spite classes in `internal/`: asserts, fault and crash reports, copy table, REPL prompt.
- **D913 to D919 (crashes worker, building D908):** the crash line shows the condition's parts, then parameters, locals and attributes, at most 12, text cut at 80 bytes (D913); a narrowing names only the first absent link (D914); a reported call is held in a local, `count()`/`length()`/`is_empty()` are read again, other calls are not reported (D915); every crash ends with `spite.frame` lines of the program's own functions, so every crashing test now expects them (D916), walked through frame pointers and a call-site table (D917); `answered=empty` for a list answer (D918); the map lists every part with its type (D919).
- **D909 and D910 (operators worker):** the library follows D907 too (only an operator's own function calls it by name), a number's `equals` by name is refused (D909); a bare `negate()` inside its own class is written `-this` (D910).
- **D891 to D897 (structs worker):** a `type` crosses into C as its struct in a frame room, C's writes copied back, a list of numbers as its storage, and `T?` numbers do not cross (D891); `C.attributes['name']` as a key of a map by attribute is checked while compiling and is the attribute itself (D892); a serializer's `keys` map is folded and must be written out, the run-time key table is not built (D893); one map per serializer class, set before it is compiled, never changed by index (D894); a folded key is used only while the serializer's own map is not empty, and a null `_as_text` answer halts (D895); `each` over `C.functions`, a filtered local of it, or a known function's `arguments` is unrolled when the walker needs the item, checked by name at run time (D896); `made(argument)` compiled once per known argument (D897).
- **D901 to D906 (formatter worker):** the formatter works on tokens and fixes only layout (D901); every blank line inside a bracket is deleted, as `conformance/blank_line` expects, where `specs/style.md` says at most one (D902); `\r\n` files keep their line breaks and an empty file stays empty (D903); a file is formatted right after it parses, before its rules (D904); the prompt's `has(key)` answered by `contains` (D905); the SpiteSpec runner formats a copy (D906). **No existing error became a fix**: the new compiler had no layout error to turn into one. Errors that have one right form and could become fixes under D800, for you to choose: the declaration order (`declaration_order`, reorder the file), a comma between enum or union entries (`declaration_comma`, delete it), `this.name` (`this_member`, write `name`), `load("x")` (`load_parentheses`), a `<...>` list of codegen values on a function (`generics list`, one `generic` line each), and `Int`/`Bool` spelled `Integer`/`Boolean`.
- **D881 to D887 (reports worker):** the `.crashes` map's lines, crash ids counted apart from assert ids (D881); a bare `--optimization-report` refused (D882); `--debug-memory` counted in the allocator's routines, the line on the error stream (D883); the report lists only written locals and leaves out objects nothing lets go, and `tests/optimization_report` needs its `tests/` filter and true lines (D884); the range of a loop total (D885); a poll that never steps halts at a million (D886); a handle's debug text never waits (D887). The report lists no `library/` or internal places, and writes paths resolved (D888).
- **D861 to D868 (calls worker):** `reflection_specialised_kept` expects line 14, the statement (D861); one singleton per literal argument list (D862); a `Spite.Call` of a declaration named in the code is the direct call, with its error texts, refilling an argument an error (D863); a run-time `Spite.Call` runs a compiler-filled thunk, and halts for a function that takes arguments (D864); calling a run-time `Spite.Class` makes one of the program's own classes as `Anything` (D865); a function value passed as a `Spite.Function` is described in place (D866); a result typed `argument.class` is compiled per specialisation and refused for a run-time object (D867); the run-time-class type error names its function while an unknown type still does not (D868).
- **D878 (analyses worker):** D872 superseded: unread attributes counted by name in the program's own folder, and only singleton bindings checked in a loaded folder, against reads from inside it.
- **D871 to D877 (analyses worker):** a temporary read for one attribute goes at the end of its statement (supersedes D833); the unread check per class for `this.x` and for folders loaded inside the program; a crash decided false once the facts are answered is refused with the place of the write, and file writes wait (`snapshot_argument_writes` expects line 19 where its crash is on 21); the hand-copy error's one-parameter words; a bare `get_at` in a program's own Vector function borrows from `this`; chain fusion for `filter_` steps only; D829 narrowed to a `this` that escapes, so comparing with `this` is allowed.
- **D856 to D858 (regressions worker):** the REPL offers no `map_` for a Boolean member (D856); `memory_replaced_branch` expects its error at the assignment, line 17 (D857); the "calls ... with each" errors only for library callees, the plain type error for a program's own function (D858).
- **D836 to D845 (control worker):** a lone hole in an untyped `var` names `: String`; two abbreviated words name the whole name; the value-switch error texts and lines, and the new style rules not applied to the library yet; repository loads refused before fetching, a valid pin refused until fetching is built; a reopening may not change an attribute's type; a literal too large for a Long is an error; the compiler's own `--build`, `--executable-path` and setting checks; D598 built as the guard written out; an assert answering a collection answers an empty one, codegen tests against unions and types; the number-class, passed-function, shape-operator and folded-constant texts.
- **D816 to D825, the reflection, walk and class errors of the meta round:** the template-walk texts (D816); an enum symbol error lists the values (D817); the 'generics' line shows its own value (D818); the name-prefix rule skips a program's root folder (D819); operator functions by name stay allowed on numbers, `Vector` and `Items` until you pick between `tests/numbers`, `tests/vector_append_copies` and `conformance/functions_direct_sum` (D820); a class test against a non-member is an error (D821); `Enum.values[n]` is `Enum?` (D822); `.is_stateful` counts a held singleton with attributes and `Spite.Class.instances` is in dotted-name order (D823); `type` and `singleton` are names (D824); `namespace` is `Spite.Namespace?` and the round's texts (D825).
- **D826 to D833 (memory worker):** calling `drop()` by name is an error; a `map_` two members could answer is an
  error naming both; the removed flags are refused only when `Build` has no such field; any `this` not read through
  keeps a class out of a `Vector` (so `Date`, `Duration` and `Time` too); a lent item's function is checked after
  every function is built, and library functions are never lent one except a list's own listing calls and a variadic
  parameter; the written-form check skips private attributes; no chosen value for unsigned numbers in the
  default-answer message; a temporary read for one attribute is let go after the call that reads it, not at the end
  of the statement yet.
- **D846 to D851 (foreign worker):** `File.lock()` answers a `LockedFile`, since `File.Lock` cannot be a folder beside
  `file.spite`; `Color(text)` is Color's one constructor, with `hexadecimal`, `read(text): Boolean` and
  `text.to_color(): Color?`, and text naming no colour prints and exits 1 until a crash line shows text; a header's
  prototype casts each foreign argument to its C type's number; the functions of a `DynamicLibrary` attribute are
  looked up when it is set; a JSON number with a fraction reads into a whole number as its whole part; `==` across
  two classes is the mismatch error.
- **D810 to D815, D798's templates as built:** an attribute template's class is the class its body reads the
  parameter on (parameters first; several classes narrow to their common attributes; no read takes the first
  class parameter); a function template's hole is any part of a name, with `p.name` the function and `p.hole` the
  text; `Spite.Argument` is a range type too, its function the call its plural fills; `Namespace.classes.each` and
  `Enum.values.each` unroll while compiling; and `Spite.Attribute`/`Spite.Function` make no template yet. Say if a
  body that reads nothing should be an error instead of taking its first class parameter.
- **D625's details:** a link joined by `or` counts once toward the three `if`s the switch rule needs (so two `if`s
  of three values each are left alone), and each value becomes its own case. Should the count be the values
  instead of the `if`s?
- **D626's details:** the two error texts in [standard_library.md](specs/standard_library.md#bytes-are-a-buffer), and
  a generic class bound to `Byte` reported at the line that binds it, not inside the class.
- **D619, the low IR's details** (`compiler/low_ir.md`): machine types `i8` to `i64`, `f32`, `f64` and `address`, a Boolean an `i8`; SSA virtual registers in flat arrays; a check spelled as an overflow result and a branch to a halt at its site; counts, frame slots and the state machines of waits lowered there; and the C printer reading the low IR, so C and machine code come from the same place. Each is easy to change before the low IR is built.
- **D609, the fixpoint check.** Until Spite has its own backend, `check.sh` keeps requiring generation 2 equal to
  generation 3 byte for byte, since Mortaro's note says binaries need not be identical in the end. Keep it until
  the own backend, or drop it now and rely on the tests passing?
- **D608's details:** `File.lock()` waits, answers `File.Lock?` (`null` only when the path cannot be opened), is let
  go by `unlock()`, by `drop()` and by the system when the process ends; no non-waiting form yet.
- **D611's details:** `.memory` is read-only everywhere and `Memory.Arena` stays a library class the compiler may
  use; a program can no longer pick an allocator, so `Memory.Arena(n)` has no user-facing use until the compiler
  chooses arenas (D485, D501).
- **D613's exemptions:** 0, 1, -1, `0.0` and `1.0` are exempt, and so is the number on the right of the `var` or
  attribute that names it. Should 2, 10 or 100 be exempt too, or any literal in a `test_` function?
- **D605:** every compile passes `-ffp-contract=off`, so floats answer the same in every build and machine,
  at the cost of the C compiler never fusing a multiply-add on its own.
- **D602's details:** `Buffer(size)` gives room for `size` bytes and starts empty (not `size` zero bytes); the
  size is always written (`Buffer(0)` when unknown); `reserve(count)` stays for a buffer that learns its size later.
- **D591, a `T?` passed where a `union` or `type` is wanted is an error**, with the text every other `T?` gets.
  It was silently accepted before. Easy to change: the text, or narrowing `not x or f(x)` on the right side so
  fewer `crash` lines are needed.
- **D589, choices made building the desugared IR**: the namespace name `Intermediate`; hot reload driven by the IR
  (no C reading, unswappable changes refused by name); a store releases what the field held; class objects answer
  retained references; the pipeline switched on only in a test-built compiler until the switch-over. Each is easy
  to change.
- **D566, the programs that test an internal class (decided by Claude Opus 5.5 under D509: (c)).** About 80 conformance programs, diagnostics and benchmarks name
  `Concurrent`, `Parallel`, `Lock`, `Atomic`, `ThreadLocal` or `Memory.Heap` because testing that class (or the lock a
  `Parallel` makes the compiler add) is their point. Once those classes are internal, naming them outside `library/` is
  an error. Which way? (a) Rewrite them to the plain forms, so they test what the compiler does with plain code (calls
  in a row, bands, the cost of a singleton two threads reach) and the internal classes are tested only through it; (b)
  let programs inside the repository's `conformance/`, `diagnostics/` and `benchmarks/` name `Spite.Internal`, which a
  user's program still cannot; (c) a mix: (a) wherever a plain form exists, (b) for the rest until it does. Chosen:
  (c), so the move can start; easy to change, since it only decides which folders may name the namespace.
- **D598's limits:** a chain through a `T?` counts as missing only inside `==` and `!=` (`made.kind == "make"` is
  `false`, `made.kind != "make"` is `true` when `made` is null); `<`, `<=`, `>` and `>=` still refuse a `T?`, and a
  bare `made.kind` outside a comparison still needs narrowing. Easy to widen.
- **`Buffer.text(position, count): String?`**, added while building Buffer: reads that range as UTF-8, `null` when
  it is not valid UTF-8 or runs past the end.
- **D590's details** (your four points are decided; these were chosen while writing the spec): `MappedFile` and
  `ForeignBytes` use the same `_at` names and take a `byte_order` (`ForeignBytes` has no position); `byte` reads only as
  `read_byte()`, the list forms cover the rest; `remaining()`, `read_bytes(count)` and `read_bytes_at(position,
  count): Buffer?` instead of a slice; the order is the enum `Buffer.ByteOrder`, made with `Buffer('big_endian')` and
  settable any time, and a `position` set outside `0` to `count()` halts; only `_at` reads can be proven; the
  binary format stays little-endian whatever the buffer's order and a serializer never moves its `position`. Each is
  easy to change.
- **D586, C written from the high level in every build, with no setting.** A function the high level's printer
  covers gets its C from the high level; everything else stays the generator's, and `check.sh` over the whole corpus
  is the proof. The other choice was a setting to turn it on, which costs a field in every program's C. Easy to
  change.
- **D588, how a fake namespace is told apart**: three or more classes in one folder starting with the same whole
  word, the error text, and the diagnostic's name `prefix_for_a_namespace`. The rule itself is yours. Easy to change
  the threshold.
- **D581, the settings' names: `spite compiler --verify`, and `--verify --release` for the full run.** You suggested
  a flag such as `--test-and-movealong`. Easy to rename.
- **D582, how `--verify` works**: the compiler keeps its own C through `c_source` in its `build.spite`; one more
  generation settles when a change alters the compiler's own C; every program is a child compiler process limited by
  GNU `timeout` (required on `PATH`); progress goes to the error output, the summary to the output; the kept compiler
  is replaced by renaming. Say "confirmed" or give the other answer.

Decided by an agent under D509 (anything that can be changed later). Each is built or documented as decided; say
"confirmed" or give the other answer, and the agent changes it.

- **D568, the error text for naming `Spite.Internal` outside the library**: "`<Name>` is internal to Spite and cannot
  be used outside the standard library: read bytes with List<Byte> or BinaryReader, describe foreign data with a plain
  type, and leave threads to the compiler". It is built and refuses any use, reopening or new class there; the
  namespace holds no class yet. Say "confirmed" or give the wording you want.
- **D562, a write-back of what the slot already holds is not written, across calls**: a call made as a statement
  is left out when the compiler proves, by following values through the calls from every known caller, that it only
  stores into slots what they already hold, checks what was already checked and leaves numbers it can set in its
  place; a branch that may break the proof keeps the call on its own path. Why: the naive engine's runner copies
  each component into a row and stores it back after the system, and that store was 3 to 4 ms of a 20 ms tick.
- **D552, a list held only by another list lives in its slot**: an inner list nothing names but through its slot is
  stored inside the outer list's block, decided per element type for the whole program and refused on any use
  that could keep or alias one. Why: the naive engine's physics grid reads a bucket per query, and the pointer to
  each bucket object was one dependent load too many.
- **D553, a list of lists filled again keeps each list's room**: clearing such a list keeps each slot's block for
  the next empty list put there, so a grid rebuilt every tick allocates nothing after the first. Why: the cost is
  only memory kept past the count, which a list filled the same way every pass uses again.
- **D510, item 280**: a walk's extra values are extra arguments to `each_<member>(...)`. Why: keeps the binary
  format's walker a stateless singleton, no allocation and no state shared between threads.
- **D510, item 281**: a loop that can never leave, and whose calls can neither end the program nor wait, is a
  compile error. No time-based halt, since it would misfire on long computations.
- **D510, item 284**: the callback ticket table stays C until the backend primitives get a home.
- **D510, item 297**: strings, numbers and generic classes stay refused as union members; wrap them in a class.
- **D510, item 300**: `Fraction<$number_type>` names a number's fractional class (`Double` stays `Double`). Why:
  making everything `Float` would silently lose precision for `Double` programs.
- **D510, item 303**: `move_to` never replaces an existing file; delete it first if you mean to.
- **D510, D505's limits**: calls in a row overlap only as bare calls without arguments that reach a loop, with
  objects told apart by class. Widening them is planned work, not a question.
- **D510, D508's run-time branch**: an optimisation may leave one branch between two forms compiled in advance,
  on a fact only the run knows (a list's length). Never a scheduler.
- **D511, the spec wins**: when a docs page and `specs/` disagree, the spec is right and the page is the bug.
- **Which section of values_and_types.md was outdated**: the link had no anchor, so the agent fixing it took the
  "Inline types and duck typing" section (a `type`'s functions taught as dispatched function values, replaced by
  "always mono it") and checked the rest of the page against the compiler. Say if you meant another section.
- **D512, the naive engine's language gaps**: assets that finish in a later frame are the compiler's job (until
  then asset loading keeps `Parallel`); bytes and foreign structs get plain library forms; thread-pinned classes are
  a fact the compiler respects; the engine keeps stages, and commands flush between them.
- **Engine, the waiting-system write rule**: with plain `List<T>` columns, the engine's rule refusing a waiting
  (IO) system that writes `Vector`-fitting components of its rows has lost its reason. Kept for now (it refuses,
  never silently drops); it goes when the compiler arranges waiting itself (D512).
- **D554, a wait in a frame does not hold the frame**: inside a frame loop (a `while` that sleeps on a `Program`,
  itself or through its own class's functions), `receiver.function()` with no arguments, answering nothing, on
  another object of the program, is started as a `Concurrent` when the function can wait. A started call nobody
  waits for still finishes before the program ends; a crash in it halts at once with its own report; at most eight
  per statement are in flight, and the ninth waits for the oldest (holding the frame, never dropping a call). The
  engine's waiting-system rule stays after all: started work writes after the frame has moved on, so a row it
  wrote would be stored stale. Why: the naive engine's `io_systems` advanced 3 frames while its lookups waited
  (main 31); now 17, with the engine unchanged. Say if eight should be another number, or if the frame's own
  calls should be started too (they are its pace, so starting them would make the loop spin). A call where either
  side would write back a value read before a wait is not started (it waits in place and is listed in
  `--optimization-report`); say if you would rather have it be a compile error.
- **D513, plain counts per class**: in a program with threads, a class no other thread can count is counted with
  plain arithmetic; one used by threaded work stays atomic for all of its objects. Why: atomic counts were the
  largest single cost of the naive engine's stress test (105 ms a tick to 73), and the fallback is always atomic.
- **D514, inline releases**: every class's release is a `static inline` count-down with its freeing out of line,
  in every build but the reloadable ones. Why: a release in another C unit was not inlined (73 ms a tick to 66).
- **D515, an item written back to its own slot**: `var item = list[index]`, changed, then `list[index] = item` in
  the same block writes nothing back when nothing between can change the slot, and the read takes no count when
  the same holds to the end of the block. Why: the write-back puts back the object already there; a plain loop
  changing each item of a list runs 2.5 times faster. The engine's runner does not get it (its read and store are
  in different functions), so its numbers do not move.
- **D516, a name read from a list and assigned is an error when nothing reads it after**: the assignment can only
  be a write meant for the list, so it is refused naming `list[index] = value`, or the function of the class
  holding the list that writes it. Why: it compiled and stored nothing (D244); the general dead store ("any
  assignment nothing reads") is a wider rule left for you, since it would refuse code that is merely redundant.
- **D517, a pool per class for objects a list holds**: in a production build, an object of a class the program
  keeps in a list (and no other thread makes or counts) comes from that class's own pool, so a list's objects sit
  side by side. Why: the naive engine's components were interleaved in the C library's heap and every read was a
  cache miss (a stress tick 64 ms to 48, despawning 113 ms to 21, peak memory 96 MB to 67). The cost to confirm: a
  class keeps the memory it once used for its own next objects, never for another class or the system, while the
  program runs. Pooling every class was measured and not kept (one benchmark 4% slower); pooling short-lived
  objects no list holds would make `vector_maths` 1.75 times faster and is listed as not built.
- **D518, a list item read only to test it**: `crash list[index]`, `assert`, `if` and `if not` of an item test the
  slot in place and count nothing. Why: the count up and down was two writes to the item's object for a test that
  only reads the slot (a stress tick 48.1 ms to 43.8). Nothing runs between the read and the test, so it holds in
  every build.
- **D519, an item a name holds from its list**: `var item = list[index]` takes no count while nothing to the end
  of the block can write that list, and `item` may be passed to calls, which take it as held. A call that assigns
  attributes (letting go of other objects) no longer stops it; a store into a list the call is handed, a local
  list or a generic class's list of its own item does. Why: the slot holds the item the whole time, so its count
  is two writes for nothing (a stress tick 43.6 ms to 41.2). The same rule now decides D515's write-back.
- **D521, where the cases live**: `benchmarks/cases/<optimisation>/` (moved up to `benchmarks/<optimisation>/`
  by D524, below), each with `naive.spite`, `naive.c`, `expert.c`, the extracted generated C, and a README; the
  docs page links to each case.
- **D523, a test against a value a list never holds**: a list of numbers or of an enum that only its own class
  fills, and that nothing else can reach, holds only the values its writes can put in, and a `==` or `!=` against
  any other value is decided while compiling (its operand still read). Why: it needs no proof that a "setup" phase
  ends, since the fact holds at every moment, and it is the first step of S1, partial evaluation per configuration
  (a stress tick 39.0 ms to 35.4 as one C file).
- **D524, the layout after the Spite-only benchmarks go**: the cases move up to `benchmarks/<case>/` (no `cases/`
  level), and the `versus_c` programs become cases with an expert C each.
- **D525, the file names**: `generated.c` is the whole generated C, `highlights.c` the excerpt that shows the
  optimisation.
- **D528, values in the caller's frame**: a function value the callee only calls is a struct in the caller's frame,
  and a variadic list the callee only reads is framed for a `var`, an assignment or a `return` as for a statement.
  Why: the plain loop `apply(scorer.score, index)` made two objects per call and took 66.8 times naive C's time
  (0.65 now), and `var biggest = largest(a, b, c)` 31.6 times (1.01 now).
- **D529, no lock for a singleton no other thread reaches**: after the program is written out, the walk that
  decides plain counts decides locks again, and a singleton no code on another thread names keeps no lock and no
  atomic attribute. Why: the walk made while compiling counts every function made into a value as one a thread
  might run, so a `filter(matches)` locked a whole engine's matchers and columns while only a recipe loader ran on
  the pool (a stress tick 43.3 ms to 36.9 as one C file). The count of locks held is kept, so nothing a program
  does changes.
- **D530, a singleton's lock backs off**: a thread that finds a singleton's lock taken pauses, doubling up to 1 024
  pauses, then yields to the system between looks. Why: under four threads the plain compare-and-swap loop took
  113 ms against 21.5 for C behind a critical section; backing off takes 7.7, faster than a system lock at every
  thread count measured (1, 2, 4, 8).
- **D532, object keys**: an object key is keyed by identity; a class with its own `equals` used as a key is a
  compile error until keying by `equals` is built; `Boolean` keys are allowed.
- **D531, an attribute a call cannot assign is passed without counting**: `total_of(amounts)` or
  `discount_of(customer.terms)` passes the attribute held, as a local already was, when nothing the call reaches
  assigns any attribute along the path. Why: each such argument was a count up and a count down on an object the
  attribute kept alive anyway (a stress tick 36.0 ms to 30.5; the case 28.4 ms to 5.4, as fast as its naive C).
- **D534, the profile**: a text file beside the entry file, committed with the program so builds are
  reproducible; `--optimization-report` names every choice it made; the profiling flag's name is provisional.
- **D536, plain bytes and plain foreign structs (items 305 to 315), confirm after the speed goal**: Mortaro chose
  `List<Byte>` for bytes and asked Claude to pick the rest until the compiler work reaches the goal, then ask. Each
  was taken as the proposal recommends ([proposal](design/proposals/plain_bytes_and_foreign_structs.md#questions-for-mortaro)):
  305 a fresh list per read; 306 `read_integer_big_endian` twins; 307 both `BinaryReader` and number reads;
  308 `ForeignBytes(address, count)`; 309 `type` inline, `type?` a pointer; 310 a one-attribute `type` for values C
  writes back; 311 `List<T>(32)` and `String(256)` only in a `type` that crosses into C; 312 a held-list class when a
  binding needs it; 313 Booleans and enums 32 bits in a struct; 314 address forms leave programs now; 315 a
  `<list>_count` attribute is written by the compiler.
- **D537, two small calls, confirm after the speed goal**: (a) a walk that hashes or prints a class's description,
  such as `BinaryWriter<T>.schema()`, counts as reading those attributes, since they are the wire format; (b) of the
  two ways to ask whether a class's objects can be stored inline, `is_fixed_size` stays (it says what the class is,
  not which container holds it, D520) and `fits_vector()` goes.
- **D538, an item used at once is not counted**: `plays[at].count()`, `tracks[first].seconds` and
  `length_of(tracks[first])` read the item in its slot with no count, the call only where it cannot change the
  list. Why: the list holds the item through the use, so the count was two writes for nothing (a stress tick 27.3
  ms to 25.9).
- **D539, a loop over a list of different classes runs them at once**: `systems.each_update()` over a list of a
  `type` runs its elements on the pool when the table of classes decided while compiling allows every two of them,
  with the classes read when the loop starts (a branch between two compiled forms), instead of the unrolling pair
  T4b proposed. Why: the naive engine builds its stages at run time from strings, so their order is not known while
  compiling; the table needs no order (the case's three voices 257 ms to 91).
- **D540, the bytes half of D536 as built, confirm after the speed goal**: what Claude chose beyond the proposal.
  (a) `File.read_bytes()` answers `null` when fewer bytes arrive than the size said; `read_bytes_at` reads up to
  `count` and halts on a negative position or count. (b) A `Socket` keeps one 64 KB receive block and copies what
  arrived into a fresh list per read (the compiler does not reuse a let-go list's block yet). (c) The library keeps
  `read_bytes_into`, `read_bytes_now_into`, `write_bytes_from`, `write_bytes_now_from` for HTTP and WebSocket, refused
  in a program like the old forms; `BinaryReader.read_memory` is removed, not kept. (d) Beyond the proposal's list:
  `tiny` and `unsigned_long` reads, and `_big_endian` twins of the appends and writes too. (e) `ForeignBytes(address,
  count)` reads as well as writes, has `get_at`/`set_at`, `read_bytes`, `write_bytes`, `count(): Long`, and takes the
  address as a `Long`. (f) New proofs: `base + k <= list.count()`; non-negative positions; `rows * stride <=
  list.count()` for `row * stride + column`; a counted loop stepping by up to its window plus one. (g) A proven read
  outside a counted loop keeps one compare, because a local alias of a list is not tracked (a stale read would be a
  silent wrong value); only counted loops get the bare load.
- **D541, how `Dictionary<Key, Value>` keys behave** (built for D532): a fraction key is found by value, so
  `0.0` and `-0.0` are one key and every `not_a_number` is one key; a `Boolean` and an enum value are keys by value;
  an untyped literal is keyed `String` or `Integer` by its first key; a key is cast to the key type as any argument
  is, so a number given to a `Dictionary<String, V>` becomes its text. Why: each answers the same as the value's
  `==` would, but for `not_a_number`, where a key that can never be found again would be a silent loss (D244).
- **D541, what cannot be a key**: a class that declares `equals` (until keying by `equals` is built), a
  nullable, a union, a shape, a function value, a `Symbol` and a value class are compile errors. Why: each would
  either miss an equal key or has no identity to key by; every one can be allowed later without breaking a program.
- **D541, writing keyed dictionaries out**: JSON writes an enum key by its name and refuses keys other than a
  `String`, a whole number or an enum; no format writes an object key; the binary schema text keeps its old
  spelling, so binary files written before still read. Why: a JSON key is text, and an object's identity does not
  survive a file.
- **D542, a loop whose passes write only their own item runs in bands**: `orbits.each_advance()` over a `List` or
  `Vector` of a class runs in bands on the pool when every pass writes only its own element and counts no
  reference, the pass weighs at least 256 and the count times the weight reaches a million; a `List` is also
  checked for an element held twice. Why: the passes are independent by proof, and the two numbers were measured
  on this machine so light passes (bound by memory) and short lists stay in order (the case 800 ms to 51).
- **D543, ranges drop overflow checks**: the compiler works out the range of every whole-number local (from
  literals, assignments, remainders, `clamp`, counts and the conditions in force, widened at each loop's entry) and
  leaves out a check its ranges prove can never fire, such as a `Long` total of `Integer` items over a list; every
  check that stays is listed in `--optimization-report` with the ranges it found. Why: D360 asks for each proven
  check to go, and the proof is made entirely while compiling. Attributes and list items keep their type's range
  for now (another function could change them).
- **D544, a store counts only when it changes the slot**: `crates[at] = crate` from a name the function holds
  compares the slot with the new object and counts only when they differ. Why: a component written back to the slot
  it came from paid four counts on one object (a stress tick 38.0 ms to 33.3 on a busy machine). It is a test at run
  time, since only the run knows whether the slot holds the object; proving it while compiling (pair B2b) is not
  built. The cost to confirm: an index outside the list is reported as the read `crates[at]` rather than by the
  list's `set_at`.
- **D545, `Benchmark(work)` instead of `clock.benchmark(work)`**: `var result = Benchmark(do_something)` runs
  `do_something` once between two clock readings; `result.answer` is what it answered and `result.duration` a
  `Duration`. Why a class: Spite has no generic functions, so only a class made from the work can carry the
  answer's type (`Benchmark<Long>`, inferred from the function). The other answers: generic functions (a language
  change, yours to decide), or a `clock.benchmark(work)` taking only work that answers nothing and returning just
  the `Duration`, with the work storing its results in attributes. The cost to confirm: the function value is made
  on the heap with its reflection (a constructor of a library class gets no framed value yet), outside the
  measured time; the call through it is one pointer. In a program that starts a `Parallel`, the work counts as
  code another thread may run (values are matched to a thread's calls by argument count), so five cases whose point
  is a lock or a plain count left out keep their two clock readings (`TODO.md`).
- **D546, the program ends after its `Concurrent`s**: when the entry function returns, every `Concurrent` still
  running runs to its end before singletons are destroyed. Why: a singleton holding a running handle let it go
  after the scheduler was destroyed, reading freed memory and halting with the wrong cause. The cost to confirm: a
  `Concurrent` that never ends (a server loop kept by a singleton) keeps the program from ending once the entry
  function returns, where before its drop at exit waited on a destroyed scheduler.
- **D551, a table filled once is read as constants**: a list an object's setup fills once (its constructor, or a
  function that starts `assert not prepared`) gets, for each function that reads it, a copy reading it as a constant
  list, one per combination of the values it can hold (at most four), and a test at the call that picks the copy
  when the object's list holds those values. Why: tables a program fills once (a checkout's steps, a matcher's kinds)
  fold into straight code. The cost to confirm: the C and the executable grow (the naive engine's by 20% and 13%,
  for about 7% of its tick, measured while the machine was in other use), since combinations the program never makes are copied too.
- **D572, an attribute read only under its singleton's lock is a plain number**: a locked singleton's attribute
  stays atomic only when some other class reads it directly. Why: nothing else reads it without the lock, so the
  atomic additions bought nothing (76.6 to 24.4 ms on the counted loop's case). The cost to confirm: none at run
  time; one read from another class anywhere in the program makes every lone atomic attribute of that singleton
  atomic again.
- **D573, a loop of calls to one singleton tests for tasks once**: a loop that would take its singleton's lock once
  but is not counted (its index is what a call answers) asks once whether a task is in flight and, when none is,
  calls the unlocked bodies for the whole loop. Why: the test, push and pop per call were most of a call (41.5 to
  9.5 ms for ten million). The cost to confirm: a loop that starts while a task is in flight takes the lock on every
  call even after the task ends, as before; the loop is written once with both calls in it.
- **D555, counts per group, and the despawn cost**: in a loop over different classes that may run them at once, a
  class only one of the calls counts keeps plain counts and its pool, and one two of them count is atomic only
  while they run. The stress update stage went from about 28 to 15 ms, but the sixty ticks after despawning
  everything got about a tenth slower (caches on the other core, and a pool start per empty tick), which D214
  would refuse; kept because the tick is the number that matters. Say if it should wait for a cost model.
- **D556, objects made for their owner are told apart**: an attribute only ever given an object constructed
  where it is given holds an object no other such attribute holds, so two calls that each keep their own meter,
  timing record or scratch list may run at once; an item of one singleton's list and an item of another's are told
  apart by comparing the two lists when the loop starts. Why: it is what keeps the naive engine's two stress
  systems apart, with no annotation.
- **D567, no hand copy of a library operator in a reopening**: a program's reopening of a library class (`Matrix4`)
  may not add a function that rewrites the object's attributes from parameters of the same class when the library
  class already has the operator (`set_product(left, right)` beside `*`). Why: it competes with the compiler's own
  form of the answer (D564). Narrow: operators only (`+ - * / %` and negation), same parameter types, and a
  function that assigns attributes; a copy of a plain library function is not caught. Say if it should reach plain
  functions too.
- **D570, every class made on one thread has a pool**: not only the classes a list holds, but a container's own
  object (a `List`, a `Vector`) only when a list holds it. Why: a chain made and dropped over and over went from 81
  to 17 ms, with no allocation per link after the first round. The cost to confirm: memory a class used stays that
  class's, now for every class, and a program that only makes one object of a class takes a run of 16 for it.
- **D571, a deep copy nothing changes is the original**: per copy site, when nothing writes the copy or the
  original while the copy lives and nothing in the program asks a copied class for its identity, the copy is the
  original counted once more. Why: a program that copies to read (a report over a snapshot) makes nothing; the
  deep copy case went from 49.5 to 2.1 ms. The cost to confirm: `--debug-memory` counts no allocation for such a
  copy, and the walk that proves it refuses a lot (any call whose body it cannot follow keeps the copy).
- **D583, the backend's Windows executable writer takes the imports and the code, and answers where an import
  lands before the code exists**: one section for the code at 4096 and one for the imports at 8192, image base
  5368709120, no relocations and no dynamic base, the imports reached by a `lea` whose offset the caller takes
  from `address_of_import` and `code_address`, and nothing laid out for a person to read (D561). The alternative
  was to place the imports after the code and patch the call itself, which needs a relocation table before there
  is a linker to hold one. Why: an import's place must not move when the code grows, or every call written before
  it would point somewhere else. The layout is in
  [backend_windows_executable_format.md](design/backend_windows_executable_format.md), and the writer's
  acceptance, an executable that calls `ExitProcess(42)` and exits with 42, runs in `check.sh` on Windows alone.
- **D584, the high level comparison is decided one kind of fact at a time**: reads and writes when every key the C
  reader gives is a field of a class with no owner tag on it, calls when every call on either side is a function one
  of the program's own classes declares, loops when the function makes no call into the library. Why: while the
  generator decides a thing, the two walks describe different programs, so the comparison for that thing can only
  wait. The cost to confirm: a kind that is not compared is reported with the key or the call that stopped it, and
  nothing is left out quietly; say if a kind should be compared anyway, naming what the generator decided.
- **D585, every setting of `Build` is a field of every program's C**: asking a compiler-only question of a program
  (`--high-level-report`, `--optimization-report`) costs one field in every program, so all 102 benchmark
  `generated.c` moved by one line when IR-1's option was added. Why: a flag is only accepted when the target
  program's `Build` declares it, and a declared setting is a field. The cost to confirm: the regenerated benchmark
  C and the updated `unknown_compiler_flag` error, and whether the settings should stop being fields as part of
  D578.
- **D616, where the executable writers and the encoder live**: `compiler/source/backend/executable_writing/` for the
  writers (one folder, the class names unchanged) and `compiler/source/backend/x86/` for the encoder and its operand
  classes, the namespace `X86`. Why: a `load` reads every file of the folder it names, and the high level IR walk
  beside the writers names `Analysis.*`, `Syntax.*` and `Generation.*`, which only the compiler loads, so M0's program
  could not be built (it had never run); and D588 wants no name prefix standing for a namespace. The alternative was to
  move `backend/high_*.spite` into `backend/high/` first (the fix D588's own text suggests, and the TODO line names),
  which touches the IR work in other branches. Say if you want the IR moved instead and the writers back at
  `compiler/source/backend/windows_executable_writer.spite`.
- **D617, the encoder's decisions**: the operand kinds and the refused forms (`compiler/source/backend/x86/`), one
  function per instruction family that switches on its operands' kinds, a whole number register at 32 or 64 bits and an
  SSE register at 128, a place carrying its width only where an instruction needs one of its own, `ud2` as
  `illegal_instruction`, no accumulator short forms, and a `Float` or `Double` from a number refused because no
  assembler here takes one. The table is 165 rows in `backend/x86_encoding`. Easy to change: the forms are one function
  each, and adding a form never changes another.
- **`check.sh` runs the backend programs**: `backend/hand_built_executable` and `backend/x86_encoding` run with
  `spite backend/<folder>` and nothing in `check.sh` runs them yet (D558, D583), so a green run says nothing about the
  backend's own programs. The step belongs on Windows alone, since M0's program writes a Windows executable.

## Open

- **A `switch` case that names several values** (raised by D625). `buffer_number_width` as a switch repeats
  `return 1` for `"byte"` and `"tiny"`, one case per value, because a case names one value today. Options: (a) keep
  one value per case (one way, nothing new to learn); (b) a case lists several values, `"byte", "tiny": return 1`,
  which reads like the `or` it replaces but is a second shape of case. Recommendation: (b), since the rule now pushes
  every such `or` into a switch and (a) makes that switch longer than the `if`s were. Blocks only the message text
  of D625, which writes the cases out.
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

