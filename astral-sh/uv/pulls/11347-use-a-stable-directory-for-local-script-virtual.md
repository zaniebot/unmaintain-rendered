```yaml
number: 11347
title: Use a stable directory for (local) script virtual environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/stable-dir
created_at: 2025-02-09T02:47:29Z
updated_at: 2025-02-12T00:45:28Z
url: https://github.com/astral-sh/uv/pull/11347
synced_at: 2026-01-10T11:10:36Z
```

# Use a stable directory for (local) script virtual environments

---

_Pull request opened by @charliermarsh on 2025-02-09 02:47_

## Summary

Today, scripts use `CachedEnvironment`, which results in a different virtual environment path every time the interpreter changes _or_ the project requirements change. This makes it impossible to provide users with a stable path to the script that they can use for (e.g.) directing their editor.

This PR modifies `uv run` to use a stable path for local scripts (we continue to use `CachedEnvironment` for remote scripts and scripts from `stdin`). The logic now looks a lot more like it does for projects: we `get_or_init` an environment, etc.

For now, the path to the script is like: `environments-v1/4485801245a4732f`, where `4485801245a4732f` is a SHA of the absolute path to the script. But I'm not picky on that :)


---

_Label `enhancement` added by @charliermarsh on 2025-02-09 02:47_

---

_Converted to draft by @charliermarsh on 2025-02-09 02:47_

---

_Comment by @charliermarsh on 2025-02-09 02:51_

As a second PR, I'll add `uv sync --script` using these stable paths.

---

_Marked ready for review by @charliermarsh on 2025-02-09 03:21_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-11 02:25_

---

_Review requested from @konstin by @charliermarsh on 2025-02-11 02:25_

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:533 on 2025-02-11 11:17_

```suggestion
            format!("{file_name}-{digest}")
```

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:532 on 2025-02-11 11:18_

missing replace?

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:1171 on 2025-02-11 11:26_

```suggestion
/// The Python environment for a script.
```

---

_@konstin approved on 2025-02-11 11:29_

---

_@zanieb approved on 2025-02-11 22:18_

---

_Merged by @charliermarsh on 2025-02-12 00:45_

---

_Closed by @charliermarsh on 2025-02-12 00:45_

---

_Branch deleted on 2025-02-12 00:45_

---
