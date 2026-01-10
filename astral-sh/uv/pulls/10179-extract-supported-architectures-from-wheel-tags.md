```yaml
number: 10179
title: Extract supported architectures from wheel tags
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/arm-wheel
created_at: 2024-12-26T19:48:53Z
updated_at: 2025-01-04T02:42:18Z
url: https://github.com/astral-sh/uv/pull/10179
synced_at: 2026-01-10T11:44:37Z
```

# Extract supported architectures from wheel tags

---

_Pull request opened by @charliermarsh on 2024-12-26 19:48_

## Summary

This PR extends #10046 to also handle architectures, which allows us to correctly include `2.5.1` on the `cu124` index for ARM Linux.

Closes https://github.com/astral-sh/uv/issues/9655.


---

_Label `bug` added by @charliermarsh on 2024-12-26 19:48_

---

_Review requested from @konstin by @charliermarsh on 2024-12-26 20:01_

---

_@charliermarsh reviewed on 2024-12-26 20:01_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:21448 on 2024-12-26 20:01_

This correctly changed to backtrack to the base version for ARM Linux.

---

_Comment by @codspeed-hq[bot] on 2024-12-26 20:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Farm-wheel)

### Merging #10179 will **not alter performance**

<sub>Comparing <code>charlie/arm-wheel</code> (2a463e7) with <code>main</code> (fbe6f1e)</sub>



### Summary

`✅ 14` untouched benchmarks  





---

_Review comment by @konstin on `crates/uv-distribution-types/src/prioritized_distribution.rs`:877 on 2025-01-03 09:54_

Can this be a regular function/closure?

---

_@konstin approved on 2025-01-03 09:54_

---

_Comment by @konstin on 2025-01-03 13:01_

It's below the alerting threshold, but creating the expressions is notably slow in codspeed (https://codspeed.io/astral-sh/uv/branches/charlie%2Farm-wheel):

![image](https://github.com/user-attachments/assets/c42b1d23-eb89-4b49-9916-aa14e4137a68)

I missed this on review but https://github.com/astral-sh/uv/pull/10046 also was a regression (https://codspeed.io/astral-sh/uv/branches/charlie%2Fincomplete-locals-only):

![image](https://github.com/user-attachments/assets/41560fb9-e6fd-43ef-a4ea-c60e4e8be679)

It's the second uptick in the airflow benchmark dashboard (https://codspeed.io/astral-sh/uv/benchmarks/crates/uv-bench/benches/uv.rs::uv::resolve_warm_airflow::resolve_warm_airflow), the first is the backtracking changes:

![image](https://github.com/user-attachments/assets/43604bbb-8f03-447f-8200-597dfffa8030)



---

_Comment by @charliermarsh on 2025-01-03 16:06_

Thanks, good catch -- let me work on the perf.

---

_@charliermarsh reviewed on 2025-01-03 16:15_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:877 on 2025-01-03 16:15_

It can... but then you don't get proper line numbers when you hit an assertion failure :(

---

_Comment by @charliermarsh on 2025-01-03 18:28_

I have some ideas for how to further simplify these in common cases, will do that before merging.

---

_Merged by @charliermarsh on 2025-01-04 02:42_

---

_Closed by @charliermarsh on 2025-01-04 02:42_

---

_Branch deleted on 2025-01-04 02:42_

---
