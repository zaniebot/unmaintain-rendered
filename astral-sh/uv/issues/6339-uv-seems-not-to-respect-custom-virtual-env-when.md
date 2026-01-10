---
number: 6339
title: uv seems not to respect custom virtual env when installing a dependency
type: issue
state: closed
author: FilipLangr
labels:
  - duplicate
assignees: []
created_at: 2024-08-21T15:10:24Z
updated_at: 2024-11-25T18:46:06Z
url: https://github.com/astral-sh/uv/issues/6339
synced_at: 2026-01-10T01:23:59Z
---

# uv seems not to respect custom virtual env when installing a dependency

---

_Issue opened by @FilipLangr on 2024-08-21 15:10_

It seems like `uv` does not respect custom virtual env.

my `uv` version is `0.3.0` running on Ubuntu 24.04

code to reproduce:
```bash
mkdir project_test
cd project_test
uv init
uv venv custom # creates ./custom folder
source custom/bin/activate
echo $VIRTUAL_ENV # prints the path to ./custom folder
uv add numpy --verbose
```
the last command prints (among others):
```
DEBUG Ignoring Python interpreter at `/.../project_test/custom/bin/python3`: system interpreter required
```
which I don't really understand why. It also creates a new virtualenv `.venv`, where it installs numpy. However, I would expect it to install it to `custom`. Is this a bug or feature? If feature, how to come around it and make `uv` to use `custom` venv? Thank you!

---

_Comment by @zanieb on 2024-08-21 15:12_

Hi!

Thanks for the nice reproduction. We don't support reading `VIRTUAL_ENV` in the project APIs. See https://github.com/astral-sh/uv/issues/5229 for the feature request.

---

_Label `duplicate` added by @zanieb on 2024-08-21 15:12_

---

_Comment by @FilipLangr on 2024-08-21 15:16_

> Hi!
> 
> Thanks for the nice reproduction. We don't support reading `VIRTUAL_ENV` in the project APIs. See #5229 for the feature request.

Ah my bad, didn't notice that one. Thank you for such a swift response!

---

_Closed by @FilipLangr on 2024-08-21 15:16_

---

_Closed by @zanieb on 2024-08-21 15:17_

---

_Comment by @zanieb on 2024-08-21 15:17_

You're welcome!

---

_Comment by @caastrill on 2024-11-25 18:40_

was this ever resolved facing this same issue today as highlighted by [FilipLangr](https://github.com/FilipLangr) on opening this thread.

---

_Comment by @zanieb on 2024-11-25 18:46_

There is extensive discussion on the linked issue.

---
