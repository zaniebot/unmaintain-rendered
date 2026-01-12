```yaml
number: 2682
title: Add an in-memory cache for Git references
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/double-git
created_at: 2024-03-27T01:28:28Z
updated_at: 2024-03-27T01:39:02Z
url: https://github.com/astral-sh/uv/pull/2682
synced_at: 2026-01-12T16:05:10Z
```

# Add an in-memory cache for Git references

---

_@charliermarsh_

## Summary

Ensures that, even if we try to resolve the same Git reference twice within an invocation, it always returns a (cached) consistent result.

Closes https://github.com/astral-sh/uv/issues/2673.

## Test Plan

```
❯ cargo run pip install git+https://github.com/pallets/flask.git --reinstall --no-cache
   Compiling uv-distribution v0.0.1 (/Users/crmarsh/workspace/uv/crates/uv-distribution)
   Compiling uv-resolver v0.0.1 (/Users/crmarsh/workspace/uv/crates/uv-resolver)
   Compiling uv-installer v0.0.1 (/Users/crmarsh/workspace/uv/crates/uv-installer)
   Compiling uv-dispatch v0.0.1 (/Users/crmarsh/workspace/uv/crates/uv-dispatch)
   Compiling uv-requirements v0.1.0 (/Users/crmarsh/workspace/uv/crates/uv-requirements)
   Compiling uv v0.1.24 (/Users/crmarsh/workspace/uv/crates/uv)
    Finished dev [unoptimized + debuginfo] target(s) in 3.95s
     Running `target/debug/uv pip install 'git+https://github.com/pallets/flask.git' --reinstall --no-cache`
 Updated https://github.com/pallets/flask.git (b90a4f1)
Resolved 7 packages in 280ms
   Built flask @ git+https://github.com/pallets/flask.git@b90a4f1f4a370e92054b9cc9db0efcb864f87ebe                                                                                                                                            Downloaded 7 packages in 212ms
Installed 7 packages in 9ms
```

---

_Label `performance` added by @charliermarsh on 2024-03-27 01:28_

---

_Marked ready for review by @charliermarsh on 2024-03-27 01:28_

---

_Merged by @charliermarsh on 2024-03-27 01:39_

---

_Closed by @charliermarsh on 2024-03-27 01:39_

---

_Branch deleted on 2024-03-27 01:39_

---
