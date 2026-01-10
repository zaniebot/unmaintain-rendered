```yaml
number: 20145
title: Better handling for panics and error messages
type: pull_request
state: closed
author: amyreese
labels: []
assignees: []
base: main
head: unwind-exitcode
created_at: 2025-08-29T00:25:33Z
updated_at: 2025-09-05T00:05:10Z
url: https://github.com/astral-sh/ruff/pull/20145
synced_at: 2026-01-10T17:46:21Z
```

# Better handling for panics and error messages

---

_Pull request opened by @amyreese on 2025-08-29 00:25_


## Summary

Improve handling of panics during linting, and increase visibility
of error messages and the call-to-action to report issues to the repo.

- Adds a new `panics` vector to the `Diagnostics` object
- Updates the `check::lint_path` function to format and record panics
  on the diagnostics result rather than immediately logging the error
- Adds a new `printer::write_panics` function that logs any panics
  followed by the message requesting the user submit a bug report
- Call `write_panics` from the other `write_*` functions, but after
  normal output is logged, so that the request is at the bottom of
  output for user visibility
- Check for panics and exit with code 2 (error)

## Test Plan

Manually added a `panic!` and ran ruff on a file:

```
$ cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py --no-cache --preview --select ASYNC250
...

     Running `target/debug/ruff check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py --no-cache --preview --select ASYNC250`
All checks passed!
error: Panicked while linting crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC250.py:
panicked at crates/ruff/src/diagnostics.rs:196:5:
thing happened
run with `RUST_BACKTRACE=1` environment variable to display a backtrace

error: This indicates a bug in Ruff. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the stack trace above, we'd be very appreciative!

> [2]
```


---

_Review requested from @ntBre by @amyreese on 2025-08-29 00:30_

---

_Comment by @github-actions[bot] on 2025-08-29 00:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-08-29 12:45_

Thanks! I definitely should have thought of this earlier, and this version still does seem reasonable, but I finally thought to double check how ty handles this:

https://github.com/astral-sh/ruff/blob/e42006ece8c53f5e4196849af5c0b3d6c6fadfd2/crates/ty/src/lib.rs#L337-L358

I think they just construct a normal diagnostic with `Severity::Fatal`. That might allow us to accomplish something similar while reusing some of our existing diagnostic infrastructure. The individual error messages can go into each fatal diagnostic, and then we can check the `max_severity` at the end for the longer message about opening an issue. What do you think?

Edit: and here's where they construct these diagnostics:

https://github.com/astral-sh/ruff/blob/e42006ece8c53f5e4196849af5c0b3d6c6fadfd2/crates/ty_project/src/lib.rs#L687-L697

---

_Comment by @amyreese on 2025-08-29 19:47_

@ntBre so from what I can see, the problem with that approach is that when printing the results, ruff wants to sort the `Diagnostic` objects, which then requires an `Annotation` which needs a `Span` which itself needs a `SourceFile` which needs file contents, and so on. And none of that information is available at the level that the current `catch_unwind` is happening, so we either need to move the `catch_unwind` inside of `diagnostics::lint_path` after it has constructed the `SourceFile` object, or we need to change the logic around sorting diagnostics to accommodate diagnostics without an `Annotation` or `SourceFile`.

---

_Closed by @amyreese on 2025-09-05 00:05_

---
