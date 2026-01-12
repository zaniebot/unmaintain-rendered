```yaml
number: 12157
title: "PLR1704 issued for `_` variable"
type: issue
state: closed
author: ktbarrett
labels:
  - bug
  - good first issue
  - rule
assignees: []
created_at: 2024-07-02T21:10:36Z
updated_at: 2024-07-04T20:09:32Z
url: https://github.com/astral-sh/ruff/issues/12157
synced_at: 2026-01-12T15:54:51Z
```

# PLR1704 issued for `_` variable

---

_@ktbarrett_

I'm not sure what pylint does, but the variable `_` is idiomatically used as a "don't care" variable (in fact unused variables that *aren't* `_` trigger a different warning) and the error PLR1704 being issued for it I consider to be flying in the face of idiomatic code.

---

_Comment by @ktbarrett on 2024-07-02 21:13_

It seems the real issue is because I use `_` for an unused argument, then use `_` as a throwaway loop variable later. Perhaps this is just a bug.

```python
def c(_):
    for _ in range(10):
        pass
```

---

_Comment by @ktbarrett on 2024-07-02 21:21_

I just checked, pylint does not issue a warning in this case.

---

_Label `bug` added by @zanieb on 2024-07-02 21:23_

---

_Comment by @zanieb on 2024-07-02 21:23_

Sounds like a bug to me. Thanks for the report.

xref https://github.com/astral-sh/ruff/pull/8159

---

_Label `good first issue` added by @zanieb on 2024-07-02 21:23_

---

_Label `rule` added by @zanieb on 2024-07-02 21:23_

---

_Comment by @charliermarsh on 2024-07-02 21:43_

Thanks, we probably need to check this against [`dummy-variable-rgx`](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx). Good first issue!

---

_Closed by @charliermarsh on 2024-07-04 20:09_

---

_Closed by @charliermarsh on 2024-07-04 20:09_

---
