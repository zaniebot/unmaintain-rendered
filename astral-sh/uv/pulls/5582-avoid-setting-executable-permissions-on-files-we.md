```yaml
number: 5582
title: Avoid setting executable permissions on files we might not own
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/perms
created_at: 2024-07-30T01:24:22Z
updated_at: 2024-07-30T12:32:53Z
url: https://github.com/astral-sh/uv/pull/5582
synced_at: 2026-01-10T13:37:23Z
```

# Avoid setting executable permissions on files we might not own

---

_Pull request opened by @charliermarsh on 2024-07-30 01:24_

## Summary

If we just created an entrypoint script, we can of course set the permissions (we just created it). However, if we're copying from the cache, we might _not_ own the file. In that case, if we need to change the permissions (we shouldn't, since the script is likely already executable -- we set the permissions when we unzip, but I guess they could _not_ be properly set in the zip itself), we have to copy it.

Closes https://github.com/astral-sh/uv/issues/5581.


---

_Label `bug` added by @charliermarsh on 2024-07-30 01:25_

---

_Marked ready for review by @charliermarsh on 2024-07-30 01:27_

---

_Comment by @charliermarsh on 2024-07-30 01:39_

@stas00 -- I can give you a link to an executable, what OS / arch?

---

_Review requested from @konstin by @charliermarsh on 2024-07-30 02:16_

---

_Comment by @stas00 on 2024-07-30 02:46_

```
Linux foo 5.15.0-1006- #6-Ubuntu SMP Thu Feb 29 17:15:35 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

---

_Comment by @charliermarsh on 2024-07-30 02:53_

You can try this: https://github.com/astral-sh/uv/actions/runs/10154584147/artifacts/1753471429 (it’s from the build-binary job).

---

_@konstin approved on 2024-07-30 11:43_

---

_Merged by @charliermarsh on 2024-07-30 12:32_

---

_Closed by @charliermarsh on 2024-07-30 12:32_

---

_Branch deleted on 2024-07-30 12:32_

---
