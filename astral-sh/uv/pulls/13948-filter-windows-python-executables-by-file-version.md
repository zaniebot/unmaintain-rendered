```yaml
number: 13948
title: Filter Windows Python executables by file version metadata during discovery
type: pull_request
state: closed
author: zanieb
labels:
  - performance
assignees: []
base: main
head: zb/filter-win
created_at: 2025-06-10T13:23:14Z
updated_at: 2025-06-12T19:41:48Z
url: https://github.com/astral-sh/uv/pull/13948
synced_at: 2026-01-10T11:10:42Z
```

# Filter Windows Python executables by file version metadata during discovery

---

_Pull request opened by @zanieb on 2025-06-10 13:23_

Closes #13927 

https://github.com/astral-sh/uv/pull/13948/commits/e0399dafd88d11a727d2393441520543a43f5d03 was written by AI then cleaned up in https://github.com/astral-sh/uv/pull/13948/commits/7438c186625d1e30d8c6f562dbdf3f1d69410f69

---

_Label `performance` added by @zanieb on 2025-06-10 13:23_

---

_Comment by @codspeed-hq[bot] on 2025-06-10 16:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Ffilter-win)

### Merging #13948 will **not alter performance**

<sub>Comparing <code>zb/filter-win</code> (7438c18) with <code>main</code> (f530565)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Assigned to @Gankra by @zanieb on 2025-06-11 17:35_

---

_Marked ready for review by @zanieb on 2025-06-11 17:35_

---

_Comment by @zanieb on 2025-06-11 17:36_

@Gankra do you think you could play with this briefly on Windows and see if it makes sense?

---

_Comment by @zanieb on 2025-06-11 20:15_

(pretty low priority, but we can lift this implementation into ty if it works here)

---

_Comment by @Gankra on 2025-06-12 13:57_

Yeah I'll mess around with this a bit, lmk if there's specific cases you're interested in.

---

_Comment by @zanieb on 2025-06-12 13:59_

Mostly like, does this short-circuit in the real world? Is it actually slow? (probably not)

---

_Comment by @Gankra on 2025-06-12 18:36_

Bad start on arm64 windows: 

```
TRACE Extracting version info from PE file: C:\Python313\python.exe
DEBUG Failed to extract version from PE file `C:\Python313\python.exe`: Version component too large: out of range integral type conversion attempted
```

booting up my x64 machine...

---

_Comment by @zanieb on 2025-06-12 18:40_

Interesting! We should emit the number there, huh

---

_Comment by @Gankra on 2025-06-12 18:41_

Same issue on x64, on top of a bunch of "file cannot be accessed by the system" errors (for some of the others)

---

_Comment by @zanieb on 2025-06-12 18:43_

Yeesh, sounds like a pain. Maybe I should just convert to a draft and leave it for someone to pick up?

---

_Comment by @Gankra on 2025-06-12 18:57_

Yeah sounds like it's gonna be more work for a marginal win.

---

_Closed by @zanieb on 2025-06-12 19:41_

---
