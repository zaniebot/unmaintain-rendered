```yaml
number: 10665
title: Store unsupported tags in wheel filename
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/small-tags
created_at: 2025-01-16T02:26:12Z
updated_at: 2025-01-17T04:41:54Z
url: https://github.com/astral-sh/uv/pull/10665
synced_at: 2026-01-10T11:45:03Z
```

# Store unsupported tags in wheel filename

---

_Pull request opened by @charliermarsh on 2025-01-16 02:26_

## Summary

We can retain the small-size advantage of our new tags by moving the "unknown tag" case into `WheelTagLarge`. This ensures that we can still represent unknown tags, but avoid paying the cost for them.


---

_Label `performance` added by @charliermarsh on 2025-01-16 02:26_

---

_Marked ready for review by @charliermarsh on 2025-01-16 02:26_

---

_Comment by @charliermarsh on 2025-01-16 02:30_

The biggest offenders for large wheel tags are: `py2.py3` (e.g., `clldutils-2.7.0-py2.py3-none-any`) and `manylinux2014` (e.g., `cffi-1.17.1-cp39-cp39-manylinux_2_17_s390x.manylinux2014_s390x`).

---

_Converted to draft by @charliermarsh on 2025-01-16 02:31_

---

_Comment by @codspeed-hq[bot] on 2025-01-16 02:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fsmall-tags)

### Merging #10665 will **improve performances by 15.42%**

<sub>Comparing <code>charlie/small-tags</code> (77b4783) with <code>main</code> (50c6465)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/small-tags` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` build_platform_tags[burntsushi-archlinux] `` | 326.5 µs | 282.9 µs | +15.42% |


---

_Marked ready for review by @charliermarsh on 2025-01-16 02:43_

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/wheel_tag.rs`:104 on 2025-01-16 08:01_

I think these comments got mixed up, is `1.2.3-73-py3-none-any-none` a supported tag?

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/wheel_tag.rs`:105 on 2025-01-16 08:03_

```suggestion
    /// The string representation of the tag.
    ///
    /// This fields preseverse the unsupported tags that were filtered out from the `TagSet`s.
```

---

_@konstin approved on 2025-01-16 08:04_

---

_@charliermarsh reviewed on 2025-01-16 15:31_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel_tag.rs`:104 on 2025-01-16 15:31_

Gah thanks, yeah

---

_Merged by @charliermarsh on 2025-01-17 04:41_

---

_Closed by @charliermarsh on 2025-01-17 04:41_

---

_Branch deleted on 2025-01-17 04:41_

---
