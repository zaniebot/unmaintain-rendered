```yaml
number: 898
title: Implement F521
type: pull_request
state: merged
author: olliemath
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-24T12:04:10Z
updated_at: 2022-11-24T23:09:36Z
url: https://github.com/astral-sh/ruff/pull/898
synced_at: 2026-01-12T05:48:46Z
```

# Implement F521

---

_Pull request opened by @olliemath on 2022-11-24 12:04_

Part of https://github.com/charliermarsh/ruff/issues/891

A little rough around the edges, but functional. I've added:
-  Vendored code to `vendored/format.rs` (to make it easy to pull in updates from rustpython, I decided to not alter this in any way except to delete dead code)
-  Small helpers and wrappers in `pyflakes/format.rs` (possibly also the place for extra tests of the vendored code)
-  Logic for detecting a `"...".format(...)` call in `check_ast.rs`
-  Actual checks in `pyflakes` directory

The main check_ast logic feels a bit clunky - we're looking for a function call which is the "format" attribute of a string constant. Also, I'm not keen on the long names of the checks (`StringDotFormatInvalidFormat`) - they're just the pyflakes names so we could change them.

Keen to know your thoughts on the initial approach before I crack on with F522-F525.

---

_Comment by @olliemath on 2022-11-24 12:53_

Since it's just copy-pasta, I should probably look into rustpython's license to do the vendoring properly - we may need to add a rustpython copyright notice.

**Edit:** done

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1252 on 2022-11-24 14:52_

We can probably move this to the top of the block, like line 1244?

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/string_dot_format.rs`:12 on 2022-11-24 14:53_

You could also choose to put this in `pyflakes/checks.rs` directly if it just returns an `Option<Check>`. (The undocumented convention is that `plugins` take `&mut Checker` while `checks` are pure functions that return checks.)

---

_Review comment by @charliermarsh on `src/vendored/format.rs`:1 on 2022-11-24 14:54_

Do you mind updating this link to include the Git hash of the commit, and the full path to the file? (That is, the permalink to `format.rs`.)

---

_Comment by @charliermarsh on 2022-11-24 14:55_

This looks great to me! I think what you have in `check_ast.rs` is very reasonable. Just a couple small comments but would be happy to merge without any major changes.

---

_@charliermarsh reviewed on 2022-11-24 14:56_

---

_@olliemath reviewed on 2022-11-24 15:20_

---

_Review comment by @olliemath on `src/check_ast.rs`:1252 on 2022-11-24 15:20_

This is mostly pre-emptive laziness on my part - once we add the 5 checks I was going to write it like:
```rust
if string_dot_format_call {
    if F521_enabled {...}
    if F522_enabled {...}
    if F523_enabled {...}
    if F524_enabled {...}
    if F525_enabled {...}
}
```

rather than
```rust
if F521_enabled {
    if string_dot_format_call { ... }
}
if F522_enabled {
    if string_dot_format_call { ... }
}
...
```

I'm OK doing it the second way if you think that's better (would just place the repeated logic in a helper function)? 

---

_@olliemath reviewed on 2022-11-24 15:21_

---

_Review comment by @olliemath on `src/pyflakes/plugins/string_dot_format.rs`:12 on 2022-11-24 15:21_

Nice, will move them to the right place

---

_@charliermarsh reviewed on 2022-11-24 15:23_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1252 on 2022-11-24 15:23_

Oh, yes, the pattern you were intending makes sense to me.

---

_@olliemath reviewed on 2022-11-24 18:33_

---

_Review comment by @olliemath on `src/pyflakes/plugins/string_dot_format.rs`:12 on 2022-11-24 18:33_

Assume you meant I can put this logic in `check_ast.rs` directly - moved it there. Let me know if that's not what you meant (couldn't find another example of `checker.add_check` in one of the `checks.rs` files).

---

_Comment by @olliemath on 2022-11-24 18:35_

I think this is basically good to go. Unfortunately the vendored code fails the clippy checks. I had hoped to include it essentially unmodified (just with deletions), but I guess can give it a small tweak to pass.

---

_Renamed from "Draft: implement F521" to "Implement F521" by @olliemath on 2022-11-24 18:39_

---

_Comment by @charliermarsh on 2022-11-24 21:23_

Awesome. Will look to review and merge this tonight or tomorrow.

---

_Merged by @charliermarsh on 2022-11-24 23:09_

---

_Closed by @charliermarsh on 2022-11-24 23:09_

---
