---
number: 11417
title: "uv pip compile --universal: should prefer `requires-python` as lower bound, rather than current Python"
type: issue
state: closed
author: paveldikov
labels:
  - bug
assignees: []
created_at: 2025-02-11T11:11:06Z
updated_at: 2025-02-11T20:35:56Z
url: https://github.com/astral-sh/uv/issues/11417
synced_at: 2026-01-10T01:25:05Z
---

# uv pip compile --universal: should prefer `requires-python` as lower bound, rather than current Python

---

_Issue opened by @paveldikov on 2025-02-11 11:11_

### Summary

(I don't know if this applies to lockfiles as well -- I assume that the two are just different presentation layers for the same algorithm?)

Say I have a project that has `pyproject.toml`:
```
requires-python = ">=3.8"
```

and I am running `uv pip compile --universal pyproject.toml > constraints.txt` in an environment that has Python 3.12.

The resulting `constraints.txt` file is not necessarily usable under Python 3.8. It may well include dependency versions that are not suitable for 3.8.

As a workaround, I find that `--python-version 3.8` will actually force the uv resolver to do the right thing, and generate a lot more forks to accommodate for a full range of Python versions.

Maybe this should be a default behaviour, at least for universal mode?

_(It is really rather useful to have constraints/lock files that are portable across multiple interpreter versions -- and `uv` is already partly doing it -- for the dependencies -- but not considering the root node itself, therefore not nearly forking as much as it should.)_

### Platform

Linux (though this is OS agnostic)

### Version

0.5.30

### Python version

3.12.3

---

_Label `bug` added by @paveldikov on 2025-02-11 11:11_

---

_Renamed from "uv pip compile --universal: should use `requires-python` as lower bound, rather than current Python" to "uv pip compile --universal: should prefer `requires-python` as lower bound, rather than current Python" by @paveldikov on 2025-02-11 11:11_

---

_Comment by @charliermarsh on 2025-02-11 13:31_

I think this might be a duplicate of https://github.com/astral-sh/uv/issues/6031.

---

_Comment by @paveldikov on 2025-02-11 20:35_

You're right, my bad. I will close this and +1 the original

---

_Closed by @paveldikov on 2025-02-11 20:35_

---
