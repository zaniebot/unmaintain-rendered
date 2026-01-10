```yaml
number: 106
title: Support prepare_metadata_for_build_wheel
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: prepare_metadata_for_build_wheel
created_at: 2023-10-16T17:20:29Z
updated_at: 2023-10-18T12:48:32Z
url: https://github.com/astral-sh/uv/pull/106
synced_at: 2026-01-10T15:50:28Z
```

# Support prepare_metadata_for_build_wheel

---

_Pull request opened by @konstin on 2023-10-16 17:20_

Support calling `prepare_metadata_for_build_wheel`, which can give you the metadata without executing the actual build if the backend supports it.

This makes the code a lot uglier since we effectively have a state machine:

* Setup: Either venv plus requires (PEP 517) or just a venv (setup.py)
* Get metadata (optional step): None (setup.py) or `prepare_metadata_for_build_wheel` and saving that result
* Build: `setup.py`, `build_wheel()` or `build_wheel(metadata_directory=metadata_directory)`, but i think i got general flow right.

@charliermarsh This is a "barely works but unblocks building on top" implementation, say if you want more polishing (i'll look at this again tomorrow)

---

_Comment by @charliermarsh on 2023-10-16 17:23_

Sounds good, I'll take a look at this today.

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:69 on 2023-10-17 15:20_

Not: I'd make this a struct, I'm not sure what the fields are.

---

_@charliermarsh approved on 2023-10-17 15:21_

---

_Comment by @konstin on 2023-10-18 10:29_

I restart CI and it still failed before even starting, not sure what's wrong with it

---

_Comment by @charliermarsh on 2023-10-18 12:39_

We ran out of free private build minutes hah. I either need to pay for them or turn off CI for now.

---

_Merged by @konstin on 2023-10-18 12:48_

---

_Closed by @konstin on 2023-10-18 12:48_

---

_Branch deleted on 2023-10-18 12:48_

---
