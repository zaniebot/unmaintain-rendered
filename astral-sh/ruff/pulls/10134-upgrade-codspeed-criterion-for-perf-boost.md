```yaml
number: 10134
title: Upgrade codspeed-criterion for perf boost
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: upgrade-criterion
created_at: 2024-02-26T11:30:17Z
updated_at: 2024-02-26T11:49:09Z
url: https://github.com/astral-sh/ruff/pull/10134
synced_at: 2026-01-12T15:55:31Z
```

# Upgrade codspeed-criterion for perf boost

---

_@MichaReiser_

## Summary

Criterion now supports building all benchmarks at once, which helps to speed up the build. 

[Feedback thread in Codspeed's discord](https://discord.com/channels/1065233827569598464/1205495010204586015)

## Test Plan

CI

The build time drops from 5min to 3min


---

_Label `ci` added by @MichaReiser on 2024-02-26 11:32_

---

_Marked ready for review by @MichaReiser on 2024-02-26 11:36_

---

_Merged by @MichaReiser on 2024-02-26 11:37_

---

_Closed by @MichaReiser on 2024-02-26 11:37_

---

_Branch deleted on 2024-02-26 11:37_

---

_Comment by @github-actions[bot] on 2024-02-26 11:49_

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
