```yaml
number: 12876
title: "[red-knot] Update source root path for benchmark"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
base: main
head: dhruv/red-knot-bench
created_at: 2024-08-14T05:12:19Z
updated_at: 2024-08-14T06:32:02Z
url: https://github.com/astral-sh/ruff/pull/12876
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Update source root path for benchmark

---

_@dhruvmanila_

## Summary

I guess the existing path is correct but we currently don't have the ability to detect packages considering the below TODO:

https://github.com/astral-sh/ruff/blob/3eac2cae847770e28f87bef4e01faab71f95b201/crates/red_knot_workspace/src/workspace/metadata.rs#L33-L34

This means that the benchmark code considers `/src` as the package while `/src/tomllib` is the actual package. This means it's unable to resolve [relative imports](https://github.com/python/cpython/blob/e03073ff20107793a4ea28cdac0d6894774dd110/Lib/tomllib/_parser.py#L12-L20).

If this is correct, I can validate and update the expected diagnostics in the benchmark code.


---

_Label `red-knot` added by @dhruvmanila on 2024-08-14 05:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-14 05:13_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-14 05:13_

---

_Comment by @github-actions[bot] on 2024-08-14 05:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (error)</summary>
<p>

```
Failed to clone langchain-ai/langchain: error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
error: 2840 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-08-14 05:43_

Closing this as it's an expected behavior.

---

_Closed by @dhruvmanila on 2024-08-14 05:43_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-08-14 05:43_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-08-14 05:43_

---

_Branch deleted on 2024-08-14 06:32_

---
