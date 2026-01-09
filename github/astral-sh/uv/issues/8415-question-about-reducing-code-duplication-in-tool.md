---
number: 8415
title: "Question about reducing code duplication in `[tool.uv.sources]`"
type: issue
state: open
author: zmeir
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-10-21T11:20:09Z
updated_at: 2025-02-25T14:45:51Z
url: https://github.com/astral-sh/uv/issues/8415
synced_at: 2026-01-07T13:12:17-06:00
---

# Question about reducing code duplication in `[tool.uv.sources]`

---

_Issue opened by @zmeir on 2024-10-21 11:20_

I'll start by saying this is probably not a very common use-case, but I wanted to ask this in case there is some better way of doing what I'm doing that I couldn't find in the documentation.

I have a project with some internal packages as dependencies. These packages are published to a private index, which is then mirrored to 2 different locations. One of these locations is on the same network segment as our development environments, so for development purposes is it faster to download packages from this index. The second one is faster for our ci/automation/production environments. I can differentiate between the environments with the `sys_platform` marker (we develop on mac and windows, but deploy on linux).

I thought about setting up the `[tool.uv.sources]` table in `pyproject.toml` like this:
```toml
[tool.uv.sources]
internal-package-1 = [
    { index = "private-index-dev", marker = "sys_platform == 'darwin' or sys_platform == 'win32'" },
    { index = "private-index-prd", marker = "sys_platform != 'darwin' and sys_platform != 'win32'" },
]
internal-package-2 = [
    { index = "private-index-dev", marker = "sys_platform == 'darwin' or sys_platform == 'win32'" },
    { index = "private-index-prd", marker = "sys_platform != 'darwin' and sys_platform != 'win32'" },
]
internal-package-3 = [
    { index = "private-index-dev", marker = "sys_platform == 'darwin' or sys_platform == 'win32'" },
    { index = "private-index-prd", marker = "sys_platform != 'darwin' and sys_platform != 'win32'" },
]
```
The same 2 lines for `private-index-dev` and `private-index-prd` are repeated for each internal package.

Is there any way to prevent this duplication?

As far as I know TOML doesn't support variable substitution, so I can't do something like this:
```toml
[tool.uv.sources]
internal-package-settings = [
    { index = "private-index-dev", marker = "sys_platform == 'darwin' or sys_platform == 'win32'" },
    { index = "private-index-prd", marker = "sys_platform != 'darwin' and sys_platform != 'win32'" },
]
internal-package-1 = {internal-package-settings}
internal-package-2 = {internal-package-settings}
internal-package-3 = {internal-package-settings}
```

---

_Comment by @zanieb on 2024-10-21 13:54_

Interesting. Thanks for sharing. I guess we could add something like `[tool.uv.source-definitions]` then allow `tool.uv.sources.<package> = { definition = ... }`

It certainly adds some complexity though, so I'm hesitant to pursue it without more requests.

---

_Label `enhancement` added by @zanieb on 2024-10-21 14:36_

---

_Label `wish` added by @zanieb on 2024-10-21 14:36_

---

_Referenced in [astral-sh/uv#8577](../../astral-sh/uv/issues/8577.md) on 2024-10-25 22:00_

---

_Comment by @sanmai-NL on 2025-02-24 10:57_

@zmeir Can you please evaluate [KCL](https://www.kcl-lang.io/docs/user_docs/getting-started/intro) for this as a more generic and powerful solution? @zanieb the functional scope of such tools may serve as a good boundary for uv's vision on the scope wrt. templating functionality. After all, implementing such functionality is costly and prone to design issues that need to be revisited later (breaking changes). Then functionally, it's also bound to be complex for users which then results in documentation cost and support cost.

@zmeir you would write a template `pyproject.k` and do the transformations/DRY stuff in there.

```sh
kcl run pyproject.k --format toml
```

---

_Comment by @zmeir on 2025-02-24 20:26_

@sanmai-NL Thanks for the suggestion. Right now, with a handful of packages, this isn't such a big deal for me. At least not enough to add another level of indirection to my projects. But if it gets too complex I'll be sure to take a crack at it.

---
