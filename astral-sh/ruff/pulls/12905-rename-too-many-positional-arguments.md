```yaml
number: 12905
title: Rename too-many-positional(-arguments)
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
assignees: []
merged: true
base: main
head: rename-too-many-positiona/-arguments
created_at: 2024-08-15T13:26:35Z
updated_at: 2024-08-15T16:23:43Z
url: https://github.com/astral-sh/ruff/pull/12905
synced_at: 2026-01-12T15:55:42Z
```

# Rename too-many-positional(-arguments)

---

_@JonathanPlasse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Fix #12619 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

- [x] Run `cargo nextest run`


---

_Comment by @codspeed-hq[bot] on 2024-08-15 13:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/JonathanPlasse:rename-too-many-positiona/-arguments)

### Merging #12905 will **improve performances by 10.7%**

<sub>Comparing <code>JonathanPlasse:rename-too-many-positiona/-arguments</code> (fbb2fe3) with <code>main</code> (b9da316)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `JonathanPlasse:rename-too-many-positiona/-arguments` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 911.3 µs | 823.2 µs | +10.7% |


---

_Comment by @github-actions[bot] on 2024-08-15 13:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-15 14:14_

---

_Removed from milestone `v0.7` by @MichaReiser on 2024-08-15 14:14_

---

_@MichaReiser approved on 2024-08-15 16:13_

Thanks

---

_Label `rule` added by @MichaReiser on 2024-08-15 16:13_

---

_Merged by @MichaReiser on 2024-08-15 16:13_

---

_Closed by @MichaReiser on 2024-08-15 16:13_

---

_Branch deleted on 2024-08-15 16:23_

---
