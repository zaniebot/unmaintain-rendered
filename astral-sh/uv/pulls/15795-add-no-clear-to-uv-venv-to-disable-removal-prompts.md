```yaml
number: 15795
title: "Add `--no-clear` to `uv venv` to disable removal prompts"
type: pull_request
state: merged
author: harshithvh
labels:
  - cli
assignees: []
merged: true
base: main
head: hvh/add-no-clear-option
created_at: 2025-09-11T16:43:19Z
updated_at: 2025-09-16T13:24:57Z
url: https://github.com/astral-sh/uv/pull/15795
synced_at: 2026-01-12T16:11:56Z
```

# Add `--no-clear` to `uv venv` to disable removal prompts

---

_@harshithvh_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Closes #15485 

---

_Review requested from @zanieb by @konstin on 2025-09-11 18:53_

---

_@zanieb reviewed on 2025-09-12 11:14_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:961 on 2025-09-12 11:14_

Why did this behavior change?

---

_@zanieb reviewed on 2025-09-12 11:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:1005 on 2025-09-12 11:15_

I don't think any of the existing behaviors should change here

---

_@harshithvh reviewed on 2025-09-12 11:19_

---

_Review comment by @harshithvh on `crates/uv/tests/it/venv.rs`:961 on 2025-09-12 11:19_

there was a conflict, checking...

---

_Comment by @harshithvh on 2025-09-12 14:55_

@zanieb, lmk if this looks good

---

_@zanieb reviewed on 2025-09-12 15:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 15:34_

This should not change

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 15:34_

Right?

---

_@zanieb reviewed on 2025-09-12 15:34_

---

_@harshithvh reviewed on 2025-09-12 15:49_

---

_Review comment by @harshithvh on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 15:49_

The change in the `non_empty_dir_exists_allow_existing` test output wasn’t related to the addition of the `--no-clear` flag. Instead, it was needed to bring the test in line with upstream changes: the behavior of `--allow-existing` on a non-empty directory was updated to succeed with a warning (instead of failing), so I updated the test output accordingly after merging with upstream.

---

_@zanieb reviewed on 2025-09-12 17:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 17:25_

`--allow-existing` isn't used in this invocation though?

---

_Review comment by @harshithvh on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 17:35_

it is used in the `non_empty_dir_exists_allow_existing`
Check the comment

---

_@harshithvh reviewed on 2025-09-12 17:35_

---

_@zanieb reviewed on 2025-09-12 19:00_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 19:00_

It is not used in this command, where the snapshot has changed.

I don't know how else to say I don't think these tests should be changing. We should only be seeing a diff for tests with `--no-clear` in them. 

---

_@zanieb reviewed on 2025-09-12 19:00_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 19:00_

It is not used in this command, where the snapshot has changed.

I don't know how else to say I don't think these tests should be changing. We should only be seeing a diff for tests with `--no-clear` in them. 

---

_@harshithvh reviewed on 2025-09-12 20:52_

---

_Review comment by @harshithvh on `crates/uv/tests/it/venv.rs`:1022 on 2025-09-12 20:52_

found the cause, this is fixed!

---

_Comment by @harshithvh on 2025-09-15 16:50_

@zanieb, it would be great if you could review this when you have time
thanks!

---

_Comment by @zanieb on 2025-09-16 13:02_

I made some changes in https://github.com/astral-sh/uv/pull/15795/commits/29ca6808f5452601af553b0af3dec22faaa2d141

It ended up feeling less complicated to me to add another variant.

---

_@zanieb approved on 2025-09-16 13:03_

---

_Label `cli` added by @zanieb on 2025-09-16 13:03_

---

_Renamed from "Add no clear option for uv venv" to "Add `--no-clear` to `uv venv` to disable removal prompts" by @zanieb on 2025-09-16 13:04_

---

_Merged by @zanieb on 2025-09-16 13:24_

---

_Closed by @zanieb on 2025-09-16 13:24_

---
