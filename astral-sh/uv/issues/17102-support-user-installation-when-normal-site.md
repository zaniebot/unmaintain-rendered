---
number: 17102
title: Support user installation when normal site-packages is not writeable
type: issue
state: closed
author: NewUserHa
labels:
  - question
assignees: []
created_at: 2025-12-12T14:55:49Z
updated_at: 2025-12-17T15:05:22Z
url: https://github.com/astral-sh/uv/issues/17102
synced_at: 2026-01-10T01:26:13Z
---

# Support user installation when normal site-packages is not writeable

---

_Issue opened by @NewUserHa on 2025-12-12 14:55_

### Summary

the default pip has this feature, when run cmd without admin permission, it will 
```
>pip install ...
Defaulting to user installation because normal site-packages is not writeable
Collecting ...
```
writing to `C:\Users\user\AppData\Roaming\Python\Python314\...`, 
however, `uv` can only install to `C:\Program Files\Python314` ,or asking create venv with `uv venv` in current directory. but the use case is not needing a seperate env, but works like normal, with the packages wants to test to be installed in a side directory which can be one-click easily deleted.

so uv should support this behavior of `pip`.

another use case is:
when using pixi, whose `pixi add` command is unable to handle `--index-url` like options, which pixi lacks it, so `uv` needs to be used, but since after `pixi init` `pixi add python` `pixi shell`, the current env is already a venv, but at macro level. at this moment, `uv install` should install to the python's own site-packages directory within the macro venv by pixi, like the before `pip install` in a `conda env`.

but `uv`'s current design is: "installing to global, use --system, otherwise, throw error 'use --system'" for showing `uv venv` ablity, so in the above two situations, the `--system` becomes a redunrant that can't get rid of in every time using `uv`.

Is it possible to make it chosse-able, like when user runs `uv pip install` rather than throw a error and exit, give a prompt that pressing enter install to `site-packages` according to permission of which `site-packages` it can, or input 'n' to install to a new venv if in venv, or press ctrl-c to exit, making life easier.
for `--system`, it can be kept, and even add `--user` for installing to user `site-packages` directory, for auto-command-scripts usage.


### Example

_No response_

---

_Label `enhancement` added by @NewUserHa on 2025-12-12 14:55_

---

_Comment by @konstin on 2025-12-16 11:11_

Please see https://github.com/astral-sh/uv/issues/2077 for why we're not supporting user site packages. We recommend using venvs for package installations.

---

_Label `enhancement` removed by @konstin on 2025-12-16 11:11_

---

_Label `question` added by @konstin on 2025-12-16 11:11_

---

_Comment by @NewUserHa on 2025-12-17 14:10_

my opinion is not "support --user option", it is to have a way to support pip's behavior. so #2077 should have no relation to this issue.

the reason people use `uv` is because `pip` is slow.

---

_Comment by @zanieb on 2025-12-17 14:51_

We won't support writing to user site-packages in general, whether it's an explicit `--user` request or an implicit fallback to the user's directory.

---

_Closed by @zanieb on 2025-12-17 15:05_

---
