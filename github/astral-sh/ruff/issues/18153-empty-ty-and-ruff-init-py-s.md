---
number: 18153
title: "Empty `ty` and `ruff` `__init__.py`s"
type: issue
state: open
author: bengsparks
labels:
  - question
assignees: []
created_at: 2025-05-17T15:32:11Z
updated_at: 2025-05-18T15:27:11Z
url: https://github.com/astral-sh/ruff/issues/18153
synced_at: 2026-01-07T13:12:16-06:00
---

# Empty `ty` and `ruff` `__init__.py`s

---

_Issue opened by @bengsparks on 2025-05-17 15:32_

### Question

Do the Python modules for ruff (and ty) only exist to distribute the resulting binary over PyPI?
I've seen that uv reexports the [find_uv_bin function](https://github.com/astral-sh/uv/blob/be1404df2a5429fdd661073b291528a047867c86/python/uv/__init__.py#L5), but the [ruff package](https://github.com/astral-sh/ruff/blob/main/python/ruff/__init__.py) and [ty package](https://github.com/astral-sh/ty/blob/main/python/ty/__init__.py) are empty.

Are there any upcoming plans to add `find_ty_bin` or `find_ruff_bin` to the respective `__init__.py`'s?
I'd be fine with adding a pull request to facilitate this 😄 

### Version

`ruff *`, `ty *` 😄 

---

_Label `question` added by @bengsparks on 2025-05-17 15:32_

---

_Comment by @MichaReiser on 2025-05-17 15:35_

CC: @charliermarsh @zanieb

---

_Comment by @calumy on 2025-05-17 17:45_

ty appears to have this functionality already in place: https://github.com/astral-sh/ty/blob/main/python/ty/__main__.py

---

_Comment by @bengsparks on 2025-05-17 17:47_

The functionality exists, but it is not exported, yes. This question is more about the export than the impl, i.e. adding
`__all__ = [ "find_ruff_bin" ]` and similar to each package

---

_Comment by @zanieb on 2025-05-17 20:58_

I've never really wanted these to be a part of the public API, but we accepted a contribution moving it after requested.

See https://github.com/astral-sh/uv/issues/1677

We have gotten various bug reports about it following that, so I'm not _enthused_ about the complexity added by exposing it.

Can you share more about your use-case?

---

_Comment by @bengsparks on 2025-05-18 11:24_

Initially I had hoped for simply allowing calling the programs more easily, so that $PATH changes are not necessary for running the binaries, e.g. scripting `subprocess.run(["ruff", "format"])` could become 
```py
ruff_bin = find_ruff_bin()
subprocess.run([ruff_bin, "format"])
```

Thinking further upon this, perhaps a type-safe CLI interface could be provided?
```py
import ruff, pathlib

# identical to `ruff format --diff --target-version py313 ./python`
# runs `os.execvp` / `subprocess.run` under the hood
ruff.format(diff=True, target_version=TargetVersion.313, target_dir=pathlib.Path.cwd() / "python")
```

Perhaps PyO3 bindings could be made, instead of calling the binary?
Many different paths to explore with this 😄 

---

_Comment by @MichaReiser on 2025-05-18 15:27_

For exposing Ruff's API, see https://github.com/astral-sh/ruff/issues/659


> We have gotten various bug reports about it following that, so I'm not enthused about the complexity added by exposing it.

could you share some references that help me make a decision here?

---

_Referenced in [astral-sh/ruff#18224](../../astral-sh/ruff/pulls/18224.md) on 2025-05-20 15:52_

---
