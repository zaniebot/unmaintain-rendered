```yaml
number: 8122
title: "`repeated-isinstance-calls` is a duplicate rule of `duplicate-isinstance-call`"
type: issue
state: closed
author: Skylion007
labels:
  - rule
assignees: []
created_at: 2023-10-22T18:55:01Z
updated_at: 2024-06-25T13:52:02Z
url: https://github.com/astral-sh/ruff/issues/8122
synced_at: 2026-01-12T15:54:47Z
```

# `repeated-isinstance-calls` is a duplicate rule of `duplicate-isinstance-call`

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
These two rules do the exact same thing but, have slightly different logic. They should be an alias of one another but appear to each be there own rule:
https://docs.astral.sh/ruff/rules/repeated-isinstance-calls/ and https://docs.astral.sh/ruff/rules/duplicate-isinstance-call/ do the same thing, and have the same autofix implemented. Seems like this wasn't notice when the PR was merged https://github.com/astral-sh/ruff/pull/4792

---

_Renamed from "`repeated-isinstance-calls` is duplicate of `duplicate-isinstance-call`" to "`repeated-isinstance-calls` is a duplicate rule of `duplicate-isinstance-call`" by @Skylion007 on 2023-10-22 18:55_

---

_Comment by @Skylion007 on 2023-10-22 19:03_

I prefer the original logic that always converted the isinstance call to check against a type tuple. While the new Union logic is really snazzy in Python 3.11, it is a little bit slower in practice and having the pyupgrade rule take care of it means we can opt out of it which is not possible with the PyLint check

---

_Comment by @charliermarsh on 2023-10-23 03:56_

Thanks, will dedupe!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-23 03:56_

---

_Label `rule` added by @charliermarsh on 2023-10-23 03:56_

---

_Added to milestone `v0.2.0` by @charliermarsh on 2024-01-29 03:58_

---

_Comment by @zanieb on 2024-01-30 01:26_

@charliermarsh do we want to deprecate one of them? which one?

---

_Comment by @charliermarsh on 2024-01-30 01:53_

@zanieb - I'd vote to keep the `flake8-simplify` version since we're more closely anchored to that ecosystem and it came first.

---

_Removed from milestone `v0.2.0` by @dhruvmanila on 2024-02-12 05:16_

---

_Added to milestone `v0.3.0` by @dhruvmanila on 2024-02-12 05:16_

---

_Removed from milestone `v0.3.0` by @MichaReiser on 2024-02-14 16:28_

---

_Added to milestone `v0.4` by @MichaReiser on 2024-02-14 16:28_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:48_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:48_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-25 04:49_

---

_Unassigned @charliermarsh by @dhruvmanila on 2024-06-25 04:49_

---

_Comment by @dhruvmanila on 2024-06-25 13:52_

Resolved in #12021 

---

_Closed by @dhruvmanila on 2024-06-25 13:52_

---
