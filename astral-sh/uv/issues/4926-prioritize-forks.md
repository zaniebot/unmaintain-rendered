```yaml
number: 4926
title: Prioritize forks 
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-09T12:34:10Z
updated_at: 2024-07-31T15:05:14Z
url: https://github.com/astral-sh/uv/issues/4926
synced_at: 2026-01-12T15:58:53Z
```

# Prioritize forks 

---

_@konstin_

Say we have requirements `a >= 1 ; sys_platform == 'linux'` and `a < 2 ; sys_platform == 'darwin'`. There's two possible solutions: `a==1` and `a==2`, and only `a==1` for both forks.

Currently, it's order dependent:

```toml
dependencies = [
  'a<2; sys_platform == "darwin"',
  'a>=1; sys_platform == "linux"',
]
```
resolves 3 packages (`a==1` and `a==2`),
```toml
dependencies = [
  'a>=1; sys_platform == "linux"',
  'a<2; sys_platform == "darwin"',
]
```
resolves 2 packages (only `a==1`).

When we resolve for latest versions (the default), we should process the fork `a<2` first, since it will resolve to a lower version that the other fork accepts, while `a >= 1` resolves to a preference (`a==2`) that `a<2` rejects. When we resolve for lowest versions, it's the inverse. While either way is correct, only `a==1` will give us a better (smaller) solution that is consistent between forks.

The same goes for requires-python: A fork with `python_version >= "3.10"`  needs to run before `python_version >= "3.12"` , since the former allows less version than the latter; here it's independent of the version order.

We need to figure out the exact rules for prioritization considering highest vs. lowest versions, requires-python, lower vs. upper bounds and handling splitting over multiple packages.

---

_Label `preview` added by @konstin on 2024-07-09 12:34_

---

_Comment by @charliermarsh on 2024-07-09 16:18_

Agree with this. I actually think that (if done right) this would've prevented the problematic resolution in https://github.com/astral-sh/uv/issues/4885. (I don't know if it would solve that problem _in general_ though.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 12:59_

---

_Label `enhancement` added by @charliermarsh on 2024-07-30 12:59_

---

_Closed by @charliermarsh on 2024-07-31 15:05_

---

_Closed by @charliermarsh on 2024-07-31 15:05_

---
