---
number: 4238
title: "[FEATURE] Remove blank comments"
type: issue
state: closed
author: andreyfedoseev
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-05-05T09:42:42Z
updated_at: 2024-01-02T18:55:49Z
url: https://github.com/astral-sh/ruff/issues/4238
synced_at: 2026-01-07T13:12:14-06:00
---

# [FEATURE] Remove blank comments

---

_Issue opened by @andreyfedoseev on 2023-05-05 09:42_

I couldn't find a rule for removing blank comments, so I made one myself.

I can submit a PR if you find that useful, I only need to know what error code to use (I currently use `RUF101` locally)

## What it does
Check for blank comments.

## Why is this bad?
Blank comments are useless and should be removed.

## Example
```python
print("Hello, World!")  #
```

Use instead:
```python
print("Hello, World!")
```


---

_Label `rule` added by @MichaReiser on 2023-05-05 12:12_

---

_Comment by @charliermarsh on 2023-05-05 18:15_

I agree that this could be useful. I could see a case for either `RUF010` (the `RUF` rules aren't meaningfully categorized by code, so it's fine to just use the next available), or `ERA002` (since `ERA001` deals with commented-out code). Leaning towards `RUF010` though to avoid deviating from an "upstream" plugin.

---

_Referenced in [astral-sh/ruff#4269](../../astral-sh/ruff/pulls/4269.md) on 2023-05-07 14:41_

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:29_

---

_Comment by @charliermarsh on 2024-01-02 18:55_

This was implemented in the latest release as a pylint rule (https://github.com/astral-sh/ruff/pull/9174).

---

_Closed by @charliermarsh on 2024-01-02 18:55_

---
