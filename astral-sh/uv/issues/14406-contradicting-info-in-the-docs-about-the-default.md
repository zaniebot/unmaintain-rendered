```yaml
number: 14406
title: contradicting info in the docs about the default build backend
type: issue
state: closed
author: DetachHead
labels:
  - question
  - build-backend
assignees: []
created_at: 2025-07-02T04:03:57Z
updated_at: 2025-07-04T12:26:17Z
url: https://github.com/astral-sh/uv/issues/14406
synced_at: 2026-01-12T16:01:48Z
```

# contradicting info in the docs about the default build backend

---

_@DetachHead_

### Question

in [the build backend docs](https://docs.astral.sh/uv/concepts/build-backend/#the-uv-build-backend) it says:

> When preview mode is not enabled, uv uses [hatchling](https://pypi.org/project/hatchling/) as the default build backend.

this contradicts [the config docs](https://docs.astral.sh/uv/concepts/projects/config/#project-packaging) which says that the default build backend is setuptools:

> Setting `tool.uv.package = true` will force a project to be built and installed into the project environment. If no build system is defined, uv will use the setuptools legacy backend.

it also implies that if preview mode is enabled, then `uv_build` is the default build backend, but that does not seem to be the case either. when i put the following in my `pyproject.toml` and run `uv build` without specifying a build backend, it still falls back to setuptools:

```toml
[tool.uv]
preview = true
```

```
> uv build
Building source distribution...
error: Multiple top-level packages discovered in a flat-layout: ['foo', 'bar'].

To avoid accidental inclusion of unwanted files or directories,
setuptools will not proceed with this build.

If you are trying to create a single distribution with multiple packages
on purpose, you should not rely on automatic discovery.
Instead, consider the following options:

1. set up custom discovery (`find` directive with `include` or `exclude`)
2. use a `src-layout`
3. explicitly set `py_modules` or `packages` with a list of names

To find more information, look for "package discovery" on setuptools docs.
  × Failed to build `C:\Users\user\project`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_sdist` failed (exit code: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

### Platform

windows 11

### Version

uv 0.7.17 (41c218a89 2025-06-29)

---

_Label `question` added by @DetachHead on 2025-07-02 04:03_

---

_Comment by @zanieb on 2025-07-02 20:44_

The former is about `uv init`, the latter is the standards-compliant behavior for building Python packages.

---

_Comment by @zanieb on 2025-07-02 20:45_

We've actually just fixed this, so you should see it updated once we release.

---

_Label `build-backend` added by @konstin on 2025-07-02 20:56_

---

_Assigned to @konstin by @konstin on 2025-07-03 17:40_

---

_Unassigned @konstin by @konstin on 2025-07-04 12:26_

---

_Closed by @konstin on 2025-07-04 12:26_

---
