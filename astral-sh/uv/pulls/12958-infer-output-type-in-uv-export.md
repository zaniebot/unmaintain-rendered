```yaml
number: 12958
title: "Infer output type in `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/infer
created_at: 2025-04-18T03:19:19Z
updated_at: 2025-04-21T21:35:06Z
url: https://github.com/astral-sh/uv/pull/12958
synced_at: 2026-01-10T11:10:40Z
```

# Infer output type in `uv export`

---

_Pull request opened by @charliermarsh on 2025-04-18 03:19_

## Summary

If the user provides a `.toml` file, we assume PEP 751; otherwise, we assume `requirements.txt`.


---

_Marked ready for review by @charliermarsh on 2025-04-18 03:19_

---

_Label `cli` added by @charliermarsh on 2025-04-18 03:19_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-18 03:22_

---

_Review requested from @Gankra by @charliermarsh on 2025-04-18 03:22_

---

_Comment by @codspeed-hq[bot] on 2025-04-18 03:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Finfer)

### Merging #12958 will **not alter performance**

<sub>Comparing <code>charlie/infer</code> (42e3546) with <code>main</code> (d8cea2f)</sub>



### Summary

`✅ 14` untouched benchmarks  





---

_@zanieb reviewed on 2025-04-21 17:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/export.rs`:271 on 2025-04-21 17:47_

Should the name need to be `pylock.toml` exactly? Or are we allowing arbitrary names here?

---

_@zanieb reviewed on 2025-04-21 17:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/export.rs`:271 on 2025-04-21 17:47_

(i.e., it'd be really confusing if I did `-o foo/pyproject.toml` and we wrote a `pylock.toml`-formatted file)

---

_@zanieb approved on 2025-04-21 17:47_

---

_@zanieb reviewed on 2025-04-21 17:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/export.rs`:271 on 2025-04-21 17:48_

It looks like https://github.com/astral-sh/uv/pull/12992/files#diff-b6d9818984b152710e4e5cc773e304c857c5b7055dca177c40d7772788944d70R34 is more strict

---

_@charliermarsh reviewed on 2025-04-21 17:48_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/export.rs`:271 on 2025-04-21 17:48_

We should allow any name that matches the spec (starts with “pylock.” and ends with “.toml”). I’ll update this site — I did that elsewhere.

---

_Merged by @charliermarsh on 2025-04-21 21:35_

---

_Closed by @charliermarsh on 2025-04-21 21:35_

---

_Branch deleted on 2025-04-21 21:35_

---
