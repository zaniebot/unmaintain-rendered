```yaml
number: 5639
title: Support build constraints
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: build-constraints
created_at: 2024-07-30T22:06:10Z
updated_at: 2024-08-02T07:59:39Z
url: https://github.com/astral-sh/uv/pull/5639
synced_at: 2026-01-12T16:06:55Z
```

# Support build constraints

---

_@blueraft_

## Summary

Partially resolves #5561. Haven't added overrides support yet but I can add it tomorrow if the current approach for constraints is ok.

## Test Plan

`cargo test`

Manually checked trace logs after changing the constraints.

---

_@blueraft reviewed on 2024-07-30 22:07_

---

_Review comment by @blueraft on `crates/uv/tests/pip_install.rs`:6295 on 2024-07-30 22:07_

I wasn't sure how to check if the built constraints were being respected for the compatible versions - this just check if the package gets resolved correctly.

---

_@T-256 reviewed on 2024-07-30 22:45_

---

_Review comment by @T-256 on `crates/uv/tests/pip_install.rs`:6295 on 2024-07-30 22:45_

isn't there anything via verbose flag?

---

_@blueraft reviewed on 2024-07-31 09:50_

---

_Review comment by @blueraft on `crates/uv/tests/pip_install.rs`:6295 on 2024-07-31 09:50_

It's there in the verbose logs, but I don't know if we should use the verbose logs for snapshot tests. 

---

_Assigned to @charliermarsh by @zanieb on 2024-07-31 13:12_

---

_Label `enhancement` added by @charliermarsh on 2024-07-31 16:36_

---

_Comment by @charliermarsh on 2024-07-31 17:28_

Thanks! Will review.

---

_@paveldikov reviewed on 2024-08-01 16:56_

---

_Review comment by @paveldikov on `crates/uv/src/commands/pip/compile.rs`:1 on 2024-08-01 16:56_

Does `uv pip compile` now include build dependencies in its output? Or do `--build-constraints` files have to be generated separately (how?)

---

_@charliermarsh reviewed on 2024-08-01 18:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:1 on 2024-08-01 18:46_

I don't believe this PR adds any way to lock the build resolution, only to constrain it, but we want to add that to `uv.lock` eventually.

---

_Comment by @hauntsaninja on 2024-08-01 19:30_

Not using `UV_CONSTRAINT` for build constraints should probably listed in the pip compatibility docs. Something like:

For constraining build time dependencies:
- pip lets you use `PIP_CONSTRAINT` environment variable to point to a constraints file. There is no CLI argument
- uv lets you use `UV_BUILD_CONSTRAINT` environment variable or a `--build-constraint` CLI argument

---

_Comment by @charliermarsh on 2024-08-01 19:30_

Makes sense, good call @hauntsaninja.

---

_@blueraft reviewed on 2024-08-01 19:43_

---

_Review comment by @blueraft on `crates/uv/src/commands/pip/compile.rs`:1 on 2024-08-01 19:43_

Related issue #5190 

---

_Comment by @charliermarsh on 2024-08-01 21:32_

This overall looks good but I have to find a way to fix the stack size issues on Windows (nothing wrong with the PR specifically, it's an ongoing problem) -- and update the docs.

---

_@charliermarsh approved on 2024-08-02 02:04_

---

_Merged by @charliermarsh on 2024-08-02 02:15_

---

_Closed by @charliermarsh on 2024-08-02 02:15_

---

_Branch deleted on 2024-08-02 07:59_

---
