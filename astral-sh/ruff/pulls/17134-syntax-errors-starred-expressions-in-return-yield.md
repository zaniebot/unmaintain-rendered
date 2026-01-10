```yaml
number: 17134
title: "[syntax-errors] Starred expressions in return, yield, and for"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-single-stars
created_at: 2025-04-01T20:21:08Z
updated_at: 2025-04-02T12:38:28Z
url: https://github.com/astral-sh/ruff/pull/17134
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Starred expressions in return, yield, and for

---

_Pull request opened by @ntBre on 2025-04-01 20:21_

Summary
--

Fixes https://github.com/astral-sh/ruff/issues/16520 by flagging single, starred expressions in `return`, `yield`, and
`for` statements.

I thought `yield from` would also be included here, but that error is emitted by
the CPython parser:

```pycon
>>> ast.parse("def f(): yield from *x")
Traceback (most recent call last):
  File "<python-input-214>", line 1, in <module>
    ast.parse("def f(): yield from *x")
    ~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.13/ast.py", line 54, in parse
    return compile(source, filename, mode, flags,
                   _feature_version=feature_version, optimize=optimize)
  File "<unknown>", line 1
    def f(): yield from *x
                        ^
SyntaxError: invalid syntax
```

And we also already catch it in our parser.

Test Plan
--

New inline tests and updates to existing tests.

---

_Label `rule` added by @ntBre on 2025-04-01 20:21_

---

_Label `preview` added by @ntBre on 2025-04-01 20:21_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-01 20:21_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-01 20:21_

---

_Comment by @github-actions[bot] on 2025-04-01 20:30_

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

_@MichaReiser approved on 2025-04-02 07:39_

---

_Merged by @ntBre on 2025-04-02 12:38_

---

_Closed by @ntBre on 2025-04-02 12:38_

---

_Branch deleted on 2025-04-02 12:38_

---
