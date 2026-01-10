```yaml
number: 16203
title: "Replace `remove_dir_all` with `remove_virtualenv`"
type: pull_request
state: open
author: MitchellBerend
labels: []
assignees: []
base: main
head: remove-dir-all-to-remove-virtualenv
created_at: 2025-10-09T11:19:15Z
updated_at: 2025-10-15T14:30:50Z
url: https://github.com/astral-sh/uv/pull/16203
synced_at: 2026-01-10T06:36:15Z
```

# Replace `remove_dir_all` with `remove_virtualenv`

---

_Pull request opened by @MitchellBerend on 2025-10-09 11:19_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This pr replaces `remove_dir_all` with `remove_virtualenv`.

Closes #14985

## Test Plan

Pipeline green

<!-- How was it tested? -->


---

_@zanieb approved on 2025-10-09 13:40_

---

_Comment by @zanieb on 2025-10-09 13:41_

Hm the CI failure seems real.

---

_@MitchellBerend reviewed on 2025-10-11 13:28_

---

_Review comment by @MitchellBerend on `crates/uv-tool/src/lib.rs`:265 on 2025-10-11 13:28_

This can also be formatted differently, not sure which is preferred here.

```rust
            Err(uv_virtualenv::Error::Io(err)) if err.kind() != io::ErrorKind::NotFound => {
                return Err(err.into());
            }
```

---

_Marked ready for review by @MitchellBerend on 2025-10-11 13:28_

---

_Review requested from @zanieb by @MitchellBerend on 2025-10-11 13:29_

---

_Comment by @MitchellBerend on 2025-10-15 14:30_

@zanieb I think the request for review got lost in the chaos. I made some edits to fix the errors CI caught and there is also a formatting option that I personally don't have a preference on. Can you re-review this?

---
