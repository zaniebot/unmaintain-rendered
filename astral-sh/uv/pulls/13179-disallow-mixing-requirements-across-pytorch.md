```yaml
number: 13179
title: Disallow mixing requirements across PyTorch indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/system-index
created_at: 2025-04-28T14:50:24Z
updated_at: 2025-04-28T20:06:19Z
url: https://github.com/astral-sh/uv/pull/13179
synced_at: 2026-01-12T16:10:35Z
```

# Disallow mixing requirements across PyTorch indexes

---

_@charliermarsh_

## Summary

If you use `--torch-backend=auto`, we want to avoid selecting (e.g.) a `+cu124` build of `torch` alongside a `+cu126` build of `torchvision`.


---

_Review requested from @konstin by @charliermarsh on 2025-04-28 14:50_

---

_@charliermarsh reviewed on 2025-04-28 14:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1804 on 2025-04-28 14:51_

For now, this only applies when using `--torch-backend=auto`. We _could_ apply it more generally, though we risk breaking some users if they rely on this behavior for reasons that aren't yet clear.

---

_Comment by @codspeed-hq[bot] on 2025-04-28 14:58_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fsystem-index)

### Merging #13179 will **not alter performance**

<sub>Comparing <code>charlie/system-index</code> (fd9c370) with <code>main</code> (6292748)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Label `enhancement` added by @charliermarsh on 2025-04-28 14:58_

---

_Label `preview` added by @charliermarsh on 2025-04-28 14:58_

---

_Marked ready for review by @charliermarsh on 2025-04-28 14:58_

---

_Review comment by @konstin on `crates/uv-torch/src/backend.rs`:307 on 2025-04-28 14:59_

This should be a method such as a `FromStr` impl

---

_@konstin approved on 2025-04-28 14:59_

A very elegant solution!

Needs a test for how the conflict is displayed.

---

_Merged by @charliermarsh on 2025-04-28 20:06_

---

_Closed by @charliermarsh on 2025-04-28 20:06_

---

_Branch deleted on 2025-04-28 20:06_

---
