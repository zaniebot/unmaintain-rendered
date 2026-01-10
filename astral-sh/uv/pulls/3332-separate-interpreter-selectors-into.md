```yaml
number: 3332
title: "Separate interpreter selectors into `implementation` and `platform` modules"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/impl-platform
created_at: 2024-05-01T17:53:11Z
updated_at: 2024-05-02T11:55:02Z
url: https://github.com/astral-sh/uv/pull/3332
synced_at: 2026-01-10T14:37:54Z
```

# Separate interpreter selectors into `implementation` and `platform` modules

---

_Pull request opened by @zanieb on 2024-05-01 17:53_

Split out of #3266

The "selector" concept doesn't seem well enough defined as-is. For example, `PythonVersion` belongs there but isn't present. Going for smaller modules instead.

---

_Label `internal` added by @zanieb on 2024-05-01 17:53_

---

_Comment by @codspeed-hq[bot] on 2024-05-01 17:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/impl-platform)

### Merging #3332 will **not alter performance**

<sub>Comparing <code>zb/impl-platform</code> (75bf10a) with <code>main</code> (5048cce)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Marked ready for review by @zanieb on 2024-05-01 18:11_

---

_@konstin approved on 2024-05-02 08:24_

---

_Merged by @zanieb on 2024-05-02 11:55_

---

_Closed by @zanieb on 2024-05-02 11:55_

---

_Branch deleted on 2024-05-02 11:55_

---
