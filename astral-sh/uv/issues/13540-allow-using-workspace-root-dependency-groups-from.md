```yaml
number: 13540
title: Allow using workspace root dependency groups from member
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-05-19T17:24:49Z
updated_at: 2025-09-23T06:36:18Z
url: https://github.com/astral-sh/uv/issues/13540
synced_at: 2026-01-12T16:01:31Z
```

# Allow using workspace root dependency groups from member

---

_@zanieb_

### Summary

It'd be nice to declare development dependencies once then re-use them in the workspace, but it isn't possible to do this today while using `--package` to ensure you are testing the package in isolation.

This seems like a bit of a crutch until the ecosystem catches up and just adds workspace aware tools, where, e.g., you could just run `pytest` from the root. Or, if we add a task runner which supports automatically running across workspace members. 

### Example

In the root `pyproject.toml`

```
[dependency-groups]
dev = ["pytest"]
```

Then, to test some member `foo`:

```
uv run --package foo --group dev pytest
```

It's not clear if should just implicitly include workspace groups or if it should be opt-in like, in `foo/pyproject.toml`:

```
[dependency-groups]
dev = { workspace = true }
```

It _seems_ like it might be fine to allow using the root dependency groups from members? Since they're development only already it doesn't really break package isolation. This seems especially safe if the workspace member doesn't declare any dependency groups at al..

---

_Label `enhancement` added by @zanieb on 2025-05-19 17:24_

---

_Comment by @effigies on 2025-06-04 13:59_

I would also be interested in going the other way. My use-case here is:

```toml
[project]
name = "my-project"
description = "mkdocs or sphinx-based website"
dependencies = [
  "helper_package",
  "mkdocs",
  "..."
]
dynamic = ["version"]

[tool.uv.sources]
helper_package = { path = "tools/helper_package", editable = true }
```

(The helper package is something I'm interested in publishing to PyPI.)

It would be nice to be able to add something like:

```toml
[dependency-groups]
doc = [
  "helper_package[group=doc]"
]
```

I want the helper package to generate an `sdist` that can be independently tested and have its docs built, so I would like to track its dependencies in its own `pyproject.toml`. This would allow me to use `uv sync --group doc` whether I'm working in the repository root or in an sdist.

Right now, the best option I have is to keep a `doc` extra and use `doc = ["helper_package[doc]"]`, which I was hoping to get away from.

---

_Comment by @zanieb on 2025-06-04 14:50_

@effigies I opened https://github.com/astral-sh/uv/issues/13838 to track that — it'll be a bit more controversial / harder :)

---

_Comment by @Northo on 2025-09-23 06:36_

Is this duplicate of or related to #8949?

---
