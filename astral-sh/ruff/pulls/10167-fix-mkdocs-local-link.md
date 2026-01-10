```yaml
number: 10167
title: Fix mkdocs local link
type: pull_request
state: merged
author: hoel-bagard
labels:
  - internal
assignees: []
merged: true
base: main
head: hoel/fix_local_documentation_link
created_at: 2024-02-29T10:00:15Z
updated_at: 2024-02-29T10:35:10Z
url: https://github.com/astral-sh/ruff/pull/10167
synced_at: 2026-01-10T22:47:01Z
```

# Fix mkdocs local link

---

_Pull request opened by @hoel-bagard on 2024-02-29 10:00_

## Summary

After following the instructions in [CONTRIBUTING.md](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#mkdocs), the documentation is served on `http://127.0.0.1:8000/ruff/` rather than `http://127.0.0.1:8000/docs/`. This PR fixes the link.

---

_Comment by @github-actions[bot] on 2024-02-29 10:19_

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

_Label `documentation` added by @MichaReiser on 2024-02-29 10:35_

---

_Label `internal` added by @MichaReiser on 2024-02-29 10:35_

---

_Label `documentation` removed by @MichaReiser on 2024-02-29 10:35_

---

_Merged by @MichaReiser on 2024-02-29 10:35_

---

_Closed by @MichaReiser on 2024-02-29 10:35_

---
