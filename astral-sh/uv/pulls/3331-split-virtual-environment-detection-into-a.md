```yaml
number: 3331
title: Split virtual environment detection into a dedicated module
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/virtualenv
created_at: 2024-05-01T17:41:22Z
updated_at: 2024-05-02T11:58:49Z
url: https://github.com/astral-sh/uv/pull/3331
synced_at: 2026-01-12T16:05:35Z
```

# Split virtual environment detection into a dedicated module

---

_@zanieb_

Split out of https://github.com/astral-sh/uv/pull/3266

---

_Label `internal` added by @zanieb on 2024-05-01 17:41_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:157 on 2024-05-01 17:41_

Moved into `uv-interpreter::virtualenv`

---

_@zanieb reviewed on 2024-05-01 17:41_

---

_@zanieb reviewed on 2024-05-01 17:42_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:190 on 2024-05-01 17:42_

Moved into `uv-interpreter::virtualenv` as `virtualenv_python_executable`

---

_@zanieb reviewed on 2024-05-01 17:42_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/virtualenv.rs`:48 on 2024-05-01 17:42_

Split into two methods so we can identify the separate sources in #3266 

---

_@zanieb reviewed on 2024-05-01 17:43_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment/cfg.rs`:8 on 2024-05-01 17:43_

Moved into `uv-interpreter::virtualenv`

---

_@zanieb reviewed on 2024-05-01 17:43_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment/cfg.rs`:10 on 2024-05-01 17:43_

This comment is fixed in the moved code

---

_Comment by @codspeed-hq[bot] on 2024-05-01 17:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/virtualenv)

### Merging #3331 will **not alter performance**

<sub>Comparing <code>zb/virtualenv</code> (ef08002) with <code>main</code> (5048cce)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_@zanieb reviewed on 2024-05-01 18:09_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/virtualenv.rs`:13 on 2024-05-01 18:09_

We might want to make this struct more powerful in the future. Opting not to make more changes now though.

---

_Marked ready for review by @zanieb on 2024-05-01 18:11_

---

_@konstin approved on 2024-05-02 08:27_

---

_Merged by @zanieb on 2024-05-02 11:58_

---

_Closed by @zanieb on 2024-05-02 11:58_

---

_Branch deleted on 2024-05-02 11:58_

---
