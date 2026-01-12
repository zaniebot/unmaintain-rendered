```yaml
number: 16766
title: uv build-backend does not allow building a single-module package (module-only, no package directory)
type: issue
state: open
author: DhavalGojiya
labels:
  - bug
  - build-backend
assignees: []
created_at: 2025-11-18T09:29:56Z
updated_at: 2025-11-18T10:44:18Z
url: https://github.com/astral-sh/uv/issues/16766
synced_at: 2026-01-12T16:02:37Z
```

# uv build-backend does not allow building a single-module package (module-only, no package directory)

---

_@DhavalGojiya_

### Summary

## What I want to do

I want to configure `uv-build-backend` to build a wheel for a project that contains **only a single Python file**:

```
pysolr.py
pyproject.toml
README.rst
...
```

I tried this in `pyproject.toml`:

```toml
[tool.uv.build-backend]
module-name = "pysolr"
module-root = ""
```

---

## The problem

Running:

```
uv build
```

gives:

```
Building source distribution (uv build backend)...
  × Failed to build `/home/dhaval/Open-Source/pysolr`
  ╰─▶ Expected a Python module at: pysolr/__init__.py
```

`uv` expects a **package directory** (`pysolr/__init__.py`), but my project is a **single-file module**, not a package.

The file is:

```
pysolr.py  <-- this is the module (file)
```

There is **no `pysolr/` directory**, and I’d like to keep it that way.

---

## Expected Behaviour

I want `uv-build-backend` to give first priority to:

```toml
[tool.uv.build-backend]
module-name = "pysolr"
module-root = ""
```

meaning:

- If a top-level `pysolr.py` module exists, `uv` should use that.
- If the module does NOT exist, it should fall back to the package directory `pysolr/__init__.py`.

Alternatively, an additional flag would also work - something that explicitly tells the build backend:

"The value in `module-name` refers to a single Python module (e.g., pysolr.py), not a package directory."

---

## Why this is needed

- Many older libraries (including `pysolr`) ship as **a single module**, not a directory package.
- Setuptools supports this automatically, but `uv` currently refuses to build unless an `__init__.py` exists.


### Platform

Linux 6.8.0-45-generic x86_64 GNU/Linux

### Version

uv 0.9.9

### Python version

Python 3.10

---

_Label `bug` added by @DhavalGojiya on 2025-11-18 09:29_

---

_Comment by @DhavalGojiya on 2025-11-18 09:33_

I think there is also an issue with `uv-build-backend`.

The following configuration:

    [tool.uv.build-backend]
    module-name = "pysolr"
    module-root = ""
    namespace = true

(I know `namespace = true` doesn't make sense here, but `uv` exits with an ambiguous error.)

Now running `uv build` gives:

    Building source distribution (uv build backend)...
    Building wheel from source distribution (uv build backend)...
      × Failed to build `/home/dhaval/Open-Source/pysolr`
      ├─▶ Failed to walk source tree: /home/dhaval/.cache/uv/sdists-v9/.tmp8wVG4G/pysolr-3.10.0
      ├─▶ IO error for operation on /home/dhaval/.cache/uv/sdists-v9/.tmp8wVG4G/pysolr-3.10.0/pysolr: No such file or directory (os error 2)
      ╰─▶ No such file or directory (os error 2)

---

_Label `build-backend` added by @konstin on 2025-11-18 10:44_

---
