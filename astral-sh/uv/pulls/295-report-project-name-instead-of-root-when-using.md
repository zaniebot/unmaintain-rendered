```yaml
number: 295
title: "Report project name instead of `root` when using `pyproject.toml` files"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/root-name
created_at: 2023-11-02T17:28:46Z
updated_at: 2023-11-03T15:22:12Z
url: https://github.com/astral-sh/uv/pull/295
synced_at: 2026-01-12T16:03:51Z
```

# Report project name instead of `root` when using `pyproject.toml` files

---

_@zanieb_

Part of https://github.com/astral-sh/puffin/issues/214

Adds a `project: Option<PackageName>` to the `Manifest`, `Resolver`, and `RequirementsSpecification`.
To populate an optional `name` for `PubGubPackage::Root`.

I'll work on removing the version number next.

Should we consider using the parent directory name when a `pyproject.toml` file is not present?

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_unsolvable_requirements.snap`:19 on 2023-11-02 17:33_

Previously... "root 0a0.dev0 depends on django ∅"

---

_@zanieb reviewed on 2023-11-02 17:33_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/requirements.rs`:153 on 2023-11-03 00:21_

My minor preference would be not to do this if multiple projects are specified.

---

_@charliermarsh approved on 2023-11-03 00:22_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:153 on 2023-11-03 00:27_

Hmm seems doable. I'm not sure I share the preference though, I guess I'm a little murky on the use-case of multiple pyproject input files in the first place.

---

_@zanieb reviewed on 2023-11-03 00:27_

---

_Merged by @zanieb on 2023-11-03 15:22_

---

_Closed by @zanieb on 2023-11-03 15:22_

---

_Branch deleted on 2023-11-03 15:22_

---
