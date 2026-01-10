---
number: 16291
title: "Allow the option to make index explicit when using `uv add`"
type: issue
state: open
author: BaoNguyen6742
labels:
  - enhancement
assignees: []
created_at: 2025-10-14T04:17:22Z
updated_at: 2025-11-14T15:49:47Z
url: https://github.com/astral-sh/uv/issues/16291
synced_at: 2026-01-10T01:26:05Z
---

# Allow the option to make index explicit when using `uv add`

---

_Issue opened by @BaoNguyen6742 on 2025-10-14 04:17_

### Summary

When I add a package using specific index, packages that are installed later give "No solution found when resolving dependencies" error.

Step to reproduce
1. Running `uv add paddlepaddle==3.0.0 --index https://www.paddlepaddle.org.cn/packages/stable/cpu/`
This will create the pyproject.toml file like this

```
[project]
name = "uv-index"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "paddlepaddle==3.0.0",
]


[tool.uv]
required-environments = [
    "sys_platform == 'win32'" # I add this since numpy 2.3.2 doesn't have a source distribution or wheel when I test on windows.
]

[[tool.uv.index]]
url = "https://www.paddlepaddle.org.cn/packages/stable/cpu/"
```
2. Run `uv add matplotlib`
This will return the error 
```
× No solution found when resolving dependencies for split (markers: python_full_version >= '3.12' and sys_platform
│ == 'win32'):
╰─▶ Because there are no versions of matplotlib and your project depends on matplotlib, we can conclude that your project's requirements are unsatisfiable.
help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

There are currently 2 ways to fix this
1. Run `uv add matplotlib` with `--index-strategy unsafe-first-match` or `--index-strategy unsafe-best-match`
2. Edit the pyproject.toml in the `tool.uv.index` section to make the index explicit
   ```
   [[tool.uv.index]]
   url = "https://www.paddlepaddle.org.cn/packages/stable/cpu/"
   explicit = true
   ```

I hope there could be a way to make the index explicit right when you add it so it doesn't affect other package when installing

### Example

_No response_

---

_Label `enhancement` added by @BaoNguyen6742 on 2025-10-14 04:17_

---

_Comment by @doddi on 2025-10-20 20:50_

Im just curious if the expected behaviour here is that all `uv add` commands with a URL should be explicit?

---

_Comment by @BaoNguyen6742 on 2025-10-21 01:34_

> Im just curious if the expected behaviour here is that all `uv add` commands with a URL should be explicit?

I think this should be the way since index are most of the time only used for limited amount of packages. If you want to use all package from 1 index then you would set `default = true` in the tool.uv.index section, so I think it would be better if index are explicit by default

---

_Comment by @doddi on 2025-10-21 09:05_

@konstin / @zanieb what are your thoughts? this seems a reasonable request and almost feels like a bug rather than an enhancement? I would like to take a look at this issue if the team thinks it is the correct approach

---

_Referenced in [my1e5/uv#3](../../my1e5/uv/pulls/3.md) on 2025-10-31 12:27_

---

_Comment by @xixixao on 2025-11-14 15:49_

I also feel that support for indexes is broken.

We have the exact issue as this one.

We also have the issue that you cannot do:

```sh
uv add --index index-name=url some-lib
uv add --index index-name some-other-lib
```
because you'll get:

> warning: Relative paths passed to `--index` or `--default-index` should be disambiguated from index names (use `./index-name`). Support for ambiguous values will be removed in the future

But I think the issues are all related.

What we'd like to have is:

1. Add extra indexes ("team", "company")
2. When adding a new dependency with `uv add`, search the team index, if not found, search the company index (and don't seach PyPi)

I don't think it's possible now without different index strategy, but that's not a nice API for our teammembers (and feels like it should be the default?).

---
