```yaml
number: 19367
title: Fix typeshed-sync workflow
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: AlexWaygood-patch-1
created_at: 2025-07-15T17:20:16Z
updated_at: 2025-07-15T18:07:40Z
url: https://github.com/astral-sh/ruff/pull/19367
synced_at: 2026-01-12T15:56:37Z
```

# Fix typeshed-sync workflow

---

_@AlexWaygood_

we need to either cd into the Ruff repo or use `-C` here to push to the upstream branch. An oversight in https://github.com/astral-sh/ruff/commit/8d7d02193e9baa094595ec8571478241cd5bc77d

---

_Label `ci` added by @AlexWaygood on 2025-07-15 17:20_

---

_Comment by @MichaReiser on 2025-07-15 17:21_

Can you try running it from here 
<img width="2040" height="892" alt="Screenshot 2025-07-15 at 19 19 36" src="https://github.com/user-attachments/assets/dbe01536-8464-40a7-aae2-22ed475e3c19" />


---

_Comment by @AlexWaygood on 2025-07-15 17:22_

Kicked off a run: https://github.com/astral-sh/ruff/actions/runs/16299976356

Edit: and another one: https://github.com/astral-sh/ruff/actions/runs/16300606212

---

_Comment by @github-actions[bot] on 2025-07-15 18:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>

```
Failed to clone prefecthq/prefect: fatal: unable to access 'https://github.com/prefecthq/prefect/': Failed to connect to github.com port 443 after 133586 ms: Couldn't connect to server
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Comment by @AlexWaygood on 2025-07-15 18:07_

Success! https://github.com/astral-sh/ruff/pull/19368

---

_Merged by @AlexWaygood on 2025-07-15 18:07_

---

_Closed by @AlexWaygood on 2025-07-15 18:07_

---

_Branch deleted on 2025-07-15 18:07_

---
