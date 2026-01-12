```yaml
number: 16891
title: Allow reading requirements from scripts with HTTP(S) paths
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/script-http
created_at: 2025-11-29T01:44:38Z
updated_at: 2025-12-02T23:45:11Z
url: https://github.com/astral-sh/uv/pull/16891
synced_at: 2026-01-12T16:12:30Z
```

# Allow reading requirements from scripts with HTTP(S) paths

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/16890.


---

_Review requested from @zanieb by @charliermarsh on 2025-11-29 01:44_

---

_Review requested from @konstin by @charliermarsh on 2025-11-29 01:44_

---

_Label `enhancement` added by @charliermarsh on 2025-11-29 01:44_

---

_Marked ready for review by @charliermarsh on 2025-11-29 01:44_

---

_Comment by @codspeed-hq[bot] on 2025-11-29 02:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fscript-http?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16891 will **not alter performance**

<sub>Comparing <code>charlie/script-http</code> (ad4fd26) with <code>main</code> (d2162e2)</sub>



### Summary

`✅ 6` untouched  





---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:752 on 2025-12-01 09:46_

This feels inconsistent with how we treat `http://` prefixes in other places, where we use the prefix to have the user control whether a resource is remote or local and which protocol to use (e.g. `git+` or the inverse case for indexes, where `./` is used to mark a local directory). In other cases, we'll check online directly, even if a local file exists, e.g. `uv pip compile http://example.com` where `cat http://example.com` exists

---

_@konstin approved on 2025-12-01 09:48_

---

_@zanieb reviewed on 2025-12-02 11:04_

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:752 on 2025-12-02 11:04_

I think this matches an existing implementation? It was chosen for safety purposes.

https://github.com/astral-sh/uv/blob/2d9fe7ca701f72b35202bd48d5758dd5b5ef12fe/crates/uv/src/commands/project/run.rs#L1733-L1740

See https://github.com/astral-sh/uv/pull/6375#pullrequestreview-2310296474

cc @BurntSushi 

---

_Renamed from "Accept scripts from HTTP(S) paths" to "Accept scripts from HTTP(S) paths in `-r`" by @zanieb on 2025-12-02 11:07_

---

_Renamed from "Accept scripts from HTTP(S) paths in `-r`" to "Allow reading requirements from scripts with HTTP(S) paths" by @zanieb on 2025-12-02 11:07_

---

_Merged by @zanieb on 2025-12-02 23:42_

---

_Closed by @zanieb on 2025-12-02 23:42_

---

_Branch deleted on 2025-12-02 23:42_

---
