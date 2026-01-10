```yaml
number: 16481
title: "[syntax-errors] Positional-only parameters before Python 3.8"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-pos-only
created_at: 2025-03-03T19:40:12Z
updated_at: 2025-03-05T13:52:07Z
url: https://github.com/astral-sh/ruff/pull/16481
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Positional-only parameters before Python 3.8

---

_Pull request opened by @ntBre on 2025-03-03 19:40_

Summary
--

Detect positional-only parameters before Python 3.8, as marked by the `/` separator in a parameter list.

Test Plan
--
Inline tests.


---

_Label `parser` added by @ntBre on 2025-03-03 19:40_

---

_Label `preview` added by @ntBre on 2025-03-03 19:40_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 19:40_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 19:40_

---

_Comment by @github-actions[bot] on 2025-03-03 19:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 08:28_

We try to avoid using abbreviations. 
```suggestion
    PositionalOnlyParameter,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:3060 on 2025-03-04 08:29_

Can you add a test that shows that the parser doesn't emit the diagnostic twice for `def foo(a, //): ...` or for `def foo(a, *args, /, b)`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:3017 on 2025-03-04 08:30_

Is this a TODO?

---

_@MichaReiser reviewed on 2025-03-04 08:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-04 11:02_

What about "positional-only parameter separator" as we're highlighting the `/` token in the source code? The word "separator" has been used in the [PEP](https://peps.python.org/pep-0570/#origin-of-as-a-separator).

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3060 on 2025-03-04 11:06_

Maybe `def foo(a, /, b, /): ...` as well or did you mean something like this with your first example?

---

_@dhruvmanila approved on 2025-03-04 11:06_

---

_@ntBre reviewed on 2025-03-04 19:39_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:3017 on 2025-03-04 19:39_

No this is along the lines of my outdated slice comment from the other PR. I'm saying we need to check inside of this `if !seen_positional_only_separator { ... }` block to avoid duplicates. I'll just delete the comment as that should be pretty obvious. I think it was originally less obvious to me and I had to move the check into the block.

---

_@ntBre reviewed on 2025-03-04 19:55_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:3060 on 2025-03-04 19:55_

I added all three of these cases.

* `def foo(a, //): ...` is just a plain `ParseError`
* `def foo(a, *args, /, b): ...` gets a `ParseError` and an `UnsupportedSyntaxError`, both on the `/`
* `def foo(a, /, b, /): ...` gets a `ParseError` on the second `/` but an `UnsupportedSyntaxError` on the first `/`

Is that what we expect? The second case feels more like a duplicate to me, but at least the third case points to two different locations. Should we only emit `ParseError`s here instead?

[pyright](https://pyright-play.net/?pythonVersion=3.7&strict=true&code=CYUwZgBGAUCGBcECWA7ALgGggeiwI0QGc0AnLbASkQDpag) actually reports three errors for the third case (more than our two) and two errors for the [second case](https://pyright-play.net/?pythonVersion=3.7&strict=true&code=CYUwZgBGD20BQEMBcECWA7ALgGggKgQCcBzAZxQxwgHpcAjCrAShQDp2g), so maybe the current behavior is okay.

---

_@dhruvmanila reviewed on 2025-03-05 03:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3060 on 2025-03-05 03:02_

I think multiple errors are fine as they both are independent.

---

_@MichaReiser approved on 2025-03-05 07:29_

---

_Merged by @ntBre on 2025-03-05 13:46_

---

_Closed by @ntBre on 2025-03-05 13:46_

---

_Branch deleted on 2025-03-05 13:46_

---
