```yaml
number: 13483
title: Add missing word in “script not supported” error
type: pull_request
state: merged
author: andrewpollack
labels:
  - error messages
assignees: []
merged: true
base: main
head: andrewpollack/add-like-to-error-messages
created_at: 2025-05-16T04:48:52Z
updated_at: 2025-05-19T06:06:32Z
url: https://github.com/astral-sh/uv/pull/13483
synced_at: 2026-01-12T16:10:42Z
```

# Add missing word in “script not supported” error

---

_@andrewpollack_

Love this tooling! Small adjustment to help on error messaging 🙏 

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The previous message was missing the word **“like,”** which made it read a tad awkwardly.  
This PR inserts the missing word so the error reads naturally:
**After:**
```
$ uvx ch1.py 
error: It looks like you tried to run a Python script at `ch1.py`, which is not supported by `uvx`

hint: Use `uv run ch1.py` instead
```

**Before:**
```
$ uvx ch1.py 
error: It looks you tried to run a Python script at `ch1.py`, which is not supported by `uvx`

hint: Use `uv run ch1.py` instead
```

## Test Plan

<!-- How was it tested? -->
- `cargo run -- uvx examples/ch1.py` shows the updated message (see “After” above).
- `cargo test` passes.


---

_@konstin approved on 2025-05-16 08:23_

thanks

---

_Label `error messages` added by @konstin on 2025-05-16 08:23_

---

_Merged by @konstin on 2025-05-16 08:23_

---

_Closed by @konstin on 2025-05-16 08:23_

---

_Renamed from "cli: adjust grammar in “script not supported” error" to "Add missing work in “script not supported” error" by @konstin on 2025-05-16 08:23_

---

_Renamed from "Add missing work in “script not supported” error" to "Add missing word in “script not supported” error" by @konstin on 2025-05-16 08:23_

---
