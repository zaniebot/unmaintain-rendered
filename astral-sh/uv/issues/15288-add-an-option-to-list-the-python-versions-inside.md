```yaml
number: 15288
title: "Add an option to list the python versions inside of `uv tool list`"
type: issue
state: closed
author: pythonweb2
labels:
  - enhancement
  - good first issue
assignees: []
created_at: 2025-08-14T20:36:48Z
updated_at: 2025-10-10T17:44:41Z
url: https://github.com/astral-sh/uv/issues/15288
synced_at: 2026-01-12T16:02:07Z
```

# Add an option to list the python versions inside of `uv tool list`

---

_@pythonweb2_

### Summary

When installing tools, you can choose which python executable to use. It would be nice to list out the python version inside of uv tool list.

It seems especially useful if you are trying to get rid of an older python version and want the check to see which tools are dependent on it. This may not be as relevant with the preview `uv python upgrade` command though, if it will automatically switch the python version for you.

### Example

```
# uv tool list
pre-commit v4.3.0 (cpython-3.13.5-windows-x86_64-none)
- pre-commit.exe
```

---

_Label `enhancement` added by @pythonweb2 on 2025-08-14 20:36_

---

_Comment by @zanieb on 2025-08-14 21:08_

A `--show-python` flag seems fine to me.

---

_Label `good first issue` added by @zanieb on 2025-08-14 21:08_

---

_Comment by @zanieb on 2025-08-14 21:08_

We might want to disambiguate between `--show-python-version` and `--show-python-path`? Maybe just have `--show-python` be the version?

---

_Comment by @pythonweb2 on 2025-08-14 21:56_

I could maybe take a stab at this, although I am not very familiar with rust. If you could point me to the argument parser for the `uv tool list` section I may be able to get started via the blame in the file though.

---

_Comment by @zanieb on 2025-08-14 23:55_

You could look at

- https://github.com/astral-sh/uv/pull/5164
- https://github.com/astral-sh/uv/pull/13783

---

_Comment by @pythonweb2 on 2025-08-15 15:53_

I started on a PR here: #15312 

---

_Comment by @pythonweb2 on 2025-10-10 17:44_

@zanieb Idk if you want to link this to #15814, or how that works? 

---

_Comment by @pythonweb2 on 2025-10-10 17:44_

It looks like there was another issue created for it, so I'll close this.

---

_Closed by @pythonweb2 on 2025-10-10 17:44_

---
