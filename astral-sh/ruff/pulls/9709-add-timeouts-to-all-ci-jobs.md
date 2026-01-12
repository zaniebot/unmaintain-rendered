```yaml
number: 9709
title: Add timeouts to all CI jobs
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/timeout
created_at: 2024-01-30T16:39:06Z
updated_at: 2024-01-30T17:13:25Z
url: https://github.com/astral-sh/ruff/pull/9709
synced_at: 2026-01-12T15:55:30Z
```

# Add timeouts to all CI jobs

---

_@zanieb_

To prevent jobs from running far beyond their expected time

---

_Label `ci` added by @zanieb on 2024-01-30 16:39_

---

_@charliermarsh approved on 2024-01-30 16:42_

---

_Comment by @github-actions[bot] on 2024-01-30 16:57_

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
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
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
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Merged by @zanieb on 2024-01-30 17:13_

---

_Closed by @zanieb on 2024-01-30 17:13_

---

_Branch deleted on 2024-01-30 17:13_

---
