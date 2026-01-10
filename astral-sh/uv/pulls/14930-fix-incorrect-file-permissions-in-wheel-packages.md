```yaml
number: 14930
title: Fix incorrect file permissions in wheel packages
type: pull_request
state: merged
author: yumeminami
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-wheel-file-permissions
created_at: 2025-07-28T04:22:45Z
updated_at: 2025-08-05T16:21:49Z
url: https://github.com/astral-sh/uv/pull/14930
synced_at: 2026-01-10T06:44:33Z
```

# Fix incorrect file permissions in wheel packages

---

_Pull request opened by @yumeminami on 2025-07-28 04:22_

Fixes #14920

## Summary

Problem: When building wheel packages, metadata files (such as RECORD, METADATA, WHEEL, and
   license files) were being created with incorrect Unix permissions (--w--wx---), lacking
  read permissions and having unexpected executable permissions.

  Solution: The fix ensures that all metadata files in wheel packages are created with proper
   644 (rw-r--r--) permissions by:
  - Adding explicit unix_permissions(0o644) setting in the write_bytes method for metadata
  files
  - Updating permission constants to use octal notation for clarity
  - Improving code comments to document the permission settings

  Impact: This change ensures wheel packages created by uv have standard file permissions
  consistent with other Python build tools like setuptools, improving compatibility and
  following Python packaging best practices.




---

_@charliermarsh approved on 2025-07-28 13:18_

---

_Review requested from @konstin by @charliermarsh on 2025-07-28 13:18_

---

_Assigned to @konstin by @zanieb on 2025-07-28 13:33_

---

_@konstin approved on 2025-07-28 13:55_

Thanks!

---

_Merged by @konstin on 2025-07-28 13:56_

---

_Closed by @konstin on 2025-07-28 13:56_

---

_Label `bug` added by @konstin on 2025-07-28 13:56_

---

_Comment by @zaporter-work on 2025-07-30 00:03_

Hi! Thank you for this fix!

Would you be willing to do a minor release for this patch? Any packages built with this version of uv_build are problematic. I've just spent a few hours tracking this down from a stubs-only transitive dependency which happened to be on 0.8.3 -- it caused our CI to fail with a PermissionDenied error while inflating our built PEX files.

---

_Comment by @charliermarsh on 2025-07-30 01:06_

Do you mean a patch release? We're planning to release tomorrow as part of our regular cadence, but likely won't release tonight.

---

_Comment by @zaporter-work on 2025-07-30 01:37_

@charliermarsh Ah perfect -- thanks for the quick response. I didn't realize your scheduled release is tomorrow -- that is great. Thanks for the work on this project!

---

_Comment by @kieran-ryan on 2025-08-05 16:21_

Thanks for the swift resolution @yumeminami! Verified resolved in v0.8.4! 🙌

---
