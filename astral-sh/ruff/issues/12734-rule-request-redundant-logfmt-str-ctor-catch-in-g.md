```yaml
number: 12734
title: "Rule Request: redundant logfmt str ctor catch in G rules or add new rule"
type: issue
state: closed
author: Skylion007
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-08-07T15:27:09Z
updated_at: 2025-09-19T20:43:45Z
url: https://github.com/astral-sh/ruff/issues/12734
synced_at: 2026-01-10T11:09:54Z
```

# Rule Request: redundant logfmt str ctor catch in G rules or add new rule

---

_Issue opened by @Skylion007 on 2024-08-07 15:27_

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
This is a really common antipattern I wish logfmt (G*) rules would catch:
```python
   log.warning("Cannot write to %s", str(profile_path))
```

is redundant and does an potentially expensive str construction which is what we are trying to avoid with these rules anyway. Instead, the `%s` ensure it will be formatted as a string anyway! As such, this can be simplified into the better code:
```python
   log.warning("Cannot write to %s", profile_path)
```
Which will be faster in both the case where it's printed and the case where it's not by either avoiding an unnecessary string copy or a unnecessary construction in the first place.

This rule could be added more generically to any string formatting in the new or old styles

---

_Comment by @adamchainz on 2024-08-07 21:03_

Replying on behalf of flake8-logging here. I think we can avoid parsing the string formatting the message and instead error for any `str` or `repr` calls in the positional arguments of a logging call. That seems like it would catch the issue without much chance of false positive. What do you think? Happy to review a PR there.

---

_Comment by @charliermarsh on 2024-08-07 22:54_

Thanks! We do have a parser for format strings so we _could_ parse it and cross-reference with the positional calls if needed.

---

_Label `rule` added by @charliermarsh on 2024-08-07 22:54_

---

_Label `accepted` added by @charliermarsh on 2024-08-07 22:54_

---

_Closed by @ntBre on 2025-09-19 20:43_

---
