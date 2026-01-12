```yaml
number: 7283
title: "Apparent duplicate rule (`S307` and `PGH001`)"
type: issue
state: closed
author: tjkuson
labels:
  - rule
assignees: []
created_at: 2023-09-11T20:10:15Z
updated_at: 2024-07-26T01:57:47Z
url: https://github.com/astral-sh/ruff/issues/7283
synced_at: 2026-01-12T15:54:46Z
```

# Apparent duplicate rule (`S307` and `PGH001`)

---

_@tjkuson_

Rules [`suspicious-eval-usage` (`S307`)](https://beta.ruff.rs/docs/rules/suspicious-eval-usage/) and [`eval` (`PGH001`)](https://beta.ruff.rs/docs/rules/eval/) seem to do the same thing. If they are, in fact, distinct, then the difference between them should probably be explained in the documentation.


---

_Comment by @zanieb on 2023-09-11 20:55_

I can confirm these look duplicated and have separate implementations. I'm not sure what we should do about it though.

---

_Label `needs-decision` added by @zanieb on 2023-09-11 20:55_

---

_Comment by @charliermarsh on 2023-09-19 02:39_

We should remove PGH001, and add a redirect to S307 (IMO).

---

_Label `rule` added by @charliermarsh on 2023-09-19 02:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-19 03:33_

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-19 03:33_

---

_Comment by @charliermarsh on 2024-07-26 01:57_

This got done!

---

_Closed by @charliermarsh on 2024-07-26 01:57_

---
