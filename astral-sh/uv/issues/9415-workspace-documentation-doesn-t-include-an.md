```yaml
number: 9415
title: "Workspace documentation doesn't include an example of a nested package `pyproject.toml`"
type: issue
state: open
author: samuelcolvin
labels:
  - documentation
assignees: []
created_at: 2024-11-25T12:54:57Z
updated_at: 2024-11-25T21:24:55Z
url: https://github.com/astral-sh/uv/issues/9415
synced_at: 2026-01-12T15:59:49Z
```

# Workspace documentation doesn't include an example of a nested package `pyproject.toml`

---

_@samuelcolvin_

See https://docs.astral.sh/uv/concepts/projects/workspaces/

Workspace documentation doesn't include an example of a nested package `pyproject.toml` file, instead there are three examples of the `pyproject.toml` file for `name = "albatross"` the root package.

I'm trying to work out what I should put in a `pyproject.toml` file of a nested package, and it's not clear.

In particular, when working with cargo, I can include stuff like `version = { workspace = true }`, but it's not clear if I can do that with uv.


---

_Comment by @samuelcolvin on 2024-11-25 12:57_

😞 

I tried

```toml
version = { workspace = true }
description = { workspace = true }
authors = { workspace = true }
license = { workspace = true }
readme = { workspace = true }
classifiers = { workspace = true }
requires-python = { workspace = true }
```

As I would do with cargo, but I get:

```
uv sync
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 7, column 11
  |
7 | version = { workspace = true }
  |           ^^^^^^^^^^^^^^^^^^^^
invalid type: map, expected a string
```


---

_Label `documentation` added by @charliermarsh on 2024-11-25 13:29_

---

_Comment by @charliermarsh on 2024-11-25 13:30_

Ah yeah, we don't inherit workspace configuration like that. In fact, we can't right now, it would be non-standards compliant and would break a lot of other tools. It's possible that we can provide something like this in the future via our own build backend, but even then, you'd need to use our build backend (rather than, e.g., maturin).

---

_Comment by @samuelcolvin on 2024-11-25 13:30_

Duplicating `description`, `authors`, `license` etc. is fine, but it would be really useful if I could set `version` as "dynamic" and have it shared across all packages.

I guess that might already be possible?

---

_Comment by @samuelcolvin on 2024-11-25 20:12_

~~Turns out the package version stuff is easy, you can just do~~

```toml
[tool.hatch.version]
path = "../pyproject.toml"

[project]
dynamic = ["version"]
```

**Update:** turns out this doesn't work, uv is unable to track version correctly if you use a dynamic version.

The ugly bit is if you want your packages to have a hard requirement on each other (e.g. you release `foo`, `bar` and `spam`, all have version `0.0.6b1` and you want `foo` and `bar` to depend on `spam==0.0.6b1`) then you have to hard code the version number in a bunch of places.

---
