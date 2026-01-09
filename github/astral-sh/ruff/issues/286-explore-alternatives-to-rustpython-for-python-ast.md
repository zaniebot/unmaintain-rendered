---
number: 286
title: Explore alternatives to RustPython for Python AST parsing
type: issue
state: closed
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2022-09-29T19:38:15Z
updated_at: 2023-05-18T15:55:44Z
url: https://github.com/astral-sh/ruff/issues/286
synced_at: 2026-01-07T13:12:14-06:00
---

# Explore alternatives to RustPython for Python AST parsing

---

_Issue opened by @charliermarsh on 2022-09-29 19:38_

RustPython has been a good foundation, but Ruff is currently limited by what the RustPython parser does and does not support. See: #282, #54, #245.

Since we _only_ need the parser (and RustPython is more ambitious -- they're trying to build an entire runtime / interpreter), there may be better options. RustPython is also going to be limited by their use of an LL(1) parser.

We could consider using the following:

- https://github.com/tree-sitter/tree-sitter-python
- https://github.com/pest-parser/pest
- https://github.com/kevinmehall/rust-peg


---

_Label `enhancement` added by @charliermarsh on 2022-09-29 19:38_

---

_Comment by @charliermarsh on 2022-09-30 13:01_

Alternatively, could we use the CPython AST parser directly? It's written in C, AFAIK it should be very fast (perhaps even faster than the RustPython parser).

---

_Comment by @charliermarsh on 2022-09-30 13:34_

_Alternatively_, we could try to continue merging improvements back to RustPython.

---

_Comment by @charliermarsh on 2022-09-30 19:50_

_Alternatively_, we could revisit the use of LibCST.

---

_Comment by @charliermarsh on 2022-09-30 22:18_

LibCST will also likely be _required_ for auto-formatting.


---

_Comment by @charliermarsh on 2022-10-01 21:02_

tree-sitter is an interesting option (demo in #295). It produces a CST, so we could use it to power auto-formatting. It's slower than RustPython but still quite fast (~350ms to iterate over the CPython codebase vs. ~230ms with RustPython). Because it's based on S-expressions, it has a whole [pattern-matching syntax built-in](https://tree-sitter.github.io/tree-sitter/using-parsers#pattern-matching-with-queries), which we could use to power plugins...

I think the ergonomics on our end won't be great, because it just exposes a single Node type with a `kind: &str` field, and so we'd have to do all pattern-matching on string identifiers rather than encoding the AST in a type system. It also may not map 1:1 with the [abstract grammar](https://docs.python.org/3/library/ast.html), I'm not certain.

Going to explore a few other options, but I'm intrigued...


---

_Comment by @charliermarsh on 2022-10-03 21:26_

There's also [rust-python-parser](https://github.com/ProgVal/rust-python-parser), which is based on [nom](https://github.com/Geal/nom).

---

_Comment by @charliermarsh on 2022-10-03 22:27_

There's also [rust-sitter](https://github.com/hydro-project/rust-sitter), which sits on top of tree-sitter and lets you define the grammar on the Rust side via macros. In return, you get semantically meaningful structs when you generate the parse tree. The downside is that we'd have to re-write the Python grammar ourselves.

---

_Comment by @charliermarsh on 2022-10-03 22:28_

If we were to use tree-sitter, we _may_ want to write code to transform the generated tree into AST types on the Rust side, similar to the `RustPython` AST. (That would have to be closer to a CST, though.)


---

_Comment by @charliermarsh on 2022-10-05 19:54_

[rust-python-parser](https://github.com/ProgVal/rust-python-parser) takes ~480ms (vs. 280ms for RustPython's parser) and fails on these files:

```
resources/test/cpython/Lib/_compression.py
resources/test/cpython/Lib/ast.py
resources/test/cpython/Lib/contextlib.py
resources/test/cpython/Lib/distutils/tests/test_sysconfig.py
resources/test/cpython/Lib/idlelib/colorizer.py
resources/test/cpython/Lib/idlelib/idle_test/mock_tk.py
resources/test/cpython/Lib/idlelib/pyparse.py
resources/test/cpython/Lib/idlelib/replace.py
resources/test/cpython/Lib/idlelib/run.py
resources/test/cpython/Lib/importlib/_bootstrap.py
resources/test/cpython/Lib/lib2to3/tests/data/py3_test_grammar.py
resources/test/cpython/Lib/linecache.py
resources/test/cpython/Lib/logging/__init__.py
resources/test/cpython/Lib/test/coding20731.py
resources/test/cpython/Lib/test/test_asyncio/test_locks.py
resources/test/cpython/Lib/test/test_asyncio/test_pep492.py
resources/test/cpython/Lib/test/test_asyncio/test_server.py
resources/test/cpython/Lib/test/test_contextlib_async.py
resources/test/cpython/Lib/test/test_coroutines.py
resources/test/cpython/Lib/test/test_dis.py
resources/test/cpython/Lib/test/test_format.py
resources/test/cpython/Lib/test/test_pkgutil.py
resources/test/cpython/Lib/test/test_sys_settrace.py
resources/test/cpython/Lib/test/test_types.py
resources/test/cpython/Lib/test/test_typing.py
resources/test/cpython/Lib/unittest/test/testmock/testasync.py
resources/test/cpython/Lib/zoneinfo/_zoneinfo.py
resources/test/cpython/Tools/c-analyzer/c_parser/preprocessor/__init__.py
resources/test/cpython/Tools/scripts/stable_abi.py
resources/test/cpython/Tools/unicode/makeunicodedata.py
```

---

_Renamed from "Replace RustPython for Python AST parsing" to "Explore alternatives to RustPython for Python AST parsing" by @charliermarsh on 2022-10-18 21:18_

---

_Referenced in [astral-sh/ruff#474](../../astral-sh/ruff/issues/474.md) on 2022-10-26 16:08_

---

_Referenced in [ast-grep/ast-grep#80](../../ast-grep/ast-grep/issues/80.md) on 2022-10-30 13:34_

---

_Referenced in [astral-sh/ruff#847](../../astral-sh/ruff/issues/847.md) on 2022-11-21 13:05_

---

_Comment by @charliermarsh on 2022-11-22 01:37_

Ok, time for me to revive this thread and start thinking on the right long-term parser strategy. We need to support the 3.10 constructs!

---

_Comment by @charliermarsh on 2022-11-22 02:32_

I'm strongly considering writing a new parser atop [rust-peg](https://github.com/kevinmehall/rust-peg) or [nom](https://github.com/Geal/nom) that adheres to the RustPython AST interface.


---

_Comment by @charliermarsh on 2022-11-22 03:33_

@Seamooo - Do you have any thoughts on or interest in working on this too?

---

_Comment by @Seamooo on 2022-11-22 04:29_

Definitely have some interest in this area. A couple of comments as I've been going down various rabbit holes of python parser implementations:
- The parser should be built from the CPython PEG, directly transpiled into either an equivalent grammar or used via proc_macro to generate the parser
  - Run into a lot of grammar specifications with weird FIXMEs about multiple edge cases because their grammar attempted to be a superset of CPython's, that didn't quite reduce down to the same grammar.
- proc_macro gives the ability to generate a perfect hash function for a set of keywords at compile time
- rust-peg seems to be an excellent compromise between exploiting the lower-bound time complexity for Packrat parsers without explicitly requiring caching every match, allowing multiple matches rather than lookups for manual performance tuning
- rust-peg also has the necessary extensions to the formalised PEG grammar such that implementing the python grammar is possible

---

_Comment by @Seamooo on 2022-11-22 04:32_

Ideally when https://github.com/charliermarsh/ruff/pull/742 becomes mergeable, implementing those traits for the IR output by the parser would make it immediately useable.

---

_Comment by @charliermarsh on 2022-11-22 04:52_

Yeah rust-peg does look good, and I believe that's what LibCST uses. My only hesitation there is that the LibCST parser is really slow compared to the RustPython parser? But I don't know how much, if any, of that is due to rust-peg. This is just based on the fixtures in the LibCST codebase:

```
rust_python_parse/all   time:   [289.30 µs 289.96 µs 290.64 µs]
                        change: [-4.8537% -4.5331% -4.2286%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 23 outliers among 100 measurements (23.00%)
  11 (11.00%) low mild
  2 (2.00%) high mild
  10 (10.00%) high severe

parse/all               time:   [1.7676 ms 1.7953 ms 1.8408 ms]
                        change: [-7.9448% -7.2284% -6.1601%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 10 outliers among 100 measurements (10.00%)
  7 (7.00%) high mild
  3 (3.00%) high severe

parse_into_cst/all      time:   [2.2262 ms 2.2477 ms 2.2723 ms]
                        change: [-2.7483% -0.8482% +0.8620%] (p = 0.40 > 0.05)
                        No change in performance detected.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) high mild
  1 (1.00%) high severe
```

(`rust_python_parse/all` is the RustPython parser, `parse/all` is the LibCST parser without inflation, and `parse_into_cst/all` is the LibCST parser with inflation.)


---

_Comment by @charliermarsh on 2022-11-22 04:52_

(And agreed on #742.)

---

_Comment by @charliermarsh on 2022-11-22 15:52_

Perhaps we could extend `pegen` with a Rust target based on `rust-peg`.

---

_Comment by @isidentical on 2022-11-22 17:53_

CC: @davidhalter (I think you were working on a Rust-based Fast Python Parser [?])

---

_Comment by @charliermarsh on 2022-11-22 19:15_

I'm going to start playing around with a straight port of pegen to Rust. (Or, more specifically, modifying pegen to output a Rust target, which IIUC will also require rewriting the Python code from `data/python.gram` in Rust.)

---

_Comment by @davidhalter on 2022-11-22 23:26_

Thanks for bringing this up @isidentical.

I have indeed written a Python parser in Rust. But I do not feel comfortable sharing it yet. While it's faster than any other Python parser I have seen, it's a bit rough around error recovery and few other things, but generally parses all valid 3.10 Python programs (AFAIK). So if I ever release it, I'm happy to let you guys know.

---

_Comment by @charliermarsh on 2022-11-22 23:43_

Thanks @davidhalter! Super cool. Would love to see it if you ever release it.

If you don't mind sharing: did you write it from scratch? Or is it built atop a parser generator?

---

_Comment by @charliermarsh on 2022-11-23 04:53_

(I started on this in earnest tonight, I don't know if it will prove to be the right approach but the lower bound is that I learn a lot.)

---

_Comment by @davidhalter on 2022-11-23 18:11_

@charliermarsh Yes I wrote it from scratch, it is a kind of weird mixture of LL and PEG, it's also a parser generator. I essentially pass it a slightly modified version of the official EBNF grammar.

I personally love writing parser generators, so when the opportunity presented itself, I took it :)

---

_Comment by @charliermarsh on 2022-11-26 15:03_

@Seamooo - Do you have any interest in collaborating w/ me on the Rust port of pegen? It's a private repo right now but would be happy to add you and talk about how I'm iterating on it.

---

_Comment by @Seamooo on 2022-11-27 03:00_

Definitely would be interested

---

_Comment by @charliermarsh on 2022-11-27 03:34_

@Seamooo - Added you, repo is rough but your help is very welcome!

---

_Comment by @ljodal on 2022-12-03 19:25_

I'd love (read) access as well, if possible. I don't think I'm much help in contributing, but I've been looking at `rust-peg` to generate a Python-compatible ast. My rust knowledge isn't there yet though, so I'm finding it a bit hard to get going (I have a working POC with a few manually defined nodes, but I don't think that'll tell us anything useful about the performance hit).

---

_Comment by @charliermarsh on 2022-12-03 19:30_

@ljodal - Added! Would love to have any help / feedback.

(Anyone is welcome to be added, I'm just avoiding making it totally public while it's still in such an incomplete state.)


---

_Referenced in [RustPython/RustPython#4329](../../RustPython/RustPython/pulls/4329.md) on 2022-12-12 07:49_

---

_Referenced in [RustPython/RustPython#2759](../../RustPython/RustPython/issues/2759.md) on 2022-12-12 07:53_

---

_Comment by @DimitrisJim on 2022-12-12 12:22_

> Anyone is welcome to be added

I'd like to take you up on that invitation if the attempt is still ongoing :-). We've recently been thinking of doing the same with RustPython's parser, porting `pegen` would just reduce the differences between grammar files between it and CPython, making syncing with it much easier. Not sure how much time I have lying around but I'd help as I could.

---

_Comment by @charliermarsh on 2022-12-12 13:42_

@DimitrisJim - Added! You'll be able to tell from the contribution dates, but I haven't been able to push on it as much as I'd like lately (just other Ruff work taking priority). Any / all help or feedback welcome.

While you're here: to be clear, my preference would be to continue using the RustPython parser :) The goal of this task is, implicitly, to remove the parser as the bottleneck for what Ruff can support, i.e., to get us to a point where the parser can support all current language features.


---

_Comment by @fanninpm on 2022-12-12 21:26_

> Rust port of pegen

I was thinking of adding a `rust_generator.py` file to CPython's copy of pegen, but I didn't quite know where to begin.

---

_Comment by @charliermarsh on 2022-12-12 21:28_

@fanninpm - I added you to my `rust-pegen` project. Feel free to collaborate there or use it as knowledge / inspiration or whatever is most useful. It adds a `rust_generator.py`.

---

_Comment by @charliermarsh on 2022-12-12 21:29_

There's also a PEG grammar (https://github.com/charliermarsh/rust-pegen/blob/main/pegen/data/rust.gram) (has to differ from the Python grammar because the PEG grammars include actual code). The generated code is [here](https://github.com/charliermarsh/rust-pegen/blob/main/pegen/data/rust_parser.rs).

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:21_

---

_Label `core` added by @charliermarsh on 2022-12-31 18:21_

---

_Referenced in [RustPython/RustPython#4423](../../RustPython/RustPython/pulls/4423.md) on 2023-01-05 21:59_

---

_Comment by @fanninpm on 2023-01-06 12:12_

@qingshi163 what do you think?

---

_Comment by @charliermarsh on 2023-01-06 12:42_

As an update: my current hope here is to continue to use RustPython, especially since there's been a lot of good progress in the parser recently, although structural pattern matching in some form is still the biggest hurdle.


---

_Comment by @LucianU on 2023-01-27 07:29_

What would be the downside to using the CPython AST parser directly, since that is guaranteed to always be up-to-date?

---

_Comment by @flying-sheep on 2023-01-31 18:33_

The big one I think is that CPython’s AST doesn’t seem to have all the formatting information (whitespace, comments) stored. That’s why CST parsers exist, like RustPython’s and [LibCST](https://github.com/Instagram/LibCST) (which is written in Rust, but as said [above](https://github.com/charliermarsh/ruff/issues/286#issuecomment-1323061200), seems to be slow). I’m pretty sure Ruff needs this information to operate on comments and to do autofixes without mangling comments and whitespace. (Actually, I think the old Parser created an intermediate CST, but the new one doesn’t anymore)

If a well maintained, fast CST library written in C existed, and we rephrased the question to “why not use that C library”, I think there would be two ways to do it, each with their downside:

1. When directly using its data structures, it would be difficult to safely manipulate on the CST (necessary for auto fixes).
2. When converting its data structures to Rust, there’s Rust code to maintain again (not a big problem, the data structures shouldn’t change too much), and there’s a performance hit (probably not too big either)


---

_Comment by @charliermarsh on 2023-01-31 19:06_

Interestingly, RustPython's AST doesn't include any CST-like information -- it's very similar to the CPython AST. We get comments and other non-AST tokens from the token stream directly (comments are included in the token stream, but _not_ in the AST).

---

_Comment by @charliermarsh on 2023-01-31 19:10_

To me the downsides to using the CPython AST parser are as follows:

1. We need to figure out how to extract it from CPython as a useable standalone module (presumedly solvable but not something I've really investigated).
2. We need to figure out how to do the FFI between Rust and C (also presumedly solvable, but also not my strength).
3. It makes it much more difficult for us to ever use a specialized parser or AST representation for Ruff. RustPython doesn't necessarily solve this either, but I suspect that Ruff could benefit a lot from having its own parser that was written specifically Ruff (and similar use-cases) -- perhaps something that _does_ produce a CST, or something that's optimized for the kinds of operations we perform.


---

_Comment by @LucianU on 2023-02-01 08:39_

So, as far as I understand, there are several things here. Ruff needs a CST representation of Python.
CPython doesn't have one. RustPython doesn't have one either. 

The existing solution is LibCST which is slow. Could the reason for its slowness be the fact that it does more work?
My understanding is that a CST contains more information than an AST.

So, I see a new question:
Which solution is more appropriate: 
- the current one that uses an AST parser and takes non-AST tokens from the stream OR
- a proper CST parser?

---

_Comment by @charliermarsh on 2023-02-28 22:50_

We've continued to improve the RustPython parser and it's working well for us! Some of these questions are coming up again with the autoformatter, where I have to map the AST to a CST by enriching it with information derived from the token stream.

In that context, I'm mostly focused right now on the CST representation itself and not the process by which it's parsed / extracted, which I'm punting until later.

---

_Comment by @charliermarsh on 2023-05-18 15:55_

I'm going to close this for now, as we're continuing to use and contribute to the RustPython parser. Nothing actionable here.

---

_Closed by @charliermarsh on 2023-05-18 15:55_

---
