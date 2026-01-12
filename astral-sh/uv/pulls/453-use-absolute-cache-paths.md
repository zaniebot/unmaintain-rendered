```yaml
number: 453
title: Use absolute cache paths
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: absolute-cache-paths
created_at: 2023-11-19T13:27:38Z
updated_at: 2023-11-19T13:32:33Z
url: https://github.com/astral-sh/uv/pull/453
synced_at: 2026-01-12T16:03:57Z
```

# Use absolute cache paths

---

_@konstin_

Previously, git requirements would fail when setting `--cache-dir`:

```console
$ cargo run --bin puffin -- pip-compile --cache-dir cache-all-kinds scripts/benchmarks/requirements/all-kinds.in
error: Failed to build distribution from URL: git+https://github.com/pydantic/pydantic-extra-types.git
  Caused by: Invalid path URL: cache-all-kinds/git-v0/db/b49ffcfeb6c2e9d8
  ```

The cause is using a relative and not an absolute path, which `Url` needs, the solution is to turn the cache dir into an absolute path.

This never showed up in the tests since the tests use absolute temp dirs for everything.

---

_Merged by @konstin on 2023-11-19 13:32_

---

_Closed by @konstin on 2023-11-19 13:32_

---

_Branch deleted on 2023-11-19 13:32_

---
