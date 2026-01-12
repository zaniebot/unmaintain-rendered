```yaml
number: 3016
title: "Use link mode for builds, in `uv pip compile` and for `uv venv` seed packages"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/link-mode-for-builds-too
created_at: 2024-04-12T14:56:17Z
updated_at: 2024-04-15T08:49:42Z
url: https://github.com/astral-sh/uv/pull/3016
synced_at: 2026-01-12T16:05:22Z
```

# Use link mode for builds, in `uv pip compile` and for `uv venv` seed packages

---

_@konstin_

Use the user specified link mode for temporary build venvs, too. It seems consistent to respect the user's link mode for all installations we perform

---

_Label `enhancement` added by @konstin on 2024-04-12 14:56_

---

_Review requested from @charliermarsh by @konstin on 2024-04-12 14:56_

---

_@charliermarsh reviewed on 2024-04-12 15:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:267 on 2024-04-12 15:01_

Yeah, can you add?

---

_@charliermarsh reviewed on 2024-04-12 15:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:201 on 2024-04-12 15:01_

Yeah, can you add?

---

_@charliermarsh approved on 2024-04-12 15:01_

---

_Renamed from "Use link mode for builds too" to "Use link mode for builds, in `uv pip compile` and for `uv venv` seed packages" by @konstin on 2024-04-15 08:36_

---

_Merged by @konstin on 2024-04-15 08:49_

---

_Closed by @konstin on 2024-04-15 08:49_

---

_Branch deleted on 2024-04-15 08:49_

---
