```yaml
number: 3021
title: Remove duplicate underline in B007 autofix message
type: pull_request
state: merged
author: SigureMo
labels: []
assignees: []
merged: true
base: main
head: duplicate-underline
created_at: 2023-02-18T22:16:01Z
updated_at: 2023-02-19T00:48:19Z
url: https://github.com/astral-sh/ruff/pull/3021
synced_at: 2026-01-12T15:55:12Z
```

# Remove duplicate underline in B007 autofix message

---

_@SigureMo_

fixes #3020

In the case of #3020, `rename` already contains an underscore (i.e. `_i`), so the error message contains two underscores (i.e. `__i`) and `__` is lost because of https://github.com/rust-lang/annotate-snippets-rs/issues/53. So the final output has no underscore (i.e. `i`)

The output after applying this fix is shown below:

```text
test.py:1:5: B007 [*] Loop control variable `i` not used within loop body
  |
1 | for i in range(0, 100):
  |     ^ B007
  |
  = help: Rename unused `i` to `_i`
```

---

_Merged by @charliermarsh on 2023-02-19 00:38_

---

_Closed by @charliermarsh on 2023-02-19 00:38_

---

_Comment by @charliermarsh on 2023-02-19 00:38_

Thanks!

---

_Branch deleted on 2023-02-19 00:48_

---
