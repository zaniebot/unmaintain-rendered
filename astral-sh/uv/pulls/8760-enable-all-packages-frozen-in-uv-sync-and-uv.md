```yaml
number: 8760
title: "Enable `--all-packages --frozen` in `uv sync` and `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/frozen
created_at: 2024-11-01T19:32:44Z
updated_at: 2024-11-02T02:48:56Z
url: https://github.com/astral-sh/uv/pull/8760
synced_at: 2026-01-10T11:59:59Z
```

# Enable `--all-packages --frozen` in `uv sync` and `uv export`

---

_Pull request opened by @charliermarsh on 2024-11-01 19:32_

## Summary

This PR improves the interaction of `--frozen` such that we reduce the dependency on the `pyproject.toml` and increase the dependency on the `uv.lock`. Specifically, we now read the list of workspace members from the `uv.lock` rather than the `pyproject.toml`, which means we don't need to discover the member `pyproject.toml` files in order to perform a `uv sync --frozen --all-packages`.


---

_Label `enhancement` added by @charliermarsh on 2024-11-01 19:32_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/target.rs`:19 on 2024-11-01 19:44_

The core change here is that we used to have `InstallTarget` defined in `uv-workspace`, and none of the variants had `Lock`. Now, the `InstallTarget` has `Lock`, and the `to_resolution` method from `Lock` got moved over here. The benefit is that we can determine the workspace members without relying on discovering the `pyproject.toml` files of each member -- we can just read them from the lockfile, which enables `--frozen --all`.

---

_@charliermarsh reviewed on 2024-11-01 19:44_

---

_Renamed from "Enable `--all --frozen` in `uv sync` and `uv export`" to "Enable `--all-packages --frozen` in `uv sync` and `uv export`" by @charliermarsh on 2024-11-02 02:33_

---

_Merged by @charliermarsh on 2024-11-02 02:48_

---

_Closed by @charliermarsh on 2024-11-02 02:48_

---

_Branch deleted on 2024-11-02 02:48_

---
