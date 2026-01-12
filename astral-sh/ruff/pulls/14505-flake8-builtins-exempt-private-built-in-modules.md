```yaml
number: 14505
title: "[`flake8-builtins`] Exempt private built-in modules (`A005`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: A005
created_at: 2024-11-21T01:24:25Z
updated_at: 2024-11-24T05:06:42Z
url: https://github.com/astral-sh/ruff/pull/14505
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-builtins`] Exempt private built-in modules (`A005`)

---

_@InSyncWithFoo_

## Summary

Resolves #12949.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @InSyncWithFoo on 2024-11-21 01:25_

(@AlexWaygood I hope you don't mind me taking over the issue.)

---

_Comment by @github-actions[bot] on 2024-11-21 01:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to pull: fatal: unable to access 'https://github.com/mesonbuild/meson-python/': The requested URL returned error: 504
```

</p>
</details>




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-21 07:15_

---

_Label `rule` added by @MichaReiser on 2024-11-21 07:15_

---

_Label `preview` added by @MichaReiser on 2024-11-21 07:15_

---

_Merged by @charliermarsh on 2024-11-24 02:39_

---

_Closed by @charliermarsh on 2024-11-24 02:39_

---

_Branch deleted on 2024-11-24 05:06_

---
