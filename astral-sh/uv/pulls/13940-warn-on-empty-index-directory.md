```yaml
number: 13940
title: Warn on empty index directory
type: pull_request
state: merged
author: aaron-ang
labels:
  - enhancement
assignees: []
merged: true
base: main
head: add-empty-index
created_at: 2025-06-10T05:23:54Z
updated_at: 2025-06-18T08:48:21Z
url: https://github.com/astral-sh/uv/pull/13940
synced_at: 2026-01-12T16:10:56Z
```

# Warn on empty index directory

---

_@aaron-ang_

Close #13922

## Summary

Add a warning if the directory given by the `--index` argument is empty.

## Test Plan

Added test case `add_index_empty_directory` in `edit.rs`


---

_Review requested from @jtfmumm by @konstin on 2025-06-10 09:44_

---

_Label `enhancement` added by @konstin on 2025-06-10 09:44_

---

_@jtfmumm reviewed on 2025-06-10 11:54_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/add.rs`:487 on 2025-06-10 11:54_

We may not be falling back to PyPI here. There could be another specified index in line or the default index could have been set to something else. I think we should just indicate we're ignoring the index instead.

---

_Assigned to @jtfmumm by @konstin on 2025-06-10 12:20_

---

_@aaron-ang reviewed on 2025-06-10 18:32_

---

_Review comment by @aaron-ang on `crates/uv/src/commands/project/add.rs`:487 on 2025-06-10 18:32_

Understood. I will change the warning to indicate that the index is ignored. Should we also remove the ignored index from the Index Vector?

---

_Comment by @codspeed-hq[bot] on 2025-06-10 19:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/aaron-ang%3Aadd-empty-index)

### Merging #13940 will **not alter performance**

<sub>Comparing <code>aaron-ang:add-empty-index</code> (1330634) with <code>main</code> (c25c800)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_@jtfmumm reviewed on 2025-06-17 15:13_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/add.rs`:488 on 2025-06-17 15:13_

To be more consistent with other messages, I think this should be:

```
warn_user_once!("Index directory `{url}` is empty; skipping");
```

---

_Comment by @jtfmumm on 2025-06-17 15:13_

@aaron-ang This looks good to me with the exception of a minor change to the message I suggested.

---

_@aaron-ang reviewed on 2025-06-17 16:54_

---

_Review comment by @aaron-ang on `crates/uv/src/commands/project/add.rs`:488 on 2025-06-17 16:54_

fixed

---

_@jtfmumm approved on 2025-06-18 08:48_

---

_Merged by @jtfmumm on 2025-06-18 08:48_

---

_Closed by @jtfmumm on 2025-06-18 08:48_

---
