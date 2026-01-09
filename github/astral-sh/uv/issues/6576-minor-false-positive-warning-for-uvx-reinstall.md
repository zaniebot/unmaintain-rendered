---
number: 6576
title: Minor false positive warning for uvx reinstall
type: issue
state: closed
author: bluss
labels:
  - error messages
assignees: []
created_at: 2024-08-24T11:28:31Z
updated_at: 2024-08-25T15:15:43Z
url: https://github.com/astral-sh/uv/issues/6576
synced_at: 2026-01-07T13:12:17-06:00
---

# Minor false positive warning for uvx reinstall

---

_Issue opened by @bluss on 2024-08-24 11:28_

Original command:

`uvx --reinstall --with dist/*.whl pytest `

Modified command (to try to avoid the warning):

```
uvx --reinstall-package my-package-name --with dist/*.whl pytest     
```

Both have the following warning. Especially in the second modified case, this is a false positive? Because the reinstall is wanted for the extra dependency added with `--with`.
             
```                                                
warning: Tools cannot be reinstalled via `uvx`; use `uv tool upgrade --reinstall`
to reinstall all installed tools, or `uvx package@latest` to run the latest
version of a tool
```

The command is a recipe for running tests vs the freshly built wheel -- from the project directory. Reinstall to ensure the new wheel is used even if version etc did not change.
Using uv 0.3.3.

---

_Comment by @charliermarsh on 2024-08-24 11:31_

I think `--refresh` might be intended here.

---

_Comment by @bluss on 2024-08-24 11:37_

Oh, thanks, I think refresh works for the use case, looks like it. I've just been keen to ensure the wheel is actually reinstalled. (With that fix, there is no issue here).

---

_Comment by @charliermarsh on 2024-08-24 11:41_

I think the warning needs to be tweaked.

---

_Label `error messages` added by @charliermarsh on 2024-08-24 11:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 02:26_

---

_Referenced in [astral-sh/uv#6609](../../astral-sh/uv/pulls/6609.md) on 2024-08-25 15:01_

---

_Closed by @charliermarsh on 2024-08-25 15:15_

---

_Closed by @charliermarsh on 2024-08-25 15:15_

---
