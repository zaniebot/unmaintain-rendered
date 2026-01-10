```yaml
number: 10173
title: Fix the sorting of the schema submited to schemastore
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: sort-json-schema-pre-commit
created_at: 2024-02-29T15:45:30Z
updated_at: 2024-02-29T18:33:23Z
url: https://github.com/astral-sh/ruff/pull/10173
synced_at: 2026-01-10T22:47:01Z
```

# Fix the sorting of the schema submited to schemastore

---

_Pull request opened by @MichaReiser on 2024-02-29 15:45_

## Summary

Schemastore uses a prettier plugin to sort the properties. See https://github.com/astral-sh/schemastore/commit/734c0e50105228741c0b76853efdd81b5f93487c

This commit makes sure we use the same plugin when running `prettier`. 

## Test Plan

https://github.com/SchemaStore/schemastore/pull/3614


---

_Review requested from @konstin by @MichaReiser on 2024-02-29 15:45_

---

_Label `internal` added by @MichaReiser on 2024-02-29 15:45_

---

_Label `internal` removed by @MichaReiser on 2024-02-29 15:46_

---

_Label `internal` added by @MichaReiser on 2024-02-29 15:46_

---

_Label `ci` added by @MichaReiser on 2024-02-29 15:46_

---

_@charliermarsh approved on 2024-02-29 15:46_

---

_Comment by @github-actions[bot] on 2024-02-29 16:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Label `internal` removed by @MichaReiser on 2024-02-29 18:15_

---

_Merged by @MichaReiser on 2024-02-29 18:21_

---

_Closed by @MichaReiser on 2024-02-29 18:21_

---

_Branch deleted on 2024-02-29 18:21_

---
