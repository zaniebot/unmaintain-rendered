```yaml
number: 10104
title: Upgrade upload/download artifact actions in CI.yaml workflow
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: ci-upload-download-artifact
created_at: 2024-02-23T20:11:13Z
updated_at: 2024-02-23T20:36:29Z
url: https://github.com/astral-sh/ruff/pull/10104
synced_at: 2026-01-10T22:57:10Z
```

# Upgrade upload/download artifact actions in CI.yaml workflow

---

_Pull request opened by @MichaReiser on 2024-02-23 20:11_

## Summary

This PR upgrades the upload and download artifact GitHub actions to v4 for the CI.yaml workflow. 

Upgrading the CI.yaml workflow is straightforward because all our artifacts have unique names (multiple artifact with the same name are no longer supported)

Part of https://github.com/astral-sh/ruff/issues/9527

## Test Plan

See GitHub Actions outcome


---

_Label `ci` added by @MichaReiser on 2024-02-23 20:11_

---

_Comment by @github-actions[bot] on 2024-02-23 20:33_

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

_Merged by @MichaReiser on 2024-02-23 20:36_

---

_Closed by @MichaReiser on 2024-02-23 20:36_

---

_Branch deleted on 2024-02-23 20:36_

---
