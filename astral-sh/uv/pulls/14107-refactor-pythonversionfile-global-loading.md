```yaml
number: 14107
title: "Refactor `PythonVersionFile` global loading"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/ref-version-file
created_at: 2025-06-17T13:57:02Z
updated_at: 2025-06-20T20:31:33Z
url: https://github.com/astral-sh/uv/pull/14107
synced_at: 2026-01-12T16:11:01Z
```

# Refactor `PythonVersionFile` global loading

---

_@zanieb_

I was looking into `uv tool` not supporting version files, and noticed this implementation was confusing and skipped handling like a tracing log if `--no-config` excludes selection a file. I've refactored it in preparation for the next change.

---

_Label `internal` added by @zanieb on 2025-06-17 13:57_

---

_@zanieb reviewed on 2025-06-17 16:56_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:128 on 2025-06-17 16:56_

I'm not sure why this was passed in previously.

---

_@zanieb reviewed on 2025-06-17 16:56_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:118 on 2025-06-17 16:56_

We'll respect the `no_config` logic above instead, which has a message for when it's applied.

---

_Marked ready for review by @zanieb on 2025-06-17 16:57_

---

_@zanieb reviewed on 2025-06-17 16:57_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:86 on 2025-06-17 16:57_

These messages are just indented, we don't want to show them when `no_local` is enabled.

---

_Comment by @zanieb on 2025-06-18 23:43_

Example usage in the `tool install` change at #14112 

https://github.com/astral-sh/uv/blob/c421cb9fa476b681896441b4325719b90946433f/crates/uv/src/commands/tool/install.rs#L77-L95

---

_Review requested from @oconnor663 by @oconnor663 on 2025-06-18 23:43_

---

_@oconnor663 reviewed on 2025-06-19 00:01_

---

_Review comment by @oconnor663 on `crates/uv-python/src/version_files.rs`:131 on 2025-06-19 00:01_

Is this last `is_file` check redundant with the same check done inside `find_in_directory`?

---

_@oconnor663 reviewed on 2025-06-19 00:05_

---

_Review comment by @oconnor663 on `crates/uv-python/src/version_files.rs`:86 on 2025-06-19 00:05_

What do you mean by just indented?

---

_@oconnor663 reviewed on 2025-06-19 00:08_

---

_Review comment by @oconnor663 on `crates/uv/src/commands/python/pin.rs`:60 on 2025-06-19 00:08_

It seems like the previous behavior was that a global pin would only be updated if `--global` is set, but with the changes in this PR, it gets updated by default if no local pin is found. Is that intended?

---

_@zanieb reviewed on 2025-06-19 16:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:60 on 2025-06-19 16:31_

Hm, good point. Confusingly, `python_pin_global_use_local_if_available` should provide test coverage for this and is passing?

---

_@zanieb reviewed on 2025-06-19 16:32_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:131 on 2025-06-19 16:32_

Sure seems like it, not sure why that was there — I'll remove it.

---

_@zanieb reviewed on 2025-06-19 16:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:60 on 2025-06-19 16:41_

This is because we only read from this file. For construction of a new file, we use this

https://github.com/astral-sh/uv/blob/6c5b1e866e1f8d344ff8f13cbbab17880932fc3d/crates/uv/src/commands/python/pin.rs#L188-L200

---

_@zanieb reviewed on 2025-06-19 16:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:60 on 2025-06-19 16:44_

The previous code was just superfluous

---

_@zanieb reviewed on 2025-06-19 16:44_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:86 on 2025-06-19 16:44_

The code is literally indented, there are no changes otherwise.

---

_Review requested from @oconnor663 by @zanieb on 2025-06-20 15:36_

---

_@oconnor663 reviewed on 2025-06-20 19:32_

---

_Review comment by @oconnor663 on `crates/uv/src/commands/python/pin.rs`:60 on 2025-06-20 19:32_

Ah never mind, I was misunderstanding the change to `PythonVersionFile::discover`. I thought the "fall back to global if nothing local is found" behavior was new, but it's not new.

---

_@oconnor663 approved on 2025-06-20 19:33_

---

_Merged by @zanieb on 2025-06-20 20:31_

---

_Closed by @zanieb on 2025-06-20 20:31_

---

_Branch deleted on 2025-06-20 20:31_

---
