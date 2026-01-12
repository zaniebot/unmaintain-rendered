```yaml
number: 7095
title: "[WIP] Add a `--suffix` option to `uv tool install`"
type: pull_request
state: closed
author: teofr
labels: []
assignees: []
draft: true
base: main
head: install_suffix_option
created_at: 2024-09-05T18:57:26Z
updated_at: 2025-01-22T19:22:48Z
url: https://github.com/astral-sh/uv/pull/7095
synced_at: 2026-01-12T16:07:41Z
```

# [WIP] Add a `--suffix` option to `uv tool install`

---

_@teofr_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Ongoing attempt to close #6365 

So far:
- Added the CLI argument and general setting
- Modified the tools enviroments and receipts to work with a suffix
  - Right now the design stores tools in the directory `{name}{suffix}`
- Made the `uv tool install` command thread and process suffixes

TODO:
- Add support for `upgrade`, `uninstall`
- Normalize suffixes (right now they're taken verbatim and not according the rules specified in the RFC #3560)
- Modify the representation of `ToolName` after discussions
- Improve testing
  - Colliding tools (Packagea `A` and `A_B` can collide if `A` is installed with suffix `_B`)
  - Preliminary discussions point towards requiring uniqueness of `{package name}{suffix}` as an id, we need to test that

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

For now, tests that the same package can be installed with different suffixes

<!-- How was it tested? -->


---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:66 on 2024-09-05 19:01_

This may be a problematic structure for `uv tool upgrade` etc., e.g., if the user specifies a tool name via the command line we won't know what's the package name and what's a suffix until sometime later?

---

_@zanieb reviewed on 2024-09-05 19:01_

---

_@zanieb reviewed on 2024-09-05 19:03_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:195 on 2024-09-05 19:03_

We'll generally probably want to see the both the name and suffix that we're performing operations on.

---

_@zanieb reviewed on 2024-09-05 19:04_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:195 on 2024-09-05 19:04_

I'm a bit curious why you did this instead of implementing `Display` for `ToolName`

---

_@zanieb reviewed on 2024-09-05 19:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:115 on 2024-09-05 19:05_

Does this handle file extensions on Windows?

---

_@zanieb reviewed on 2024-09-05 19:07_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:249 on 2024-09-05 19:07_

Do we need to write the tool package name somewhere now?

---

_@teofr reviewed on 2024-09-05 19:10_

---

_Review comment by @teofr on `crates/uv-tool/src/lib.rs`:66 on 2024-09-05 19:10_

Agree, thanks for pointing it out

---

_@teofr reviewed on 2024-09-05 19:12_

---

_Review comment by @teofr on `crates/uv-tool/src/lib.rs`:195 on 2024-09-05 19:12_

Yes, didn't change to showing the whole tool name to simplify for now.
Of course, I complicated myself by not implementing Display, I blame my late little use of Rust, will change to that

---

_@teofr reviewed on 2024-09-05 19:18_

---

_Review comment by @teofr on `crates/uv/tests/tool_install.rs`:249 on 2024-09-05 19:18_

That's a good question, I think it depends a bit on what's the point of the receipt

If the goal is to differentiate package installations, I think not, since a package should behave exactly the same if it was installed with a given suffix or not. (ie, could we want to have a cache of package installations? if so, are the receipts the keys of the cache?)

However, if the receipt is just general information used by `uv` then yes, specially since upgrading a package may add entry points, and we may need to use the suffix there.

I lean towards adding it

---

_@teofr reviewed on 2024-09-05 19:37_

---

_Review comment by @teofr on `crates/uv/src/commands/tool/common.rs`:115 on 2024-09-05 19:37_

Added the suffix between the stem and the extension, thanks for noticing

More thought and testing is needed for this bit (as in, what happens with tools that have a `.` in their name)

---

_Review comment by @teofr on `crates/uv-tool/src/lib.rs`:195 on 2024-09-05 19:39_

I added a TODO for changing how ToolNames are displayed, since it needs discussion

---

_@teofr reviewed on 2024-09-05 19:39_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:249 on 2024-09-05 19:43_

Yeah, I presumed we would need the package name during future operations, e.g. during `uv tool upgrade` to retrieve the entry points for the package as you mentioned.

---

_@zanieb reviewed on 2024-09-05 19:43_

---

_Assigned to @zanieb by @zanieb on 2024-09-15 23:27_

---

_Closed by @zanieb on 2025-01-22 19:22_

---
