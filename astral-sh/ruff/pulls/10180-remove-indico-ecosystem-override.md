```yaml
number: 10180
title: Remove indico ecosystem override
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: remove-indico-ecosystem-override
created_at: 2024-03-01T09:19:00Z
updated_at: 2024-03-01T09:38:14Z
url: https://github.com/astral-sh/ruff/pull/10180
synced_at: 2026-01-12T15:55:31Z
```

# Remove indico ecosystem override

---

_@MichaReiser_

## Summary

`indico` updated to ruff 0.3 and removed S410 from the ignore list.

## Test Plan

GitHub ecosystem check 


---

_Label `ci` added by @MichaReiser on 2024-03-01 09:19_

---

_Comment by @github-actions[bot] on 2024-03-01 09:36_

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

_Merged by @MichaReiser on 2024-03-01 09:38_

---

_Closed by @MichaReiser on 2024-03-01 09:38_

---

_Branch deleted on 2024-03-01 09:38_

---
