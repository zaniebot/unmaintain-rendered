---
number: 17342
title: "`uv add --editable` does not remove old dependency"
type: issue
state: open
author: rzuckerm
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2026-01-07T00:39:59Z
updated_at: 2026-01-07T15:32:00Z
url: https://github.com/astral-sh/uv/issues/17342
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv add --editable` does not remove old dependency

---

_Issue opened by @rzuckerm on 2026-01-07 00:39_

### Summary

- Create new package:
  ```
  uv init --package my-pkg
  cd my-pkg
  ```
- Add `ansicolors` as a dependency:
  ```
  uv add ansicolors
  ```
  This sets `project.dependencies` to `["ansicolors>=1.1.8"]`
- Create new local `ansicolors` package
  ```
  cd ..
  uv init --package ansicolors
  cd test-pkg
  ```
- Add local `ansicolors` package as an editable dependency:
  ```
  uv add --editable ../ansicolors
  ```

The result is a `pyproject.toml` that looks like this:
```
[project]
name = "my-pkg"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "rzuckerm", email = "<redacted>" }
]
requires-python = ">=3.8"
dependencies = [
    "ansicolors>=1.1.8",
]

[project.scripts]
my-pkg = "my_pkg:main"

[build-system]
requires = ["uv_build>=0.9.22,<0.10.0"]
build-backend = "uv_build"

[tool.uv.sources]
ansicolors = { path = "../ansicolors", editable = true }
```
I would expect `project.dependencies` to be `ansicolors` without the version range. BTW, poetry would have replaced the old dependency with the new editable dependency.

### Platform

Ubuntu 22.04 amd64

### Version

0.9.22

### Python version

Python 3.8.20

---

_Label `bug` added by @rzuckerm on 2026-01-07 00:40_

---

_Comment by @zanieb on 2026-01-07 00:52_

I'm not sure I agree we should change the bounds in the `pyproject.toml`? Is there a reason you expected this other than Poetry's behavior?

I think it's reasonable to require removing the dependency first or explicitly including new bounds if that's the intent?

It seems ambiguous either way and I'm hesitant to change existing behavior without strong justification.



---

_Label `needs-decision` added by @zanieb on 2026-01-07 00:52_

---

_Comment by @rzuckerm on 2026-01-07 13:43_

> I'm not sure I agree we should change the bounds in the `pyproject.toml`? Is there a reason you expected this other than Poetry's behavior?

Yes, partly because this is how poetry work, but also for consistency. If a user runs `uv add --editable` for a dependency that is not already in the `pyproject.toml`, `project.dependencies` just shows the package name without a version range.

> I think it's reasonable to require removing the dependency first or explicitly including new bounds if that's the intent?

It's more convenient for the user to have `uv add --editable` remove the existing entry first instead of having the user run `uv remove` before `uv add --editable`. The way we use `uv add --editable` is to allow the developer to try out changes to a dependency in another package. For example, `test-pkg` depends on `test-pkg-b`, and the user wants to test out changes to `test-pkg-b` in `test-pkg`.

> It seems ambiguous either way and I'm hesitant to change existing behavior without strong justification.

I think the justification is consistency; it all works the same whether you are adding a new editable dependency, changing existing dependency to editable, or changing an editable dependency back to a regular dependency.

BTW, there doesn't seem to be a good way to change an editable dependency back to a regular dependency with `uv add` without the `--editable`. The `tool.uv.sources` entry remains. I guess this is really the same problem that `uv remove` needs to be run first.

---

_Comment by @zanieb on 2026-01-07 15:32_

> I think the justification is consistency; it all works the same whether you are adding a new editable dependency, changing existing dependency to editable, or changing an editable dependency back to a regular dependency.

I'm not feeling convinced that "changing a dependency to editable" should have behavior consistent with "adding a new editable dependency". I think the operation is inherently different if the dependency already exists in the `pyproject.toml`.

---
