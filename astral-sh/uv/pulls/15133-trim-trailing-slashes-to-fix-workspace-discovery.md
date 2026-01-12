```yaml
number: 15133
title: Trim trailing slashes to fix workspace discovery
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - breaking
assignees: []
merged: true
base: zb/090
head: konsti/trim-trailing-slashes
created_at: 2025-08-07T12:24:00Z
updated_at: 2025-10-07T20:59:13Z
url: https://github.com/astral-sh/uv/pull/15133
synced_at: 2026-01-12T16:11:35Z
```

# Trim trailing slashes to fix workspace discovery

---

_@konstin_

Previously, workspace discovery would miss a valid workspace root is the project path has a trailing slash. This would cause `uv build` to use the wrong target directory.

This is a breaking change. Specifically, the output path changes from `<workspace root>/<child>/dist/` to `<workspace root>/dist/` when using `uv build <child>/` in the workspace root.

**Setup** Create a workspace with a member.

```
uv init
uv init --lib child
```

**Before**

```
$ uv build child/
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built child/dist/child-0.1.0.tar.gz
Successfully built child/dist/child-0.1.0-py3-none-any.whl
$ uv build child
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built dist/child-0.1.0.tar.gz
Successfully built dist/child-0.1.0-py3-none-any.whl
```

**After**

```
$ uv build child/
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built dist/child-0.1.0.tar.gz
Successfully built dist/child-0.1.0-py3-none-any.whl
$ uv build child
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built dist/child-0.1.0.tar.gz
Successfully built dist/child-0.1.0-py3-none-any.whl
```

Fixes #13914

---

_Label `bug` added by @konstin on 2025-08-07 12:24_

---

_Label `breaking` added by @konstin on 2025-08-07 12:24_

---

_Added to milestone `v0.9.0` by @konstin on 2025-08-07 15:21_

---

_Comment by @konstin on 2025-08-07 15:22_

Tentatively putting this into the 0.9 milestone for the output path change.

---

_@zanieb approved on 2025-10-07 20:49_

---

_Comment by @geofft on 2025-10-07 20:49_

I don't see the actual diff in the before/new sections above, I think?

---

_Merged by @konstin on 2025-10-07 20:59_

---

_Closed by @konstin on 2025-10-07 20:59_

---

_Branch deleted on 2025-10-07 20:59_

---
