```yaml
number: 11592
title: uv does not support --constraint with a URL in double quotes
type: issue
state: closed
author: turbotankist
labels:
  - good first issue
  - help wanted
  - compatibility
assignees: []
created_at: 2025-02-18T11:01:21Z
updated_at: 2025-02-21T16:21:03Z
url: https://github.com/astral-sh/uv/issues/11592
synced_at: 2026-01-12T16:00:41Z
```

# uv does not support --constraint with a URL in double quotes

---

_@turbotankist_

### Summary

pip supports this syntax, but uv fails to parse it.

### Steps to reproduce:

reqs.txt
```
--constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.10.4/constraints-3.11.txt"
...
```
### Actual behavior
```
uv pip install -r reqs.txt
error: Error parsing included file in `./reqs.txt` at position 86
 Caused by: failed to read from file `"https://raw.githubusercontent.com/apache/airflow/constraints-2.10.4/constraints-3.11.txt"`
```

### Platform

all

### Version

0.6.1

### Python version

3.11

---

_Label `bug` added by @turbotankist on 2025-02-18 11:01_

---

_Label `bug` removed by @charliermarsh on 2025-02-18 14:29_

---

_Label `compatibility` added by @charliermarsh on 2025-02-18 14:29_

---

_Comment by @charliermarsh on 2025-02-18 14:29_

Need to decide whether we want to support this. It's similar to https://github.com/astral-sh/uv/issues/3621.

---

_Label `good first issue` added by @charliermarsh on 2025-02-19 01:43_

---

_Label `help wanted` added by @charliermarsh on 2025-02-19 01:43_

---

_Comment by @charliermarsh on 2025-02-19 02:07_

I think it'd be ok to support this just for `--constraint` and the other flags that are supported here as a start, since those are intended to mirror the CLI.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-19 19:13_

---

_Comment by @charliermarsh on 2025-02-19 19:26_

Gonna do this since it's easy to do on the plane, unlike things that require Internet.

---

_Closed by @charliermarsh on 2025-02-20 20:13_

---

_Closed by @charliermarsh on 2025-02-20 20:13_

---

_Comment by @notatallshaw on 2025-02-20 21:45_

FYI, this will be a behavior difference with pip, which unfortunately reads in constraints files specified inside requirements files like this as though they were requirement files 😞

---

_Comment by @charliermarsh on 2025-02-20 21:46_

Sorry, what do you mean?

---

_Comment by @charliermarsh on 2025-02-20 21:47_

(I don't think this is a behavior change; it just allows users to surround things we already supported in quotes.)

---

_Comment by @notatallshaw on 2025-02-21 16:20_

Apologies, I misunderstood the request, as you say this created no behavior difference for uv 

---
