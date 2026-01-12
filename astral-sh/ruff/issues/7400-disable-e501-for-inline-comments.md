```yaml
number: 7400
title: Disable E501 for inline comments
type: issue
state: closed
author: Actticus
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-09-15T05:32:45Z
updated_at: 2023-10-01T07:29:55Z
url: https://github.com/astral-sh/ruff/issues/7400
synced_at: 2026-01-12T15:54:47Z
```

# Disable E501 for inline comments

---

_@Actticus_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Our project has pytest-cov, and some lines are disabled from being checked. We use the # noqa: nocover comment, and sometimes it goes beyond the length of the line. I would like to disable the E501 error for inline comments, is there any way to do this?

---

_Comment by @charliermarsh on 2023-09-19 02:38_

Do you want E501 to be disabled for _all_ inline comments, or only pragma comments like `# noqa`?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-09-19 02:38_

---

_Comment by @Actticus on 2023-09-19 05:21_

I think it's better for all inline comments

---

_Label `waiting-on-author` removed by @dhruvmanila on 2023-09-25 05:24_

---

_Label `needs-decision` added by @dhruvmanila on 2023-09-25 05:24_

---

_Label `rule` added by @dhruvmanila on 2023-09-25 05:24_

---

_Comment by @charliermarsh on 2023-10-01 03:32_

For now, we are disabling E501 for pragma comments, but not for all inline comments (https://github.com/astral-sh/ruff/pull/7692).

---

_Closed by @charliermarsh on 2023-10-01 03:32_

---

_Comment by @Actticus on 2023-10-01 07:29_

Ty

---
