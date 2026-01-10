```yaml
number: 14646
title: Add UV_LIBC to allow libc selection in multi-libc environment
type: pull_request
state: merged
author: nathanscain
labels: []
assignees: []
merged: true
base: main
head: feature/select-libc
created_at: 2025-07-16T06:23:09Z
updated_at: 2025-07-16T14:07:57Z
url: https://github.com/astral-sh/uv/pull/14646
synced_at: 2026-01-10T06:53:02Z
```

# Add UV_LIBC to allow libc selection in multi-libc environment

---

_Pull request opened by @nathanscain on 2025-07-16 06:23_

Closes #14262 

## Description

Adds `UV_LIBC` environment variable and implements check within `Libc::from_env` as recommended here: https://github.com/astral-sh/uv/issues/14262#issuecomment-3014600313

Gave this a few passes to make sure I follow dev practices within uv as best I am able. Feel free to call out anything that could be improved.

## Test Plan

Planned to simply run existing test suite. Open to adding more tests once implementation is validated due to my limited Rust experience.


---

_Renamed from "Add UV_LIBC to all libc selection in multi-libc environment" to "Add UV_LIBC to allow libc selection in multi-libc environment" by @nathanscain on 2025-07-16 06:23_

---

_Comment by @nathanscain on 2025-07-16 06:33_

Not sure what is causing the check system failure - pylint was installed: https://github.com/astral-sh/uv/actions/runs/16312109803/job/46069841658?pr=14646#step:5:272

Update: appears present in other recent PRs from both the [github-actions bot](https://github.com/astral-sh/uv/actions/runs/16310314528/job/46064726866) and [Zanie](https://github.com/astral-sh/uv/actions/runs/16310422184/job/46065032541)

---

_Comment by @zanieb on 2025-07-16 13:52_

Yeah that's a new problem https://github.com/astral-sh/uv/pull/14652 — don't worry about that.

This LGTM

---

_@zanieb approved on 2025-07-16 13:52_

---

_Merged by @zanieb on 2025-07-16 13:52_

---

_Closed by @zanieb on 2025-07-16 13:52_

---

_Comment by @nathanscain on 2025-07-16 14:07_

Awesome - thanks for your help on this!

---
