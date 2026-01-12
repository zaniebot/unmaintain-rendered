```yaml
number: 3241
title: Expand the range of the COM812 autofix to include the preceding token
type: pull_request
state: merged
author: matthewlloyd
labels:
  - bug
assignees: []
merged: true
base: main
head: bug-fix-com812-up034-autofixes
created_at: 2023-02-27T01:01:23Z
updated_at: 2023-02-27T03:47:08Z
url: https://github.com/astral-sh/ruff/pull/3241
synced_at: 2026-01-12T15:55:12Z
```

# Expand the range of the COM812 autofix to include the preceding token

---

_@matthewlloyd_

This prevents the UP034 autofix simultaneously stripping the parentheses from generators in the same linter pass, which causes a SyntaxError.

Closes #3234.

With this fix:

```python
$ cat test.py
the_first_one = next(
    (i for i in range(10) if i // 2 == 0)
)

$ cargo run --bin ruff check test.py --no-cache --select UP034,COM812 --fix
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/ruff check test.py --no-cache --select UP034,COM812 --fix`
Found 1 error (1 fixed, 0 remaining).

$ cat test.py
the_first_one = next(
    i for i in range(10) if i // 2 == 0
)
```

---

_Label `bug` added by @charliermarsh on 2023-02-27 03:46_

---

_Merged by @charliermarsh on 2023-02-27 03:47_

---

_Closed by @charliermarsh on 2023-02-27 03:47_

---
