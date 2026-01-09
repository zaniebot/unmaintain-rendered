---
number: 4916
title: Incorrect ignore logic
type: issue
state: closed
author: dcragusa
labels:
  - question
assignees: []
created_at: 2023-06-07T03:45:22Z
updated_at: 2023-06-07T21:14:50Z
url: https://github.com/astral-sh/ruff/issues/4916
synced_at: 2026-01-07T13:12:15-06:00
---

# Incorrect ignore logic

---

_Issue opened by @dcragusa on 2023-06-07 03:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff version `ruff 0.0.271`.

pyproject.toml:
```
[tool.ruff]
select = ["ALL"]
```
test.py:
```python
"""Module to test ruff ignores."""
print("test")
a = 20  # TODO

```

Command run: `ruff .` in project directory.

Output seen:
```
test.py:2:1: T201 `print` found
test.py:3:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
test.py:3:11: TD004 Missing colon in TODO
test.py:3:11: TD005 Missing issue description after `TODO`
test.py:3:11: TD003 Missing issue link on the line following this TODO
test.py:3:11: T002 Line contains TODO
```

This is fine. Now we add `ignore = ["T20"]` to the toml file. Output seen:
```
test.py:3:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
test.py:3:11: TD004 Missing colon in TODO
test.py:3:11: TD005 Missing issue description after `TODO`
test.py:3:11: TD003 Missing issue link on the line following this TODO
test.py:3:11: T002 Line contains TODO
```

This is also fine. But if we change it to `ignore = ["T"]`, we see:
```
test.py:3:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
test.py:3:11: TD004 Missing colon in TODO
test.py:3:11: TD005 Missing issue description after `TODO`
test.py:3:11: TD003 Missing issue link on the line following this TODO
test.py:3:11: T002 Line contains TODO
```

Instead we should be seeing:
```
test.py:2:1: T201 `print` found
test.py:3:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
test.py:3:11: TD004 Missing colon in TODO
test.py:3:11: TD005 Missing issue description after `TODO`
test.py:3:11: TD003 Missing issue link on the line following this TODO
```

Ruff is confusing T and T20.

---

_Comment by @charliermarsh on 2023-06-07 03:58_

If we change it to `ignore = ["T"]`, should we not be seeing:

```
test.py:3:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
test.py:3:11: TD004 Missing colon in TODO
test.py:3:11: TD005 Missing issue description after `TODO`
test.py:3:11: TD003 Missing issue link on the line following this TODO
```

(Your final cell included `test.py:2:1: T201 `print` found` at the top?)

---

_Label `question` added by @charliermarsh on 2023-06-07 03:58_

---

_Comment by @dcragusa on 2023-06-07 04:00_

I want to ignore the T rules though, not the T20 rules as well.

---

_Comment by @dcragusa on 2023-06-07 04:02_

I've double checked, with `ignore = ["T"]` we still see the T002 rule found.

---

_Comment by @charliermarsh on 2023-06-07 04:06_

`T` should select `T201` and `T002`, since they both belong under the `T` prefix. So the bug here is that `T002` isn't being ignored when `ignore = ["T"]` is specified.

If you want to ignore the TODO rules, I'd recommend `ignore = ["T0"]`, or something that differentiates from the `T20` rules, which you want to keep. `T` alone isn't sufficiently discriminant.

As an aside, I'll likely reindex `T002` under a more precise prefix in the next release. It was an oversight to reuse `T` like this, I didn't catch it in the review.

---

_Referenced in [astral-sh/ruff#4917](../../astral-sh/ruff/pulls/4917.md) on 2023-06-07 04:09_

---

_Closed by @charliermarsh on 2023-06-07 21:14_

---

_Referenced in [astral-sh/ruff#4945](../../astral-sh/ruff/issues/4945.md) on 2023-06-07 23:19_

---
