```yaml
number: 225
title: Provide pre-comit actions that does not require cargo crate run 
type: issue
state: closed
author: Czaki
labels:
  - bug
assignees: []
created_at: 2022-09-19T11:58:45Z
updated_at: 2022-09-20T03:06:40Z
url: https://github.com/astral-sh/ruff/issues/225
synced_at: 2026-01-12T15:54:40Z
```

# Provide pre-comit actions that does not require cargo crate run 

---

_@Czaki_

The current pre-commit hooks require cargo and may fail. As ruff provides wheels on pypi it should be easy to create a separate repository with a dummy package that will only have `ruff` in dependencies and simplify adoptation of this hook (as currently, all project contributors need to have cargo setup in the system). 

I would like to propose adding this hook (maybe even replacing existing flake8 calls) in a few projects, but this is blocking me. 

---

_Assigned to @charliermarsh by @charliermarsh on 2022-09-20 01:41_

---

_Label `bug` added by @charliermarsh on 2022-09-20 01:41_

---

_Closed by @charliermarsh on 2022-09-20 03:06_

---
