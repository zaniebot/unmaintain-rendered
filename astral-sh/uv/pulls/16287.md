```yaml
number: 16287
title: "Fix: uv fails if requirements.txt files contain remote constraints"
type: pull_request
state: open
author: mlanett
labels: []
assignees: []
base: main
head: fix-remote-constraint-files
created_at: 2025-10-13T19:53:53Z
updated_at: 2025-11-23T20:01:24Z
url: https://github.com/astral-sh/uv/pull/16287
synced_at: 2026-01-10T05:58:11Z
```

# Fix: uv fails if requirements.txt files contain remote constraints

---

_Pull request opened by @mlanett on 2025-10-13 19:53_

## Summary

Fixes handling of remote constraint files in requirements.txt.

uv would fail with "Network connectivity is disabled" when trying to recompile a requirements.txt that contains a remote `--constraint` directive. This is required for AWS MWAA workflows where the constraint must be in the output file.

## Cause

When reading the output file to identify the existing constraints, uv operates in offline mode.
When it encounters a remote --constraint directive in the output file and tries to process it, because it was is offline mode, it will fail with "Network connectivity is disabled". This is a bug - we are not specifying offline mode.

## Fix

The fix is to skip the constraints file in this case because it is not needed for preferences.

This does not affect actual resolution because in true offline mode, the resolution will still fail.

## Test Code

There is a new test case `compile_with_remote_constraint_in_output`.
It sets up a mock server for the constraints file, a mock requirement, then compiles it.
It verifies a successful output which constains the constraint.
It adds the constraint to the output (as per MWAA), then compiles it again.
It verifies successful output again, which would have failed previously.


---

_Comment by @konstin on 2025-10-14 17:21_

Can you explain more about why we want this behavior? Intuitively, it seems wrong to ignore constraints files when we would otherwise read them.

---

_Converted to draft by @konstin on 2025-10-21 13:04_

---

_Comment by @mlanett on 2025-11-12 01:12_

My understanding is that uv reads constraints for two purposes:
- resolve dependencies
- read current dependencies

The bug is that when uv is reading current dependencies, it forces the client builder into offline mode. This is reasonable because it doesn't actually need to check any remote settings at this time. However if you have a remote constraints file, then it will generate a failure. The code is simply wrong for anyone who uses remote constraint files.

This fix is to skip a remote constraint files when in offline mode. Because they wouldn't get read ANYWAY, this can't be worse. What it does is to skip the failure. This has no effect on reading current dependencies (because they didn't need the remote constraints) but *fixes* the problem so people who have remote constraints no longer get a (false) failure.

For the first case "resolve dependencies", then this fix doesn't affect it. If someone has a remote constraint but runs in offline mode, it will still fail as expected. I believe install_constraints_respects_offline_mode at https://github.com/astral-sh/uv/blob/main/crates/uv/tests/it/pip_install.rs#L3650 verifies this already.

---

_Comment by @mlanett on 2025-11-12 01:13_

I think I should put this PR back into "ready" state.

---

_Marked ready for review by @mlanett on 2025-11-12 01:13_

---

_Renamed from "Fix: Support requirements.txt files which contain a remote constraint file" to "Fix: uv fails if requirements.txt files contain remote constraints" by @mlanett on 2025-11-23 19:59_

---
