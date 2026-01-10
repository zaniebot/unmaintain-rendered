```yaml
number: 14996
title: Remove retry wrapper when matching on error kind
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/re
created_at: 2025-07-31T14:54:25Z
updated_at: 2025-07-31T21:00:04Z
url: https://github.com/astral-sh/uv/pull/14996
synced_at: 2026-01-10T06:53:02Z
```

# Remove retry wrapper when matching on error kind

---

_Pull request opened by @charliermarsh on 2025-07-31 14:54_

## Summary

We often match on `ErrorKind` to figure out how to handle an error (e.g., to treat a 404 as "Not found" rather than aborting the program). Unfortunately, if we retry, we wrap the error in a new kind that includes the retry count. This PR adds an unwrapping mechanism to ensure that callers always look at the underlying error.

Closes https://github.com/astral-sh/uv/issues/14941.

Closes https://github.com/astral-sh/uv/issues/14989.


---

_Review requested from @zanieb by @charliermarsh on 2025-07-31 14:54_

---

_Review comment by @charliermarsh on `crates/uv-client/src/error.rs`:282 on 2025-07-31 14:55_

Not a huge fan of this, but not sure what else to do.

---

_@charliermarsh reviewed on 2025-07-31 14:55_

---

_@charliermarsh reviewed on 2025-07-31 14:56_

---

_Review comment by @charliermarsh on `crates/uv-client/src/error.rs`:61 on 2025-07-31 14:56_

There's some risk here. If you do, like, `Error::from(err.into_kind())`, you end up losing the retry wrapper. We do that in at least one place (`get_or_build_wheel_metadata`), which I guess I need to fix.

---

_Label `bug` added by @charliermarsh on 2025-07-31 14:57_

---

_@WilliamStam reviewed on 2025-07-31 16:24_

---

_Review comment by @WilliamStam on `crates/uv-client/src/error.rs`:61 on 2025-07-31 16:24_

not sure how you have it implemented and its not in my language house but wouldnt have a kinda enum of error types then having a error.get_type() on the error and then use that to trigger against? then you can play around with contexts n stuff easier. like enum.HARD_ERROR, enum.SOFT_CONTINUE or some stuff. if status_code == 404 then return enum.NOT_FOUND or whatever

---

_Review requested from @konstin by @charliermarsh on 2025-07-31 18:46_

---

_Marked ready for review by @charliermarsh on 2025-07-31 18:46_

---

_Comment by @charliermarsh on 2025-07-31 18:46_

Okay, changed the implementation to store the retry count on `Error` rather than as its own variant.

---

_@konstin approved on 2025-07-31 19:06_

---

_@zanieb approved on 2025-07-31 20:17_

---

_Merged by @charliermarsh on 2025-07-31 21:00_

---

_Closed by @charliermarsh on 2025-07-31 21:00_

---

_Branch deleted on 2025-07-31 21:00_

---
