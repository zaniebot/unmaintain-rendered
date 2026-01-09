---
number: 9362
title: "Indicate in workspaces' doc that workspace dependencies are editable by default "
type: issue
state: open
author: ReinforcedKnowledge
labels:
  - documentation
assignees: []
created_at: 2024-11-22T18:57:31Z
updated_at: 2024-11-23T02:17:45Z
url: https://github.com/astral-sh/uv/issues/9362
synced_at: 2026-01-07T13:12:18-06:00
---

# Indicate in workspaces' doc that workspace dependencies are editable by default 

---

_Issue opened by @ReinforcedKnowledge on 2024-11-22 18:57_

Hi!

I have just noticed that dependencies between workspace members are editable by default.

Steps:
```bash
uv init --package my_package
cd my_package/src/my_package
uv init --lib core_lib
uv init --package cli
cd cli
```

Whether we do `uv add ../core-lib` or `uv add --editable ../core-lib` we get the same result.

The `pyproject.toml` in `cli` doesn't reflect an editable or not dependency. It only adds this:
```toml
...
dependencies = [
    "core-lib",
]

...

[tool.uv.sources]
core-lib = { workspace = true }
```

We only know if the dependency is editable or not through the lockfile:
```
[[package]]
name = "cli"
version = "0.1.0"
source = { editable = "src/my_package/cli" }
dependencies = [
    { name = "core-lib" },
]

[package.metadata]
requires-dist = [{ name = "core-lib", editable = "src/my_package/core_lib" }]
```

And it's the same case for both types of installations.

I'm not suggesting that this behavior should be changed, but maybe a small addition to the doc for that would be cool 😄 

I have added a very note to the doc in case it can lighten the workload for the maintainers, but don't hesitate to close the PR / this issue or just signal that to me and I'll do it myself.


---

_Referenced in [astral-sh/uv#9363](../../astral-sh/uv/pulls/9363.md) on 2024-11-22 18:57_

---

_Label `documentation` added by @samypr100 on 2024-11-23 02:17_

---
