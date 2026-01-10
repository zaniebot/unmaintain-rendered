```yaml
number: 14530
title: "feature: shorthand for --with (-w) in uvx and uv tool run"
type: pull_request
state: merged
author: noamteyssier
labels:
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2025-07-09T23:57:49Z
updated_at: 2025-07-10T18:50:50Z
url: https://github.com/astral-sh/uv/pull/14530
synced_at: 2026-01-10T06:53:02Z
```

# feature: shorthand for --with (-w) in uvx and uv tool run

---

_Pull request opened by @noamteyssier on 2025-07-09 23:57_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is a small quality of life feature that adds a shorthand (`-w`) to the `--with` flag for minimizing keystrokes.

Pretty minor, but I didn't see any conflicts with `-w` and thought this could be a nice place for it. 

```bash
# proposed addition (short)
uvx -w numpy ipython

# original (long)
uvx --with numpy ipython
```

## Test Plan

Added testing already in the P.R. - just copied over tests from the `--with` flag

<!-- How was it tested? -->


---

_Label `cli` added by @zanieb on 2025-07-10 18:49_

---

_@zanieb approved on 2025-07-10 18:50_

Thanks!

---

_Merged by @zanieb on 2025-07-10 18:50_

---

_Closed by @zanieb on 2025-07-10 18:50_

---
