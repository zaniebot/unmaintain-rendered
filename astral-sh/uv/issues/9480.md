```yaml
number: 9480
title: "Mismatched name errors aren't detected when builds are cached"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-11-27T17:32:34Z
updated_at: 2024-11-27T21:51:02Z
url: https://github.com/astral-sh/uv/issues/9480
synced_at: 2026-01-10T04:36:21Z
```

# Mismatched name errors aren't detected when builds are cached

---

_Issue opened by @charliermarsh on 2024-11-27 17:32_

Given:

```
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = [
    "repro-uv-8148-a>=1.0.1",
]

[tool.uv]
override-dependencies = ["repro-uv-8148-a"]

[tool.uv.sources]
repro-uv-8148-a = { path = ".", editable = true }
```

You get:

```
❯ cargo run sync
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `/Users/crmarsh/workspace/uv/target/debug/uv sync`
Resolved 2 packages in 6ms
  × Failed to build `repro-uv-8148-a @ file:///Users/crmarsh/workspace/uv/foo`
  ╰─▶ Package metadata name `foo` does not match given name `repro-uv-8148-a`
  help: `repro-uv-8148-a` was included because `foo` (v0.1.0) depends on `repro-uv-8148-a`
```

But if you re-run...

```
❯ cargo run sync
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.16s
     Running `/Users/crmarsh/workspace/uv/target/debug/uv sync`
Resolved 2 packages in 6ms
Installed 1 package in 3ms
 + foo==0.1.0 (from file:///Users/crmarsh/workspace/uv/foo)
```


---

_Label `bug` added by @charliermarsh on 2024-11-27 17:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-27 17:44_

---

_Closed by @charliermarsh on 2024-11-27 21:51_

---
