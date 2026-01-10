```yaml
number: 20258
title: "[ruff] Treat panics as fatal diagnostics, sort panics last"
type: pull_request
state: merged
author: amyreese
labels:
  - cli
  - diagnostics
assignees: []
merged: true
base: main
head: amy/panic-diagnostics
created_at: 2025-09-05T00:08:28Z
updated_at: 2025-09-17T07:12:44Z
url: https://github.com/astral-sh/ruff/pull/20258
synced_at: 2026-01-10T17:40:28Z
```

# [ruff] Treat panics as fatal diagnostics, sort panics last

---

_Pull request opened by @amyreese on 2025-09-05 00:08_

- Convert panics to diagnostics with id `Panic`, severity `Fatal`, and the error as the diagnostic message, annotated with a `Span` with empty code block and no range.
- Updates the post-linting message diagnostic handling to track the maximum severity seen, and then prints the "report a bug in ruff" message only if the max severity was `Fatal`

This depends on the sorting changes since it creates diagnostics with no range specified.



---

_Review requested from @carljm by @amyreese on 2025-09-05 18:41_

---

_Review requested from @MichaReiser by @amyreese on 2025-09-05 18:41_

---

_Review requested from @sharkdp by @amyreese on 2025-09-05 18:41_

---

_Review requested from @dcreager by @amyreese on 2025-09-05 18:41_

---

_Comment by @github-actions[bot] on 2025-09-05 18:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-09-05 19:23_

Thanks!

I still think it could make sense to try to follow ty's diagnostic model a bit more closely, as I mentioned on https://github.com/astral-sh/ruff/pull/20252. Did you have any luck trying to add a test showing the message? That might be a nice way to play around with different message formats. Let me know if you want me to take a look, it sounded like it might be trickier than I expected in https://github.com/astral-sh/ruff/pull/20252#issuecomment-3255784376.

I also don't think we can land this without auditing our usage of `Diagnostic::expect_range` more fully. I took another look at its references just now, and I think this call chain would cause a panic in diagnostic rendering:

https://github.com/astral-sh/ruff/blob/16d674e63264acdf640349c81ec6263e813a3234/crates/ruff_linter/src/message/github.rs#L22-L24

https://github.com/astral-sh/ruff/blob/16d674e63264acdf640349c81ec6263e813a3234/crates/ruff_db/src/diagnostic/mod.rs#L459

If we try to render a new panic diagnostic with the GitHub output format, it will call `Diagnostic::expect_ruff_start_location`, which calls `Diagnostic::expect_range`.

Feel free to spin this off into a separate PR, but I'd feel better introducing diagnostics without ranges if we can remove `Diagnostic::expect_range` entirely first.

I tested this out locally to make sure I wasn't off base:

```shell
 > ./target/debug/ruff check --no-cache tmp/foo.py
panic: Panicked while linting tmp/foo.py:
panicked at crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs:430:17:
brent's panic
run with `RUST_BACKTRACE=1` environment variable to display a backtrace

--> tmp/foo.py:1:1
 |
 |

Found 1 error.
error: Panic during linting indicates a bug in Ruff. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the stack trace above, we'd be very appreciative!

> ./target/debug/ruff check --no-cache tmp/foo.py --output-format github

error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!


thread 'main' panicked at crates/ruff_db/src/diagnostic/mod.rs:499:22:
Expected a range for the primary span
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Review request for @carljm removed by @carljm on 2025-09-05 21:31_

---

_Comment by @amyreese on 2025-09-06 00:18_

Check-pointed where I got to with trying to get a test rule — will look at teasing out the `expect_range` parts on Monday.

---

_Renamed from "Better handling for panics during linting" to "[ruff] Better handling for panics during linting" by @amyreese on 2025-09-12 22:40_

---

_Label `diagnostics` added by @amyreese on 2025-09-12 22:40_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5989 on 2025-09-15 13:48_

We try to avoid multiline content in diagnostic messages because it can break rendering for some output formats (as seen here). 

You can see how we accomplish this in ty here:

https://github.com/astral-sh/ruff/blob/b3cc733f068c93166dd58c01240b862b400ccc89/crates/ty_project/src/lib.rs#L669-L746

The downside of ty's approach is that we only show the "Please open an issue" part if a user uses `output-format="full"` (or any other format that renders sub diagnostics). 

I don't feel strongly about this but I thought it was worth mentioning in case this wasn't an intentional design decision.

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5935 on 2025-09-15 13:49_

I think I sort of prefer the old rendering where this message was next to the panic-diagnostic. As a user, it's now less clear to me what I have to report (imagine a few 100 diagnostics).

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:502 on 2025-09-15 13:51_

We could consider comparing by severity here (in reverse order)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/test_rules.rs`:498 on 2025-09-15 13:52_

Nice way of testing this (we don't have this in ty)

---

_@MichaReiser reviewed on 2025-09-15 13:53_

This looks great to me but I'll defer to @ntBre to do the final review as he's more up to date with what you discussed

---

_@amyreese reviewed on 2025-09-15 15:46_

---

_Review comment by @amyreese on `crates/ruff/tests/lint.rs`:5935 on 2025-09-15 15:46_

Part of the change was making sure that panics always sort to the bottom of the diagnostics, so other than being on stderr, to an interactive user it should always render "next to" the actual panic messages, which should always render after all of the non-fatal diagnostics.

---

_@ntBre reviewed on 2025-09-15 17:26_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:5989 on 2025-09-15 17:26_

The PR looks great to me overall too! I think this is the main thing I'd find interesting to try out (plus the full severity sort). It looks like we can also clean up the message a bit since the diagnostic infrastructure already adds the filename automatically. Something like:

```
[TMP]/panic.py: panic: Panicked while linting
```

instead of the current

```
[TMP]/panic.py: panic: Panicked while linting [TMP]/panic.py:
```

`panic: Panicked` also seems a bit redundant, but I think `panic: while linting` would also be awkward.

---

_Review comment by @amyreese on `crates/ruff_db/src/diagnostic/mod.rs`:502 on 2025-09-15 19:54_

As a user I still prefer having them sorted by file/lineno, because that lets me visually go through the list without jumping back and forth between different files or different parts of a single file. Eg, all of the lints for a single function or class would always be together in the final output.

But because panics trigger Ruff to discard all other diagnostics for a file, there's currently nothing for them to be grouped with, so I think it's reasonable to sort those at the end so that it's more obvious to the user.

---

_@amyreese reviewed on 2025-09-15 19:54_

---

_@amyreese reviewed on 2025-09-15 23:26_

---

_Review comment by @amyreese on `crates/ruff/tests/lint.rs`:5989 on 2025-09-15 23:26_

Moved the full error/backtrace into a subdiagnostic, and made the main diagnostic message a single line like `fatal error while linting: <panic summary>`.  I've left the "please open an issue" as a separate log to stderr, so that it would appear even in output formats like `concise`, but presumably shouldn't affect ability to process output on stdout.

The alternate is moving that message into a second subdiagnostic, but then it would be repeated for every single panic rather than just once at the end, and would also end up being hidden in `concise` output (or we make the main message more verbose? 😐).

As-is, in `concise` and `full` output format, it now looks like:

```
amethyst@lunatone ~/workspace/ruff amy/panic-diagnostics+ » cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC25*.py --no-cache --preview --select ASYNC --output-format=concise
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC251.py --no-cache --preview --select ASYNC --output-format=concise`
crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC251.py:6:5: ASYNC251 Async functions should not call `time.sleep`
crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py: panic: Fatal error while linting: fake panic
Found 2 errors.
error: Panic during linting indicates a bug in Ruff. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the stack trace above, we'd be very appreciative!

> [2]

amethyst@lunatone ~/workspace/ruff amy/panic-diagnostics+ » cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC25*.py --no-cache --preview --select ASYNC --output-format=full
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC251.py --no-cache --preview --select ASYNC --output-format=full`
ASYNC251 Async functions should not call `time.sleep`
 --> crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC251.py:6:5
  |
5 | async def func():
6 |     time.sleep(1)  # ASYNC251
  |     ^^^^^^^^^^
  |

panic: Fatal error while linting: fake panic
--> crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py:1:1
 |
 |
info: panicked at crates/ruff_linter/src/rules/flake8_async/rules/blocking_input.rs:50:13:
fake panic
run with `RUST_BACKTRACE=1` environment variable to display a backtrace


Found 2 errors.
error: Panic during linting indicates a bug in Ruff. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the stack trace above, we'd be very appreciative!

> [2]
```

---

_@MichaReiser reviewed on 2025-09-16 07:18_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5935 on 2025-09-16 07:18_

Oh I see, it might be worth adding an inline comment explaining that this is the intended behavior as it wasn't obvious to me.

---

_@MichaReiser reviewed on 2025-09-16 07:25_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:502 on 2025-09-16 07:25_

> As a user I still prefer having them sorted by file/lineno, because that lets me visually go through the list without jumping back and forth between different files or different parts of a single file. Eg, all of the lints for a single function or class would always be together in the final output.

That makes sense and is consistent with what we have in ty: https://github.com/astral-sh/ruff/blob/b6a740f86a8372f45d24fb3b28e8f3148472bf11/crates/ruff_db/src/diagnostic/mod.rs#L534-L565

The only difference is that ty only compares by severity if the file doesn't have a range and fatal errors come first rather than last and ty prints an extra warning as part of the summary if there were any panics (to avoid users missing them). 

I don't have a strong preference towards either approach and using `is_fatal` here seems fine after reading your reasoning.

---

_@MichaReiser approved on 2025-09-16 07:25_

This looks good to me

---

_Review comment by @ntBre on `crates/ruff/src/commands/check.rs`:208 on 2025-09-16 14:35_

I think we might want to call `set_file_level(true)` on this `Annotation` to avoid the empty snippet (`|` lines) in the diagnostic from your example:

```
--> crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py:1:1
 |
 |
info: panicked at crates/ruff_linter/src/rules/flake8_async/rules/blocking_input.rs:50:13:
```

---

_@ntBre approved on 2025-09-16 14:36_

Looks good to me too! Just one small suggestion about the diagnostic

---

_Label `cli` added by @amyreese on 2025-09-16 18:31_

---

_Renamed from "[ruff] Better handling for panics during linting" to "[ruff] Treat panics as fatal diagnostics, sort panics last" by @amyreese on 2025-09-16 18:32_

---

_Merged by @amyreese on 2025-09-16 18:33_

---

_Closed by @amyreese on 2025-09-16 18:33_

---

_Branch deleted on 2025-09-16 18:33_

---

_@MichaReiser reviewed on 2025-09-17 07:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/check.rs`:208 on 2025-09-17 07:12_

If you're interested in removing the need for `set_file_level`, this is listed as a potential improvement in your welcome document:

> This task is related to the one above. Ruff currently uses an empty text range at position 0 (start of the file) for diagnostics that apply to the entire file. We need the range for suppression comments to work, but we don’t want to render a code frame for those. Brent added a flag for annotations (the diagnostic system renders code snippets for each annotation) so that they can be marked as file-level, in which case the code snippets aren’t rendered....

---
