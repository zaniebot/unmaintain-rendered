```yaml
number: 910
title: B023 location is wrong when occurring inside format string 
type: issue
state: closed
author: layday
labels:
  - bug
assignees: []
created_at: 2022-11-26T12:53:16Z
updated_at: 2022-11-26T15:31:24Z
url: https://github.com/astral-sh/ruff/issues/910
synced_at: 2026-01-10T12:09:58Z
```

# B023 location is wrong when occurring inside format string 

---

_Issue opened by @layday on 2022-11-26 12:53_

A late-bound loop variable used inside a format string is reported on the first line of the document.

```py
# stuff

# more stuff

for i in range(3):
    lambda: f'{i}'
```

ruff 0.0.139 output:

```
Found 1 error(s).
-:1:2: B023 Function definition does not bind loop variable `i`
```

---

_Comment by @charliermarsh on 2022-11-26 15:06_

Ugh, f-strings. Best language feature as a user, worst language feature for tooling!

---

_Label `bug` added by @charliermarsh on 2022-11-26 15:06_

---

_Comment by @charliermarsh on 2022-11-26 15:06_

(Thanks for filing, will fix.)

---

_Closed by @charliermarsh on 2022-11-26 15:31_

---
