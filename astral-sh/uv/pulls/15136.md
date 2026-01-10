```yaml
number: 15136
title: Harden ZIP streaming to reject repeated entries and other malformed ZIP files
type: pull_request
state: merged
author: charliermarsh
labels:
  - security
assignees: []
merged: true
base: main
head: charlie/zip
created_at: 2025-08-07T13:54:05Z
updated_at: 2025-08-07T14:31:50Z
url: https://github.com/astral-sh/uv/pull/15136
synced_at: 2026-01-10T06:44:33Z
```

# Harden ZIP streaming to reject repeated entries and other malformed ZIP files

---

_Pull request opened by @charliermarsh on 2025-08-07 13:54_

## Summary

uv will now reject ZIP files that meet any of the following conditions:

- Multiple local header entries exist for the same file with different contents.
- A local header entry exists for a file that isn't included in the end-of-central directory record.
- An entry exists in the end-of-central directory record that does not have a corresponding local header.
- The ZIP file contains contents after the first end-of-central directory record.
- The CRC32 doesn't match between the local file header and the end-of-central directory record.
- The compressed size doesn't match between the local file header and the end-of-central directory record.
- The uncompressed size doesn't match between the local file header and the end-of-central directory record.
- The reported central directory offset (in the end-of-central-directory header) does not match the actual offset.
- The reported ZIP64 end of central directory locator offset does not match the actual offset.

We also validate the above for files with data descriptors, which we previously ignored.

Wheels from the most recent releases of the top 15,000 packages on PyPI have been confirmed to pass these checks, and PyPI will also reject ZIPs under many of the same conditions (at upload time) in the future.

In rare cases, this validation can be disabled by setting `UV_INSECURE_NO_ZIP_VALIDATION=1`. Any validations should be reported to the uv issue tracker and to the upstream package maintainer.


---

_Review requested from @zanieb by @charliermarsh on 2025-08-07 13:54_

---

_Review requested from @woodruffw by @charliermarsh on 2025-08-07 13:54_

---

_Review requested from @konstin by @charliermarsh on 2025-08-07 13:54_

---

_Label `security` added by @charliermarsh on 2025-08-07 13:54_

---

_Marked ready for review by @charliermarsh on 2025-08-07 13:54_

---

_Review comment by @woodruffw on `crates/uv-dev/src/validate_zip.rs`:25 on 2025-08-07 13:57_

Nitpick, not worth blocking on: this error should probably be `Only archive URLs are supported` I think?

---

_@woodruffw approved on 2025-08-07 13:59_

---

_Merged by @charliermarsh on 2025-08-07 14:31_

---

_Closed by @charliermarsh on 2025-08-07 14:31_

---

_Branch deleted on 2025-08-07 14:31_

---
