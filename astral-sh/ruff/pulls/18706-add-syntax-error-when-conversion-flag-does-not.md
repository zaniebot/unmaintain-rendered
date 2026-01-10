```yaml
number: 18706
title: Add syntax error when conversion flag does not immediately follow exclamation mark
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: fstring-conv-after-exclam
created_at: 2025-06-16T12:20:40Z
updated_at: 2025-06-16T16:44:42Z
url: https://github.com/astral-sh/ruff/pull/18706
synced_at: 2026-01-10T18:45:04Z
```

# Add syntax error when conversion flag does not immediately follow exclamation mark

---

_Pull request opened by @dylwil3 on 2025-06-16 12:20_

Closes #18671

Note that while this has, I believe, always been invalid syntax, it was reported as a different syntax error until Python 3.12:

Python 3.11:

```pycon
>>> x = 1
>>> f"{x! s}"
  File "<stdin>", line 1
    f"{x! s}"
             ^
SyntaxError: f-string: invalid conversion character: expected 's', 'r', or 'a'
```

Python 3.12:

```pycon
>>> x = 1
>>> f"{x! s}"
  File "<stdin>", line 1
    f"{x! s}"
        ^^^
SyntaxError: f-string: conversion type must come right after the exclamanation mark
```


---

_Review requested from @MichaReiser by @dylwil3 on 2025-06-16 12:20_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-06-16 12:20_

---

_Label `bug` added by @dylwil3 on 2025-06-16 12:20_

---

_Label `parser` added by @dylwil3 on 2025-06-16 12:20_

---

_Comment by @github-actions[bot] on 2025-06-16 12:29_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:66 on 2025-06-16 16:34_

Hehe, the comment is sort of less precise than the variant name. 

---

_@MichaReiser approved on 2025-06-16 16:35_

---

_Merged by @dylwil3 on 2025-06-16 16:44_

---

_Closed by @dylwil3 on 2025-06-16 16:44_

---
