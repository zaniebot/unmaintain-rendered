```yaml
number: 4409
title: "feat: pythonw support on gui scripts"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: pythonw-search
created_at: 2024-06-19T00:46:16Z
updated_at: 2024-06-23T15:12:34Z
url: https://github.com/astral-sh/uv/pull/4409
synced_at: 2026-01-10T13:48:28Z
```

# feat: pythonw support on gui scripts

---

_Pull request opened by @samypr100 on 2024-06-19 00:46_

## Summary

Closes https://github.com/astral-sh/uv/issues/2956

This changes the bootstrap launcher script to use `pythonw.exe` instead of `python.exe` on `gui_scripts` via a helper fn both in the shebang and the python exe path encoded before `UVUV` magic, that way uv-trampoline's `find_python_exe` can use the right pythonw executable.

## Test Plan

New unit tests for the helper was added.
Tested on example from #2956 on Windows to make sure it works as expected.

## Questions

I noticed the docs in `fn windows_script_launcher` says ```The launcher will look for `python[w].exe` adjacent to it in the same directory to start the embedded script.``` but I didn't find such functionality when I looked in uv-trampoline.
I only saw `clear_app_starting_state` getting called when `is_gui` is set.

Was the intention to do this in uv-trampoline at some point instead?

---

_Review requested from @konstin by @zanieb on 2024-06-19 00:51_

---

_Marked ready for review by @samypr100 on 2024-06-19 01:35_

---

_@konstin approved on 2024-06-23 09:39_

---

_Comment by @konstin on 2024-06-23 09:41_

> I noticed the docs in fn windows_script_launcher says The launcher will look for `python[w].exe` adjacent to it in the same directory to start the embedded script. but I didn't find such functionality when I looked in uv-trampoline.

Thanks, that's indeed wrong, i removed it.

---

_Merged by @konstin on 2024-06-23 09:48_

---

_Closed by @konstin on 2024-06-23 09:48_

---

_Branch deleted on 2024-06-23 13:42_

---

_Review comment by @samypr100 on `crates/install-wheel-rs/src/wheel.rs`:1 on 2024-06-23 13:50_

Good catch, thanks. I forgot is_file already checks file metadata.

---

_@samypr100 reviewed on 2024-06-23 13:50_

---

_Comment by @charliermarsh on 2024-06-23 14:54_

Nice, thank you for this @samypr100!

---

_Label `enhancement` added by @zanieb on 2024-06-23 15:12_

---
