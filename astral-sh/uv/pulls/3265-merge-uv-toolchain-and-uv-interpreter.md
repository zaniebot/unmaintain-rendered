```yaml
number: 3265
title: "Merge `uv-toolchain` and `uv-interpreter`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/interp-request
created_at: 2024-04-25T17:46:14Z
updated_at: 2024-04-30T17:49:47Z
url: https://github.com/astral-sh/uv/pull/3265
synced_at: 2026-01-12T16:05:32Z
```

# Merge `uv-toolchain` and `uv-interpreter`

---

_@zanieb_

Moves all of `uv-toolchain` into `uv-interpreter`. We may split these out in the future, but the refactoring I want to do for interpreter discovery is easier if I don't have to deal with entanglement. Includes some restructuring of `uv-interpreter`.

Part of #2386 

---

_Renamed from "Refactor Python interpreter discovery" to "Merge `uv-toolchain` and `uv-interpreter`" by @zanieb on 2024-04-25 17:56_

---

_Label `internal` added by @zanieb on 2024-04-25 17:58_

---

_Marked ready for review by @zanieb on 2024-04-30 17:11_

---

_@konstin approved on 2024-04-30 17:17_

---

_Comment by @codspeed-hq[bot] on 2024-04-30 17:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/interp-request)

### Merging #3265 will **not alter performance**

<sub>Comparing <code>zb/interp-request</code> (e1a351e) with <code>main</code> (1d2c57a)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Merged by @zanieb on 2024-04-30 17:49_

---

_Closed by @zanieb on 2024-04-30 17:49_

---

_Branch deleted on 2024-04-30 17:49_

---
