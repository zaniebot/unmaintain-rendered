```yaml
number: 9147
title: Fix panic in D208 with multibyte indent
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: konstin/fix-gh9080
created_at: 2023-12-15T10:52:23Z
updated_at: 2023-12-15T17:02:16Z
url: https://github.com/astral-sh/ruff/pull/9147
synced_at: 2026-01-10T23:31:11Z
```

# Fix panic in D208 with multibyte indent

---

_Pull request opened by @konstin on 2023-12-15 10:52_

Fix #9080

Example, where `[]` is a 2 byte non-breaking space:
```
def f():
    """ Docstring header
^^^^ Real indentation is 4 chars
      docstring body, over-indented
^^^^^^ Over-indentation is 6 - 4 = 2 chars due to this line
   [] []  docstring body 2, further indented
^^^^^ We take these 4 chars/5 bytes to match the docstring ...
     ^^^ ... and these 2 chars/3 bytes to remove the `over_indented_size` ...
        ^^ ... but preserve this real indent
```

---

_@MichaReiser approved on 2023-12-15 10:54_

---

_Comment by @github-actions[bot] on 2023-12-15 11:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-12-15 17:02_

We've had so many fuzzer bugs related to this, strings are really hard.

---

_Label `bug` added by @charliermarsh on 2023-12-15 17:02_

---

_Label `fuzzer` added by @charliermarsh on 2023-12-15 17:02_

---

_Merged by @charliermarsh on 2023-12-15 17:02_

---

_Closed by @charliermarsh on 2023-12-15 17:02_

---

_Branch deleted on 2023-12-15 17:02_

---
