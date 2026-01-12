```yaml
number: 5524
title: "Add support for benchmarking `uv sync` and `uv lock`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - benchmarks
assignees: []
merged: true
base: main
head: charlie/benchmarks
created_at: 2024-07-28T20:54:27Z
updated_at: 2024-07-28T21:09:09Z
url: https://github.com/astral-sh/uv/pull/5524
synced_at: 2026-01-12T16:06:52Z
```

# Add support for benchmarking `uv sync` and `uv lock`

---

_@charliermarsh_

## Summary

This PR adds support for `uv lock` and `uv sync` in the standardized benchmarks script.

Part of: https://github.com/astral-sh/uv/issues/5263.

## Test Plan

For example:

```sh
python scripts/bench/__main__.py --uv-project --benchmark resolve-cold ./scripts/requirements/trio.in --verbose
```

---

_Renamed from "Charlie/benchmarks" to "Add support for benchmarking `uv sync` and `uv lock`" by @charliermarsh on 2024-07-28 20:54_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1986 on 2024-07-28 20:55_

I need this for the benchmarking script (we can't change directories because we use a single hyperfine command, and that has to run from a consistent directory), but we can discuss whether it should be exposed to users. (I think it should... Very useful.)

---

_@charliermarsh reviewed on 2024-07-28 20:55_

---

_Label `benchmarks` added by @charliermarsh on 2024-07-28 20:58_

---

_Merged by @charliermarsh on 2024-07-28 21:09_

---

_Closed by @charliermarsh on 2024-07-28 21:09_

---

_Branch deleted on 2024-07-28 21:09_

---
