```yaml
number: 15400
title: Fix uv_build wheel hashes
type: pull_request
state: merged
author: anuraaga
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-hash
created_at: 2025-08-21T05:06:08Z
updated_at: 2025-08-21T08:54:27Z
url: https://github.com/astral-sh/uv/pull/15400
synced_at: 2026-01-12T16:11:43Z
```

# Fix uv_build wheel hashes

---

_@anuraaga_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Currently record hashes are the hex encoded sha-256 sum. However, they're supposed to be urlsafe-base64-nopad.

https://packaging.python.org/en/latest/specifications/recording-installed-packages/#the-record-file

Fixes #15398 

## Test Plan

<!-- How was it tested? -->

Build any wheel

```
uv build --wheel
```

Unpack the wheel

```
uvx wheel unpack dist/*.whl
```

Before this change, it will fail with a hash mismatch. I could confirm with a local build that now the wheel can be unpacked with the `wheel` command. While I don't enable hash checking when syncing, presumably it would also currently fail.

---

_@anuraaga reviewed on 2025-08-21 05:10_

---

_Review comment by @anuraaga on `crates/uv-build-backend/src/lib.rs`:676 on 2025-08-21 05:10_

I noticed the tests in `wheel.rs` don't exercise `write_hashed`, so I added this integration test

---

_@konstin approved on 2025-08-21 08:06_

Thank you!

---

_Merged by @konstin on 2025-08-21 08:54_

---

_Closed by @konstin on 2025-08-21 08:54_

---

_Label `bug` added by @konstin on 2025-08-21 08:54_

---
