```yaml
number: 16446
title: "[syntax-errors] `except*` before Python 3.11"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-except-star
created_at: 2025-02-28T22:08:21Z
updated_at: 2025-03-03T13:35:11Z
url: https://github.com/astral-sh/ruff/pull/16446
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] `except*` before Python 3.11

---

_Pull request opened by @ntBre on 2025-02-28 22:08_

Summary
--

One of the simpler ones, just detect the use of `except*` before 3.11.

Test Plan
--

New inline tests.

I'm thinking it may be okay to leave out linter CLI tests for these since we have a few tests for the plumbing already, but I'm happy to add those again if that's preferable.


---

_Label `preview` added by @ntBre on 2025-02-28 22:08_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-28 22:08_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-28 22:08_

---

_Comment by @codspeed-hq[bot] on 2025-02-28 22:13_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fsyn-except-star)

### Merging #16446 will **not alter performance**

<sub>Comparing <code>brent/syn-except-star</code> (cc2f852) with <code>main</code> (0d615b8)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-02-28 22:56_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1474 on 2025-03-02 15:20_

It's not very useful here but I wonder if we should move the `options.target_version < X` into `add_unsupported_syntax_error` because the minimal version is already encoded in `UnsupportedSyntaxErrorKind`.

---

_@MichaReiser approved on 2025-03-02 15:20_

I agree, we don't need more CLI test here. The inline tests are sufficient and easier to maintain.

---

_@ntBre reviewed on 2025-03-02 16:27_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:1474 on 2025-03-02 16:27_

Ooh, I think that would be nice. I considered that briefly but thought I'd still have to pass a version. I forgot about `UnsupportedSyntaxErrorKind::minimum_version`.

Do you think I would need to rename `add_unsupported_syntax_error`? It might look like we're unconditionally adding an error if the condition is internal. Maybe something like `check_unsupported_syntax` for a new name.

---

_@MichaReiser reviewed on 2025-03-02 17:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1474 on 2025-03-02 17:14_

I'm fine keeping it's existing name

---

_Merged by @ntBre on 2025-03-02 18:20_

---

_Closed by @ntBre on 2025-03-02 18:20_

---

_Branch deleted on 2025-03-02 18:20_

---

_@dhruvmanila reviewed on 2025-03-03 05:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@except_star_py310.py.snap`:72 on 2025-03-03 05:34_

I think I'd prefer to just highlight the `except*` part instead of the entire `try` statement. [Pyright only highlights the `*` token.](https://pyright-play.net/?pythonVersion=3.8&strict=true&code=C4JwngXAUABHMDolQKYA8DGKAOwBUMAagIYA2ArigKIggD2I08iSQA).

---

_Label `parser` added by @dhruvmanila on 2025-03-03 05:34_

---

_@ntBre reviewed on 2025-03-03 13:35_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@except_star_py310.py.snap`:72 on 2025-03-03 13:35_

Ah okay. I did try that at one point, but I wasn't sure how to handle multiple `except*` lines after the first. It looks like `pyright` emits a separate error for each one, which makes sense.

---
