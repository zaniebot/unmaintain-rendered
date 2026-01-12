```yaml
number: 2294
title: "Move source distribution unpacking out of `build`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: charlie/unpack
created_at: 2024-03-08T03:23:10Z
updated_at: 2024-03-08T03:40:12Z
url: https://github.com/astral-sh/uv/pull/2294
synced_at: 2026-01-12T16:04:57Z
```

# Move source distribution unpacking out of `build`

---

_@charliermarsh_

## Summary

If a user provides a source distribution via a direct path, it can either be an archive (like a `.tar.gz` or `.zip` file) or a directory. If the former, we need to extract (e.g., unzip) the contents at some point. Previously, this extraction was in `uv-build`; this PR lifts it up to the distribution database.

The first benefit here is that various methods that take the distribution are now simpler, as they can assume a directory.

The second benefit is that we no longer extract _multiple times_ when working with a source distribution. (Previously, if we tried to get the metadata, then fell back and built the wheel, we'd extract the wheel _twice_.)


---

_Label `performance` added by @charliermarsh on 2024-03-08 03:23_

---

_Label `internal` added by @charliermarsh on 2024-03-08 03:23_

---

_Merged by @charliermarsh on 2024-03-08 03:40_

---

_Closed by @charliermarsh on 2024-03-08 03:40_

---

_Branch deleted on 2024-03-08 03:40_

---
