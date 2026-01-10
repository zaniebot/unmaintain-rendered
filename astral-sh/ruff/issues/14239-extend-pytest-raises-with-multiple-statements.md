---
number: 14239
title: "Extend `pytest-raises-with-multiple-statements (PT012)` to cover `pytest.warns`"
type: issue
state: closed
author: harupy
labels:
  - rule
assignees: []
created_at: 2024-11-10T04:19:05Z
updated_at: 2025-01-13T01:47:00Z
url: https://github.com/astral-sh/ruff/issues/14239
synced_at: 2026-01-10T01:22:54Z
---

# Extend `pytest-raises-with-multiple-statements (PT012)` to cover `pytest.warns`

---

_Issue opened by @harupy on 2024-11-10 04:19_

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

[`pytest-raises-with-multiple-statements`](https://docs.astral.sh/ruff/rules/pytest-raises-with-multiple-statements/) only covers `pytest.raises`. It'd be nice if it covers `pytest.warns` as well.

```python
import pytest


def test_raises():
    with pytest.raises(Exception):  # PT012
        f3()
        f4()


def test_warns():
    with pytest.warns(UserWarning):
        f1()  # does this warn?
        f2()  # or perhaps this one warns?
        # -> Not obvious which line warns.
```

Playground to ensure `pytest.warns` is not covered: https://play.ruff.rs/271e2f8e-751a-4368-971c-322eeeb10b39

References:

- https://docs.pytest.org/en/stable/reference/reference.html#pytest.warns

---

_Renamed from "Extend `pytest-raises-with-multiple-statements` to cover `pytest.warns`" to "Extend `pytest-raises-with-multiple-statements (PT012)` to cover `pytest.warns`" by @harupy on 2024-11-10 04:26_

---

_Comment by @harupy on 2024-11-10 08:40_

Adding a new rule is another option.

---

_Comment by @MichaReiser on 2024-11-11 10:11_

I think that would have to be its own rule unless we rename the existing rule

---

_Comment by @harupy on 2024-11-11 10:13_

> I think that would have to be its own rule unless we rename the existing rule

Makes sense.

---

_Label `rule` added by @MichaReiser on 2024-11-11 10:19_

---

_Comment by @harupy on 2024-11-12 04:20_

@MichaReiser do we need to add the same rule in https://github.com/m-burst/flake8-pytest-style to add it in ruff?

---

_Comment by @MichaReiser on 2024-11-12 11:19_

I think it should be fine. We just need to make sure that we use a rule code that's unlikely to collide with any new rule added to the upstream plugin

---

_Comment by @tjkuson on 2024-11-17 12:20_

Created an issue in the upstream repo: https://github.com/m-burst/flake8-pytest-style/issues/317

---

_Comment by @MichaReiser on 2024-11-17 13:20_

Thanks @tjkuson 

---

_Comment by @snowdrop4 on 2024-11-22 20:54_

This lint makes a lot of sense to me.

I'll make a draft branch.

I'll just use a placeholder rule code until the flake8 plugin decides on one.

There could also be `pytest.warns` versions of `PT010` and `PT011` (not just `PT012`), since they are all analagous.

---

_Comment by @snowdrop4 on 2024-11-22 22:22_

I have a finished branch: https://github.com/snowdrop4/ruff/tree/AVK/PytestWarnsWithMultipleStatements

But yeah, I'll wait and see what the flake8 plugin people say before PRing.

---

_Comment by @MichaReiser on 2025-01-10 15:23_

The upstream issue has been closed as completed in case someone's interested to work on this

---

_Comment by @tjkuson on 2025-01-10 16:37_

I volunteer unless @snowdrop4 fancies it

---

_Referenced in [astral-sh/ruff#15444](../../astral-sh/ruff/pulls/15444.md) on 2025-01-12 20:33_

---

_Closed by @charliermarsh on 2025-01-13 01:47_

---

_Closed by @charliermarsh on 2025-01-13 01:47_

---
