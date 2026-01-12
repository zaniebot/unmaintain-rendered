```yaml
number: 6210
title: PERF401 false positive when appended list items depend on previous items
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2023-07-31T20:26:20Z
updated_at: 2023-08-01T03:56:54Z
url: https://github.com/astral-sh/ruff/issues/6210
synced_at: 2026-01-12T15:54:45Z
```

# PERF401 false positive when appended list items depend on previous items

---

_@andersk_

This incorrectly triggers [PERF401 manual-list-comprehension](https://beta.ruff.rs/docs/rules/manual-list-comprehension/) (#5298):

```python
fibonacci = [0, 1]
for i in range(20):
    fibonacci.append(sum(fibonacci[-2:]))
print(fibonacci)
```

This cannot be converted to a list comprehension because each appended item depends on the previous items. Similar to #5581, but without an `if` conditional.

---

_Comment by @charliermarsh on 2023-07-31 20:27_

Thanks, will fix.

---

_Label `bug` added by @charliermarsh on 2023-07-31 20:27_

---

_Closed by @charliermarsh on 2023-08-01 03:56_

---
