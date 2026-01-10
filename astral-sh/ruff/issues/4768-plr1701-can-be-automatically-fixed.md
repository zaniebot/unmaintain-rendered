```yaml
number: 4768
title: "PLR1701: can be automatically fixed"
type: issue
state: closed
author: saippuakauppias
labels:
  - fixes
assignees: []
created_at: 2023-05-31T21:43:32Z
updated_at: 2023-10-22T18:59:22Z
url: https://github.com/astral-sh/ruff/issues/4768
synced_at: 2026-01-10T11:09:47Z
```

# PLR1701: can be automatically fixed

---

_Issue opened by @saippuakauppias on 2023-05-31 21:43_

Rule: https://beta.ruff.rs/docs/rules/repeated-isinstance-calls/

The text of the error provides an example to correct it, you can use it to fix it automatically.

It is important to note that if the settings option `target-version` is set to a version higher than 3.9 - you can use the syntax: `isinstance(x, int | float)`

---

_Comment by @charliermarsh on 2023-06-01 01:07_

I actually thought we _did_ have autofix for this, but looks like we don't!

(We do have `isinstance(x, (int, float))` to `isinstance(x, int | float)` conversion implemented as part of the `UP` ruleset.)

---

_Label `autofix` added by @charliermarsh on 2023-06-01 01:07_

---

_Comment by @dhruvmanila on 2023-06-01 16:46_

I'll take this one up :)

---

_Assigned to @dhruvmanila by @charliermarsh on 2023-06-01 19:39_

---

_Closed by @charliermarsh on 2023-06-01 20:43_

---

_Comment by @Skylion007 on 2023-10-22 18:59_

This rule already had an autofix in: https://docs.astral.sh/ruff/rules/duplicate-isinstance-call/, We really need a better way to track aliases/ duplicate rules

---
