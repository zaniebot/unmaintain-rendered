```yaml
number: 7281
title: Add preview documentation section
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zanie/preview-docs
created_at: 2023-09-11T19:06:25Z
updated_at: 2023-09-12T15:53:25Z
url: https://github.com/astral-sh/ruff/pull/7281
synced_at: 2026-01-12T15:55:23Z
```

# Add preview documentation section

---

_@zanieb_

Adds a basic documentation section for preview mode based on the FAQ entry and versioning RFC.

I intend to expand this with more details as we play with the user experience here. We could also consider generating an enumerated list of preview features 🤷‍♀️ 

---

_Label `documentation` added by @zanieb on 2023-09-11 19:06_

---

_Comment by @github-actions[bot] on 2023-09-11 19:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `docs/faq.md`:389 on 2023-09-12 07:20_

```suggestion
See the [preview documentation](https://beta.ruff.rs/docs/preview/) for more details.
```

i'd probably collapse this section into a single paragraph

---

_Review comment by @konstin on `docs/preview.md`:3 on 2023-09-12 07:21_

if i read "collect community feedback" it unfortunately sounds to me as if we're enabling telemetry in this mode

---

_@konstin approved on 2023-09-12 07:22_

---

_@zanieb reviewed on 2023-09-12 13:56_

---

_Review comment by @zanieb on `docs/preview.md`:3 on 2023-09-12 13:56_

I'll rephrase, thanks!

---

_Marked ready for review by @zanieb on 2023-09-12 15:30_

---

_Comment by @codspeed-hq[bot] on 2023-09-12 15:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/preview-docs)

### Merging #7281 will **degrade performances by 5.33%**

<sub>Comparing <code>zanie/preview-docs</code> (2198bb7) with <code>main</code> (773ba5f)</sub>



### Summary

`❌ 5` regressions
`✅ 20` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/zanie/preview-docs)._

### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/preview-docs` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 33 ms | 34.8 ms | -5.33% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 15.2 ms | 15.7 ms | -3.02% |
| ❌ | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.1 ms | -2.58% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 69.5 ms | 73 ms | -4.75% |
| ❌ | `linter/all-rules[large/dataset.py]` | 156.8 ms | 163.2 ms | -3.95% |


---

_Merged by @zanieb on 2023-09-12 15:43_

---

_Closed by @zanieb on 2023-09-12 15:43_

---

_Branch deleted on 2023-09-12 15:43_

---
