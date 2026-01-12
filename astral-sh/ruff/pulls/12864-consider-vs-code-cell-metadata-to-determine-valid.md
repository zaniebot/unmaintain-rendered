```yaml
number: 12864
title: Consider VS Code cell metadata to determine valid code cells
type: pull_request
state: merged
author: dhruvmanila
labels:
  - notebook
assignees: []
merged: true
base: main
head: dhruv/vscode-language-id
created_at: 2024-08-13T15:53:19Z
updated_at: 2024-08-13T16:45:42Z
url: https://github.com/astral-sh/ruff/pull/12864
synced_at: 2026-01-12T15:55:42Z
```

# Consider VS Code cell metadata to determine valid code cells

---

_@dhruvmanila_

## Summary

This PR adds support for VS Code specific cell metadata to consider when collecting valid code cells.

For context, Ruff only runs on valid code cells. These are the code cells that doesn't contain cell magics. Previously, Ruff only used the notebook's metadata to determine whether it's a Python notebook. But, in VS Code, a notebook's preferred language might be Python but it could still contain code cells for other languages. This can be determined with the `metadata.vscode.languageId` field.

### References:
* https://code.visualstudio.com/docs/languages/identifiers
* https://github.com/microsoft/vscode/blob/e6c009a3d4ee60f352212b978934f52c4689fbd9/extensions/ipynb/src/serializers.ts#L104-L107
* https://github.com/microsoft/vscode/blob/e6c009a3d4ee60f352212b978934f52c4689fbd9/extensions/ipynb/src/serializers.ts#L117-L122

This brings us one step closer to fixing #12281.

## Test Plan

Add test cases for `is_valid_python_code_cell` and an integration test case which showcase running it end to end. The test notebook contains a JavaScript code cell and a Python code cell.


---

_Label `notebook` added by @dhruvmanila on 2024-08-13 15:53_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-13 15:53_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-13 15:53_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-13 15:53_

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/schema.rs`:174 on 2024-08-13 16:00_

What's the reason for using a `BTreeMap` over a normal `HashMap`? Or should we even use an `IndexMap` to preserve the field ordering?

---

_@MichaReiser approved on 2024-08-13 16:01_

Awesome, thank you.

I think it would be good to add a round-trip test case with additional `CellMetadata`.

---

_Comment by @codspeed-hq[bot] on 2024-08-13 16:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/vscode-language-id)

### Merging #12864 will **degrade performances by 4.09%**

<sub>Comparing <code>dhruv/vscode-language-id</code> (142de81) with <code>main</code> (899a523)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 30` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/vscode-language-id)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/vscode-language-id` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 765 µs | 726.4 µs | +5.31% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.09% |


---

_Comment by @github-actions[bot] on 2024-08-13 16:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-08-13 16:26_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/schema.rs`:174 on 2024-08-13 16:26_

Yeah, I don't think it's required to use `BTreeMap` as we would sort the keys before writing it back to the file.

---

_Comment by @dhruvmanila on 2024-08-13 16:32_

Added a simple roundtrip test case with additional cell metadata. I also tested it using `ruff_dev round-trip notebooks/test.ipynb` where the cell metadata isn't sorted and the output gives sorted metadata with all information intact.

---

_Merged by @dhruvmanila on 2024-08-13 16:39_

---

_Closed by @dhruvmanila on 2024-08-13 16:39_

---

_Branch deleted on 2024-08-13 16:39_

---
