```yaml
number: 8537
title: "Add error message when `uv build` is used with non-packaged projects"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/build-warn
created_at: 2024-10-24T18:01:58Z
updated_at: 2024-10-25T19:06:48Z
url: https://github.com/astral-sh/uv/pull/8537
synced_at: 2026-01-10T12:54:11Z
```

# Add error message when `uv build` is used with non-packaged projects

---

_Pull request opened by @charliermarsh on 2024-10-24 18:01_

## Summary

Closes https://github.com/astral-sh/uv/issues/8530.


---

_Review requested from @zanieb by @charliermarsh on 2024-10-24 18:02_

---

_Review requested from @konstin by @charliermarsh on 2024-10-24 18:02_

---

_Label `bug` added by @charliermarsh on 2024-10-24 18:02_

---

_Label `error messages` added by @charliermarsh on 2024-10-24 18:02_

---

_@charliermarsh reviewed on 2024-10-24 18:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:198 on 2024-10-24 18:02_

Should I syntax-highlight the code...?

---

_Review comment by @konstin on `crates/uv/src/commands/build_frontend.rs`:198 on 2024-10-25 07:36_

I'd offset the part of the code you need to copy.

Could you wrap the line? I like indoc for multiline strings.

Should we be recommending setuptools here when we're using hatchling in uv init?

---

_@konstin approved on 2024-10-25 07:37_

---

_Merged by @charliermarsh on 2024-10-25 19:06_

---

_Closed by @charliermarsh on 2024-10-25 19:06_

---

_Branch deleted on 2024-10-25 19:06_

---
