```yaml
number: 6740
title: "Avoid showing duplicate paths in `uv python list`"
type: pull_request
state: merged
author: leaf-soba
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2024-08-28T08:20:25Z
updated_at: 2024-08-29T01:21:32Z
url: https://github.com/astral-sh/uv/pull/6740
synced_at: 2026-01-12T16:07:30Z
```

# Avoid showing duplicate paths in `uv python list`

---

_@leaf-soba_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
- Resolves issue #6690
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
```
$ cargo run python list
```
<!-- How was it tested? -->


---

_Review requested from @zanieb by @konstin on 2024-08-28 10:03_

---

_Comment by @zanieb on 2024-08-28 13:22_

Hi! Thanks for contributing this fix.

I applied a small patch (https://github.com/astral-sh/uv/pull/6740/commits/4d4cd93ada926005bfeec5eb39c50919b209b734) to move the filtering up where we do all the other filtering. What do you think?

---

_Assigned to @zanieb by @zanieb on 2024-08-28 13:22_

---

_Renamed from "Update list.rs" to "Avoid showing duplicate paths in `uv python list`" by @zanieb on 2024-08-28 13:22_

---

_Comment by @zanieb on 2024-08-28 15:48_

Tested with 

```
❯ PATH="/Users/zb/Library/Application Support/uv/python/cpython-3.10.13-macos-aarch64-none/bin/:$PATH" uv python list | wc -l
      16
❯ PATH="/Users/zb/Library/Application Support/uv/python/cpython-3.10.13-macos-aarch64-none/bin/:$PATH" ./target/debug/uv python list | wc -l
      15
```

---

_Merged by @zanieb on 2024-08-28 15:48_

---

_Closed by @zanieb on 2024-08-28 15:48_

---

_Comment by @leaf-soba on 2024-08-29 01:21_

> Hi! Thanks for contributing this fix.
> 
> I applied a small patch ([4d4cd93](https://github.com/astral-sh/uv/commit/4d4cd93ada926005bfeec5eb39c50919b209b734)) to move the filtering up where we do all the other filtering. What do you think?

thanks it looks great! sorry I'm new to Rust and I didn't run all test, so my code failed in two cases.

---
