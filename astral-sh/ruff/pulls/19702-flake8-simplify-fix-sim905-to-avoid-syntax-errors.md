```yaml
number: 19702
title: "[`flake8-simplify`] Fix `SIM905` to avoid syntax errors with r-string unescapable characters"
type: pull_request
state: open
author: danparizher
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-19610
created_at: 2025-08-01T23:28:54Z
updated_at: 2025-10-24T04:16:58Z
url: https://github.com/astral-sh/ruff/pull/19702
synced_at: 2026-01-10T17:34:34Z
```

# [`flake8-simplify`] Fix `SIM905` to avoid syntax errors with r-string unescapable characters

---

_Pull request opened by @danparizher on 2025-08-01 23:28_

## Summary

Fixes #19610


---

_Comment by @github-actions[bot] on 2025-08-01 23:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MeGaGiGaGon on 2025-08-02 00:01_

I thought of some edge cases that weren't in my original issue that combine the other edge cases, how is this handled?
```py
r"\n" "\n'\"".split("1")
```


---

_Comment by @danparizher on 2025-08-02 00:24_

That resolves to
```py
["\\n\n'\""]
```
I'll add the test case in

---

_Comment by @MichaReiser on 2025-08-04 07:56_

It seems there were some recent changes. Would you mind taking a look at the merge conflicts?

---

_Comment by @MichaReiser on 2025-08-06 06:26_

It looks like one of the fixes is introducing a syntax error (see failing tests)

---

_Converted to draft by @MichaReiser on 2025-08-06 06:26_

---

_Label `bug` added by @ntBre on 2025-08-06 16:52_

---

_Label `fixes` added by @ntBre on 2025-08-06 16:52_

---

_Comment by @MichaReiser on 2025-08-18 07:43_

@danparizher is this PR ready for review, now that you fixed the syntax error or is there more to do?

---

_Marked ready for review by @danparizher on 2025-08-21 01:56_

---

_Comment by @danparizher on 2025-08-21 01:58_

@MichaReiser Ready for review!

---

_Review requested from @ntBre by @ntBre on 2025-08-21 15:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:141 on 2025-10-21 20:22_

I think we should try to preserve the old comments and update them to reflect the new code. This is pretty tricky so they seem helpful. I also slightly preferred the old if-else-if style over all of these explicit returns.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:145 on 2025-10-21 20:24_

`TripleQuotes::No` should be the default, so anywhere we're using `empty().with_triple_quotes(No)`, we could just omit the `with_triple_quotes` call.

Using the default flags doesn't seem exactly right to me, but I'm not sure of a better alternative without playing with the code myself.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:143 on 2025-10-21 20:25_

nit: I think we could use a single `contains` call here:


```suggestion
    if elt.contains(['\n', '\r']) {
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM905.py`:140 on 2025-10-21 20:26_

This might be from the merge, but would you mind moving these back to the end of the file? That makes it a lot easier to review the snapshots.

---

_@ntBre reviewed on 2025-10-21 20:29_

Thanks! This looks reasonable to me. I just had a few small suggestions, and there are some new merge conflicts. It might be good for @dylwil3 to take a look too since he worked on `replace_flags` in #19591.

---
