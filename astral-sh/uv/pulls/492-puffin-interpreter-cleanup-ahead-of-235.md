```yaml
number: 492
title: "puffin_interpreter cleanup ahead of #235"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: puffin-interpreter-refacotring
created_at: 2023-11-23T08:53:42Z
updated_at: 2023-11-23T08:57:34Z
url: https://github.com/astral-sh/uv/pull/492
synced_at: 2026-01-10T15:44:44Z
```

# puffin_interpreter cleanup ahead of #235

---

_Pull request opened by @konstin on 2023-11-23 08:53_

Preparing for #235, some refactoring to `puffin_interpreter`.

* Added a dedicated error type instead of anyhow
* `InterpreterInfo` -> `Interpreter`
* `detect_virtual_env` now returns an option so it can be chained for #235

---

_Merged by @konstin on 2023-11-23 08:57_

---

_Closed by @konstin on 2023-11-23 08:57_

---

_Branch deleted on 2023-11-23 08:57_

---
