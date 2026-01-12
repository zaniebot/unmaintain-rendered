```yaml
number: 4607
title: "Allow `uv add` to specify optional dependency groups"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: uv-add-optional
created_at: 2024-06-28T00:01:26Z
updated_at: 2024-06-28T01:24:22Z
url: https://github.com/astral-sh/uv/pull/4607
synced_at: 2026-01-12T16:06:20Z
```

# Allow `uv add` to specify optional dependency groups

---

_@ibraheemdev_

## Summary

Implements `uv add --optional <group>`, which adds a dependency to `project.optional-dependency.<group>`.

Resolves https://github.com/astral-sh/uv/issues/4585.

---

_@charliermarsh reviewed on 2024-06-28 01:05_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/remove.rs`:141 on 2024-06-28 01:05_

Should this just be part of the `anyhow::bail!`? It took me a sec to realize that this running after we've been unable to find the dependency in some _other_ group. I wonder if there's a way to structure the code such that the data flow is clearer. Maybe just a matter of expanding the documentation.

---

_@charliermarsh approved on 2024-06-28 01:05_

---

_Label `preview` added by @charliermarsh on 2024-06-28 01:05_

---

_@ibraheemdev reviewed on 2024-06-28 01:17_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/remove.rs`:141 on 2024-06-28 01:17_

Renamed to `warn_if_present` and updated the docs.

---

_Merged by @ibraheemdev on 2024-06-28 01:24_

---

_Closed by @ibraheemdev on 2024-06-28 01:24_

---

_Branch deleted on 2024-06-28 01:24_

---
