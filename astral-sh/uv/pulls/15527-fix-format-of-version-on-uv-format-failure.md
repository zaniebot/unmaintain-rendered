```yaml
number: 15527
title: " Fix format of {version} on `uv format` failure"
type: pull_request
state: merged
author: harsh-ps-2003
labels:
  - preview
assignees: []
merged: true
base: main
head: format
created_at: 2025-08-26T11:13:49Z
updated_at: 2025-08-29T15:42:57Z
url: https://github.com/astral-sh/uv/pull/15527
synced_at: 2026-01-12T16:11:47Z
```

#  Fix format of {version} on `uv format` failure

---

_@harsh-ps-2003_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #15512 

## Test Plan

Manually tested :
```
~/uv/target/release/uv format --version 999.999.999 --preview-features format
error: Failed to install ruff 999.999.999
  Caused by: Failed to download from: https://github.com/astral-sh/ruff/releases/download/999.999.999/ruff-x86_64-unknown-linux-gnu.tar.gz
  Caused by: HTTP status client error (404 Not Found) for url (https://github.com/astral-sh/ruff/releases/download/999.999.999/ruff-x86_64-unknown-linux-gnu.tar.gz)
  ```


---

_@zanieb approved on 2025-08-26 11:21_

---

_Comment by @zanieb on 2025-08-26 11:21_

Thank you!

---

_Label `preview` added by @zanieb on 2025-08-26 11:22_

---

_Renamed from " format {version} on failure" to " Fix format of {version} on `uv format` failure" by @zanieb on 2025-08-26 11:22_

---

_Merged by @zanieb on 2025-08-26 11:26_

---

_Closed by @zanieb on 2025-08-26 11:26_

---

_Branch deleted on 2025-08-29 15:42_

---
