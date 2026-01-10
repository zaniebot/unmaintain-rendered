```yaml
number: 15764
title: Remove fs2 dependency and update Rust to 1.89
type: pull_request
state: merged
author: ponchofiesta
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-fs2
created_at: 2025-09-10T07:52:15Z
updated_at: 2025-11-02T21:33:53Z
url: https://github.com/astral-sh/uv/pull/15764
synced_at: 2026-01-10T06:28:12Z
```

# Remove fs2 dependency and update Rust to 1.89

---

_Pull request opened by @ponchofiesta on 2025-09-10 07:52_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? --> 

This PR removes the crate fs2 and updates Rust version to 1.89.

*Why?*

Crate fs2 is unmaintained for a long time now and has unfixed issues. Especially it doesn't build on AIX, which is the reason I started fixing it.

*How?*

I removed fs2 and replaced it by std:fs:File methods.

## Test Plan

<!-- How was it tested? -->
- I built it on Windows and AIX only.
- I did not test the artifacts.


---

_@konstin reviewed on 2025-09-10 08:11_

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:681 on 2025-09-10 08:11_

Once https://github.com/andrewhickman/fs-err/issues/74 is fixed, we can simplify this code by using fs_err methods directly which have the paths attached to the error message.

---

_Comment by @konstin on 2025-09-10 08:12_

Thank you!

I'm putting this PR in draft mode while we're waiting for 1.91.

---

_Label `internal` added by @konstin on 2025-09-10 08:12_

---

_Converted to draft by @konstin on 2025-09-10 08:12_

---

_Comment by @Arkenstone999 on 2025-09-10 08:28_

this line : 'file.file().lock().map_err(|err| {' looks more accurate. (only my opinion :)

---

_Comment by @charliermarsh on 2025-11-02 18:43_

Now that we're on 1.89, we might be able to merge this?

---

_@charliermarsh approved on 2025-11-02 20:40_

---

_Marked ready for review by @charliermarsh on 2025-11-02 20:40_

---

_Comment by @charliermarsh on 2025-11-02 20:40_

This should be good to go.

---

_Merged by @charliermarsh on 2025-11-02 21:33_

---

_Closed by @charliermarsh on 2025-11-02 21:33_

---
