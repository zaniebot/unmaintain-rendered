```yaml
number: 20152
title: Less confidently mark f-strings as empty when inferring truthiness
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: empty-fstring
created_at: 2025-08-29T14:10:12Z
updated_at: 2025-08-29T22:20:26Z
url: https://github.com/astral-sh/ruff/pull/20152
synced_at: 2026-01-10T17:46:21Z
```

# Less confidently mark f-strings as empty when inferring truthiness

---

_Pull request opened by @dylwil3 on 2025-08-29 14:10_

When computing the boolean value of an f-string, we over-eagerly interpreted some f-string interpolations as empty. In this PR we now mark the truthiness of f-strings involving format specs, debug text, and bytes literals as "unknown".

This will probably result in some false negatives, which may be further refined (for example - there are probably many cases where `is_not_empty_f_string` should be modified to return `true`), but for now at least we should have fewer false positives.

Affected rules (may not be an exhaustive list):

- [unnecessary-empty-iterable-within-deque-call (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/#unnecessary-empty-iterable-within-deque-call-ruf037)
- [falsy-dict-get-fallback (RUF056)](https://docs.astral.sh/ruff/rules/falsy-dict-get-fallback/#falsy-dict-get-fallback-ruf056)
- [pytest-assert-always-false (PT015)](https://docs.astral.sh/ruff/rules/pytest-assert-always-false/#pytest-assert-always-false-pt015)
- [expr-or-not-expr (SIM221)](https://docs.astral.sh/ruff/rules/expr-or-not-expr/#expr-or-not-expr-sim221)
- [expr-or-true (SIM222)](https://docs.astral.sh/ruff/rules/expr-or-true/#expr-or-true-sim222)
- [expr-and-false (SIM223)](https://docs.astral.sh/ruff/rules/expr-and-false/#expr-and-false-sim223)

Closes #19935


---

_Label `bug` added by @dylwil3 on 2025-08-29 14:10_

---

_Label `rule` added by @dylwil3 on 2025-08-29 14:10_

---

_Comment by @github-actions[bot] on 2025-08-29 14:22_

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

_Comment by @ntBre on 2025-08-29 14:27_

Thanks! I'll take a look at this after planning. Do you think we can still use `is_empty_f_string` for RUF037 (#20109), or will we need a slightly different version? I just suggested using that yesterday :laughing: 

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1413 on 2025-08-29 15:48_

This did seem confusing initially, but this cleared it up a bit for me:

```pycon
>>> f"{b""}"
"b''"
>>> str(b'')
"b''"
```

So this is actually correct for RUF037 too:

```pycon
>>> from collections import deque
>>> deque(f"")
deque([])
>>> deque(f"{b""}")
deque(['b', "'", "'"])
```

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1419 on 2025-08-29 15:56_

Should we factor out this closure now that it's more complicated? It looks the same as below.

---

_@ntBre approved on 2025-08-29 15:57_

Thanks! I tested all of these cases with `deque`, and I think this is a fix for RUF037 too!

```pycon
>>> deque(f"{b""}")
deque(['b', "'", "'"])
>>> deque(f"{""=}")
deque(['"', '"', '=', "'", "'"])
>>> deque(f"{""!a}")
deque(["'", "'"])
>>> deque(f"{""!r}")
deque(["'", "'"])
>>> deque(f"{"":1}")
deque([' '])
```

---

_Renamed from "[`flake8-pytest-style`] Less confidently mark f-strings as empty for e.g. `pytest-assert-always-false` (`PT015`)" to "Less confidently mark f-strings as empty when inferring truthiness" by @dylwil3 on 2025-08-29 21:10_

---

_Merged by @dylwil3 on 2025-08-29 22:12_

---

_Closed by @dylwil3 on 2025-08-29 22:12_

---
