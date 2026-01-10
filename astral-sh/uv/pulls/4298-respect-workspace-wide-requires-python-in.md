```yaml
number: 4298
title: "Respect workspace-wide `requires-python` in interpreter selection"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/virtual
created_at: 2024-06-13T02:38:33Z
updated_at: 2024-06-13T12:55:58Z
url: https://github.com/astral-sh/uv/pull/4298
synced_at: 2026-01-10T13:54:02Z
```

# Respect workspace-wide `requires-python` in interpreter selection

---

_Pull request opened by @charliermarsh on 2024-06-13 02:38_

## Summary

Closes https://github.com/astral-sh/uv/issues/4296.

## Test Plan

Ran `cargo run lock --verbose` from `scripts/workspaces/albatross-virtual-workspace`:

```
DEBUG uv 0.2.11 (ef3bc1612 2024-06-12)
warning: `uv lock` is experimental and may change without warning.
DEBUG Found workspace root: `/Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-virtual-workspace`
DEBUG Adding discovered workspace member: /Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-virtual-workspace/packages/albatross
DEBUG Adding discovered workspace member: /Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder
DEBUG Adding discovered workspace member: /Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-virtual-workspace/packages/seeds
DEBUG Searching for Python >=3.12 in search path or managed toolchains
DEBUG Searching for managed toolchains at `/Users/crmarsh/Library/Application Support/uv/toolchains`
DEBUG Found managed toolchain `cpython-3.12.3-macos-aarch64-none`
DEBUG Found CPython 3.12.3 at `/Users/crmarsh/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3` (managed toolchains)
Using Python 3.12.3 interpreter at: /Users/crmarsh/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3
```


---

_Label `bug` added by @charliermarsh on 2024-06-13 02:38_

---

_Label `preview` added by @charliermarsh on 2024-06-13 02:38_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-13 02:38_

---

_Review requested from @konstin by @charliermarsh on 2024-06-13 02:38_

---

_Comment by @charliermarsh on 2024-06-13 02:39_

Something that needs coverage as part of https://github.com/astral-sh/uv/issues/3943.

---

_@zanieb reviewed on 2024-06-13 12:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:160 on 2024-06-13 12:43_

We could (maybe should) just take `RequiresPython` in `interpreter_meets_requirements`. I only made it take the specifiers because that was what we had around.

---

_@zanieb reviewed on 2024-06-13 12:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:160 on 2024-06-13 12:43_

I guess this makes it easier to call with a custom requirement in the future?

---

_@zanieb approved on 2024-06-13 12:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:160 on 2024-06-13 12:49_

Good call.

---

_@charliermarsh reviewed on 2024-06-13 12:49_

---

_Merged by @charliermarsh on 2024-06-13 12:55_

---

_Closed by @charliermarsh on 2024-06-13 12:55_

---

_Branch deleted on 2024-06-13 12:55_

---
