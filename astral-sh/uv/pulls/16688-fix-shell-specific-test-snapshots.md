```yaml
number: 16688
title: Fix shell-specific test snapshots
type: pull_request
state: merged
author: zsol
labels:
  - testing
assignees: []
merged: true
base: main
head: zsol/jj-otzultwkyovu
created_at: 2025-11-11T18:37:42Z
updated_at: 2025-11-12T13:55:36Z
url: https://github.com/astral-sh/uv/pull/16688
synced_at: 2026-01-12T16:12:23Z
```

# Fix shell-specific test snapshots

---

_@zsol_

## Summary

Two tests were failing when run with `SHELL=fish`:
1. `create_venv_current_working_directory` failed because the "activate" filter didn't apply properly when the venv was in the CWD. This PR fixes the filter.
2. `tool_install_warn_path` failed because the messages are different between fish and bash. This PR hardcodes `SHELL=fish`.

## Test Plan

CI

---

_Review requested from @konstin by @zsol on 2025-11-11 18:38_

---

_Review requested from @zanieb by @zsol on 2025-11-11 18:38_

---

_Marked ready for review by @zsol on 2025-11-11 18:49_

---

_Review comment by @konstin on `crates/uv/tests/it/venv.rs`:1717 on 2025-11-11 19:03_

How did this fail when we had `.arg(".")`?

---

_@konstin reviewed on 2025-11-11 19:04_

---

_@zanieb reviewed on 2025-11-11 19:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:2851 on 2025-11-11 19:30_

Why fish? Why not bash?

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:2851 on 2025-11-11 19:31_

Should we just set a default value for `SHELL` in the test context?

---

_@zanieb reviewed on 2025-11-11 19:31_

---

_@zsol reviewed on 2025-11-11 19:47_

---

_Review comment by @zsol on `crates/uv/tests/it/tool_install.rs`:2851 on 2025-11-11 19:47_

fish is clearly more interesting :) in all seriousness though, I don't have a strong opinion between:
1. the test has more value if it exercises cases that we wouldn't normally catch (we'd never ever manually test this)
2. the test has more value if it exercises the vastly more popular code path

Do you have a preference? 

---

_@zsol reviewed on 2025-11-11 19:48_

---

_Review comment by @zsol on `crates/uv/tests/it/venv.rs`:1717 on 2025-11-11 19:48_

The filter regex didn't match this because it requires a `/` before `bin`, which in this case isn't there (uv chooses to display this as `bin/activate` instead of `./bin/activate`).

---

_@zanieb reviewed on 2025-11-11 19:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:2851 on 2025-11-11 19:49_

If we want to test alternative shells, it should be in a test that's focused on those. I don't think a test that's focusing on capturing the general behavior of tool install warnings should use a non-default / uncommon code-path. so, I guess (2)!

---

_@zanieb approved on 2025-11-11 20:17_

---

_Label `testing` added by @zsol on 2025-11-12 11:32_

---

_@konstin approved on 2025-11-12 12:27_

---

_Merged by @zanieb on 2025-11-12 13:55_

---

_Closed by @zanieb on 2025-11-12 13:55_

---

_Branch deleted on 2025-11-12 13:55_

---
