```yaml
number: 6370
title: "Discover and respect `.python-version` files in parent directories"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: tracking/050
head: zb/version-file-discover
created_at: 2024-08-21T19:15:57Z
updated_at: 2024-11-04T19:48:15Z
url: https://github.com/astral-sh/uv/pull/6370
synced_at: 2026-01-12T16:07:20Z
```

# Discover and respect `.python-version` files in parent directories

---

_@zanieb_

Uses #6369 for test coverage.

Updates version file discovery to search up into parent directories. Also refactors Python request determination to avoid duplicating the user request / version file / workspace lookup logic in every command (this supersedes the work started in https://github.com/astral-sh/uv/pull/6372).

There is a bit of remaining work here, mostly around documentation. There are some edge-cases where we don't use the refactored request utility, like `uv build` — I'm not sure how I'm going to handle that yet as it needs a separate root directory.

---

_Label `breaking` added by @zanieb on 2024-10-02 19:04_

---

_@zanieb reviewed on 2024-10-04 14:03_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:1134 on 2024-10-04 14:03_

This code isn't used in production, but this updates the behavior in tandem with copying the implementation for `PythonVersionFile::discover` to solve a common footgun where you pass the project root as the place to stop discovery and discovery stopped _before_ looking in that directory.

---

_Marked ready for review by @zanieb on 2024-10-04 18:05_

---

_Added to milestone `v0.5.0` by @zanieb on 2024-10-04 18:06_

---

_@zanieb reviewed on 2024-10-04 18:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:399 on 2024-10-04 18:09_

Instead of making `workspace` optional here, I might introduce a new abstraction which is used by `WorkspacePython`? It's doing a bit more work than it ought to as-is, but it's not clear another abstraction is worth it.

---

_@zanieb reviewed on 2024-10-24 17:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:162 on 2024-10-24 17:43_

Annoyingly, I don't see a way to avoid this clone unless we make `PEP723Item` accept a borrowed script or create an `into_script -> Option<...>` method which we'd unwrap later. I think since it's in `add` it's fine to clone.

---

_Review requested from @charliermarsh by @zanieb on 2024-10-24 17:43_

---

_Comment by @zanieb on 2024-10-24 19:56_

I'll do a self-review of this too. It'd been a minute since I wrote it...

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:495 on 2024-10-28 20:45_

We may wan to use lowercase `requires-python` for clarity.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:132 on 2024-10-28 20:46_

Why do we prefer `.python-versions` here but nowhere else?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:144 on 2024-10-28 20:46_

Remove?

---

_@charliermarsh approved on 2024-10-28 20:46_

---

_@zanieb reviewed on 2024-10-28 20:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:132 on 2024-10-28 20:48_

Installation can operate on multiple versions, while the other operations require a single version. This is the existing behavior, just clearly written here.

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:144 on 2024-10-28 20:51_

Good point flagging this. We need to decide if `--no-config` should continue to omit `.python-version` file discovery. I'm not sure it does consistently today, but it did here. I think I dropped it because it's confusing — but we should discuss that choice. That's why this is unused now.

---

_@zanieb reviewed on 2024-10-28 20:51_

---

_@zanieb reviewed on 2024-10-28 21:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:144 on 2024-10-28 21:12_

There are a bunch of places today where we don't handle this. For example, `uv export` doesn't even have a `--no-config` flag.

---

_Merged by @zanieb on 2024-11-04 19:48_

---

_Closed by @zanieb on 2024-11-04 19:48_

---

_Branch deleted on 2024-11-04 19:48_

---
