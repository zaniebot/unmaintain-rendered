```yaml
number: 7412
title: "Add maximum length for `line-length` to JSON schema"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zanie/schema-linewidth
created_at: 2023-09-15T17:11:43Z
updated_at: 2023-09-22T22:16:16Z
url: https://github.com/astral-sh/ruff/pull/7412
synced_at: 2026-01-12T02:39:10Z
```

# Add maximum length for `line-length` to JSON schema

---

_Pull request opened by @zanieb on 2023-09-15 17:11_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Adds the maximum of 320 for the line-length setting to the JSON schema for better integration with IDEs.

Related https://github.com/astral-sh/ruff/pull/6873

## Test Plan

<!-- How was it tested? -->


---

_Comment by @zanieb on 2023-09-15 17:15_

Hopefully https://github.com/GREsau/schemars/issues/174 is not a problem.

---

_Label `documentation` added by @zanieb on 2023-09-15 17:15_

---

_@MichaReiser approved on 2023-09-15 17:28_

---

_Marked ready for review by @zanieb on 2023-09-15 17:57_

---

_Comment by @codspeed-hq[bot] on 2023-09-15 18:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/schema-linewidth)

### Merging #7412 will **improve performances by 2.19%**

<sub>Comparing <code>zanie/schema-linewidth</code> (43739be) with <code>main</code> (f936d31)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/schema-linewidth` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 35.1 ms | 34.4 ms | +2.19% |


---

_Merged by @zanieb on 2023-09-15 18:10_

---

_Closed by @zanieb on 2023-09-15 18:10_

---

_Branch deleted on 2023-09-15 18:10_

---

_Comment by @github-actions[bot] on 2023-09-15 18:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-09-22 21:30_

I couldn't get this to pass when updating SchemaStore: https://github.com/SchemaStore/schemastore/actions/runs/6278758287/job/17053160848

I manually edited the file to remove these.

---

_Comment by @charliermarsh on 2023-09-22 22:16_

I think the issue is that we want to range to be applied to the `#/definitions/LineLength`, not the `line_length` attribute itself, maybe? But I can't figure it out.

---
