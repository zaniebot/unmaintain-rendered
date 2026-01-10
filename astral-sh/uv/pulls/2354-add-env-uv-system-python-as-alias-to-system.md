```yaml
number: 2354
title: Add env UV_SYSTEM_PYTHON as alias to --system
type: pull_request
state: merged
author: edenhaus
labels:
  - configuration
assignees: []
merged: true
base: main
head: system-env
created_at: 2024-03-11T09:52:03Z
updated_at: 2024-03-12T08:08:03Z
url: https://github.com/astral-sh/uv/pull/2354
synced_at: 2026-01-10T14:49:08Z
```

# Add env UV_SYSTEM_PYTHON as alias to --system

---

_Pull request opened by @edenhaus on 2024-03-11 09:52_

## Summary

Add a new env variable `UV_SYSTEM` as alias for the cli argument
`--system`.
Use cases:
- No need to specify on each uv call inside the docker container the
`--system` flag
- It allows installing and configuring uv in a base container and in the
child containers nobody needs to know if you need the `--system` cli
flag
- The Home Assistant development env can be set up via devcontainer or a
venv. Both use some common scripts. Instead of adding duplicate or
special code to identify the dev container to set the `--system` flag,
it would be nicer to set it via an env variable.

I'm unfamiliar with Rust and tried to add the support by looking at the
code.

## Test Plan

I did test it manually
`UV_SYSTEM_PYTHON=true uv pip install requests`


---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-11 14:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-11 14:21_

---

_Label `configuration` added by @charliermarsh on 2024-03-11 14:21_

---

_Marked ready for review by @edenhaus on 2024-03-11 14:46_

---

_@charliermarsh approved on 2024-03-11 20:52_

Thanks! I changed it to `UV_SYSTEM_PYTHON`, since `UV_SYSTEM` felt a little too generic. I know it deviates from the CLI but I think it's worth it.

---

_Merged by @charliermarsh on 2024-03-11 20:56_

---

_Closed by @charliermarsh on 2024-03-11 20:56_

---

_Renamed from "Add env UV_SYSTEM as alias to --system" to "Add env UV_SYSTEM_PYTHON as alias to --system" by @konstin on 2024-03-11 20:58_

---

_Branch deleted on 2024-03-12 08:08_

---
