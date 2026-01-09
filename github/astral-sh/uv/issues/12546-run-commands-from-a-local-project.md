---
number: 12546
title: Run commands from a local project
type: issue
state: closed
author: perillo
labels:
  - enhancement
assignees: []
created_at: 2025-03-29T15:11:33Z
updated_at: 2025-03-30T10:49:30Z
url: https://github.com/astral-sh/uv/issues/12546
synced_at: 2026-01-07T13:12:18-06:00
---

# Run commands from a local project

---

_Issue opened by @perillo on 2025-03-29 15:11_

### Summary

NOTE: I'm new to the `uv` tool, so some comments may be incorrect.
I tried searching existing issues, but I was enabled to find issues related to my problem.

In my first `uv` project, I have only one package.
The project has a `__main__.py` file and a project-script referencing `pkg.__main__:main`.

I can run `uv run -m pkg` and `uv run pkg pk/src/pkg/__main__.py`.
The problem is that `uv` does support running these commands from a different directory.

`uvx` support installing commands from the python index, but in my case I don't plan to publishing the project.

Is there a reason why `uv` does not support this use case?

Currently I can use either `pip install --break-system-package` to install the package and run `python -m pkg` or `pipx install` to have the package script installed to `~/.local/bin`.

Thanks

### Example

_No response_

---

_Label `enhancement` added by @perillo on 2025-03-29 15:11_

---

_Comment by @tonnico on 2025-03-29 18:03_

I'm just trying the same and I found a solution for this.

However, currently it seems to be undocumented and maybe it is not intended?

installing it:
```
uv tool install file:.
```

updating:
```
uv tool upgrade pkg --reinstall
```

when you change the version of your pkg, you do not need `--reinstall`.

---

_Comment by @perillo on 2025-03-29 18:14_

@Wurstnase Thanks!

It was documented; I just missed it.
This also works:
`uv tool install .`

---

_Closed by @perillo on 2025-03-30 10:49_

---
