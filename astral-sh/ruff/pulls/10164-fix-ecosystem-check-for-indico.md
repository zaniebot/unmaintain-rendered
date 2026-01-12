```yaml
number: 10164
title: Fix ecosystem check for indico
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: fix-ecosystem-check
created_at: 2024-02-29T08:18:46Z
updated_at: 2024-02-29T09:29:00Z
url: https://github.com/astral-sh/ruff/pull/10164
synced_at: 2026-01-12T15:55:31Z
```

# Fix ecosystem check for indico

---

_@MichaReiser_

## Summary
Remove the `S410` rule from the indico ecosystem check

---

_Label `ci` added by @MichaReiser on 2024-02-29 08:34_

---

_Marked ready for review by @MichaReiser on 2024-02-29 09:01_

---

_@MichaReiser reviewed on 2024-02-29 09:01_

---

_Review comment by @MichaReiser on `python/ruff-ecosystem/ruff_ecosystem/defaults.py`:99 on 2024-02-29 09:01_

This is a bit silly but I couldn't find a way to remove a single rule from` ignore`

---

_Comment by @github-actions[bot] on 2024-02-29 09:08_

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

_Merged by @MichaReiser on 2024-02-29 09:27_

---

_Closed by @MichaReiser on 2024-02-29 09:27_

---

_Branch deleted on 2024-02-29 09:27_

---
