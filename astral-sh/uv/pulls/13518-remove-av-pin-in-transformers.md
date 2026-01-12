```yaml
number: 13518
title: "Remove `av` pin in `transformers`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/av
created_at: 2025-05-18T23:51:00Z
updated_at: 2025-05-18T23:59:36Z
url: https://github.com/astral-sh/uv/pull/13518
synced_at: 2026-01-12T16:10:43Z
```

# Remove `av` pin in `transformers`

---

_@charliermarsh_

## Summary

This test started failing, and it fails at least back to v0.6, so I don't think it's on our end. I'm wondering if all the wheels here were yanked? They're visible in the lockfile, but not on PyPI: https://pypi.org/project/av/9.2.0/#files. So to get this passing, let's just unpin it.

Edit: Ahh, ok. It looks like the project ran out of space, so they removed wheels for all the older versions: https://github.com/PyAV-Org/PyAV/issues/1879.


---

_Label `testing` added by @charliermarsh on 2025-05-18 23:51_

---

_Marked ready for review by @charliermarsh on 2025-05-18 23:51_

---

_Merged by @charliermarsh on 2025-05-18 23:59_

---

_Closed by @charliermarsh on 2025-05-18 23:59_

---

_Branch deleted on 2025-05-18 23:59_

---
