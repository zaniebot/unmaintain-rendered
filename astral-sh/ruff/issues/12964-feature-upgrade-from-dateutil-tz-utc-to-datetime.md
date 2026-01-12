```yaml
number: 12964
title: "Feature: upgrade from `dateutil.tz.UTC` to `datetime.UTC` or `datetime.timezone.utc`"
type: issue
state: open
author: trim21
labels:
  - rule
  - needs-design
assignees: []
created_at: 2024-08-18T14:17:13Z
updated_at: 2024-08-19T08:29:33Z
url: https://github.com/astral-sh/ruff/issues/12964
synced_at: 2026-01-12T15:54:52Z
```

# Feature: upgrade from `dateutil.tz.UTC` to `datetime.UTC` or `datetime.timezone.utc`

---

_@trim21_

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

there are some legacy code using `dateutil.tz.UTC` as UTC timezone, suggest a new rule to replace it with `datetime.UTC` for python >= 3.11

---

_Comment by @ember91 on 2024-08-18 18:42_

This is related to what UP017 does.

---

_Label `rule` added by @charliermarsh on 2024-08-18 22:18_

---

_Comment by @charliermarsh on 2024-08-18 22:18_

I'd be open to adding that to `UP017`.

---

_Comment by @trim21 on 2024-08-19 03:27_

`datetime.timezone.utc` is `datetime.UTC`, but `dateutil.tz.UTC` is not `datetime.UTC`.

There may be some potential edge case problem, should be a `unsafe-fix` rule, I think.

---

_Comment by @trim21 on 2024-08-19 03:51_

there are also `pytz.UTC` and `pytz.utc`



---

_Label `needs-design` added by @MichaReiser on 2024-08-19 08:29_

---
