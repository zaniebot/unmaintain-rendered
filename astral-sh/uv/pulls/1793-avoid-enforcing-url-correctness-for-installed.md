```yaml
number: 1793
title: Avoid enforcing URL correctness for installed distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/direct
created_at: 2024-02-21T04:20:41Z
updated_at: 2024-02-21T14:06:32Z
url: https://github.com/astral-sh/uv/pull/1793
synced_at: 2026-01-10T15:33:24Z
```

# Avoid enforcing URL correctness for installed distributions

---

_Pull request opened by @charliermarsh on 2024-02-21 04:20_

## Summary

Allows the corresponding `pypi_types` struct to use any URL, since other installers can put those into the environment, and Poetry seems to write invalid URLs.

If we see a distribution with an invalid URL, we just treat it as a registry distribution, which isn't ideal, but is better than (1) erroring, and (2) changing `Url` to `String` everywhere internally. (I'm torn on this second option.)

Closes https://github.com/astral-sh/uv/issues/1744.

## Test Plan

- Added `flask = { git = "git@github.com:pallets/flask.git", rev = "b90a4f1f4a370e92054b9cc9db0efcb864f87ebe" }` to `scripts/editable-installs/poetry_editable/pyproject.toml`.
- Ran `poetry install`.
- Ran `cargo pip freeze`. Verified that it errored on `main`, but passed here.
- Ran `cargo run pip install "flask==3.0.0"`. Verified that it uninstalled the existing Flask, and installed a new version from the registry.


---

_Review requested from @konstin by @charliermarsh on 2024-02-21 04:20_

---

_Label `bug` added by @charliermarsh on 2024-02-21 04:20_

---

_Comment by @konstin on 2024-02-21 11:44_

I'm trying to get this fixed upstream: https://github.com/pypa/packaging.python.org/pull/1506

---

_Comment by @charliermarsh on 2024-02-21 13:24_

@konstin - I think we should still merge this? It means uv is otherwise unusable (even to list installed dependencies) in certain Poetry environments.

---

_@konstin approved on 2024-02-21 13:49_

I think we have to until there's a poetry release with the fix

---

_Merged by @charliermarsh on 2024-02-21 14:06_

---

_Closed by @charliermarsh on 2024-02-21 14:06_

---

_Branch deleted on 2024-02-21 14:06_

---
