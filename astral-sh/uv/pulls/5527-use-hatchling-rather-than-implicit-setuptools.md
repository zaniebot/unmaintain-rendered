```yaml
number: 5527
title: "Use `hatchling` rather than implicit `setuptools` default"
type: pull_request
state: merged
author: charliermarsh
labels:
  - needs-decision
  - preview
assignees: []
merged: true
base: main
head: charlie/hatchling
created_at: 2024-07-28T21:30:41Z
updated_at: 2024-07-29T14:00:13Z
url: https://github.com/astral-sh/uv/pull/5527
synced_at: 2026-01-10T13:37:23Z
```

# Use `hatchling` rather than implicit `setuptools` default

---

_Pull request opened by @charliermarsh on 2024-07-28 21:30_

## Summary

Closes https://github.com/astral-sh/uv/issues/5461.


---

_Label `needs-decision` added by @charliermarsh on 2024-07-28 21:30_

---

_Label `preview` added by @charliermarsh on 2024-07-28 21:30_

---

_Comment by @charliermarsh on 2024-07-28 21:30_

@mitsuhiko -- What's your feeling on this based on your experience in Rye?

---

_Review requested from @zanieb by @charliermarsh on 2024-07-28 23:18_

---

_Review requested from @konstin by @charliermarsh on 2024-07-28 23:18_

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:273 on 2024-07-29 09:04_

Isn't that implicit?

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:269 on 2024-07-29 09:10_

With this pick, we're defaulting to making hatch's features and configuration part of our public api, we'll probably have to commit to making a good hatch interaction when configuration or features collide.

---

_@konstin approved on 2024-07-29 09:10_

---

_@helderco reviewed on 2024-07-29 11:02_

---

_Review comment by @helderco on `crates/uv/src/commands/project/init.rs`:273 on 2024-07-29 11:02_

It is, if it's a package (which seems to be the case). See: https://github.com/dagger/dagger/blob/124ce8215eef97a4aa4555608ad95edd107f9626/core/integration/module_python_test.go#L148-L161

But not if it's a module. See: https://github.com/dagger/dagger/blob/124ce8215eef97a4aa4555608ad95edd107f9626/core/integration/module_python_test.go#L162-L177

---

_@helderco reviewed on 2024-07-29 11:04_

---

_Review comment by @helderco on `crates/uv/src/commands/project/init.rs`:269 on 2024-07-29 11:04_

Yeah, I think this doesn't need to be in the initial template, just keep the build-backend. This is what I've been using: https://github.com/dagger/dagger/blob/124ce8215eef97a4aa4555608ad95edd107f9626/sdk/python/runtime/template/pyproject.toml

---

_Review comment by @bluss on `crates/uv/src/commands/project/init.rs`:273 on 2024-07-29 12:42_

Rye had a reason for this one https://github.com/astral-sh/rye/issues/515 (just for info)

---

_@bluss reviewed on 2024-07-29 12:42_

---

_Merged by @charliermarsh on 2024-07-29 14:00_

---

_Closed by @charliermarsh on 2024-07-29 14:00_

---

_Branch deleted on 2024-07-29 14:00_

---
