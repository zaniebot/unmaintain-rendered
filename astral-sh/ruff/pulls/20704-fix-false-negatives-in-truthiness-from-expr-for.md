```yaml
number: 20704
title: "Fix false negatives in `Truthiness::from_expr` for lambdas, generators, and f-strings"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-20698
created_at: 2025-10-05T22:27:11Z
updated_at: 2025-10-14T11:45:00Z
url: https://github.com/astral-sh/ruff/pull/20704
synced_at: 2026-01-12T15:57:07Z
```

# Fix false negatives in `Truthiness::from_expr` for lambdas, generators, and f-strings

---

_@danparizher_

## Summary

Fixes #20703


---

_Comment by @github-actions[bot] on 2025-10-05 22:41_

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

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1 on 2025-10-06 13:06_

Thank you! This looks good. Instead of these unit tests, could you pick some of the affected rules and add snapshot tests?

---

_@dylwil3 requested changes on 2025-10-06 13:06_

---

_Review requested from @dylwil3 by @danparizher on 2025-10-06 20:59_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1406 on 2025-10-10 19:22_

I'm not sure I understand this line. Which of the deductions in `inner` is incorrect for string, ascii, or `repr` conversion flags?

---

_@dylwil3 requested changes on 2025-10-10 19:22_

Sorry for the delay - I have one question that I didn't notice the first time around.

---

_@danparizher reviewed on 2025-10-12 23:15_

---

_Review comment by @danparizher on `crates/ruff_python_ast/src/helpers.rs`:1406 on 2025-10-12 23:15_

When an f-string interpolation has a conversion flag like `!s`, `!a`, or `!r`, Python's `str()`, `ascii()`, and `repr()` functions always produce a non-empty string representation of the object.

This means that all the complex expressions in the `inner` function (like `BoolOp`, `BinOp`, `Call`, `Attribute`, etc.) that normally return `false` because we can't determine if they're empty become guaranteed to be non-empty when conversion flags are applied.

The conversion flags essentially override the uncertainty by ensuring the result is always a meaningful string representation, which is why the condition correctly returns `true` whenever any conversion flag is present.

---

_@dscorbett reviewed on 2025-10-13 00:31_

---

_Review comment by @dscorbett on `crates/ruff_python_ast/src/helpers.rs`:1406 on 2025-10-13 00:31_

All three conversions can produce empty strings.
```console
$ cat >counterexamples.py <<'# EOF'
from configparser import Error
error = Error()
print(len(f"{error!s}"))
print(len(f"{error!a}"))
print(len(f"{error!r}"))
# EOF

$ python counterexamples.py
0
0
0
```

---

_@danparizher reviewed on 2025-10-13 01:01_

---

_Review comment by @danparizher on `crates/ruff_python_ast/src/helpers.rs`:1406 on 2025-10-13 01:01_

Thanks for this; I was wrong about that then. In that case, we should have the condition removed.

---

_@dylwil3 reviewed on 2025-10-13 18:14_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1406 on 2025-10-13 18:14_

I don't think those counterexamples are relevant here, as far as I can tell. The function in question returns `true` when we can be _certain_ that the f-string is nonempty, and `false` otherwise. It already returns false for `Name` expressions, so it would already be okay for `counterexamples.py` with or without worrying about conversion flags.

My claim is that the expressions for which we return `true` would be unaffected by the presence of these flags (insofar as their boolean value).

---

_@dylwil3 approved on 2025-10-13 18:15_

---

_Merged by @dylwil3 on 2025-10-14 08:06_

---

_Closed by @dylwil3 on 2025-10-14 08:06_

---

_Label `bug` added by @dylwil3 on 2025-10-14 08:06_

---

_Branch deleted on 2025-10-14 11:45_

---
