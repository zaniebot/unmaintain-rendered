```yaml
number: 10110
title: "Include extras in `uv-build` `Requires-Dist` metadata"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/extras
created_at: 2024-12-23T00:33:26Z
updated_at: 2024-12-23T15:04:40Z
url: https://github.com/astral-sh/uv/pull/10110
synced_at: 2026-01-12T16:09:07Z
```

# Include extras in `uv-build` `Requires-Dist` metadata

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/10091.


---

_Label `bug` added by @charliermarsh on 2024-12-23 00:33_

---

_Label `preview` added by @charliermarsh on 2024-12-23 00:33_

---

_Review requested from @konstin by @charliermarsh on 2024-12-23 00:33_

---

_Comment by @cthoyt on 2024-12-23 08:15_

Thanks @charliermarsh ! Looking forwards, is there already a well defined way of including dependency groups in the PKG-INFO? If so, this could also be a place to include them 

---

_Comment by @charliermarsh on 2024-12-23 13:47_

Do you mean `[dependency-group]`? No, those are intentionally (in the spec) _not_ included in the published package metadata.

---

_Merged by @charliermarsh on 2024-12-23 13:56_

---

_Closed by @charliermarsh on 2024-12-23 13:56_

---

_Branch deleted on 2024-12-23 13:56_

---

_Comment by @cthoyt on 2024-12-23 15:04_

Thanks for the clarification!

---
