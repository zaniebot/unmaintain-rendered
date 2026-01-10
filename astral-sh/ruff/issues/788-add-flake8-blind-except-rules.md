---
number: 788
title: Add flake8-blind-except rules
type: issue
state: closed
author: JonathanPlasse
labels:
  - rule
assignees: []
created_at: 2022-11-17T14:02:29Z
updated_at: 2022-11-18T19:15:40Z
url: https://github.com/astral-sh/ruff/issues/788
synced_at: 2026-01-10T01:22:38Z
---

# Add flake8-blind-except rules

---

_Issue opened by @JonathanPlasse on 2022-11-17 14:02_

This would check when `BaseException` or `Exception` is used instead of a specific one.

I would like to work on it.
In which rule group would this go?

---

_Comment by @charliermarsh on 2022-11-17 16:28_

I think we could add it as B902 to match the existing plugin.

---

_Label `rule` added by @charliermarsh on 2022-11-17 16:33_

---

_Comment by @charliermarsh on 2022-11-18 19:05_

@JonathanPlasse - Can we close? Or was there another rule to add here?

---

_Comment by @JonathanPlasse on 2022-11-18 19:12_

Yes, the other rule is redundant.

---

_Closed by @JonathanPlasse on 2022-11-18 19:13_

---

_Comment by @charliermarsh on 2022-11-18 19:15_

Awesome.

---
