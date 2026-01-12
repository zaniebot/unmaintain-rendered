```yaml
number: 7279
title: Update release workflow to checkout the given sha
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - release
assignees: []
merged: true
base: main
head: zanie/release-ref
created_at: 2023-09-11T18:27:57Z
updated_at: 2023-09-13T19:59:42Z
url: https://github.com/astral-sh/ruff/pull/7279
synced_at: 2026-01-12T15:55:23Z
```

# Update release workflow to checkout the given sha

---

_@zanieb_

As far as I can tell, the release workflow would just run on the latest commit from `main` then crash and burn if that did not match the given SHA? It seems much clearer to actually checkout the given SHA so we can publish a release after making additional commits to `main`.

---

_Label `internal` added by @zanieb on 2023-09-11 18:33_

---

_Marked ready for review by @zanieb on 2023-09-11 18:39_

---

_Label `release` added by @zanieb on 2023-09-11 19:11_

---

_Comment by @konstin on 2023-09-12 07:40_

(history note: we had it this way in an earlier draft but then changed it to the current solution, iirc mostly for convenience but i unfortunately can't find the original reasoning anymore)

---

_Review comment by @konstin on `.github/workflows/release.yaml`:10 on 2023-09-12 07:41_

do you want to add a sanity check that this is actually a commit on main?

---

_@konstin approved on 2023-09-12 07:41_

---

_Comment by @charliermarsh on 2023-09-12 11:33_

I think it was written this way as a sanity check: so you provided the version (like v0.0.288), the branch, and the expected SHA, and it failed if the SHA didn't match that of the branch.

---

_@zanieb reviewed on 2023-09-12 13:55_

---

_Review comment by @zanieb on `.github/workflows/release.yaml`:10 on 2023-09-12 13:55_

We could do that. It'd address the concern at https://github.com/astral-sh/ruff/pull/7279#issuecomment-1715552999

---

_Comment by @zanieb on 2023-09-12 16:25_

I'm not sure if https://github.com/astral-sh/ruff/pull/7279/commits/410551bae1d7da43b8a58edd2c004a9c93714322 will work with a shallow clone 

---

_Comment by @codspeed-hq[bot] on 2023-09-12 16:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/release-ref)

### Merging #7279 will **degrade performances by 4.58%**

<sub>Comparing <code>zanie/release-ref</code> (410551b) with <code>main</code> (ff0feb1)</sub>



### Summary

`❌ 5 (👁 5)` regressions
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/release-ref` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.1 ms | -2.6% |
| 👁 | `linter/all-rules[large/dataset.py]` | 155.7 ms | 163.2 ms | -4.58% |
| 👁 | `linter/all-rules[numpy/ctypeslib.py]` | 33.5 ms | 34.5 ms | -2.97% |
| 👁 | `linter/all-rules[pydantic/types.py]` | 69.9 ms | 72 ms | -2.95% |
| 👁 | `linter/all-rules[unicode/pypinyin.py]` | 15.3 ms | 15.7 ms | -2.58% |


---

_Merged by @zanieb on 2023-09-13 19:59_

---

_Closed by @zanieb on 2023-09-13 19:59_

---

_Branch deleted on 2023-09-13 19:59_

---
