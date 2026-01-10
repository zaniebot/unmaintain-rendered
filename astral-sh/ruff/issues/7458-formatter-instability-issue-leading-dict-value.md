---
number: 7458
title: "Formatter instability issue: Leading dict value comment becomes trailing key comment"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-09-17T14:56:41Z
updated_at: 2023-09-19T19:17:23Z
url: https://github.com/astral-sh/ruff/issues/7458
synced_at: 2026-01-10T01:22:47Z
---

# Formatter instability issue: Leading dict value comment becomes trailing key comment

---

_Issue opened by @MichaReiser on 2023-09-17 14:56_

## Input

```python
query = {
    "must":
    # queries => map(pluck("fragment")) => flatten()
        [
            clause
            for kf_pair in queries
            for clause in kf_pair["fragment"]
        ],

}
```

## Format 1

```python
query = {
    "must": # queries => map(pluck("fragment")) => flatten()
    [clause for kf_pair in queries for clause in kf_pair["fragment"]],
}
```

Notice how `# queries` is now a trailing end of line comment of `must`

## Format 2

```python
query = {
    "must": [  # queries => map(pluck("fragment")) => flatten()
        clause for kf_pair in queries for clause in kf_pair["fragment"]
    ],
}
```

Sourced by #7445

---

_Label `formatter` added by @MichaReiser on 2023-09-17 14:56_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-17 14:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-17 15:13_

---

_Comment by @charliermarsh on 2023-09-17 15:13_

Happy to take this one, I've worked on similar issues.

---

_Referenced in [astral-sh/ruff#7495](../../astral-sh/ruff/pulls/7495.md) on 2023-09-18 16:24_

---

_Closed by @charliermarsh on 2023-09-19 19:17_

---
