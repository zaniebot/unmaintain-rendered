---
number: 12358
title: C419 in --preview does not warn if a second parameter is passed to sum()
type: issue
state: closed
author: ember91
labels:
  - bug
assignees: []
created_at: 2024-07-17T09:06:13Z
updated_at: 2024-07-18T12:37:29Z
url: https://github.com/astral-sh/ruff/issues/12358
synced_at: 2026-01-07T13:12:15-06:00
---

# C419 in --preview does not warn if a second parameter is passed to sum()

---

_Issue opened by @ember91 on 2024-07-17 09:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## List of keywords searched before creating this issue:

`C419`

## Minimal code snippet that reproduces the bug

```python
# test.py
a = [1, 2]
print(sum([x for x in a]))
print(sum([x for x in a], 0))
```

## Invoked command

Assuming `test.py` contains the content pasted above:

```bash
ruff check --preview test.py
```

## Current Ruff settings

```toml
[tool.ruff.lint]
select = [
    "C419",
]
```

## Current Ruff version

`0.5.2`

## Description

Perhaps issues aren't supposed to be created for `--preview` warnings. If so, let me know.

The C419 warning will trigger for line 3 but not for line 4. I expected that it would trigger for both lines.

---

_Renamed from "C419 does not warn if the second parameter is passed to sum()" to "C419 in --preview does not warn if a second parameter is passed to sum()" by @ember91 on 2024-07-17 09:14_

---

_Comment by @charliermarsh on 2024-07-17 13:57_

> Perhaps issues aren't supposed to be created for --preview warnings. If so, let me know.

No, `--preview` issues are great! Thank you! That's one of the goals of shipping in `--preview`.


---

_Label `bug` added by @charliermarsh on 2024-07-17 13:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 14:10_

---

_Referenced in [astral-sh/ruff#12364](../../astral-sh/ruff/pulls/12364.md) on 2024-07-17 14:10_

---

_Closed by @charliermarsh on 2024-07-18 12:37_

---

_Closed by @charliermarsh on 2024-07-18 12:37_

---
