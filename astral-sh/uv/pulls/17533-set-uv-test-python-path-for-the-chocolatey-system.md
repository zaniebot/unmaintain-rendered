```yaml
number: 17533
title: "Set `UV_TEST_PYTHON_PATH` for the chocolatey system test to ensure uv uses the right python"
type: pull_request
state: open
author: EliteTK
labels:
  - bug
  - "test:system"
assignees: []
base: main
head: tk/fix-ci-system-chocolatey
created_at: 2026-01-16T17:40:54Z
updated_at: 2026-01-16T18:54:11Z
url: https://github.com/astral-sh/uv/pull/17533
synced_at: 2026-01-16T19:02:00Z
```

# Set `UV_TEST_PYTHON_PATH` for the chocolatey system test to ensure uv uses the right python

---

_@EliteTK_

## Summary

Fix #17524. 

## Test Plan

N/A

---

_Label `bug` added by @EliteTK on 2026-01-16 17:40_

---

_Label `test:system` added by @EliteTK on 2026-01-16 17:40_

---

_Marked ready for review by @EliteTK on 2026-01-16 17:47_

---

_Review requested from @zanieb by @EliteTK on 2026-01-16 17:47_

---

_Converted to draft by @EliteTK on 2026-01-16 17:48_

---

_Comment by @EliteTK on 2026-01-16 17:49_

Seems like the python path printing is borked now...

---

_Marked ready for review by @EliteTK on 2026-01-16 17:56_

---

_Comment by @zanieb on 2026-01-16 18:17_

I think using `UV_TEST_PYTHON_PATH` isn't okay in the context of the system tests.

---

_Comment by @zanieb on 2026-01-16 18:18_

I think the interpreter that invokes the system test script is the one that should be used, right?

---

_Comment by @EliteTK on 2026-01-16 18:54_

The test script interpreter is the one whose parent directory I am putting in `UV_TEST_PYTHON_PATH`. Although I agree it's kind of jank in retrospect. It could hide a bug in our path parsing.

As an alternative we could prepend to `$env:PATH` [^gh-path] before running the script.

I think the real weird thing here is that there's no actual guarantee that `py -3.9` would pick the chocolatey version if some other 3.9 was installed, but I couldn't find any good way to verify this invariant.

But at least I feel that setting `$env:PATH` prior to running the verification script would at least ensure that the script and uv both use the same python version.

Lastly, I could try to investigate why chocolatey's path changes aren't being picked up although I have a feeling that the answer is going to be either: "the registry changes can't be propagated to the runner without restarting the runner" or "github runners explicitly sanitize that somehow so the environment change doesn't persist".

[^gh-path]: GITHUB_PATH seems to be for appending only, which isn't helpful here.

---
