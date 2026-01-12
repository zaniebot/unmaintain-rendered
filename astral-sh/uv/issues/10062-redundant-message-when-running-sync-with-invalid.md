```yaml
number: 10062
title: Redundant message when running sync with invalid dependencies and optional dependencies
type: issue
state: open
author: mirober
labels:
  - error messages
assignees: []
created_at: 2024-12-20T17:17:31Z
updated_at: 2024-12-20T17:40:04Z
url: https://github.com/astral-sh/uv/issues/10062
synced_at: 2026-01-12T16:00:06Z
```

# Redundant message when running sync with invalid dependencies and optional dependencies

---

_@mirober_

UV version: 0.5.9

Repro pyproject.toml:

```
[project]
name = "my-project"
version = "0.1.0"
description = "...."
requires-python = ">=3.10"
readme = "README.md"
dependencies = [
    "pydantic>=7",
]

[project.optional-dependencies]
dev = [
    "ruff>=0.6",
]
```

pydantic only has up to version 2, so this should fail, and it does, but it fails with the following message:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only pydantic<=2.10.4 is available and your project depends on pydantic>=7, we can conclude that your project's requirements are unsatisfiable.
      And because your project requires my-project[dev], we can conclude that your project's requirements are unsatisfiable.
```

The second sentence here seems redundant and reads oddly.

Removing the optional dependencies removes the redundant line:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only pydantic<=2.10.4 is available and your project depends on pydantic>=7, we can conclude that your project's requirements are unsatisfiable.
```

I would consider also removing 'we can conclude that' - seems unnecessary.

This is the biggest issue I have found in 2 days of converting projects to use UV, so great job and thank you!

---

_Label `error messages` added by @zanieb on 2024-12-20 17:26_

---

_Assigned to @zanieb by @zanieb on 2024-12-20 17:26_

---

_Comment by @zanieb on 2024-12-20 17:28_

Thanks for the report!

Glad you're enjoying the tool :)

---

_Comment by @zanieb on 2024-12-20 17:40_

For my own reference, the derivation tree is

```
Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on my-project[dev]*
  term my-project[dev]*
    term my-project==0.1.0
      my-project==0.1.0 depends on pydantic>=7
      no versions of pydantic>=7
    no versions of my-project[dev]<0.1.0 | >0.1.0
Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on my-project[dev]*
  term my-project==0.1.0
    my-project==0.1.0 depends on pydantic>=7
    no versions of pydantic>=7
```

The clause `root==0a0.dev0 depends on my-project[dev]*` is useless and wrong? It's possible there's a bug in the resolver here.

---
