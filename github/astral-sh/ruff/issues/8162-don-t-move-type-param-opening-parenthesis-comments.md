---
number: 8162
title: "Don't move type param opening parenthesis comments"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-24T11:29:47Z
updated_at: 2023-10-25T08:25:01Z
url: https://github.com/astral-sh/ruff/issues/8162
synced_at: 2026-01-07T13:12:15-06:00
---

# Don't move type param opening parenthesis comments

---

_Issue opened by @dhruvmanila on 2023-10-24 11:29_

Given:

```python
type foo[  # comment
    a,
    b
] = ...
```

We format it as:
```python
type foo[a, b] = ...  # comment
```

But, the list formatting is from:
```python
foo = [  # comment 0
    a,
    b
]
```

To:
```python
foo = [  # comment 0
    a,
    b,
]
```

---

_Label `formatter` added by @dhruvmanila on 2023-10-24 11:29_

---

_Comment by @konstin on 2023-10-24 11:36_

I think the correct solution is to keep type param opening parentheses comment on the opening parenthesis

---

_Renamed from "Inconsistent formatting of dangling comments for type params" to "Don't move type param opening parenthesis comments" by @konstin on 2023-10-24 11:36_

---

_Label `bug` added by @konstin on 2023-10-24 11:36_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-10-24 11:44_

---

_Referenced in [astral-sh/ruff#8163](../../astral-sh/ruff/pulls/8163.md) on 2023-10-24 11:54_

---

_Closed by @dhruvmanila on 2023-10-24 12:02_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-10-25 08:25_

---
