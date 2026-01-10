---
number: 3733
title: Editable- and workspace-aware lookahead resolver
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-05-22T10:43:33Z
updated_at: 2024-06-01T18:21:12Z
url: https://github.com/astral-sh/uv/issues/3733
synced_at: 2026-01-10T01:23:30Z
---

# Editable- and workspace-aware lookahead resolver

---

_Issue opened by @konstin on 2024-05-22 10:43_

Dependencies in uv come in two shapes: Index dependencies, with a name and a version specifier, and url dependencies, pointing to a remote file, a git repository, local file or local directory. Only url dependencies are allowed to have url dependencies, and of these only local directories are allowed to be editables. 

Currently, editables are collected and built separately, and then the lookahead resolver does a DAG traversal through all url requirements and their dependencies to collect a list of the names and urls of all url requirements. This tells us when a name-version requirement elsewhere in the dependency tree is fulfilled by a url requirement and gives us the list of allowed urls.

Instead, we should stop special casing editables and make them a regular path dependency, resolved by the lookahead resolver. Editables are special because we don't store them in the cache (following the spec), so the lookahead resolver needs to return a handle to the editable with non-static metadata it had to build. In addition the lookahead resolver should consider all `uv.tool` metadata and perform (and cache) workspace discovery for directory dependencies (editable or not). The lookahead resolver will consider workspaces both for static and dynamic metadata, in the latter case building with PEP 517 hooks to use the 517 metadata from the build plus the resolved workspace for lowering the requirements.

---

_Label `internal` added by @konstin on 2024-05-22 10:43_

---

_Referenced in [astral-sh/uv#3404](../../astral-sh/uv/issues/3404.md) on 2024-05-26 18:55_

---

_Comment by @charliermarsh on 2024-06-01 18:21_

@konstin - I think this was solved this week.

---

_Closed by @charliermarsh on 2024-06-01 18:21_

---
