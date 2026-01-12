```yaml
number: 14200
title: Preserve non-inline comments when removing dependencies
type: pull_request
state: open
author: christeefy
labels:
  - bug
assignees: []
base: main
head: bug/issue-13966
created_at: 2025-06-22T19:52:45Z
updated_at: 2025-09-07T15:12:18Z
url: https://github.com/astral-sh/uv/pull/14200
synced_at: 2026-01-12T16:11:05Z
```

# Preserve non-inline comments when removing dependencies

---

_@christeefy_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Properly handling comments when editing `pyproject.toml` has been brought up in various issues. 

This particular PR preserves non-inline comments when removing dependencies from the `pyproject.toml` file. 
Fixes #13966 and fixes #9856.

## Test Plan

<!-- How was it tested? -->
Added a few tests to handle removing dependencies in various scenarios.

---

_Renamed from "Bug/issue 13966" to "Preserve non-inline comments when removing dependencies" by @christeefy on 2025-06-22 19:53_

---

_Comment by @zanieb on 2025-06-27 17:47_

Thanks for the contribution!

---

_Review requested from @konstin by @zanieb on 2025-06-27 17:47_

---

_Assigned to @konstin by @zanieb on 2025-06-27 17:47_

---

_Label `bug` added by @zanieb on 2025-06-27 17:47_

---

_Comment by @christeefy on 2025-09-02 16:03_

Hi @konstin, it's been a while. Assuming it's still relevant, any chance you can take a look at this PR? Thanks!

---

_Comment by @konstin on 2025-09-07 15:11_

Sorry, this fell of my review queue.

I fixed a bug when there's both a trailing comment on the last item and on the array, but there's still a bug with complex comments (`remove_handle_oddly_placed_trailing_comma`). I don't really understand where it comes from, it works in a scratch project with only `prepare_comments_for_removal`.

---
