```yaml
number: 12328
title: Add automatic .py extension for script initialization
type: pull_request
state: closed
author: chhoumann
labels: []
assignees: []
base: main
head: auto-py-extension
created_at: 2025-03-20T06:52:13Z
updated_at: 2025-03-21T18:23:04Z
url: https://github.com/astral-sh/uv/pull/12328
synced_at: 2026-01-12T16:10:13Z
```

# Add automatic .py extension for script initialization

---

_@chhoumann_

## Summary

When initializing a script with `uv init --script`, automatically append .py extension if the user didn't specify it.

This is a quality-of-life feature that follows the principle of least surprise. Most users would expect a Python script to have a .py extension, and many may forget to add it manually. At least, I did prior to making this PR :)

## Test Plan

Added a new test case `init_script_auto_py_extension()` that verifies the behavior works correctly:
- Verified the extension is correctly added, and the file is created with the right path
- Confirmed the output messages to the user correctly display the path with extension

---

_Comment by @chhoumann on 2025-03-20 07:14_

I noticed the test `cache_prune::prune_unzipped` is failing, but I believe it's unrelated to the changes in this PR. PR #12327 (which adds UV_PROJECT environment variable support) is seeing the same test failure despite making completely different changes.

My local testing shows the new `init_script_auto_py_extension` test passes, and the functionality works as expected.

The error message in the failing test changed slightly in format. Happy to help address this if needed, or we could handle it in a separate PR if that would be preferred. Thanks for reviewing!

---

_Comment by @zanieb on 2025-03-20 18:40_

I think if we want to do this, we'll probably want to do it everywhere `--script` is accepted? I'm not sure how I feel. It's a bit easier to sell here, but the lack of consistency would bother me. Let me try to get some opinions from the rest of the team.

---

_Comment by @zanieb on 2025-03-21 18:13_

I think we're leaning away from this for now — it's valid to write Python scripts without the extension. Thanks for contributing though.

---

_Closed by @zanieb on 2025-03-21 18:13_

---

_Comment by @chhoumann on 2025-03-21 18:23_

Thanks for considering this idea\! Totally understand that Python doesn't require extensions. Appreciate the feedback and looking forward to finding other ways to contribute to uv\!

---
