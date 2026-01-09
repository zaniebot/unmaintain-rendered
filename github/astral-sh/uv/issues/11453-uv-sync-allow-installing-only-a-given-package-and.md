---
number: 11453
title: "uv sync: allow installing only a given package and its dependencies"
type: issue
state: open
author: gaborbernat
labels:
  - enhancement
assignees: []
created_at: 2025-02-12T16:47:17Z
updated_at: 2025-02-13T13:55:21Z
url: https://github.com/astral-sh/uv/issues/11453
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync: allow installing only a given package and its dependencies

---

_Issue opened by @gaborbernat on 2025-02-12 16:47_

### Summary

I have a use case where I have an application containing many different "products". These products are collected together as dependencies for the application. We'd like to install the pinned dependencies of a given product without needing to `sync` the whole world.

### Example

It would be nice if I could do `uv sync` and then pass a package name, and `uv sync` would only install that package and its dependencies, not the whole lock file. Something like: `uv sync --only-dep ruff`.

---

_Label `enhancement` added by @gaborbernat on 2025-02-12 16:47_

---

_Comment by @zanieb on 2025-02-12 17:11_

I'm not sure where we talked about this... but in discussion of https://github.com/astral-sh/uv/issues/4028 I feel like we considered `--only-install-package`. Maybe another direction for this would be changing `--package` to allow arbitrary roots in your dependency tree (instead of just workspace members).

---

_Comment by @gaborbernat on 2025-02-12 17:23_

> I'm not sure where we talked about this... but in discussion of [#4028](https://github.com/astral-sh/uv/issues/4028) I feel like we considered `--only-install-package`. Maybe another direction for this would be changing `--package` to allow arbitrary roots in your dependency tree (instead of just workspace members).

Yeah that would be acceptable too 👍 The one difference is that this would not be only the package itself, but a package and its dependencies.

---

_Comment by @konstin on 2025-02-13 13:55_

An option for modelling this in the meantime would an extra per product, then selecting which product to install through its associated extra.

---
