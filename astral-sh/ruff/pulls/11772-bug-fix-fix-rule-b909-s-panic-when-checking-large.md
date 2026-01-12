```yaml
number: 11772
title: "[Bug fix] Fix rule B909's panic when checking large loop blocks"
type: pull_request
state: merged
author: Embers-of-the-Fire
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-06-06T09:18:39Z
updated_at: 2024-06-06T11:19:42Z
url: https://github.com/astral-sh/ruff/pull/11772
synced_at: 2026-01-12T15:55:38Z
```

# [Bug fix] Fix rule B909's panic when checking large loop blocks

---

_@Embers-of-the-Fire_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This pr should close #11771 by changing `LoopMutationsVisitor::<field>branch` from `u8` to `u32`.

## Test Plan

<!-- How was it tested? -->
Tested using the example programme mentioned in the related issue.


---

_Comment by @codspeed-hq[bot] on 2024-06-06 09:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Embers-of-the-Fire:main)

### Merging #11772 will **improve performances by 4.67%**

<sub>Comparing <code>Embers-of-the-Fire:main</code> (06e69e0) with <code>main</code> (6c1fa1d)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `Embers-of-the-Fire:main` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 839.1 µs | 801.7 µs | +4.67% |


---

_Comment by @github-actions[bot] on 2024-06-06 09:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-06-06 10:23_

Thanks

---

_Merged by @MichaReiser on 2024-06-06 10:23_

---

_Closed by @MichaReiser on 2024-06-06 10:23_

---

_Label `bug` added by @dhruvmanila on 2024-06-06 11:19_

---
