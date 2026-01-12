```yaml
number: 14877
title: Symlink (or copy) binary entry points into ephemeral environments
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/link-entrypoints
created_at: 2025-07-24T20:17:55Z
updated_at: 2025-08-06T17:35:23Z
url: https://github.com/astral-sh/uv/pull/14877
synced_at: 2026-01-12T16:11:27Z
```

# Symlink (or copy) binary entry points into ephemeral environments

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/14874

In addition to copying entry points and rewriting their shebangs, if we encounter entry points that do not have shebangs (e.g., binaries), we'll symlink (or copy, on Windows) them into the directory.

I'm uncertain if this is the correct approach or if we should change binary discovery in Ruff to account for this case.

---

_Label `bug` added by @zanieb on 2025-07-24 20:17_

---

_Comment by @geofft on 2025-08-06 17:35_

One think to think about with a copy is that executable-relative dependencies will break. This will be fine for symlinks, and so in particular `$ORIGIN`-relative dependencies will be fine... but is there an equivalent on Windows? I believe Windows executables load .dlls out of their current directory, do people rely on that in stuff shipped in wheels?

---
