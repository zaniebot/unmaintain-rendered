```yaml
number: 16201
title: "Replace `fs_err` alias"
type: pull_request
state: merged
author: MitchellBerend
labels:
  - internal
assignees: []
merged: true
base: main
head: fs-to-fs-err-swap
created_at: 2025-10-09T09:11:46Z
updated_at: 2025-10-09T13:18:39Z
url: https://github.com/astral-sh/uv/pull/16201
synced_at: 2026-01-10T06:36:15Z
```

# Replace `fs_err` alias

---

_Pull request opened by @MitchellBerend on 2025-10-09 09:11_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
This pr started with me trying to replace `fs` with `fs_err` in a couple places (#14985). As @konstin pointed out, `fs` was actually just an alias for `fs_err`. This pr removes that alias so the code is easier to reason about when only looking at the isolated pieces of code.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Green pipeline should suffice
<!-- How was it tested? -->


---

_Comment by @konstin on 2025-10-09 09:13_

There's some `use fs_err as fs;` renames in some of those files. We should probably use `fs_err::` directly instead of renaming, but it's relevant take those imports into account

---

_Comment by @MitchellBerend on 2025-10-09 09:19_

I can not believe I missed that one 🤦, I should have been a little more thorough with this one.

I can pivot this pr to clean that up when I get back to my computer if you think that is a better approach. I found this stuff by running `rg ":: remove_dir_all"` on the code base and found these which seemed odd (see #14985).

---

_Renamed from "Replace `fs::remove_dir_all` with `fs_err::remove_dir_all`" to "Replace `fs_err` alias" by @MitchellBerend on 2025-10-09 10:23_

---

_Marked ready for review by @MitchellBerend on 2025-10-09 11:03_

---

_Label `internal` added by @konstin on 2025-10-09 13:18_

---

_Merged by @konstin on 2025-10-09 13:18_

---

_Closed by @konstin on 2025-10-09 13:18_

---
