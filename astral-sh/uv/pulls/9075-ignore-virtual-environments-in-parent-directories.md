```yaml
number: 9075
title: Ignore virtual environments in parent directories when choosing Python version for new projects
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/init-venv
created_at: 2024-11-13T01:22:50Z
updated_at: 2024-11-13T03:53:59Z
url: https://github.com/astral-sh/uv/pull/9075
synced_at: 2026-01-12T16:08:38Z
```

# Ignore virtual environments in parent directories when choosing Python version for new projects

---

_@zanieb_

`uv init` shouldn't have been using `EnvironmentPreference::Any` for discovery of a Python interpreter, it seems like an oversight that it was reading from virtual environments. I changed it to `EnvironmentPreference::OnlySystem` so we'll use the first Python on the `PATH` instead. However, I think we actually do want to respect a virtual environment's Python version if it's in the target project directory already, so I've implemented that as well.

Closes https://github.com/astral-sh/uv/issues/9072
Closes https://github.com/astral-sh/uv/issues/8092

---

_Label `bug` added by @zanieb on 2024-11-13 01:22_

---

_@charliermarsh approved on 2024-11-13 02:56_

Nice.

---

_Merged by @zanieb on 2024-11-13 03:53_

---

_Closed by @zanieb on 2024-11-13 03:53_

---

_Branch deleted on 2024-11-13 03:53_

---
