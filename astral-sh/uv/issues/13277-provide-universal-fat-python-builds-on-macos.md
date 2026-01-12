```yaml
number: 13277
title: Provide universal (fat) Python builds on macOS
type: issue
state: closed
author: danra
labels:
  - enhancement
assignees: []
created_at: 2025-05-03T18:01:11Z
updated_at: 2025-05-04T21:16:31Z
url: https://github.com/astral-sh/uv/issues/13277
synced_at: 2026-01-12T16:01:24Z
```

# Provide universal (fat) Python builds on macOS

---

_@danra_

### Summary

Apparently, `uv` currently provides only single-architecture python out of the box, either arm64 or x86_64:

```
✗ uv python list --all-arches
cpython-3.14.0a6-macos-x86_64-none                  <download available>
cpython-3.14.0a6+freethreaded-macos-x86_64-none     <download available>
cpython-3.14.0a6-macos-aarch64-none                 <download available>
cpython-3.14.0a6+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.3-macos-x86_64-none                    <download available>
cpython-3.13.3+freethreaded-macos-x86_64-none       <download available>
cpython-3.13.3-macos-aarch64-none                   <download available>
cpython-3.13.3+freethreaded-macos-aarch64-none      <download available>
```

It would be helpful if universal builds were provided.

My use case: I have a universal Xcode C++ project which I build under `uv` activated and which uses `pybind11` to operate some Python code. The project is only able to link on the architecture matching the installed Python version; on the other architecture, the linker fails with missing symbols because it can't find the Python `.dylib` of the matching architecture to link with.

### Example

_No response_

---

_Label `enhancement` added by @danra on 2025-05-03 18:01_

---

_Comment by @FishAlchemist on 2025-05-04 08:29_

I'll link the relevant issues under `Python Standalone Builds`.
* https://github.com/astral-sh/python-build-standalone/issues/140

---

_Comment by @konstin on 2025-05-04 21:16_

Tracking this in https://github.com/astral-sh/python-build-standalone/issues/140 instead to keep the discussion in one place.

---

_Closed by @konstin on 2025-05-04 21:16_

---
