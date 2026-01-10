```yaml
number: 15308
title: "Support UV_PYTHON_PLATFORM when executing `uv sync`"
type: pull_request
state: open
author: acehinnnqru
labels: []
assignees: []
base: main
head: main
created_at: 2025-08-15T13:26:12Z
updated_at: 2025-09-23T06:37:06Z
url: https://github.com/astral-sh/uv/pull/15308
synced_at: 2026-01-10T06:36:15Z
```

# Support UV_PYTHON_PLATFORM when executing `uv sync`

---

_Pull request opened by @acehinnnqru on 2025-08-15 13:26_

Closes #15241.

## Summary

This PR only adds support of setting env `UV_PYTHON_PLATFORM` for the command `uv sync`. I am not familiar with other commands' `--python-platform` and tests.

If it's better to support other commands, I will update this PR.

## Test Plan

Add a new env test in `sync_python_platform`.


---

_@zanieb reviewed on 2025-08-15 13:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3694 on 2025-08-15 13:28_

I think we'd need to respect this everywhere, e.g., https://github.com/astral-sh/uv/blob/f6a9b55eb73be4f1fb9831362a192cdd8312ab96/crates/uv-cli/src/lib.rs#L2061 too

---

_@zanieb reviewed on 2025-08-15 13:29_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:535 on 2025-08-15 13:29_

We'll need to think about the name here, because `UV_PYTHON` is generally the namespace for settings that affect Python versions, e.g., in the `uv python` interface. I think we'll need something else here... 🤔 

---

_@acehinnnqru reviewed on 2025-08-15 13:44_

---

_Review comment by @acehinnnqru on `crates/uv-cli/src/lib.rs`:3694 on 2025-08-15 13:44_

Yeah, my first version was to add it everywhere but I don't know where to add the tests..

---

_Converted to draft by @acehinnnqru on 2025-08-15 13:46_

---

_@zanieb reviewed on 2025-08-15 14:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3694 on 2025-08-15 14:07_

We don't need to test every single invocation. These immediately map to be equivalent to the CLI flag.

---

_Review comment by @acehinnnqru on `crates/uv-static/src/env_vars.rs`:535 on 2025-08-16 01:48_

Maybe `UV_PLATFORM` or something like `UV_DEPLOYMENT_TARGET`, but it's a bit irrelevant to the flag `--python-platform`

---

_@acehinnnqru reviewed on 2025-08-16 01:48_

---

_@zanieb reviewed on 2025-08-16 12:31_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:535 on 2025-08-16 12:31_

I guess we could do `UV_SYNC_PYTHON_PLATFORM` and `UV_PIP_PYTHON_PLATFORM`, but I don't love splitting them up. `UV_TARGET_PYTHON_PLATFORM` seems okay? @charliermarsh I'd appreciate your input here.

---

_Renamed from "Support UV_PYTHON_PLATFORM when executing `uv sync`" to "Support UV_TARGET_PYTHON_PLATFORM when executing `uv sync`" by @acehinnnqru on 2025-08-19 12:38_

---

_Marked ready for review by @acehinnnqru on 2025-08-19 12:43_

---

_@acehinnnqru reviewed on 2025-08-19 12:45_

---

_Review comment by @acehinnnqru on `crates/uv-static/src/env_vars.rs`:535 on 2025-08-19 12:45_

I think UV_TARGET_PYTHON_PLATFORM is OK. It would be used for a long time, so it may require a second-thought.

---

_@charliermarsh reviewed on 2025-09-06 02:45_

---

_Review comment by @charliermarsh on `crates/uv-static/src/env_vars.rs`:535 on 2025-09-06 02:45_

I guess I still sort of prefer `UV_PYTHON_PLATFORM` since it matches the command-line argument, and it's probably what users will reach for naturally? I understand the hesitation though.

---

_Comment by @harshithvh on 2025-09-12 17:37_

@zanieb 
The community appears to have accepted `UV_PYTHON_PLATFORM`.
should be good to go?

---

_Renamed from "Support UV_TARGET_PYTHON_PLATFORM when executing `uv sync`" to "Support UV_PYTHON_PLATFORM when executing `uv sync`" by @acehinnnqru on 2025-09-23 06:35_

---

_Comment by @acehinnnqru on 2025-09-23 06:37_

I have updated my PR which uses UV_PYTHON_PLATFORM this time.

---
