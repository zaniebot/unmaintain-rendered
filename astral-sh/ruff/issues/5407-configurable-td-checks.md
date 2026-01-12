```yaml
number: 5407
title: Configurable TD checks
type: issue
state: open
author: sirrus233
labels:
  - needs-decision
assignees: []
created_at: 2023-06-27T23:00:07Z
updated_at: 2025-02-10T19:32:42Z
url: https://github.com/astral-sh/ruff/issues/5407
synced_at: 2026-01-12T15:54:45Z
```

# Configurable TD checks

---

_@sirrus233_

It would be nice if the `flake8-todos (TD)` checks could be configured with the specific tag that triggers them. Right now they run against `TODO`, `FIXME`, and `XXX`. I would want to restrict that to run against `TODO` only, for example.

This would be especially nice to cut down on noise against lines that contain `FIXME`, now that `flake8-fixme (FIX)` is also implemented. I would personally want `TD` to run against `TODO` comments, and `FIX` to run against `FIXME`s


---

_Label `question` added by @charliermarsh on 2023-06-28 00:20_

---

_Label `configuration` added by @charliermarsh on 2023-06-28 00:20_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:08_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:08_

---

_Label `configuration` removed by @charliermarsh on 2023-07-10 01:09_

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:09_

---

_Label `accepted` removed by @charliermarsh on 2023-07-11 13:05_

---

_Comment by @arthur-st on 2025-02-07 14:12_

Agreed, this would be great. I think #11278 is related to this as well.

---

_Comment by @bwnance on 2025-02-10 19:32_

Would like this as well - i have a similar use-case to #6808 - XXXs require immediate resolution and TODOs are allowed provided they have a ticket link. however, use of an XXX fails both TD001, TD003, and FIX003 - ideally it would only fail FIX003.

---
