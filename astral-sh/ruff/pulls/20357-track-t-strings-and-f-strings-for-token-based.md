```yaml
number: 20357
title: Track t-strings and f-strings for token-based rules and suppression comments
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
  - suppression
  - python314
assignees: []
merged: true
base: main
head: t-string-isc
created_at: 2025-09-11T21:05:41Z
updated_at: 2025-09-12T18:00:13Z
url: https://github.com/astral-sh/ruff/pull/20357
synced_at: 2026-01-12T15:56:59Z
```

# Track t-strings and f-strings for token-based rules and suppression comments

---

_@dylwil3_

Our token-based rules and `noqa` extraction used an `Indexer` that kept track of f-string ranges but not t-strings. We've updated the `Indexer` and downstream uses thereof to handle both f-strings and t-strings.

Most of the diff is renaming and adding tests.

Note that much of the "new" logic gets to be naive because the lexer has already ensured that f and t-string "starts" are paired with their respective "ends", even amidst nesting and so on.

Finally: one could imagine wanting to know if a given interpolated string range corresponds to an f-string or a t-string, but I didn't find a place where we actually needed this.

Closes #20310


---

_Label `bug` added by @dylwil3 on 2025-09-11 21:05_

---

_Label `rule` added by @dylwil3 on 2025-09-11 21:05_

---

_Label `suppression` added by @dylwil3 on 2025-09-11 21:05_

---

_Label `python314` added by @dylwil3 on 2025-09-11 21:05_

---

_Comment by @github-actions[bot] on 2025-09-11 21:18_

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

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:146 on 2025-09-12 16:06_

I think you addressed this in the summary, and it sounds like it should be okay, but it would feel slightly more natural to me to write it as:


```suggestion
            (TokenKind::FStringEnd, TokenKind::FStringStart)
             | (TokenKind::TStringEnd, TokenKind::TStringStart) => {
```

or can they actually be mixed?

---

_Review comment by @ntBre on `crates/ruff_linter/src/directives.rs`:518 on 2025-09-12 16:12_

This looks like a test for an f-string in a t-string then a t-string in an f-string. Do we need one for a t-string in a t-string? These are probably the trickier cases, just an idea

---

_Review comment by @ntBre on `crates/ruff_python_index/src/indexer.rs`:21 on 2025-09-12 16:13_

```suggestion
    /// The range of all interpolated strings in the source document.
```

I guess we might as well fix an existing typo while we're here

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__multiline_ISC001_ISC.py.snap`:564 on 2025-09-12 16:23_

Just curious, why is this the only case with a fix? I'm assuming that's an existing feature of the rule, but I hadn't noticed it before.

---

_@ntBre approved on 2025-09-12 16:26_

Nice, thank you! This looks good to me, I just had a few questions for my understanding and a couple of very minor nits.

---

_@dylwil3 reviewed on 2025-09-12 17:40_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__multiline_ISC001_ISC.py.snap`:564 on 2025-09-12 17:40_

Oh good catch! Fixed

---

_@dylwil3 reviewed on 2025-09-12 17:40_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:146 on 2025-09-12 17:40_

Your way is better (but they are equivalent on valid code because of the explanation in the PR summary)

---

_Merged by @dylwil3 on 2025-09-12 18:00_

---

_Closed by @dylwil3 on 2025-09-12 18:00_

---
