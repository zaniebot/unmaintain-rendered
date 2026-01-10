```yaml
number: 9227
title: Fixing UP032 when string is formatted with an element selected from a dict literal introduces a syntax error
type: issue
state: closed
author: lshug
labels:
  - bug
assignees: []
created_at: 2023-12-21T09:57:17Z
updated_at: 2023-12-21T21:52:18Z
url: https://github.com/astral-sh/ruff/issues/9227
synced_at: 2026-01-10T11:09:51Z
```

# Fixing UP032 when string is formatted with an element selected from a dict literal introduces a syntax error

---

_Issue opened by @lshug on 2023-12-21 09:57_

Fix for rule UP032 tried to replace instance of `"some string literal".format` with equivalent f-strings, by converting the string literal intro an f-string and placing the argument expression of `format` directly into the curly braces inside the f-string. This can cause a syntax error due to not all argument expressions being valid contents of curly braces within an f-string (at least until Python 3.12, where f-string parsing rules were updated, but I haven't checked). Specifically, f-strings such as `f"{{0: 1}[0]}"` are not valid.

Snippet to reproduce:
```python3
"{}".format({0: 1}[0])

```

command:
```ruff snippet.py --select UP032 --isolated --fix```

Ruff version: ```ruff 0.1.8```

Colab reproducing the issue:
https://colab.research.google.com/drive/1SHvTSAXxDeAoamaeOLe8nwatab4uyyM-?usp=sharing



---

_Label `bug` added by @charliermarsh on 2023-12-21 20:49_

---

_Comment by @charliermarsh on 2023-12-21 20:49_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-21 21:30_

---

_Closed by @charliermarsh on 2023-12-21 21:51_

---

_Comment by @charliermarsh on 2023-12-21 21:52_

Fixed in the next release.

---
