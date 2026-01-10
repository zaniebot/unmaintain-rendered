```yaml
number: 2409
title: "new rule: unnecessary brackets in class definition"
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-01-31T19:20:25Z
updated_at: 2023-06-12T18:43:08Z
url: https://github.com/astral-sh/ruff/issues/2409
synced_at: 2026-01-10T11:09:45Z
```

# new rule: unnecessary brackets in class definition

---

_Issue opened by @spaceone on 2023-01-31 19:20_

```
class Foo():
    pass
```
should be written as:
```
class Foo:
    pass
```

A new rule could cover this.

---

_Label `rule` added by @charliermarsh on 2023-01-31 19:29_

---

_Comment by @sciyoshi on 2023-02-01 00:27_

Not to weigh in one way or another, but just a note that black will rewrite the first example as the second.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-12 18:06_

---

_Closed by @charliermarsh on 2023-06-12 18:43_

---
