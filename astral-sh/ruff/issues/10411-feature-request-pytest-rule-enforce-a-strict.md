```yaml
number: 10411
title: "[Feature Request][Pytest Rule] Enforce a strict argument being passed into pytest.mark.xfail"
type: issue
state: closed
author: rshanker779
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-03-14T11:38:34Z
updated_at: 2024-03-20T16:42:38Z
url: https://github.com/astral-sh/ruff/issues/10411
synced_at: 2026-01-12T15:54:50Z
```

# [Feature Request][Pytest Rule] Enforce a strict argument being passed into pytest.mark.xfail

---

_@rshanker779_

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

Hi, 
[pytest.mark.xfail](https://docs.pytest.org/en/latest/reference/reference.html#pytest-mark-xfail-ref) has an optional strict argument that defaults to False. When strict is False, if an xfailed test unexpectedly passes, the test suite as a whole will not fail, but when strict is True, an unexpectedly passing test will be marked as a failure.

It would be nice to have a ruff rule that require the strict argument to be passed (with either value), so when writing tests with xfail it is explicit if passing cases are allowed or not.

Thanks
Rohan


---

_Label `rule` added by @charliermarsh on 2024-03-14 15:47_

---

_Label `needs-decision` added by @charliermarsh on 2024-03-14 15:47_

---

_Comment by @bluetech on 2024-03-14 18:21_

I think a better way is to set `xfail_strict = true` in the pytest config.

---

_Comment by @Kilo59 on 2024-03-16 02:50_

I agree with @bluetech; this is the native way of globally enforcing strict xfails.
https://docs.pytest.org/en/stable/reference/reference.html#confval-xfail_strict

---

_Closed by @rshanker779 on 2024-03-20 15:17_

---

_Comment by @jack-mcivor on 2024-03-20 16:42_

> I think a better way is to set `xfail_strict = true` in the pytest config.

FWIW I discovered that there is a tool that can check this kind of configuration - see https://learn.scientific-python.org/development/guides/repo-review/. Each check has a specific code associated with it - xfail_strict is PP305, mentioned here https://learn.scientific-python.org/development/guides/pytest/#configuring-pytest 



---
