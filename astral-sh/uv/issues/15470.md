```yaml
number: 15470
title: uv tree to show package size
type: issue
state: closed
author: laoshaw
labels:
  - enhancement
assignees: []
created_at: 2025-08-23T15:38:40Z
updated_at: 2025-12-03T04:06:18Z
url: https://github.com/astral-sh/uv/issues/15470
synced_at: 2026-01-10T03:23:54Z
```

# uv tree to show package size

---

_Issue opened by @laoshaw on 2025-08-23 15:38_

### Summary

let 'uv tree' to show all package sizes so we know when new dependencies are pulled in, how much size it will increase. 

### Example

_No response_

---

_Label `enhancement` added by @laoshaw on 2025-08-23 15:38_

---

_Comment by @zanieb on 2025-08-23 18:54_

I think calculating package size is non-trivial since we store them unpacked.

---

_Comment by @charliermarsh on 2025-08-24 13:11_

We can show the sizes of the wheels themselves (we store them in the lockfile).

---

_Comment by @zanieb on 2025-08-24 14:45_

Then we'll be showing compressed size, but I'm okay with that.

---

_Closed by @charliermarsh on 2025-12-03 04:06_

---

_Comment by @charliermarsh on 2025-12-03 04:06_

Looks like this happened in https://github.com/astral-sh/uv/pull/15531.

---
