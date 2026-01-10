---
number: 12097
title: "New check: B040, dropped exception after `.add_note()`"
type: issue
state: open
author: Zac-HD
labels:
  - rule
assignees: []
created_at: 2024-06-29T04:46:31Z
updated_at: 2025-08-28T21:01:01Z
url: https://github.com/astral-sh/ruff/issues/12097
synced_at: 2026-01-10T01:22:51Z
---

# New check: B040, dropped exception after `.add_note()`

---

_Issue opened by @Zac-HD on 2024-06-29 04:46_

https://github.com/PyCQA/flake8-bugbear/issues/474 / https://github.com/PyCQA/flake8-bugbear/pull/477

---

_Label `rule` added by @zanieb on 2024-06-29 04:49_

---

_Comment by @zanieb on 2024-06-29 04:49_

Makes sense to me!

---

_Label `help wanted` added by @zanieb on 2024-06-29 04:49_

---

_Comment by @jakkdl on 2024-06-29 16:10_

note that I explicitly decided not to cover some thorny cases, but given you have the full logic for unused variables already, I think it might make sense for you to cover them.
See discussion in the linked PR and the end of the file with test cases

---

_Label `good first issue` added by @charliermarsh on 2024-07-17 02:15_

---

_Comment by @Heidar-An on 2024-07-30 12:15_

Hey! I've been using this crate for some time now and I'd love to contribute back! However, I'm a bit confused by the initial comment, what exactly are we trying to fix?

---

_Comment by @Zac-HD on 2024-07-30 16:35_

We'd like to implement the new lint rule from upstream - this is basically a 'please port it' feature request!

---

_Comment by @divident on 2024-08-08 21:42_

@Zac-HD Hi, I have added the rule that covers the same set of errors as one in upstream and even covers the unhandled cases there. Currently it only doesn't cover the reassignment of the exception like 

```
try:
    ...
except Exception as e:
    e2 = ValueError()  # should error
    e2.add_note(str(e))
``` 

---

_Label `help wanted` removed by @MichaReiser on 2024-12-02 17:23_

---

_Label `good first issue` removed by @MichaReiser on 2024-12-02 17:23_

---

_Renamed from "New check: B040" to "New check: B040, dropped exception after `.add_note()`" by @Zac-HD on 2025-08-28 21:01_

---
