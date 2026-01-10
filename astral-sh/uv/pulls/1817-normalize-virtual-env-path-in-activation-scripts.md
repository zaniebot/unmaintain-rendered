```yaml
number: 1817
title: "Normalize `VIRTUAL_ENV` path in activation scripts"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: normalize-venv-path
created_at: 2024-02-21T15:48:18Z
updated_at: 2024-02-21T15:52:34Z
url: https://github.com/astral-sh/uv/pull/1817
synced_at: 2026-01-10T15:33:24Z
```

# Normalize `VIRTUAL_ENV` path in activation scripts

---

_Pull request opened by @MichaReiser on 2024-02-21 15:48_

## Summary

Fixes https://github.com/astral-sh/uv/issues/1763

This PR fixes the issue where the Path to the virtual environment used NT Paths thar are unsupported in CMD on Win 10 (I can't repro on win11) by normalizing the path. 

## Test Plan

I can't reproduce the mentioned issue locally because it works fine under Win 11 and I don't have access to Win 10. 

But I verified that the path in the generated `activation.bat` isn't an NT Path. 

![grafik](https://github.com/astral-sh/uv/assets/1203881/0b7f1a9b-eb66-4685-9a40-ea5e96aab040)

I verified that I can activate the virtual env in CMD and Powershell by running the activation scripts.


---

_@charliermarsh approved on 2024-02-21 15:48_

---

_Label `bug` added by @charliermarsh on 2024-02-21 15:48_

---

_Label `windows` added by @charliermarsh on 2024-02-21 15:48_

---

_Merged by @MichaReiser on 2024-02-21 15:52_

---

_Closed by @MichaReiser on 2024-02-21 15:52_

---

_Branch deleted on 2024-02-21 15:52_

---
