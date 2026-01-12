```yaml
number: 4194
title: "universal-lock: ignore dependencies that are disjoint with a forked resolver state's marker expressions"
type: issue
state: closed
author: BurntSushi
labels: []
assignees: []
created_at: 2024-06-10T12:49:41Z
updated_at: 2024-06-17T13:30:38Z
url: https://github.com/astral-sh/uv/issues/4194
synced_at: 2026-01-12T15:58:48Z
```

# universal-lock: ignore dependencies that are disjoint with a forked resolver state's marker expressions

---

_@BurntSushi_

This is very similar to https://github.com/astral-sh/uv/issues/4102, but for general marker expressions. For example, if a resolver state was forked via a dependency specification like `quux ; sys_platform == "linux"`, and then we come across a dependency specification like `foo ; sys_platform == "windows"`, then we should ignore that dependency entirely in that specific fork of the resolver because it can never happen.

---

_Assigned to @BurntSushi by @BurntSushi on 2024-06-10 12:49_

---

_Closed by @BurntSushi on 2024-06-17 13:30_

---
