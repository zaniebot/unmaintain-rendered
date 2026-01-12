```yaml
number: 4879
title: "[flake8-comprehensions] C413 autofix bug"
type: issue
state: closed
author: zariiii9003
labels:
  - bug
  - question
assignees: []
created_at: 2023-06-05T20:55:17Z
updated_at: 2023-06-07T22:23:21Z
url: https://github.com/astral-sh/ruff/issues/4879
synced_at: 2026-01-12T15:54:45Z
```

# [flake8-comprehensions] C413 autofix bug

---

_@zariiii9003_

The autofix for C413 can lead to bugs if the `key` parameter is used. Here is a little code example to show, that the results before and after the autofix are different.

```python3
original = [(1, 0), (2, 0), (3, 1), (4, 1)]
print(list(reversed(sorted(original, key=lambda x: x[1]))))
print(sorted(original, key=lambda x: x[1], reverse=True))
```

Here is the output:
```text
[(4, 1), (3, 1), (2, 0), (1, 0)]
[(3, 1), (4, 1), (1, 0), (2, 0)]
```


---

_Comment by @charliermarsh on 2023-06-05 21:02_

Do `reversed(...)` and `reverse=True` break ties differently?

---

_Label `bug` added by @charliermarsh on 2023-06-05 21:02_

---

_Label `question` added by @charliermarsh on 2023-06-05 21:02_

---

_Comment by @charliermarsh on 2023-06-06 01:37_

Yeah, it looks like `reversed` does a full reversal, whereas `reverse` ends up retaining the original order for ties.

---

_Comment by @dhruvmanila on 2023-06-06 04:04_

Wow, didn't know about this. I think the solution would be to make the auto-fix "sometimes" and not fix it when there's a `key` parameter present. The `key` parameter makes the sorting different for `reversed` and `sorted(..., reverse=True)`.

Edit: Not making the auto-fix "sometimes" but rather not flagging this.

---

_Closed by @charliermarsh on 2023-06-07 22:23_

---
