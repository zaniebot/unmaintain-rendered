```yaml
number: 16182
title: " Log Python choice in `uv init`"
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/log-requires-python-uv-init
created_at: 2025-10-08T11:26:22Z
updated_at: 2025-10-09T13:55:28Z
url: https://github.com/astral-sh/uv/pull/16182
synced_at: 2026-01-10T06:36:15Z
```

#  Log Python choice in `uv init`

---

_Pull request opened by @konstin on 2025-10-08 11:26_

Example output:

```
$ uv-debug init foo -v
  DEBUG uv 0.9.0+6 (https://github.com/astral-sh/uv/commit/d3324baf68823262f14d8e518b4292ed8f343c61 2025-10-08)
  DEBUG Acquired shared lock for `/home/konsti/.cache/uv`
  DEBUG No Python version file found in ancestors of working directory: /home/konsti/projects/foo
  DEBUG Checking for Python environment at: `foo/.venv`
  DEBUG Searching for default Python interpreter in managed installations or search path
  DEBUG Searching for managed installations at `/home/konsti/.local/share/uv/python`
  DEBUG Found managed installation `cpython-3.14.0-linux-x86_64-gnu`
  DEBUG Found `cpython-3.14.0-linux-x86_64-gnu` at `/home/konsti/.local/share/uv/python/cpython-3.14.0-linux-x86_64-gnu/bin/python3.14` (managed installations)
  DEBUG Using Python version `>=3.14` from default interpreter
  DEBUG `git rev-parse --is-inside-work-tree` failed but didn't contain `not a git repository` in stderr for `/home/konsti/projects/foo`
  DEBUG No Python version file found in ancestors of working directory: /home/konsti/projects/foo
  DEBUG Writing Python versions to `/home/konsti/projects/foo/.python-version`
  Initialized project `foo` at `/home/konsti/projects/foo`
  DEBUG Released lock at `/home/konsti/.cache/uv/.lock`
```

First commit is refactoring, second commit is the actual change.

---

_Review requested from @geofft by @konstin on 2025-10-08 11:26_

---

_Review requested from @zanieb by @konstin on 2025-10-08 11:26_

---

_Label `tracing` added by @konstin on 2025-10-08 11:26_

---

_@zsol approved on 2025-10-08 11:31_

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia2.giphy.com%2Fmedia%2FyyZRSvISN1vvW%2Fgiphy.gif&token=nice" data-gitmeme-token="nice"><img src="https://media2.giphy.com/media/yyZRSvISN1vvW/giphy.gif" title="Created by gitme.me with /nice"
        alt="nice"/></a>

---

_@zanieb reviewed on 2025-10-08 13:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:696 on 2025-10-08 13:29_

This could be a managed interpreter too (I know the names are confusing). Maybe we should say "found interpreter"?

---

_@zanieb approved on 2025-10-08 13:29_

---

_Closed by @konstin on 2025-10-09 13:42_

---

_Reopened by @konstin on 2025-10-09 13:42_

---

_Merged by @konstin on 2025-10-09 13:55_

---

_Closed by @konstin on 2025-10-09 13:55_

---

_Branch deleted on 2025-10-09 13:55_

---
