```yaml
number: 10203
title: Respect external codes in file-level exemptions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - suppression
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2024-03-03T00:06:00Z
updated_at: 2024-03-03T00:26:28Z
url: https://github.com/astral-sh/ruff/pull/10203
synced_at: 2026-01-10T22:47:01Z
```

# Respect external codes in file-level exemptions

---

_Pull request opened by @charliermarsh on 2024-03-03 00:06_

We shouldn't warn when an "external" code is used in a file-level exemption.

Closes https://github.com/astral-sh/ruff/issues/10202.

---

_Label `bug` added by @charliermarsh on 2024-03-03 00:06_

---

_Label `suppression` added by @charliermarsh on 2024-03-03 00:06_

---

_Comment by @codspeed-hq[bot] on 2024-03-03 00:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/ext)

### Merging #10203 will **improve performances by 5.33%**

<sub>Comparing <code>charlie/ext</code> (1f652b7) with <code>main</code> (39a3031)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/ext` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 20.1 ms | 19.1 ms | +5.33% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 1.6 ms | 1.5 ms | +4.7% |


---

_Merged by @charliermarsh on 2024-03-03 00:20_

---

_Closed by @charliermarsh on 2024-03-03 00:20_

---

_Branch deleted on 2024-03-03 00:20_

---

_Comment by @github-actions[bot] on 2024-03-03 00:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
