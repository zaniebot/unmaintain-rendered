```yaml
number: 9284
title: "Do not take `explicit-preview-rules` into account when selecting `fixable` rules"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/fixable-preview
created_at: 2023-12-26T18:43:53Z
updated_at: 2024-01-15T21:01:12Z
url: https://github.com/astral-sh/ruff/pull/9284
synced_at: 2026-01-10T22:57:09Z
```

# Do not take `explicit-preview-rules` into account when selecting `fixable` rules

---

_Pull request opened by @zanieb on 2023-12-26 18:43_

Closes https://github.com/astral-sh/ruff/issues/9282

---

_@zanieb reviewed on 2023-12-26 18:44_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:718 on 2023-12-26 18:44_

There is additional logic downstream for `fixable` and `unfixable` that may require a similar change. I need to create some example test cases.

---

_Comment by @github-actions[bot] on 2023-12-26 18:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-01-02 18:32_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:718 on 2024-01-02 18:32_

Can we use `RuleSelector::All.all_rules()` instead here?

---

_@zanieb reviewed on 2024-01-02 19:05_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:718 on 2024-01-02 19:05_

That's a good idea.

Should we require explicit codes for fixable and unfixable for preview rules though? Or should the setting only affect select? 

---

_Closed by @zanieb on 2024-01-15 21:01_

---
