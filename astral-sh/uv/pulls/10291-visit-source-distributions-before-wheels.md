```yaml
number: 10291
title: Visit source distributions before wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/sd
created_at: 2025-01-03T16:24:27Z
updated_at: 2025-01-03T17:07:31Z
url: https://github.com/astral-sh/uv/pull/10291
synced_at: 2026-01-10T11:44:40Z
```

# Visit source distributions before wheels

---

_Pull request opened by @charliermarsh on 2025-01-03 16:24_

## Summary

This should address the comment here: https://github.com/astral-sh/uv/pull/10179#issuecomment-2569189265. We don't compute implied markers if the marker is already `TRUE`, and we set it to `TRUE` as soon as we see a source distribution. So if we visit the source distribution before the wheels, we'll avoid computing these for any irrelevant distributions.


---

_Label `performance` added by @charliermarsh on 2025-01-03 16:24_

---

_Review requested from @konstin by @charliermarsh on 2025-01-03 16:24_

---

_Comment by @codspeed-hq[bot] on 2025-01-03 16:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fsd)

### Merging #10291 will **improve performances by 15.66%**

<sub>Comparing <code>charlie/sd</code> (c513714) with <code>main</code> (9f1ba2b)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/sd` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_jupyter` | 110.7 ms | 95.7 ms | +15.66% |


---

_Merged by @charliermarsh on 2025-01-03 17:07_

---

_Closed by @charliermarsh on 2025-01-03 17:07_

---

_Branch deleted on 2025-01-03 17:07_

---
