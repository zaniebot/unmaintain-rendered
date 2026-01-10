```yaml
number: 7513
title: "uv: migrate to rkyv 0.8"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/rkyv-update
created_at: 2024-09-18T18:34:58Z
updated_at: 2024-09-18T18:49:56Z
url: https://github.com/astral-sh/uv/pull/7513
synced_at: 2026-01-10T12:53:49Z
```

# uv: migrate to rkyv 0.8

---

_Pull request opened by @BurntSushi on 2024-09-18 18:34_

Recently, rkyv 0.8 was released. Its API is a fair bit simpler now for
higher level uses (like for us in `uv`) and results in us being able to
delete a fair bit of code. This also removes our last dependency on `syn
1.0`, and thus drops that dependency.

Performance (via testing on the `transformers` example) seems to remain
about the same, which is what was expected:

```
$ hyperfine -w5 -r100 'uv lock' 'uv-ag-rkyv-update lock'
Benchmark 1: uv lock
  Time (mean ± σ):      55.6 ms ±   6.4 ms    [User: 30.4 ms, System: 35.1 ms]
  Range (min … max):    43.0 ms …  73.1 ms    100 runs

Benchmark 2: uv-ag-rkyv-update lock
  Time (mean ± σ):      56.5 ms ±   7.2 ms    [User: 30.5 ms, System: 36.3 ms]
  Range (min … max):    39.1 ms …  71.5 ms    100 runs

Summary
  uv lock ran
    1.02 ± 0.18 times faster than uv-ag-rkyv-update lock
```

Closes #7415


---

_Review requested from @charliermarsh by @BurntSushi on 2024-09-18 18:35_

---

_@charliermarsh approved on 2024-09-18 18:36_

---

_@zanieb approved on 2024-09-18 18:41_

Sweet!

---

_Label `internal` added by @zanieb on 2024-09-18 18:41_

---

_Comment by @codspeed-hq[bot] on 2024-09-18 18:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ag/rkyv-update)

### Merging #7513 will **improve performances by 11.59%**

<sub>Comparing <code>ag/rkyv-update</code> (62dcd7b) with <code>main</code> (91a574c)</sub>



### Summary

`⚡ 1` improvements
`✅ 13` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `ag/rkyv-update` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_jupyter` | 97.4 ms | 87.3 ms | +11.59% |


---

_Merged by @BurntSushi on 2024-09-18 18:49_

---

_Closed by @BurntSushi on 2024-09-18 18:49_

---

_Branch deleted on 2024-09-18 18:49_

---
