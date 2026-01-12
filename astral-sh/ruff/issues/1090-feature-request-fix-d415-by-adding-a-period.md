```yaml
number: 1090
title: "[feature-request] Fix `D415` by adding a period"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-05T23:49:52Z
updated_at: 2022-12-06T01:24:57Z
url: https://github.com/astral-sh/ruff/issues/1090
synced_at: 2026-01-12T15:54:41Z
```

# [feature-request] Fix `D415` by adding a period

---

_@smackesey_

Just ran `ruff --select=D415` on Dagster and got a whopping 1276 errors!

```
D415 First line should end with a period, question mark, or exclamation point
```

No fix offered though-- 99% of the time, the first line should end with a period. That's actually probably 100% of cases in our violations, so IMO there should be a fix that adds the period.

---

_Label `enhancement` added by @charliermarsh on 2022-12-06 00:05_

---

_Comment by @charliermarsh on 2022-12-06 00:13_

Haha ok, I can add this, as long as we agree that it will do the wrong thing some times!

---

_Comment by @smackesey on 2022-12-06 00:20_

I think if the fix applies to the first logical line (i.e. whether it's a single line or a paragraph) as opposed to physical line then it will do a good thing near 100% of the time

---

_Comment by @charliermarsh on 2022-12-06 01:02_

@smackesey - This will also go out tonight (just a heads up in case it saves you time).

---

_Closed by @charliermarsh on 2022-12-06 01:24_

---
