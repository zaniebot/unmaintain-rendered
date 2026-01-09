---
number: 16798
title: uv tries to install package version that is incompatible with platform
type: issue
state: closed
author: eigenvivek
labels:
  - question
assignees: []
created_at: 2025-11-20T23:06:37Z
updated_at: 2025-11-21T15:33:36Z
url: https://github.com/astral-sh/uv/issues/16798
synced_at: 2026-01-07T13:12:19-06:00
---

# uv tries to install package version that is incompatible with platform

---

_Issue opened by @eigenvivek on 2025-11-20 23:06_

### Summary

I am trying to install a nightly build of a package with this command:

```bash
uv add vtk --index https://wheels.vtk.org --pre --upgrade
```

which raises the following error and hint:

```
error: Distribution `vtk==9.5.20251115.dev0 @ registry+https://wheels.vtk.org/` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on macOS (`macosx_14_0_arm64`), but `vtk` (v9.5.20251115.dev0) only has wheels for the following platforms: `manylinux_2_17_x86_64`, `manylinux_2_28_aarch64`, `manylinux2014_x86_64`, `macosx_10_10_x86_64`, `win_amd64`; consider adding "sys_platform == 'darwin' and platform_machine == 'arm64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

Locking a previous version with an arm64 wheel works:

```
uv add vtk==v9.5.20251101.dev0 --index https://wheels.vtk.org --pre --upgrade
```

However, I would prefer a command that installs the latest wheel that is compatible with the user's platform (e.g., linux users could get the latest nightly build). Is there a way to do this with uv?

Apologies if this is in the docs / has been asked somewhere else before and I missed it!

### Platform

Darwin 23.4.0 arm64

### Version

uv 0.9.10 (44f5a14f4 2025-11-17)

### Python version

Python 3.11.11

---

_Label `bug` added by @eigenvivek on 2025-11-20 23:06_

---

_Comment by @charliermarsh on 2025-11-21 02:30_

There's a primer on this here: https://docs.astral.sh/uv/concepts/resolution/#limited-resolution-environments. The gist of it is: if a package only ships wheels, and it's not a pure-Python package, it's not possible for us to guarantee a build for all possible environments. But you _can_ tell uv that you require wheels for specific environments (hence the message referencing `tool.uv.required-environments`), which will do the backtracking you've described.

---

_Label `bug` removed by @charliermarsh on 2025-11-21 02:30_

---

_Label `question` added by @charliermarsh on 2025-11-21 02:30_

---

_Comment by @eigenvivek on 2025-11-21 15:33_

Ahh I see, thank you for the link to the docs and apologies for missing this.

I thought adding `"sys_platform == 'darwin' and platform_machine == 'arm64'"` to my `pyproject.toml` would make the package exclusive to mac, but now I see that's not the case.

Thanks for your help and for uv!

---

_Closed by @eigenvivek on 2025-11-21 15:33_

---
