---
number: 12516
title: Complain about redundant ternaries?
type: issue
state: closed
author: janosh
labels:
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2024-07-25T18:58:08Z
updated_at: 2024-09-04T06:23:44Z
url: https://github.com/astral-sh/ruff/issues/12516
synced_at: 2026-01-10T01:22:52Z
---

# Complain about redundant ternaries?

---

_Issue opened by @janosh on 2024-07-25 18:58_

while refactoring i got this code and was fully expecting `ruff` to complain about the redundant ternary but it doesn't. maybe worth adding a rule for this?

```py
def test_the_answer():
    expected = 42 if below_311 else 42
    assert the_answer == expected
```

---

_Label `rule` added by @charliermarsh on 2024-07-25 21:30_

---

_Comment by @charliermarsh on 2024-07-25 21:30_

That seems reasonable to me.

---

_Label `good first issue` added by @charliermarsh on 2024-07-27 21:35_

---

_Label `help wanted` added by @charliermarsh on 2024-07-27 21:35_

---

_Comment by @njhearp on 2024-08-15 02:19_

I'm interested in working on this, but I'll be away for a week. If this issue is still open when I return, I would be happy to work on it.

---

_Comment by @iamlucasvieira on 2024-09-01 14:56_

Hey @njhearp, have you started working on this? If not, I’d love to take it. Let me know!

---

_Comment by @njhearp on 2024-09-01 20:21_

Hey @iamlucasvieira, you can take the issue. I tried working on the issue, but had difficulties because it is my first time trying to contribute.

---

_Assigned to @iamlucasvieira by @MichaReiser on 2024-09-02 08:30_

---

_Referenced in [astral-sh/ruff#13218](../../astral-sh/ruff/pulls/13218.md) on 2024-09-02 21:16_

---

_Comment by @MichaReiser on 2024-09-04 06:23_

Implemented in https://github.com/astral-sh/ruff/pull/13218

---

_Closed by @MichaReiser on 2024-09-04 06:23_

---
