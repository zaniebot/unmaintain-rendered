```yaml
number: 1885
title: Fix windows py spurious stderr failure
type: pull_request
state: merged
author: jnnnnn
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: fix/windows-py-stderr
created_at: 2024-02-22T22:18:00Z
updated_at: 2024-02-23T22:36:34Z
url: https://github.com/astral-sh/uv/pull/1885
synced_at: 2026-01-12T16:04:46Z
```

# Fix windows py spurious stderr failure

---

_@jnnnnn_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

On Windows `10.0.19045` the `py` command prints to `stderr` even when working correctly.  This means that uv should not treat this as a failure.

Fixes https://github.com/astral-sh/uv/issues/1904

## Test Plan

I ran the modified code and it worked. I expect the pull request to run automated tests.


---

_@zanieb approved on 2024-02-22 22:25_

Sounds good to me!

---

_Label `bug` added by @zanieb on 2024-02-22 22:25_

---

_Review requested from @MichaReiser by @zanieb on 2024-02-22 22:25_

---

_Review requested from @konstin by @zanieb on 2024-02-22 22:25_

---

_Comment by @charliermarsh on 2024-02-22 23:57_

(Would be good to get eyes on this from whoever wrote it, to know why it was here in the first place.)

---

_Label `windows` added by @charliermarsh on 2024-02-22 23:57_

---

_Comment by @zanieb on 2024-02-23 00:29_

We had similar logic like this in the LSP? I feel like it's bad form to use content in stderr as a bad exit. I agree it'd be nice to get another review.

---

_Comment by @charliermarsh on 2024-02-23 00:29_

Agree.

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:347 on 2024-02-23 07:41_

That's funny that it only does this sometimes. I tried to reproduce this on my machine with the official `py` launcher and never managed to get it write to stderr. 

I don't mind the change but we may want to log the `stderr` output with `tracing::debug!()` if it's non empty. It might be helpful when debugging issues related to `py`. 

---

_@MichaReiser approved on 2024-02-23 07:41_

---

_@konstin approved on 2024-02-23 15:00_

---

_Comment by @konstin on 2024-02-23 15:04_

> (Would be good to get eyes on this from whoever wrote it, to know why it was here in the first place.)

This was just defensive programming

---

_Merged by @zanieb on 2024-02-23 15:05_

---

_Closed by @zanieb on 2024-02-23 15:05_

---

_@jnnnnn reviewed on 2024-02-23 22:36_

---

_Review comment by @jnnnnn on `crates/uv-interpreter/src/python_query.rs`:347 on 2024-02-23 22:36_

![image](https://github.com/astral-sh/uv/assets/403995/a4f538a8-0bbc-4aae-909b-46094264f2d0)

It's consistent on my machine; I meant "sometimes" as in depending on which windows box you're on.

My `py` does this in a command window as well as bash (mingw).

---
