---
number: 4600
title: flake8-to-ruff ignores nursery rules
type: issue
state: closed
author: ThiefMaster
labels: []
assignees: []
created_at: 2023-05-23T13:51:07Z
updated_at: 2024-01-02T18:57:18Z
url: https://github.com/astral-sh/ruff/issues/4600
synced_at: 2026-01-07T13:12:14-06:00
---

# flake8-to-ruff ignores nursery rules

---

_Issue opened by @ThiefMaster on 2023-05-23 13:51_

I got warnings about unsupported rules during the conversion of [my flake8 config](https://raw.githubusercontent.com/indico/indico/master/.flake8), one of them being `E265` which is in fact supported, it's just in the "nursery" category.

I think showing a (different) warning makes sense, but removing them in the converted config seems like a but or at least a rather bad idea. In fact, maybe this should be taken as an indicator to explicitly select such a rule (in my case I had a specific per-file ignore for `E265` which implies that I want to use it elsewhere).

```
flake8-to-ruff==0.0.233
ruff==0.0.269
```


---

_Label `flake8-to-ruff` added by @charliermarsh on 2023-05-24 01:22_

---

_Comment by @charliermarsh on 2024-01-02 18:57_

(Closing as we've deprecated `flake8-to-ruff`.)

---

_Closed by @charliermarsh on 2024-01-02 18:57_

---
