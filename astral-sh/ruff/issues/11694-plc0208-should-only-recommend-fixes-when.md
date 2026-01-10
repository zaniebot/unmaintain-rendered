```yaml
number: 11694
title: PLC0208 should only recommend fixes when iterating over literals
type: issue
state: closed
author: Skylion007
labels:
  - rule
assignees: []
created_at: 2024-06-02T17:00:01Z
updated_at: 2024-10-21T11:14:03Z
url: https://github.com/astral-sh/ruff/issues/11694
synced_at: 2026-01-10T11:09:53Z
```

# PLC0208 should only recommend fixes when iterating over literals

---

_Issue opened by @Skylion007 on 2024-06-02 17:00_

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
I tried PLC0208 on my codebase, and found several spots where iterating over a set was clearly intentional. Here is a salient example:
```python
for rg in {dtype.is_floating_point, False}:
```
This will would get corrected to a sequence, when I want this test case to run over `True` and `False` is `dtype.is_floating_point` is true. Otherwise, only False.

Another useful idiom is:
```python
for index in {0, sparse.shape[dim] // 2, sparse.shape[dim] - 1}:
```
Where I want to loop over all unique values. If the `sparse.shape[dim]` is 2, then I only go over index (0, 1) instead of (0, 1, 1). It doesn't seem like this fix can be safe if it doesn't know what it's referencing. At the very least, the fix should be marked unsafe.

---

_Comment by @Skylion007 on 2024-06-02 17:03_

Ugh, looks like https://github.com/astral-sh/ruff/pull/4907 it suppose to cover this, but fails with parameters or getter calls.

---

_Comment by @charliermarsh on 2024-06-02 17:45_

I think this is different than #4907? My read is that change was about comprehensions and `set` calls. Pylint does flag the cases you've included here, though we could consider changing our rule.

---

_Label `rule` added by @charliermarsh on 2024-06-02 17:45_

---

_Comment by @rdaysky on 2024-10-11 03:05_

I’ve also just encountered PLC0208 on code like this:
```
dt: datetime.datetime = ...
slightly_earlier = dt - datetime.timedelta(hours=4)

for day in {dt.date(), slightly_earlier.date()}:
    ...  # could be the same date or two consecutive dates
```
where the intention is for the loop body to handle each distinct date exactly once.

Please consider restricting the rule to literals only.

---

_Closed by @AlexWaygood on 2024-10-21 11:14_

---
