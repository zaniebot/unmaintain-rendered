---
number: 14005
title: default-groups are not scoped to their package
type: issue
state: open
author: Gankra
labels:
  - bug
assignees: []
created_at: 2025-06-12T20:25:59Z
updated_at: 2025-06-13T13:10:14Z
url: https://github.com/astral-sh/uv/issues/14005
synced_at: 2026-01-07T13:12:18-06:00
---

# default-groups are not scoped to their package

---

_Issue opened by @Gankra on 2025-06-12 20:25_

### Summary

In a workspace if I have the root project with:

```toml
[dependency-groups]
alpha = ["pytest"]
beta = ["iniconfig"]

[tool.uv]
default-groups = ["alpha"]
```

and a child with

```toml
[dependency-groups]
alpha = ["sniffio"]
beta = ["sortedcontainers"]

[tool.uv]
default-groups = ["beta"]
```

`uv sync --all-packages` will install sniffio (child.alpha)
`uv sync --package child` will install sortedcontainers (child.beta)

This is because we only ever read default groups *once* from *a single pyproject.toml* (whichever we consider "the main one" for the operation) and then apply those *group names* to every single package the operation selects.

### Platform

all

### Version

all

### Python version

_No response_

---

_Label `bug` added by @Gankra on 2025-06-12 20:26_

---

_Assigned to @Gankra by @Gankra on 2025-06-12 20:26_

---

_Comment by @Gankra on 2025-06-12 20:26_

Fixing this is a ton of work, unfortunately, and also a breaking change since people could accidentally be relying on it.

---

_Comment by @Gankra on 2025-06-12 20:27_

Thankfully the happy path of just using `dev` as your default-group for all packages is fine here.

---

_Comment by @Gankra on 2025-06-13 13:09_

I think maybe the abstraction is fine we just need to lookup default-groups as late as possible -- i.e. when we're specifically applying them to a package, grab the package's default groups and apply them.

---

_Comment by @Gankra on 2025-06-13 13:10_

Although this will require passing down whether defaults are enabled by the interface we're using, so maybe the abstraction needs to change a bit in that regard.

---

_Referenced in [astral-sh/uv#14377](../../astral-sh/uv/issues/14377.md) on 2025-06-30 21:34_

---
