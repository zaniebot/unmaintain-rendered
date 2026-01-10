```yaml
number: 9602
title: Corrected code block hyperlink
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: correct-hyperlink
created_at: 2024-01-21T22:59:43Z
updated_at: 2024-01-22T03:57:35Z
url: https://github.com/astral-sh/ruff/pull/9602
synced_at: 2026-01-10T22:57:09Z
```

# Corrected code block hyperlink

---

_Pull request opened by @trag1c on 2024-01-21 22:59_

## Summary

Apparently MkDocs doesn't like when reference-style links have formatting inside :)

<details>
<summary>Screenshots (before and after the change)</summary>
<img width="1235" alt="61353" src="https://github.com/astral-sh/ruff/assets/77130613/e32a82bd-0c5d-4edb-998f-b53659a6c54d">

<img width="1237" alt="15526" src="https://github.com/astral-sh/ruff/assets/77130613/bdafcda5-eb9c-4af6-af03-b4849c1e5c81">
</details>


---

_Comment by @github-actions[bot] on 2024-01-21 23:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-01-21 23:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs`:51 on 2024-01-21 23:29_

Do you mind removing this line entirely, and then reverting the `[namespace-packages]` additions? I thought options were special-cased such that they didn't need to be added as references (see, e.g., https://docs.astral.sh/ruff/rules/too-many-public-methods/, the settings linked in there aren't formatted as references in the source code). If it doesn't work though, we can merge as-is.

---

_@trag1c reviewed on 2024-01-22 01:34_

---

_Review comment by @trag1c on `crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs`:51 on 2024-01-22 01:34_

After some playing around I found that this bit also needed stripping because it would mess up during generation otherwise (which could be why it was done like this in the first place?):
```diff
 /// ## Options
-/// - [`namespace-packages`][namespace-packages]: List of packages that are defined as namespace
-///   packages.
+/// - `namespace-packages`
```

---

_@charliermarsh reviewed on 2024-01-22 03:57_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-01-22 03:57_

---

_Merged by @charliermarsh on 2024-01-22 03:57_

---

_Closed by @charliermarsh on 2024-01-22 03:57_

---
