```yaml
number: 11637
title: Implement a new rule for FastApi routes
type: issue
state: closed
author: TomerBin
labels:
  - rule
assignees: []
created_at: 2024-05-31T13:07:59Z
updated_at: 2024-07-26T01:27:11Z
url: https://github.com/astral-sh/ruff/issues/11637
synced_at: 2026-01-12T15:54:51Z
```

# Implement a new rule for FastApi routes

---

_@TomerBin_

Hi there,

I recently came up with the idea to remove the redundant usage of the response_model keyword in FastAPI routes. FastAPI looks at the return type annotation of the decorated function by default, so using response_model while having a return type annotation with the same value is a code duplication that should be avoided.

As someone new to Rust and contributing to Ruff, I took this opportunity to learn and implemented this rule in #11579. If you find this rule aligns with Ruff’s goals, I would kindly ask for a code review.

Thank you!
Tomer Bin

---

_Label `rule` added by @charliermarsh on 2024-05-31 23:21_

---

_Label `needs-decision` added by @charliermarsh on 2024-05-31 23:21_

---

_Comment by @yannayh on 2024-06-01 12:07_

As someone using FastAPI that had no idea that this is a code duplication (which exists in my project unfortunately 😅) I would love for this rule to be implemented 👍🏼 

---

_Comment by @charliermarsh on 2024-07-26 01:27_

Added in #11579 -- thanks @TomerBin!

---

_Label `needs-decision` removed by @charliermarsh on 2024-07-26 01:27_

---

_Closed by @charliermarsh on 2024-07-26 01:27_

---
