```yaml
number: 17372
title: Use latest Pyodide version for each python version
type: pull_request
state: merged
author: ryanking13
labels:
  - bug
assignees: []
merged: true
base: main
head: pyodide-version-fetch
created_at: 2026-01-09T06:40:20Z
updated_at: 2026-01-12T23:59:27Z
url: https://github.com/astral-sh/uv/pull/17372
synced_at: 2026-01-13T00:22:58Z
```

# Use latest Pyodide version for each python version

---

_@ryanking13_

## Summary

This changes how the `download-metadata.json` file is generated for Pyodide.

Previously, if we have a different Pyodide version with a same Python version, the older Pyodide version is selected.

For example, say we have

- Pyodide 0.29.0 with Python 3.13.2
- Pyodide 0.28.3 with Python 3.13.2

then, pyodide 0.28.3 was stored in `download-metadata.json` as we iterate in decending order with overwriting the value that was previously written.

I fixed it by picking up the latest Pyodide version for each Python version.

## Test Plan

Ran `uv run -- crates/uv-python/fetch-download-metadata.py` locally, and verified the output:

```
2026-01-09 15:39:23 INFO Fetching Pyodide checksums: 0/4
2026-01-09 15:39:24 INFO Selected cpython-3.13.2-emscripten-wasm32-musl (0.29.1)
2026-01-09 15:39:24 INFO Selected cpython-3.12.7-emscripten-wasm32-musl (0.27.7)
2026-01-09 15:39:24 INFO Selected cpython-3.12.1-emscripten-wasm32-musl (0.26.4)
2026-01-09 15:39:24 INFO Selected cpython-3.11.3-emscripten-wasm32-musl (0.25.1)
```


---

_Review requested from @zanieb by @konstin on 2026-01-09 11:19_

---

_Comment by @konstin on 2026-01-09 11:19_

CC @hoodmane

---

_Comment by @zanieb on 2026-01-10 16:22_

The implementation here looks a bit different than the GraalPy and PyPy ones, e.g., they do something like

```python
                # Only keep the latest pypy version of each arch/platform
                if (python_version, arch, platform) not in results:
                    results[(python_version, arch, platform)] = download
```

It's not like that's beautiful Python, but it might be nice for the pattern to be consistent? What do you think?



---

_Comment by @ryanking13 on 2026-01-11 03:40_

@zanieb Sounds reasonable. I update the code to make it look similar to other finders.

---

_@zanieb approved on 2026-01-12 23:59_

---

_Label `bug` added by @zanieb on 2026-01-12 23:59_

---

_Merged by @zanieb on 2026-01-12 23:59_

---

_Closed by @zanieb on 2026-01-12 23:59_

---
