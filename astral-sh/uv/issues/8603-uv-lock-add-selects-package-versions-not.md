---
number: 8603
title: uv lock/add selects package versions not available for current platform
type: issue
state: closed
author: per11235813
labels:
  - question
assignees: []
created_at: 2024-10-27T05:41:32Z
updated_at: 2024-10-27T19:03:29Z
url: https://github.com/astral-sh/uv/issues/8603
synced_at: 2026-01-10T01:24:30Z
---

# uv lock/add selects package versions not available for current platform

---

_Issue opened by @per11235813 on 2024-10-27 05:41_

First, I would like to thank you for `uv`, it is just awesome, but I do have a problem

The pyqt5 package has different versions available for linux and windows, Linux:
```
$ pip index versions pyqt5-qt5
pyqt5-qt5 (5.15.15)
Available versions: 5.15.15, 5.15.14, 5.15.2
```
and Windows:
```
(Notes) > pip index versions pyqt5-qt5
pyqt5-qt5 (5.15.2)
Available versions: 5.15.2
```

On windows `uv lock` wants to use 5.15.15, which is not available so I get a lock file that does not work on Windows. `uv add` fails with similar logic:
```
> uv add pyqt5-qt5            
Resolved 2 packages in 762ms
error: distribution pyqt5-qt5==5.15.15 @ registry+https://pypi.org/simple can't be installed because it doesn't have a source distribution or wheel for the current platform
```

Are there any way to specify a platform requirement for package selection for `uv lock` and `uv add`?

For now I use `uv pip compile` and `uv pip sync`.

---

_Comment by @FishAlchemist on 2024-10-27 07:55_

If you don't mind relying on different versions across platforms, you can actually use PEP 508's Environment Markers.
https://docs.astral.sh/uv/concepts/dependencies/#dependency-specifiers-pep-508

You can modify the pyproject.toml file like this, or include Environment Markers when using ``uv add``.
```toml
dependencies = [
    "pyqt5-qt5==5.15.15 ; platform_system != 'Windows'",
    "pyqt5-qt5==5.15.2 ; platform_system == 'Windows'",
]
```
Or do you think UV should use a version that is compatible across all platforms(Windows、Linux、Macos)?

Edit: 
However, if your sole requirement is Windows compatibility and there's no need for cross-platform functionality, following @bluss's approach(https://github.com/astral-sh/uv/issues/8603#issuecomment-2439899569) would suffice.



---

_Comment by @bluss on 2024-10-27 08:06_

That package version has the case that 'pyqt5-qt5' has both no windows wheels and no sdist, so it can't be built from source either. A windows-only resolution of the project generates a lockfile with no artifacts at all (no sdist and no wheels)


Forcing only locking for windows looks like this configuration pyproject.toml - but as said, it does not help.

```
[tool.uv]
environments = ["platform_system == 'Windows'"]
```

---

_Comment by @charliermarsh on 2024-10-27 18:56_

This is tracked in https://github.com/astral-sh/uv/issues/5182. I wrote a long explanation of it here: https://github.com/astral-sh/uv/issues/8536#issuecomment-2436123242. But in this case, the above solutions are correct: you can either tell the resolver to only solve for Windows, or you can tell the resolver to use different versions on Windows or non-Windows.

---

_Closed by @charliermarsh on 2024-10-27 18:56_

---

_Label `question` added by @charliermarsh on 2024-10-27 18:56_

---

_Comment by @charliermarsh on 2024-10-27 18:56_

(Happy to answer follow-ups but the core issue is tracked elsewhere.)

---

_Comment by @bluss on 2024-10-27 19:01_

I was probably going out on a confusing tangent: but my comment does not have a solution. Instead with `environments` the experience for the user is the same. It ends up in a scenario where uv could throw a warning or error - the package has no installable artifacts in the lockfile, so uv knows that resolution is 'bad'.  (Do you see what I mean - no sdist and no wheels at all in the lockfile for that package?)


---

_Referenced in [astral-sh/uv#9711](../../astral-sh/uv/issues/9711.md) on 2024-12-07 20:47_

---

_Referenced in [astral-sh/uv#9928](../../astral-sh/uv/pulls/9928.md) on 2024-12-16 03:23_

---

_Referenced in [neuroinformatics-unit/movement#683](../../neuroinformatics-unit/movement/pulls/683.md) on 2025-10-29 20:15_

---
