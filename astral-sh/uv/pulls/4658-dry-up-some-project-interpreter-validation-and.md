```yaml
number: 4658
title: DRY up some project interpreter validation and discovery
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - preview
assignees: []
merged: true
base: main
head: charlie/python-request
created_at: 2024-06-30T15:13:01Z
updated_at: 2024-07-01T12:31:43Z
url: https://github.com/astral-sh/uv/pull/4658
synced_at: 2026-01-10T13:48:28Z
```

# DRY up some project interpreter validation and discovery

---

_Pull request opened by @charliermarsh on 2024-06-30 15:13_

## Summary

I noticed that `init_environment` and `find_interpreter` were both calling `find_environment`, which seemed like a code smell to me. Instead, `find_interpreter` now returns either a compatible environment or an interpreter (if no compatible environment was found).

Additionally, `interpreter_meets_requirements` now no longer validates `requires-python` if `--python` or `.python-version` is set. Instead, we warn, which matches the behavior we get when creating a new environment at the bottom of `find_interpreter`.

In total, I think this makes the data flow in project interpreter discovery less repetitive and easier to reason about.


---

_Label `internal` added by @charliermarsh on 2024-06-30 15:13_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-30 15:14_

---

_Label `preview` added by @charliermarsh on 2024-06-30 15:14_

---

_@charliermarsh reviewed on 2024-06-30 15:22_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:139 on 2024-06-30 15:22_

Maybe this demonstrates that we should just be erroring early? Because if the interpret doesn't meet the lockfile requirements, we error out.

---

_Review requested from @konstin by @charliermarsh on 2024-06-30 15:22_

---

_@zanieb reviewed on 2024-06-30 16:57_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:139 on 2024-06-30 16:57_

Yeah I saw this elsewhere too we probably shouldn't delete the virtual environment if we're going to fail anyway.

---

_@charliermarsh reviewed on 2024-06-30 18:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:139 on 2024-06-30 18:14_

Ok, we can change that.

---

_@konstin approved on 2024-07-01 08:10_

---

_Merged by @charliermarsh on 2024-07-01 12:31_

---

_Closed by @charliermarsh on 2024-07-01 12:31_

---

_Branch deleted on 2024-07-01 12:31_

---
