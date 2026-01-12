```yaml
number: 63
title: Implement F841 (for exception handlers)
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/F841
created_at: 2022-08-31T22:16:54Z
updated_at: 2022-09-07T21:14:11Z
url: https://github.com/astral-sh/ruff/pull/63
synced_at: 2026-01-12T05:48:44Z
```

# Implement F841 (for exception handlers)

---

_Pull request opened by @charliermarsh on 2022-08-31 22:16_

_No description provided._

---

_Merged by @charliermarsh on 2022-08-31 22:40_

---

_Closed by @charliermarsh on 2022-08-31 22:40_

---

_Branch deleted on 2022-08-31 22:40_

---

_Comment by @amotl on 2022-09-07 21:14_

Hi Charlie,

apologies if this is not the right place to report back on this feature. As it is a positive response, I think it does not satisfy creating an issue.

On the code base I am currently working on, those to warnings appeared after the upgrade to the most recent version of `ruff`:
```
./secpi/webinterface.py:242:13: F841 Local variable `exc_type` is assigned to but never used
./secpi/webinterface.py:242:34: F841 Local variable `exc_traceback` is assigned to but never used
```

I checked back, but `flake8` did not report them. My `flake8` configuration is pretty standard. Maybe I missed some flags?

In any case, I like that `ruff` reports those spots.

With kind regards,
Andreas.

---
