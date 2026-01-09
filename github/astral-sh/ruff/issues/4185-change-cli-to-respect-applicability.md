---
number: 4185
title: "Change CLI to respect `Applicability`"
type: issue
state: closed
author: MichaReiser
labels:
  - breaking
  - cli
  - help wanted
assignees: []
created_at: 2023-05-02T09:24:05Z
updated_at: 2023-10-06T03:41:44Z
url: https://github.com/astral-sh/ruff/issues/4185
synced_at: 2026-01-07T13:12:14-06:00
---

# Change CLI to respect `Applicability`

---

_Issue opened by @MichaReiser on 2023-05-02 09:24_

This task is part of #4181 and depends on #4183. The goal is to change the CLI to respect the `Fix::applicability`. 

* Change `--fix` to only apply `Applicability::Safe` fixes. Print a message with the number of fixes that were skipped because they have an `Applicability` other than `Safe` and print the number at the end.
* Add a new `--fix-unsafe` CLI option that applies all fixes 
* Change `--diff` and `--fix-only` to imply `--fix` but runs all fixes if the user also provides `--fix-unsafe`. 


## Considerations

* We may want to hold off with this change until we've categorized a significant part of the fixes (see #4184). 
* I recommend introducing a new `--fix-unsafe` to apply unsafe fixes of past experience at Rome. Rome used to apply all fixes with `--fix` but we got multiple reports from users that they did not expect Rome to make changes that may change the program's semantic. That's why we decided to rename the CLI option to `--apply` and `--apply-unsafe` to make it clear, that the latter applies fixes that may be incorrect. The downside of not running all fixes with `--fix` is that I expect that some users may be confused why ruff doesn't apply all fixes when running `--fix`. 

---

_Referenced in [astral-sh/ruff#4181](../../astral-sh/ruff/issues/4181.md) on 2023-05-02 09:24_

---

_Label `breaking` added by @MichaReiser on 2023-05-02 09:24_

---

_Label `help wanted` added by @MichaReiser on 2023-05-02 09:24_

---

_Label `cli` added by @MichaReiser on 2023-05-02 09:24_

---

_Referenced in [astral-sh/ruff#4186](../../astral-sh/ruff/issues/4186.md) on 2023-05-02 09:36_

---

_Comment by @charliermarsh on 2023-05-19 17:55_

Are `Applicability::Manual` fixes included with `--fix-unsafe`?

---

_Comment by @MichaReiser on 2023-05-19 19:15_

> Are `Applicability::Manual` fixes included with `--fix-unsafe`?

No. Manual fixes are never applied automatically. Similar to clippy when it tells you to use a if let chain instead of match

---

_Comment by @evanrittenhouse on 2023-06-14 13:11_

You can assign this to me - seems like there have been several rules that have already been categorized, so there's at least a good sample size to test with. :) 

---

_Assigned to @evanrittenhouse by @charliermarsh on 2023-06-14 14:58_

---

_Referenced in [astral-sh/ruff#5119](../../astral-sh/ruff/pulls/5119.md) on 2023-06-15 13:01_

---

_Comment by @MichaReiser on 2023-07-03 14:21_

design discussion: #5476

---

_Referenced in [astral-sh/ruff#6835](../../astral-sh/ruff/issues/6835.md) on 2023-08-24 13:59_

---

_Referenced in [astral-sh/ruff#4845](../../astral-sh/ruff/issues/4845.md) on 2023-08-24 15:09_

---

_Referenced in [astral-sh/ruff#7019](../../astral-sh/ruff/issues/7019.md) on 2023-08-31 13:09_

---

_Referenced in [astral-sh/ruff#7214](../../astral-sh/ruff/issues/7214.md) on 2023-09-08 16:19_

---

_Referenced in [astral-sh/ruff#7769](../../astral-sh/ruff/pulls/7769.md) on 2023-10-03 17:27_

---

_Closed by @zanieb on 2023-10-06 03:41_

---
