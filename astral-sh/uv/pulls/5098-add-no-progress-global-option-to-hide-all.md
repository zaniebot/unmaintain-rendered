```yaml
number: 5098
title: "Add `--no-progress` global option to hide all progress animations"
type: pull_request
state: merged
author: silvanocerza
labels:
  - cli
assignees: []
merged: true
base: main
head: no-progress-bar-option
created_at: 2024-07-16T09:31:05Z
updated_at: 2024-07-17T11:56:53Z
url: https://github.com/astral-sh/uv/pull/5098
synced_at: 2026-01-10T13:42:52Z
```

# Add `--no-progress` global option to hide all progress animations

---

_Pull request opened by @silvanocerza on 2024-07-16 09:31_

## Summary

Fixes #5082.

Adds a new `Printer::NoProgress` that is identical to `Printer::Default` but doesn't draw any progress bar.

## Test Plan

It seems to me that as of now it's not possible to use `insta-cmd` to get any progress bar in the comparable output of command.

Best way to test this would be to run any command that usually shows progress indicators like `uv pip install` with and without `--no-progress` options.

---

_Review requested from @ibraheemdev by @zanieb on 2024-07-16 14:44_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:159 on 2024-07-16 14:49_

Why conflict with quiet? Seems fine to request silence twice without erroring :)

---

_@zanieb reviewed on 2024-07-16 14:49_

---

_@zanieb reviewed on 2024-07-16 14:50_

The approach looks reasonable to me!

---

_@silvanocerza reviewed on 2024-07-16 15:34_

---

_Review comment by @silvanocerza on `crates/uv-cli/src/lib.rs`:159 on 2024-07-16 15:34_

Ah yes, it makes sense. I thought since it conflicts with `--verbose` it should also conflict with `--quiet`. 
I'll change that. 👍 

---

_Comment by @ibraheemdev on 2024-07-16 17:52_

Maybe `--no-progress` is a better name, because this hides all progress output, not just bars? We previously discussed an option that would hide the concurrent progress bars but still show a top-level progress indicator.

---

_Marked ready for review by @silvanocerza on 2024-07-16 21:19_

---

_@silvanocerza reviewed on 2024-07-16 21:21_

---

_Review comment by @silvanocerza on `crates/uv-cli/src/lib.rs`:159 on 2024-07-16 21:21_

I removed both in the end. `--verbose` doesn't show progress indicators already so it doesn't conflict, and `--quiet` doesn't show anything. 😁 

---

_Label `cli` added by @zanieb on 2024-07-16 21:48_

---

_Renamed from "Add `--no-progress-bar` global option to hide all progress bars" to "Add `--no-progress` global option to hide all progress animations" by @zanieb on 2024-07-16 21:48_

---

_@zanieb approved on 2024-07-16 21:48_

---

_Merged by @zanieb on 2024-07-16 21:48_

---

_Closed by @zanieb on 2024-07-16 21:48_

---

_Comment by @zanieb on 2024-07-16 21:49_

Thanks again!

---

_Branch deleted on 2024-07-17 11:56_

---
