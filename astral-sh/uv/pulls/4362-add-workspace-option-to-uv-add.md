```yaml
number: 4362
title: "Add `--workspace` option to `uv add`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: ibraheem/uv-add-workspace
created_at: 2024-06-17T17:00:28Z
updated_at: 2024-06-18T16:26:08Z
url: https://github.com/astral-sh/uv/pull/4362
synced_at: 2026-01-12T16:06:11Z
```

# Add `--workspace` option to `uv add`

---

_@ibraheemdev_

## Summary

Implements `uv add foo --workspace`, which adds `foo` as a workspace dependency with the corresponding `tool.uv.sources` entry.

Part of https://github.com/astral-sh/uv/issues/3959.


---

_Label `preview` added by @ibraheemdev on 2024-06-17 17:07_

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-17 17:07_

---

_Review requested from @konstin by @ibraheemdev on 2024-06-17 17:07_

---

_@ibraheemdev reviewed on 2024-06-17 17:08_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:45 on 2024-06-17 17:08_

`toml_edit` does some implicit conversions/insertion when indexing directly on an `Item` which makes this form a lot better.. but very verbose. Maybe an extension trait could help for some of the common conversions.

---

_@zanieb approved on 2024-06-17 21:07_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:505 on 2024-06-18 09:15_

Should this be `uv remove --dev`?

---

_Review comment by @konstin on `crates/uv/src/cli.rs`:1616 on 2024-06-18 09:19_

I'm on the fence whether we need an explicit argument for this, we could also do workspace discovery and know that it has to be a workspace dep, otoh it's consistent with us requiring `workspace = true` in `pyproject.toml` (it's something we took from cargo, it's helpful to make we don't accidentally flip between workspace and index and to keep the information about the package source local)

---

_@konstin approved on 2024-06-18 09:19_

---

_@ibraheemdev reviewed on 2024-06-18 15:58_

---

_Review comment by @ibraheemdev on `crates/uv/tests/edit.rs`:505 on 2024-06-18 15:58_

Great catch, thanks.

---

_Merged by @ibraheemdev on 2024-06-18 16:26_

---

_Closed by @ibraheemdev on 2024-06-18 16:26_

---

_Branch deleted on 2024-06-18 16:26_

---
