```yaml
number: 12661
title: Changing a package name with sync --frozen reports the wrong error
type: issue
state: open
author: Halkcyon
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2025-04-03T22:52:15Z
updated_at: 2025-04-07T15:25:58Z
url: https://github.com/astral-sh/uv/issues/12661
synced_at: 2026-01-12T16:01:09Z
```

# Changing a package name with sync --frozen reports the wrong error

---

_@Halkcyon_

### Summary

Changing the name of a package and using `uv sync --frozen` reports `error: "Could not find root package $name"`.  Removing `--frozen` resolves the issue.

### Platform

Windows 11 x86_64

### Version

0.6.12

### Python version

3.12.8

---

_Label `bug` added by @Halkcyon on 2025-04-03 22:52_

---

_Comment by @zanieb on 2025-04-03 22:53_

`--frozen` explicitly only reads from the lockfile, if the lockfile is outdated because you changed the name of the project, then it should fail.

---

_Label `bug` removed by @zanieb on 2025-04-03 22:53_

---

_Label `error messages` added by @zanieb on 2025-04-03 22:53_

---

_Label `help wanted` added by @zanieb on 2025-04-03 22:54_

---

_Comment by @zanieb on 2025-04-03 22:54_

(I presume this is not hard to set up a better error message for)

Could you share a reproduction example?

---

_Comment by @Halkcyon on 2025-04-07 15:25_

> (I presume this is not hard to set up a better error message for)

Yeah, the error message was the confusing part since I was using build scripts that used `--frozen` by default.

The repro is creating a scaffold project with `uv init`, `uv sync` to set up the venv, then change the package name in `pyproject.toml` and rename the package directory to match.  Run `uv sync --frozen` and you can observe the error.



---
