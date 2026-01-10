```yaml
number: 9679
title: "[`flake8-pyi`] Mark `unaliased-collections-abc-set-import` fix as safe"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
  - preview
assignees: []
merged: true
base: release/0.2.0
head: charlie/PYI025
created_at: 2024-01-29T17:30:35Z
updated_at: 2024-01-29T17:56:19Z
url: https://github.com/astral-sh/ruff/pull/9679
synced_at: 2026-01-10T22:57:09Z
```

# [`flake8-pyi`] Mark `unaliased-collections-abc-set-import` fix as safe

---

_Pull request opened by @charliermarsh on 2024-01-29 17:30_

## Summary

Prompted by https://github.com/astral-sh/ruff/issues/8482#issuecomment-1859299411. The rename is only unsafe when the symbol is exported, so we can narrow the conditions.

---

_Added to milestone `v0.2.0` by @charliermarsh on 2024-01-29 17:30_

---

_Label `autofix` added by @charliermarsh on 2024-01-29 17:30_

---

_Label `preview` added by @charliermarsh on 2024-01-29 17:30_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-29 17:30_

---

_Comment by @charliermarsh on 2024-01-29 17:30_

For 0.2.

---

_@zanieb approved on 2024-01-29 17:33_

I guess it's unsafe if the name is being exported?

---

_Comment by @charliermarsh on 2024-01-29 17:39_

That's... true

---

_Comment by @github-actions[bot] on 2024-01-29 17:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select PYI</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/3dbb2573e68a288ac5ad1b4967a9c5ab994b81be/stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi#L10'>stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi:10:22:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-01-29 17:56_

I marked it as safe, unless it's at the top level.

---

_Merged by @charliermarsh on 2024-01-29 17:56_

---

_Closed by @charliermarsh on 2024-01-29 17:56_

---

_Branch deleted on 2024-01-29 17:56_

---
