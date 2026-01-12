```yaml
number: 10201
title: CallPath newtype wrapper
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: call-path-new-type
created_at: 2024-03-02T18:07:17Z
updated_at: 2024-03-03T15:54:25Z
url: https://github.com/astral-sh/ruff/pull/10201
synced_at: 2026-01-12T15:55:31Z
```

# CallPath newtype wrapper

---

_@MichaReiser_

## Summary

This PR changes the `CallPath` type alias to a newtype wrapper. 

A newtype wrapper allows us to limit the API and to experiment with alternative ways to implement matching on `CallPath`s. 



## Test Plan

`cargo test`


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-02 18:07_

---

_Label `internal` added by @MichaReiser on 2024-03-02 18:09_

---

_Comment by @codspeed-hq[bot] on 2024-03-02 18:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/call-path-new-type)

### Merging #10201 will **not alter performance**

<sub>Comparing <code>call-path-new-type</code> (85155d3) with <code>main</code> (ba7f678)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-02 18:58_

---

_Comment by @github-actions[bot] on 2024-03-02 19:10_

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

_@charliermarsh approved on 2024-03-02 20:18_

Excellent! Any idea why this improves perf on the benchmarks?

---

_Comment by @MichaReiser on 2024-03-03 14:20_

@charliermarsh not fully. Two changes could improve performance:

* `CallPath::from_qualified_name` now uses `find` instead of `contain` and uses `split` on the rest rather than starting from the start again
* `collect_call_path`: Now uses `SmallVec::from` instead of `from_slice` for call paths with 8 segments.

However, it seems strange for me that either change would improve performance that much. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-03-03 14:55_

---

_Merged by @MichaReiser on 2024-03-03 15:54_

---

_Closed by @MichaReiser on 2024-03-03 15:54_

---

_Branch deleted on 2024-03-03 15:54_

---
