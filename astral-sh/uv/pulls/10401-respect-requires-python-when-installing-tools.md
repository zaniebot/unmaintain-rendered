```yaml
number: 10401
title: "Respect `requires-python` when installing tools"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/too
created_at: 2025-01-08T17:14:11Z
updated_at: 2025-01-08T17:38:19Z
url: https://github.com/astral-sh/uv/pull/10401
synced_at: 2026-01-10T11:44:47Z
```

# Respect `requires-python` when installing tools

---

_Pull request opened by @charliermarsh on 2025-01-08 17:14_

## Summary

This PR revives https://github.com/astral-sh/uv/pull/7827 to improve tool resolutions such that, if the resolution fails, and the selected interpreter doesn't match the required Python version from the solve, we attempt to re-solve with a newly-discovered interpreter that _does_ match the required Python version.

For now, we attempt to choose a Python interpreter that's greater than the inferred `requires-python`, but compatible with the same Python minor. This helps avoid successive failures for cases like Posting, where choosing Python 3.13 fails because it has a dependency that lacks source distributions and doesn't publish any Python 3.13 wheels. We should further improve the strategy to solve _that_ case too, but this is at least the more conservative option...

In short, if you do `uv tool instal posting`, and we find Python 3.8 on your machine, we'll detect that `requires-python: >=3.11`, then search for the latest Python 3.11 interpreter and re-resolve.

Closes https://github.com/astral-sh/uv/issues/6381.
Closes https://github.com/astral-sh/uv/issues/10282.

## Test Plan

The following should succeed:

```
cargo run python uninstall --all
cargo run python install 3.8
cargo run tool install posting
```

In the logs, we see:

```
...
DEBUG No compatible version found for: posting
DEBUG Refining interpreter with: Python >=3.11, <3.12
DEBUG Searching for Python >=3.11, <3.12 in managed installations or search path
DEBUG Searching for managed installations at `/Users/crmarsh/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.8.20-macos-aarch64-none`
DEBUG Found `cpython-3.13.1-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from search path: does not satisfy request `>=3.11, <3.12`
DEBUG Found `cpython-3.11.7-macos-aarch64-none` at `/opt/homebrew/bin/python3.11` (search path)
DEBUG Re-resolving with Python 3.11.7
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.7
DEBUG Solving with target Python version: >=3.11.7
DEBUG Adding direct dependency: posting*
DEBUG Searching for a compatible version of posting (*)
...
```


---

_Review requested from @zanieb by @charliermarsh on 2025-01-08 17:14_

---

_Review requested from @konstin by @charliermarsh on 2025-01-08 17:14_

---

_Label `bug` added by @charliermarsh on 2025-01-08 17:14_

---

_Marked ready for review by @charliermarsh on 2025-01-08 17:14_

---

_Comment by @charliermarsh on 2025-01-08 17:20_

I think this is a significant improvement but not perfect. I have a few more ideas to improve it further. (But I have no idea how to test it in our suite.)

---

_@zanieb reviewed on 2025-01-08 17:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:141 on 2025-01-08 17:35_

For some reason I don't love "refining", but I don't have a great alternative off the top of my head :)

---

_@zanieb approved on 2025-01-08 17:36_

Cool, thank you for taking this on!

---

_@charliermarsh reviewed on 2025-01-08 17:38_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/common.rs`:141 on 2025-01-08 17:38_

Lol, fair!

---

_Merged by @charliermarsh on 2025-01-08 17:38_

---

_Closed by @charliermarsh on 2025-01-08 17:38_

---

_Branch deleted on 2025-01-08 17:38_

---
