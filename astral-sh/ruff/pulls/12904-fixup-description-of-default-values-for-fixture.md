```yaml
number: 12904
title: "Fixup description of default values for `fixture-parentheses` and `mark-parentheses`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - configuration
assignees: []
merged: true
base: main
head: pt-default
created_at: 2024-08-15T13:22:45Z
updated_at: 2024-08-15T14:21:02Z
url: https://github.com/astral-sh/ruff/pull/12904
synced_at: 2026-01-10T21:38:32Z
```

# Fixup description of default values for `fixture-parentheses` and `mark-parentheses`

---

_Pull request opened by @AlexWaygood on 2024-08-15 13:22_

I noticed while creating a PR to update the schemastore entry for Ruff that there was still some language here that implied that these defaulted to true; but they default to false on ruff 0.6+. #12838 changed some but not all the language here.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-15 13:22_

---

_Comment by @codspeed-hq[bot] on 2024-08-15 13:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/pt-default)

### Merging #12904 will **improve performances by 10.63%**

<sub>Comparing <code>pt-default</code> (408301a) with <code>main</code> (b9da316)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `pt-default` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 911.3 µs | 823.7 µs | +10.63% |


---

_Comment by @github-actions[bot] on 2024-08-15 13:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-15 14:14_

Thanks

---

_Merged by @AlexWaygood on 2024-08-15 14:20_

---

_Closed by @AlexWaygood on 2024-08-15 14:20_

---

_Branch deleted on 2024-08-15 14:20_

---

_Label `documentation` added by @AlexWaygood on 2024-08-15 14:21_

---

_Label `configuration` added by @AlexWaygood on 2024-08-15 14:21_

---
