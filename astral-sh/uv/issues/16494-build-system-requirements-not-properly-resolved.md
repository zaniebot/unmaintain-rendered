---
number: 16494
title: build system requirements not properly resolved
type: issue
state: open
author: nschloe
labels:
  - bug
assignees: []
created_at: 2025-10-29T12:21:57Z
updated_at: 2025-10-31T14:26:38Z
url: https://github.com/astral-sh/uv/issues/16494
synced_at: 2026-01-10T01:26:06Z
---

# build system requirements not properly resolved

---

_Issue opened by @nschloe on 2025-10-29 12:21_

### Summary

I try to `uv build .` a local package that has 
```toml
[build-system]
build-backend = "foo.build_meta"
requires = [
  "foo>=0.1a2",
]
```
`foo` depends on `bar>=0.2a7`. uv reports
```
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `foo>=0.1a2`
  ╰─▶ Because only bar<0.2a7 is available and
      foo==0.1a2 depends on bar>=0.2a7, we can
      conclude that foo==0.1a2 cannot be used.
      And because only foo<=0.1a2 is available and you require
      foo>=0.1a2, we can conclude that your requirements are unsatisfiable.
```
After manually adding
```toml
[build-system]
build-backend = "foo.build_meta"
requires = [
  "foo>=0.1a2",
  "bar>=0.2a7",
]
```
it all works fine. This suggests that the build system requirements resolver is buggy. Perhaps the fact that there are alpha-versions involved plays a role here.

### Platform

Linux 6.12.55-1-lts x86_64 GNU/Linux

### Version

uv 0.9.5

### Python version

all

---

_Label `bug` added by @nschloe on 2025-10-29 12:21_

---

_Renamed from "build system requirements not probably resolved" to "build system requirements not properly resolved" by @nschloe on 2025-10-29 12:22_

---

_Comment by @nschloe on 2025-10-29 12:40_

It _does_ have to do with prereleases. When doing
```
uv pip install foo==0.1a2
```
on the command line, I get the same error message with the added
```
      hint: `bar` was requested with a pre-release marker
      (e.g., bar>=0.2a7), but pre-releases weren't enabled
      (try: `--prerelease=allow`)
```
Calling
```
uv pip install foo==0.1a2 --prerelease=allow
```
does indeed work. How can I use this for building the package?

---

_Comment by @zanieb on 2025-10-29 14:18_

Using `--prerelease=allow` seems correct.

We do not allow selection of pre-releases in child packages without opt-in at the top-level.

See https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility

---

_Referenced in [astral-sh/uv#16496](../../astral-sh/uv/issues/16496.md) on 2025-10-29 14:48_

---

_Closed by @charliermarsh on 2025-10-31 01:15_

---

_Comment by @zanieb on 2025-10-31 03:55_

Per https://github.com/astral-sh/uv/issues/16496#issuecomment-3462197600 and #8192 we don't actually propagate `--prerelease=allow` to build environments, so that's not the fix. I'm not sure what is.

---

_Reopened by @zanieb on 2025-10-31 03:55_

---

_Comment by @charliermarsh on 2025-10-31 14:26_

Yeah I think right now, explicitly requesting the pre-release dependencies like that is the only solution.

---
