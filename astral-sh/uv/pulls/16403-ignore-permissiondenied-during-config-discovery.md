```yaml
number: 16403
title: Ignore PermissionDenied during config discovery
type: pull_request
state: open
author: ahundt
labels:
  - bug
assignees: []
base: main
head: feat/graceful-permission-denied-in-config-discovery
created_at: 2025-10-22T01:46:11Z
updated_at: 2025-11-05T03:28:11Z
url: https://github.com/astral-sh/uv/pull/16403
synced_at: 2026-01-12T16:12:14Z
```

# Ignore PermissionDenied during config discovery

---

_@ahundt_

Add PermissionDenied to error handling patterns in user(), system(), and find() methods to prevent crashes when configuration files are inaccessible.

- Include PermissionDenied in existing I/O error patterns alongside NotFound and NotADirectory
- Provide helpful warnings when user, system, or workspace configuration files are inaccessible
- Continue config discovery traversal when workspace directories have permission issues
- Add test coverage comparing PermissionDenied warnings vs silent --no-config behavior
- Ensure consistent behavior across all configuration discovery methods

Resolves crashes that occurred when uv encountered permission denied errors while searching for configuration files in parent directories for which access is denied, and addresses inconsistencies between UV_NO_CONFIG and --no-config. 

This caused uv to fail when only given permission to access a specific subdirectory, uv tried to keep going up the dir tree then crashed when access was denied.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->



---

_Renamed from "fix(settings): handle PermissionDenied during config discovery" to "Ignore PermissionDenied during config discovery" by @konstin on 2025-10-23 10:59_

---

_Label `bug` added by @konstin on 2025-10-23 11:00_

---

_Comment by @charliermarsh on 2025-10-27 02:24_

Thanks! Do you have an MRE for uv hitting this issue when `UV_NO_CONFIG` is set or `--no-config` is provided? AFAICT, the changes here only affect cases in which those flags are _not_ set.

---

_Comment by @ahundt on 2025-10-28 01:14_

Hey @charliermarsh thanks for following up! I believe the change does affect both when both of the flags are variously set and not set, as stated in the description if it is run in a subdirectory where permission is allowed with a parent directory where permission is disallowed, and --no-config is set, uv will crash and it will not succeed prior to this fix. If I recall, --no-config didn't work properly and it tried to read the files, perhaps it still correctly didn't use them, but when permission is denied even a read or an attempted access of the file takes uv down and causes a crash. However, it is possible some aspects of those those details changed since I originally ran into the problem.

One easy way to test this on macos is use the macos app sandbox-exec cli and deny parent dir access but allow project dir access and run uv with and without the various flags. None of them should cause uv to crash, uv should be generally robust to valid permissions variations.

Also, I think the unit test included in the pull request should be a minimal reproduction, if you run the test on the main branch, I expect uv will fail the test and probably crash. If you run it with this patch, the test should pass and uv should succeed.

---

_Comment by @samypr100 on 2025-10-29 23:41_

> One easy way to test this on macos is use the macos app sandbox-exec cli and deny parent dir access but allow project dir access and run uv with and without the various flags. None of them should cause uv to crash, uv should be generally robust to valid permissions variations.

Is there an example / configuration you can provide using `sandbox-exec`? I'm curious what other things may not be behaving as expected under the sandbox constraints and what kind of support should be targeted on these type of environments.

---

_Comment by @ahundt on 2025-11-05 03:28_

Everything else I tried seemed to work so long as the no config flag was set. 

I don't have code ready to go I have a larger codebase in prep for potential later release but it isn't ready yet

---
