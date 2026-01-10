```yaml
number: 10258
title: UP032 complains about format args performing function calls
type: issue
state: closed
author: ThiefMaster
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-03-06T23:43:35Z
updated_at: 2024-03-07T04:33:20Z
url: https://github.com/astral-sh/ruff/issues/10258
synced_at: 2026-01-10T11:09:52Z
```

# UP032 complains about format args performing function calls

---

_Issue opened by @ThiefMaster on 2024-03-06 23:43_

```py
x = 'CASE WHEN {0} > {1} THEN {0} ELSE {1} END'.format(process(arg1), process(arg2))
```

In this case one can assume the `format()` call is intentional to avoid calling `process()` more than once per arg.

```
$ ruff version
ruff 0.3.1

$ ruff check --isolated --select UP032 --preview --no-cache --output-format concise rufftest/ruff_sample.py
rufftest/ruff_sample.py:1:5: UP032 Use f-string instead of `format` call
Found 1 error.
```

Of course it could be refactored like this, but the rule triggering there feels incorrect.

```py
arg1 = process(arg1)
arg2 = process(arg2)
return f'CASE WHEN {arg1} > {arg2} THEN {arg1} ELSE {arg2} END'
```

---

_Label `bug` added by @charliermarsh on 2024-03-07 03:38_

---

_Comment by @charliermarsh on 2024-03-07 03:38_

Good call, let's fix this.

---

_Label `good first issue` added by @charliermarsh on 2024-03-07 03:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 03:46_

---

_Closed by @charliermarsh on 2024-03-07 04:33_

---
