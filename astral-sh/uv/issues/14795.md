```yaml
number: 14795
title: "Error message on direct references isn't super helpful"
type: issue
state: closed
author: thatch
labels:
  - error messages
assignees: []
created_at: 2025-07-21T16:33:52Z
updated_at: 2025-07-22T19:08:34Z
url: https://github.com/astral-sh/uv/issues/14795
synced_at: 2026-01-10T03:32:45Z
```

# Error message on direct references isn't super helpful

---

_Issue opened by @thatch on 2025-07-21 16:33_

### Summary

Context: We run a private index that allows/allowed direct references.

A transitive dep (let's call it `a > b > metaflow` started backtracking and hit an old artifact of `b` that had a git dependency for metaflow. In this case, what I'd like to see is info on `b` -- the one that has the dependency, not `metaflow` (because it's not that project's fault).

Diagnosing:

I managed to run `uv -v` and exhaustively check metadata urls looking for the string and figure out what was going on.

Solution Finding:

The error message is a little ambiguous on first read -- it seems like it just wants me to add `metaflow @` to the beginning of the dep line in `b` and it would then accept the url dependency, but that comes from an interpretation of "your dependencies".  In reality, it doesn't allow direct references at all?  If there's an escape hatch, it's not mentioned.

Current error:

```
error: Package `metaflow` attempted to resolve via URL: git+https://github.com/Netflix/metaflow.git@2.12.29. URL dependencies must be expressed as direct requirements or constraints. Consider adding `metaflow @ git+https://github.com/Netflix/metaflow.git@2.12.29` to your dependencies or constraints file.
```

A better error would be:

```
error: Package `foo==2.13.0` contains a url dependency.  URL dependencies MUST be done using direct project requirements or constraints, or with config setting <blah>.
```

### Platform

macOS 15.5 arm64 (M2)

### Version

uv 0.8.0 (0b2357294 2025-07-17)

### Python version

Python 3.10.16 (but that shouldn't matter)

---

_Label `bug` added by @thatch on 2025-07-21 16:33_

---

_Label `bug` removed by @zanieb on 2025-07-21 17:39_

---

_Label `error messages` added by @zanieb on 2025-07-21 17:39_

---

_Comment by @charliermarsh on 2025-07-21 20:52_

Just to clarify, doesn't adding `metaflow @ git+https://github.com/Netflix/metaflow.git@2.12.29` as a direct dependency (as suggested in the error) solve this?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-21 21:00_

---

_Comment by @charliermarsh on 2025-07-21 21:10_

I think we should try to show the derivation chain here.

---

_Closed by @charliermarsh on 2025-07-22 19:08_

---
