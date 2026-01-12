```yaml
number: 3912
title: Include all extras when generating lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/ex-i
created_at: 2024-05-29T17:08:24Z
updated_at: 2024-05-29T19:08:21Z
url: https://github.com/astral-sh/uv/pull/3912
synced_at: 2026-01-12T16:05:55Z
```

# Include all extras when generating lockfile

---

_@charliermarsh_

## Summary

This PR just ensures that when running `uv lock` (or `uv run`), we lock with all extras. When we later install, we'll also _install_ with all extras, but that will be changed in a future PR.


---

_Marked ready for review by @charliermarsh on 2024-05-29 17:08_

---

_Label `preview` added by @charliermarsh on 2024-05-29 17:08_

---

_Comment by @codspeed-hq[bot] on 2024-05-29 17:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/ex-i)

### Merging #3912 will **degrade performances by 9.11%**

<sub>Comparing <code>charlie/ex-i</code> (0b95208) with <code>main</code> (fb0dfef)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/ex-i)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/ex-i` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter` | 82.4 ms | 90.7 ms | -9.11% |


---

_Merged by @charliermarsh on 2024-05-29 19:08_

---

_Closed by @charliermarsh on 2024-05-29 19:08_

---

_Branch deleted on 2024-05-29 19:08_

---
