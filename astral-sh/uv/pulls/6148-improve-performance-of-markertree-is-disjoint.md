```yaml
number: 6148
title: "Improve performance of `MarkerTree::is_disjoint`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: ibraheem/marker-disjoint-perf
created_at: 2024-08-16T15:24:40Z
updated_at: 2024-08-16T17:05:05Z
url: https://github.com/astral-sh/uv/pull/6148
synced_at: 2026-01-12T16:07:14Z
```

# Improve performance of `MarkerTree::is_disjoint`

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/uv/issues/6137.

---

_Label `performance` added by @ibraheemdev on 2024-08-16 15:24_

---

_Label `internal` added by @ibraheemdev on 2024-08-16 15:24_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-08-16 15:25_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-08-16 15:25_

---

_@BurntSushi approved on 2024-08-16 15:29_

This makes sense! Does this improve perf on any resolutions?

---

_Comment by @ibraheemdev on 2024-08-16 16:11_

I'm struggling to find complex resolution cases that are also consistent, but it does seem to be a small improvement in most cases.

---

_Comment by @ibraheemdev on 2024-08-16 16:57_

Worked on this a little more and ran some microbenchmarks, this does seem to be a significant improvement, especially for markers that are *not* disjoint.
```
marker_disjointness     time:   [118.69 ns 119.12 ns 119.55 ns]
                        change: [-40.348% -40.036% -39.771%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  12 (12.00%) high mild

marker_disjointness_current
                        time:   [307.31 ns 307.92 ns 308.60 ns]
                        change: [-1.0642% -0.8529% -0.5884%] (p = 0.00 < 0.05)
                        Change within noise threshold.
Found 18 outliers among 100 measurements (18.00%)
  15 (15.00%) low mild
  1 (1.00%) high mild
  2 (2.00%) high severe
```
```
marker_disjointness     time:   [308.82 ns 309.33 ns 309.85 ns]
                        change: [-49.069% -48.924% -48.768%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 8 outliers among 100 measurements (8.00%)
  1 (1.00%) low mild
  6 (6.00%) high mild
  1 (1.00%) high severe

marker_disjointness_current
                        time:   [1.2548 µs 1.2555 µs 1.2562 µs]
                        change: [-1.9563% -1.6812% -1.4606%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
  ```
  
How much this affects a real world resolution is unclear, but I think it's good to have in case we decide to rely on disjointness checking more heavily in the future (for aggressive forking, for example).

---

_Comment by @BurntSushi on 2024-08-16 17:03_

Nice win! Order of magnitude improvement in the second benchmark. Is that the case where the markers aren't disjoint?

---

_Merged by @ibraheemdev on 2024-08-16 17:03_

---

_Closed by @ibraheemdev on 2024-08-16 17:03_

---

_Branch deleted on 2024-08-16 17:03_

---

_Comment by @ibraheemdev on 2024-08-16 17:03_

Yup, that's when the markers are not disjoint, so the current implementation ends up creating the conjoined markers that we just throw away.

---
