```yaml
number: 3946
title: Unify editable and unnamed URL parsing
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - enhancement
assignees: []
merged: true
base: main
head: charlie/editable-url
created_at: 2024-05-31T20:14:27Z
updated_at: 2024-05-31T21:08:01Z
url: https://github.com/astral-sh/uv/pull/3946
synced_at: 2026-01-12T16:05:56Z
```

# Unify editable and unnamed URL parsing

---

_@charliermarsh_

## Summary

This will help prevent bugs like #3934 by unifying the implementations for editables and non-editable unnamed requirements. Specifically, both of these now go through the same parsing paths and use the same struct representations (with the exception that the editable flag is flipped in the first case):

```
-e ./foo/bar
./foo/bar
```

We also now support PEP 508 in editable URLs. It turns out this is just a limitation in pip, so it's correct to support it. For example, this now works:

```
-e black[d] @ file://${PROJECT_ROOT}/scripts/packages/black_editable
```

Closes #3941.

Closes #3942.


---

_Label `bug` added by @charliermarsh on 2024-05-31 20:16_

---

_Label `enhancement` added by @charliermarsh on 2024-05-31 20:16_

---

_Marked ready for review by @charliermarsh on 2024-05-31 20:16_

---

_Review comment by @MichaReiser on `crates/pep508-rs/src/unnamed.rs`:379 on 2024-05-31 20:22_

Is it guaranteed that the parentheses are always balanced?

---

_@MichaReiser reviewed on 2024-05-31 20:23_

---

_@charliermarsh reviewed on 2024-05-31 20:51_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/unnamed.rs`:379 on 2024-05-31 20:51_

Good call, thanks.

---

_Comment by @codspeed-hq[bot] on 2024-05-31 20:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/editable-url)

### Merging #3946 will **improve performances by 6.5%**

<sub>Comparing <code>charlie/editable-url</code> (8146a88) with <code>main</code> (8c11f99)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/editable-url` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 955.8 ns | 897.5 ns | +6.5% |


---

_Comment by @charliermarsh on 2024-05-31 21:03_

Deleted a few hundred lines of code but added a few hundred in tests and snapshots.

---

_Merged by @charliermarsh on 2024-05-31 21:08_

---

_Closed by @charliermarsh on 2024-05-31 21:08_

---

_Branch deleted on 2024-05-31 21:08_

---
