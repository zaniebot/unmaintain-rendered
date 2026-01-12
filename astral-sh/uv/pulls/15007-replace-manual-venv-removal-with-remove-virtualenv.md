```yaml
number: 15007
title: replace manual venv removal with remove_virtualenv
type: pull_request
state: merged
author: Zaloog
labels:
  - internal
assignees: []
merged: true
base: main
head: lg/use-remove-virtualenv
created_at: 2025-07-31T20:12:14Z
updated_at: 2025-08-07T20:52:57Z
url: https://github.com/astral-sh/uv/pull/15007
synced_at: 2026-01-12T16:11:31Z
```

# replace manual venv removal with remove_virtualenv

---

_@Zaloog_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
At some places the virtualenv directory was manually removed instead of using `remove_virtualenv`.
I also adjusted the error type.
#14985 

## Test Plan

<!-- How was it tested? -->


---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:192 on 2025-07-31 20:15_

I think here, we should remove the Windows self-replace handling above because `remove_virtualenv` does that already.

---

_@zanieb reviewed on 2025-07-31 20:15_

---

_Comment by @zanieb on 2025-07-31 20:15_

Thank you!

---

_Label `internal` added by @zanieb on 2025-07-31 20:15_

---

_@Zaloog reviewed on 2025-07-31 20:28_

---

_Review comment by @Zaloog on `crates/uv-tool/src/lib.rs`:192 on 2025-07-31 20:28_

done

---

_Comment by @Zaloog on 2025-07-31 21:06_

An error is no longer properly propagated for the tools when uninstalling a not installed tool. Can reproduce it locally and will try to fix that

---

_Marked ready for review by @Zaloog on 2025-08-01 08:28_

---

_@zanieb reviewed on 2025-08-01 12:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:114 on 2025-08-01 12:47_

I don't think this is quite right. We should still have an `if err.kind()` check and we shouldn't have commented out code.

---

_@Zaloog reviewed on 2025-08-01 15:09_

---

_Review comment by @Zaloog on `crates/uv/src/commands/tool/uninstall.rs`:114 on 2025-08-01 15:09_

is there a way to access the err.kind somehow?
Since the error is no longer a `uv_tool::Error::Io`, but `uv::tool::Error::VirtualEnvError` `.kind()` is not defined on `err`.
Do you have an idea on how to handle that?


---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:114 on 2025-08-01 16:13_

You should be able to do `tool::Error::VirtualEnvError(uv_virtualenv::Error::Io(err))` 

---

_@zanieb reviewed on 2025-08-01 16:13_

---

_@Zaloog reviewed on 2025-08-01 17:13_

---

_Review comment by @Zaloog on `crates/uv/src/commands/tool/uninstall.rs`:114 on 2025-08-01 17:13_

yes that did it. 
Fixed and removed comments

---

_@zanieb approved on 2025-08-07 20:52_

---

_Merged by @zanieb on 2025-08-07 20:52_

---

_Closed by @zanieb on 2025-08-07 20:52_

---
