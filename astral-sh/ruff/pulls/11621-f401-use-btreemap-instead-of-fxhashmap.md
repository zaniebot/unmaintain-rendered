```yaml
number: 11621
title: F401 use BTreeMap instead of FxHashMap
type: pull_request
state: merged
author: plredmond
labels: []
assignees: []
merged: true
base: main
head: ruff.f401.nondet
created_at: 2024-05-30T16:54:59Z
updated_at: 2024-05-30T17:59:16Z
url: https://github.com/astral-sh/ruff/pull/11621
synced_at: 2026-01-12T15:55:38Z
```

# F401 use BTreeMap instead of FxHashMap

---

_@plredmond_

* Potentially resolves #11619 (nondeterministic hashmap order across different architectures) in F401 by replacing a hashmap with nondeterministic traversal order with an ordered mapping.

I'm not sure how to test this with our CI/CD. I don't have an s390x machine at home. Should I try it in Qemu?

---

_Review requested from @MichaReiser by @plredmond on 2024-05-30 16:54_

---

_Assigned to @plredmond by @plredmond on 2024-05-30 16:55_

---

_Comment by @github-actions[bot] on 2024-05-30 17:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-05-30 17:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:224 on 2024-05-30 17:29_

Can this not be a `BTreeMap`?

---

_@plredmond reviewed on 2024-05-30 17:34_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:224 on 2024-05-30 17:34_

Could be. I'll make that change so we don't add the dependency.

---

_@plredmond reviewed on 2024-05-30 17:38_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:224 on 2024-05-30 17:38_

Ok to add `Ord` to `ruff_python_semantic::binding::Exceptions`?

---

_@charliermarsh reviewed on 2024-05-30 17:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:224 on 2024-05-30 17:40_

Yeah

---

_Renamed from "F401 use indexmap instead of hashmap" to "F401 use BTreeMap instead of FxHashmap" by @plredmond on 2024-05-30 17:45_

---

_Renamed from "F401 use BTreeMap instead of FxHashmap" to "F401 use BTreeMap instead of FxHashMap" by @plredmond on 2024-05-30 17:45_

---

_@charliermarsh approved on 2024-05-30 17:50_

---

_Comment by @charliermarsh on 2024-05-30 17:50_

I would just ship it and ask the original filer if it's resolved.

---

_Comment by @plredmond on 2024-05-30 17:54_

SGTM. I was having "uncompression error" trying to get an alpine s390x iso to start in qemu. Merging now.

---

_Merged by @plredmond on 2024-05-30 17:54_

---

_Closed by @plredmond on 2024-05-30 17:54_

---

_Branch deleted on 2024-05-30 17:54_

---
