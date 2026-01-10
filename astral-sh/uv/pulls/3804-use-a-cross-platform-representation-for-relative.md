```yaml
number: 3804
title: "Use a cross-platform representation for relative paths in `pip compile`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/rel
created_at: 2024-05-23T21:20:35Z
updated_at: 2024-05-24T03:02:43Z
url: https://github.com/astral-sh/uv/pull/3804
synced_at: 2026-01-10T14:32:20Z
```

# Use a cross-platform representation for relative paths in `pip compile`

---

_Pull request opened by @charliermarsh on 2024-05-23 21:20_

## Summary

I haven't tested on Windows yet, but the idea here is that we should use a portable representation when printing paths.

I decided to limit the scope here to paths that we write to output files.

Closes https://github.com/astral-sh/uv/issues/3800.


---

_Label `bug` added by @charliermarsh on 2024-05-23 21:20_

---

_Label `windows` added by @charliermarsh on 2024-05-23 21:20_

---

_@zanieb approved on 2024-05-23 21:21_

Cool if it works!

---

_Comment by @charliermarsh on 2024-05-23 21:23_

Famous last words.

---

_Comment by @codspeed-hq[bot] on 2024-05-23 21:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/rel)

### Merging #3804 will **not alter performance**

<sub>Comparing <code>charlie/rel</code> (0183c34) with <code>main</code> (dd27f2f)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @charliermarsh on 2024-05-23 21:41_

Alright, this isn't quite what we want. But I think I _can_ use this crate to solve this specific problem.

---

_Comment by @charliermarsh on 2024-05-23 21:47_

Specifically, I think I can use this when we `.join`.

---

_Comment by @charliermarsh on 2024-05-23 23:43_

Ok, this definitely is not doing what I want.

---

_Closed by @charliermarsh on 2024-05-23 23:43_

---

_Reopened by @charliermarsh on 2024-05-23 23:58_

---

_Comment by @charliermarsh on 2024-05-23 23:59_

Okay, this actually _does_ do what we want.

---

_Renamed from "Use a cross-platform representation for relative paths" to "Use a cross-platform representation for relative paths in `pip compile`" by @charliermarsh on 2024-05-24 02:40_

---

_Merged by @charliermarsh on 2024-05-24 03:02_

---

_Closed by @charliermarsh on 2024-05-24 03:02_

---

_Branch deleted on 2024-05-24 03:02_

---
