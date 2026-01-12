```yaml
number: 5071
title: "`uvx` warn when executable is not part of `--from PKG`"
type: pull_request
state: merged
author: blueraft
labels: []
assignees: []
merged: true
base: main
head: uvx-warn-outside-from
created_at: 2024-07-15T13:44:43Z
updated_at: 2024-07-15T19:27:10Z
url: https://github.com/astral-sh/uv/pull/5071
synced_at: 2026-01-12T16:06:37Z
```

# `uvx` warn when executable is not part of `--from PKG`

---

_@blueraft_

## Summary

Resolves #5017.

Note: This re-uses the same function defined in #5019 to find matching packages.

## Test Plan

`cargo test`

```console
❯ ./target/debug/uvx --from fastapi fastapi
warning: `uvx` is experimental and may change without warning.
Resolved 33 packages in 427ms
warning: The fastapi executable is not part of the fastapi package. It is provided by the fastapi-cli package. Use `uvx --from fastapi-cli fastapi` instead.
Usage: fastapi [OPTIONS] COMMAND [ARGS]...
Try 'fastapi --help' for help.
```


---

_@charliermarsh reviewed on 2024-07-15 13:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:142 on 2024-07-15 13:55_

I think this should probably run regardless of whether the user explicitly passed `--from`. E.g., `uv tool run fastapi` should also show this warning, right?

---

_@blueraft reviewed on 2024-07-15 14:13_

---

_Review comment by @blueraft on `crates/uv/src/commands/tool/run.rs`:142 on 2024-07-15 14:13_

Ah yes, that's true. Done.

---

_Comment by @zanieb on 2024-07-15 19:18_

Some small changes in https://github.com/astral-sh/uv/pull/5071/commits/a169aae0dd710263a23cb699f2325be8511bb342 — as before lmk if you have questions.

---

_Merged by @zanieb on 2024-07-15 19:27_

---

_Closed by @zanieb on 2024-07-15 19:27_

---
