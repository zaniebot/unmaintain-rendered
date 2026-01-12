```yaml
number: 15473
title: Global Python Pin should be ignored when incompatible with project
type: pull_request
state: open
author: harsh-ps-2003
labels: []
assignees: []
base: main
head: pin
created_at: 2025-08-23T17:36:36Z
updated_at: 2025-08-30T08:30:00Z
url: https://github.com/astral-sh/uv/pull/15473
synced_at: 2026-01-12T16:11:45Z
```

# Global Python Pin should be ignored when incompatible with project

---

_@harsh-ps-2003_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

@zanieb in #14916 found an interesting bug. Global pin is the user preference across projects, but if the project locally has a  local pin, it should be an authoritative constraint and end up with error. This avoids blocking new projects that intentionally require a newer Python version than the user’s global default. We still error on a local `.python-version` inside the project to preserve explicit, repo-scoped intent. 

* Explicit `--python` → Always wins regardless of constraints
* Local `.python-version` → Project-scoped, errors on conflict
* Global `.python-version` → User preference, ignored if conflicts with project. Global pins are suggestions that can be overridden by project requirements
* Project `requires-python` → Fallback when no pins exist

Implementation :
* If a global `~/.config/uv/.python-version` conflicts with a project `requires-python`, we ignore the pin and use the project requirement
* If a local project `.python-version` conflicts, we error, with guidance to update the pin
* Explicit `--python` continues to override both

Now, global pins are suggestions that can be overridden by project requirements, rather than hard constraints that block project setup.

## Test Plan

A new has been added in `sync_python_version()` along with manual testing :
```
harshps22ugp@lab:~/projects/uv$ target/debug/uv python pin --global 3.10
Pinned `/home/harshps22ugp/.config/uv/.python-version` to `3.10`
harshps22ugp@lab:~/projects/uv$ mkdir -p /tmp/uv-global-pin && cd /tmp/uv-global-pin
harshps22ugp@lab:/tmp/uv-global-pin$ cat > pyproject.toml <<'EOF'
> [project]
> name = "project"
> version = "0.1.0"
> requires-python = ">=3.11"
> dependencies = ["anyio==3.7.0"]
> EOF
harshps22ugp@lab:/tmp/uv-global-pin$ /home/harshps22ugp/projects/uv/target/debug/uv sync
Using CPython 3.13.5
Creating virtual environment at: .venv
Resolved 4 packages in 276ms
Prepared 3 packages in 149ms
░░░░░░░░░░░░░░░░░░░░ [0/3] Installing wheels...                                                                                                               warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 3 packages in 35ms
 + anyio==3.7.0
 + idna==3.10
 + sniffio==1.3.1
harshps22ugp@lab:/tmp/uv-global-pin$ . .venv/bin/activate
(project) harshps22ugp@lab:/tmp/uv-global-pin$ python -V
Python 3.13.5
(project) harshps22ugp@lab:/tmp/uv-global-pin$
```
No error was thrown!

---

_@zanieb reviewed on 2025-08-29 18:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1123 on 2025-08-29 18:37_

Can we put this on `PythonRequest` as `as_pep440_version` or something instead?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1135 on 2025-08-29 18:38_

Can we do an exhaustive match here instead? 

---

_@zanieb reviewed on 2025-08-29 18:38_

---

_@zanieb reviewed on 2025-08-29 18:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1138 on 2025-08-29 18:39_

Do we need to roundtrip to a string and parse? Can't we just construct a `Version` from the u8s? I'd probably make this a `VersionRequest::as_version` utility too.

---

_@zanieb reviewed on 2025-08-29 18:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1165 on 2025-08-29 18:41_

Do we need to clone here? Can you use `PythonVersionFile::version`?

---

_@zanieb reviewed on 2025-08-29 18:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1196 on 2025-08-29 18:42_

This repeated else chain seems indicative of a need to refactor.

---

_@zanieb reviewed on 2025-08-29 18:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/find.rs`:98 on 2025-08-29 18:42_

I'm not sure I follow why this new comment is needed here.

---

_@zanieb reviewed on 2025-08-29 20:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:63 on 2025-08-29 20:01_

This import is not properly grouped

---
