---
number: 16675
title: Support transparent upgrades for non-CPython managed interpreters
type: issue
state: open
author: terror
labels: []
assignees: []
created_at: 2025-11-10T22:05:21Z
updated_at: 2025-11-10T23:00:24Z
url: https://github.com/astral-sh/uv/issues/16675
synced_at: 2026-01-07T13:12:19-06:00
---

# Support transparent upgrades for non-CPython managed interpreters

---

_Issue opened by @terror on 2025-11-10 22:05_

Our "transparent upgrade" machinery (minor-version links/junctions) [only runs for CPython](https://github.com/astral-sh/uv/blob/9a21897f3d5a36413c1d1305a5cd5a3da05b1391/crates/uv-python/src/managed.rs#L754), so prerelease warnings are suppressed for PyPy/GraalPy/Pyodide installs. We should extend the link/update flow and warning logic to cover every managed implementation we ship (PyPy, GraalPy, etc.) so `uv python upgrade` can actually promote their prereleases to stable builds.

---

_Comment by @terror on 2025-11-10 22:05_

Related: https://github.com/astral-sh/uv/pull/16619#discussion_r2511901322

---

_Comment by @zanieb on 2025-11-10 22:57_

Related #16637 

---

_Comment by @zanieb on 2025-11-10 23:00_

One nuance here is that `uv python upgrade` is usable without the transparent machinery, e.g., for the purposes of #16619, as long as we don't find the interpreter in a virtual environment, e.g., if we discover it from the managed directory directly, then `uv python upgrade` will work fine to get you a newer version.

---
