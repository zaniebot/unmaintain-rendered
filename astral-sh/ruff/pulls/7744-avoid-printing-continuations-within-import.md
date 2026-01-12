```yaml
number: 7744
title: Avoid printing continuations within import identifiers
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/continuation
created_at: 2023-10-01T18:22:51Z
updated_at: 2023-10-02T13:51:08Z
url: https://github.com/astral-sh/ruff/pull/7744
synced_at: 2026-01-12T02:39:10Z
```

# Avoid printing continuations within import identifiers

---

_Pull request opened by @charliermarsh on 2023-10-01 18:22_

## Summary

It turns out that _some_ identifiers can contain newlines -- specifically, dot-delimited import identifiers, like:
```python
import foo\
    .bar
```

At present, we print all identifiers verbatim, which causes us to retain the `\` in the formatted output. This also leads to violating some debug assertions (see the linked issue, though that's a symptom of this formatting failure).

This PR adds detection for import identifiers that contain newlines, and formats them via `text` (slow) rather than `source_code_slice` (fast) in those cases.

Closes https://github.com/astral-sh/ruff/issues/7734.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2023-10-01 18:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/continuation)

### Merging #7744 will **improve performances by 2.02%**

<sub>Comparing <code>charlie/continuation</code> (a307da9) with <code>main</code> (d8a6279)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/continuation` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 19 ms | 18.6 ms | +2.02% |


---

_@konstin approved on 2023-10-02 09:51_

I think that fundamentally that is bug in our AST: Indentifiers in python can not contain dots, and `import foo.bar` consists of two dot-separated keywords, not one. 

I think we should merge this fix and then solve this properly with https://github.com/astral-sh/ruff/issues/7760

---

_Merged by @charliermarsh on 2023-10-02 13:51_

---

_Closed by @charliermarsh on 2023-10-02 13:51_

---

_Branch deleted on 2023-10-02 13:51_

---
