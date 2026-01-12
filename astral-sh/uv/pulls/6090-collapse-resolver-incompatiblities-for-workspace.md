```yaml
number: 6090
title: Collapse resolver incompatiblities for workspace members
type: pull_request
state: closed
author: zanieb
labels:
  - error messages
assignees: []
draft: true
base: main
head: zb/resolver-workspace-member
created_at: 2024-08-14T17:09:45Z
updated_at: 2024-08-14T19:49:13Z
url: https://github.com/astral-sh/uv/pull/6090
synced_at: 2026-01-12T16:07:12Z
```

# Collapse resolver incompatiblities for workspace members

---

_@zanieb_

Part of https://github.com/astral-sh/uv/pull/6066

Transforms the derivation tree to exclude "no versions" incompatibilities for workspace members so we don't say things like (where `bird` is a workspace member):

> Because only bird==0.1.0 is available and bird==0.1.0 depends on anyio==4.3.0, we can conclude that all versions of bird depend on anyio==4.3.0.

Instead, we say:

> Because bird==0.1.0 depends on anyio==4.3.0

There's more to be done, more in the flavor of #6066 where we add custom handling for workspace members in the formatter to avoid saying things like "all versions of bird" (which doesn't make sense, because there's only the one local version) and "you require bird" (which could be clearer, i.e., "the workspace includes bird").

Note that https://github.com/astral-sh/uv/pull/6090/commits/0403e802576f2ad853e4b4586060a5fbc699021c isn't actually necessary for this change. I may drop it and just match on `NoVersions` directly. I'm not sure of the long-term trade-offs here.

---

_Label `error messages` added by @zanieb on 2024-08-14 17:09_

---

_@zanieb reviewed on 2024-08-14 17:18_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:289 on 2024-08-14 17:18_

This is a display utility, need to find a home for it.

---

_Comment by @zanieb on 2024-08-14 19:49_

These commits are being used in #6092 

---

_Closed by @zanieb on 2024-08-14 19:49_

---
