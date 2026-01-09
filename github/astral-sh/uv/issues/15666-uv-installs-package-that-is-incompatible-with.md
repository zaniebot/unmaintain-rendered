---
number: 15666
title: "`uv` installs package that is incompatible with environment python version"
type: issue
state: closed
author: vdyma
labels:
  - bug
assignees: []
created_at: 2025-09-03T16:06:44Z
updated_at: 2025-09-05T09:12:12Z
url: https://github.com/astral-sh/uv/issues/15666
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv` installs package that is incompatible with environment python version

---

_Issue opened by @vdyma on 2025-09-03 16:06_

### Summary

I have an environment with Python 3.11 (specified as `requires-python = "~=3.11.0"` in pyproject.toml).
One of the dependencies is: `"ai-edge-litert>=1.4.0"`.
For some reason, `uv sync` also installs `backports-strenum` which requires python<3.11.
I added to my dependencies list the following `"backports-strenum; python_version < '3.11'"`, but this package still gets installed, even though env python version contradicts the specified marker

### Platform

Windows 11 x86_65, WSL 2, Ubuntu 24.04

### Version

0.8.11

### Python version

3.11.13

---

_Label `bug` added by @vdyma on 2025-09-03 16:06_

---

_Comment by @zanieb on 2025-09-03 16:44_

Please see https://github.com/astral-sh/uv/issues/14711 (and the linked issues / discussions)

---

_Comment by @vdyma on 2025-09-04 08:54_

Thanks for the quick response!
The linked issue describes why `requires-python` might ignore the upper bound of the python version. Can the same reasoning be applied to `python_version` markers too?
Also, if I set: `override-dependencies = ["backports-strenum; python_version < '3.11'"]` - it works as expected and `backports-strenum` doesn't get installed in my environment. How come `override-dependencies` is stricter about specified markers?

---

_Comment by @zanieb on 2025-09-04 17:09_

We do respect `python_version` markers.

While you added `"backports-strenum; python_version < '3.11'"` as a first-party marker, that isn't propagated to other package's dependencies, so `ai-edge-litert` still includes it as a dependency unconditional on the Python version

https://inspector.pypi.io/project/ai-edge-litert/1.4.0/packages/8b/4a/2efc281909c8e132aac6c8e2e27a02775405dda62c8dc21803990def0163/ai_edge_litert-1.4.0-cp310-cp310-macosx_12_0_arm64.whl/ai_edge_litert-1.4.0.dist-info/METADATA#line.26

Using `override-dependencies` _does_ override the dependency declaration in `ai-edge-litert`, which is an appropriate fix. I'd encourage the upstream to add this marker to the dependency if the package should be usable on 3.11+ and the dependency is not required.

If we were to respect the upper bound on the `requires-python` in `backports-strenum`, resolution would just fail.

---

_Comment by @vdyma on 2025-09-05 09:12_

Thanks for explaining. I will use `override-dependencies` as a solution then.

---

_Closed by @vdyma on 2025-09-05 09:12_

---

_Referenced in [google-ai-edge/LiteRT#4421](../../google-ai-edge/LiteRT/issues/4421.md) on 2025-11-06 11:30_

---
