```yaml
number: 12360
title: Retain end-of-line comment position when adding dependency
type: pull_request
state: merged
author: christeefy
labels:
  - bug
assignees: []
merged: true
base: main
head: bug/retain-trailing-comment-position
created_at: 2025-03-21T09:52:22Z
updated_at: 2025-03-22T00:30:54Z
url: https://github.com/astral-sh/uv/pull/12360
synced_at: 2026-01-10T11:10:39Z
```

# Retain end-of-line comment position when adding dependency

---

_Pull request opened by @christeefy on 2025-03-21 09:52_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This fixes a case described in #12333, where trailing comments in dependencies can be unexpectedly shifted when a new dependency is added.

Fixes #12333.

## Test Plan

<!-- How was it tested? -->

`cargo test` (Added a snapshot test)


---

_Renamed from "🐛  Bug/retain trailing comment position" to "🐛  Retain trailing comment position when adding dependency" by @christeefy on 2025-03-21 09:52_

---

_Converted to draft by @christeefy on 2025-03-21 09:57_

---

_Comment by @christeefy on 2025-03-21 10:09_

I'm realizing that testing is done using `insta`. I can move my test logic to the other module, and refactor the documentation, before marking this as ready.

---

_Marked ready for review by @christeefy on 2025-03-21 11:48_

---

_Renamed from "🐛  Retain trailing comment position when adding dependency" to "🐛  Retain end-of-line comment position when adding dependency" by @christeefy on 2025-03-21 11:52_

---

_Review requested from @Gankra by @zanieb on 2025-03-21 13:41_

---

_@Gankra approved on 2025-03-21 14:15_

Wow this function does some real heroics for formatting, thanks for the fix!

---

_Label `bug` added by @Gankra on 2025-03-21 14:15_

---

_Renamed from "🐛  Retain end-of-line comment position when adding dependency" to "Retain end-of-line comment position when adding dependency" by @Gankra on 2025-03-21 14:15_

---

_Merged by @Gankra on 2025-03-21 14:16_

---

_Closed by @Gankra on 2025-03-21 14:16_

---

_Branch deleted on 2025-03-22 00:30_

---
