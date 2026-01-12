```yaml
number: 17374
title: "fix: add Win32_Storage_FileSystem feature and improve handle closing logic"
type: pull_request
state: merged
author: zelosleone
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: fixinvalidhandlers
created_at: 2026-01-09T12:09:24Z
updated_at: 2026-01-10T19:48:49Z
url: https://github.com/astral-sh/uv/pull/17374
synced_at: 2026-01-12T02:26:33Z
```

# fix: add Win32_Storage_FileSystem feature and improve handle closing logic

---

_Pull request opened by @zelosleone on 2026-01-09 12:09_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

fix https://github.com/astral-sh/uv/issues/17174 mentioned the invalid issues here:

results of diagnostics:

before:
```
--- TRAMPOLINE DIAGNOSTICS: STARTUP ---
  STDIN: Handle=184
    -> Console Mode: 503
  STDOUT: Handle=536
    -> Console Mode: 7
  STDERR: Handle=540
    -> Console Mode: 7
-------------------------------------------
--- TRAMPOLINE DIAGNOSTICS: AFTER CLOSE_HANDLES ---
  STDIN: INVALID
  STDOUT: INVALID
  STDERR: Handle=540
    -> Console Mode: 7
```

after:
```
--- TRAMPOLINE DIAGNOSTICS: STARTUP ---
  STDIN: Handle=716
    -> Console Mode: 503
  STDOUT: Handle=580
    -> Console Mode: 7
  STDERR: Handle=604
    -> Console Mode: 7
-------------------------------------------
--- TRAMPOLINE DIAGNOSTICS: AFTER CLOSE_HANDLES ---
  STDIN: Handle=716
    -> Console Mode: 503
  STDOUT: Handle=580
    -> Console Mode: 7
  STDERR: Handle=604
    -> Console Mode: 7
```

the problem was we were closing the handlers whatever they were from pipes (byte streams) or consoles, this will make sure we close only the handlers from pipes by using `FILE_TYPE_PIPE` 

## Test Plan

<!-- How was it tested? -->
completely manual by adding debug statements and running a simple script to open the pdb.

---

_Review requested from @samypr100 by @konstin on 2026-01-09 12:09_

---

_Label `bug` added by @konstin on 2026-01-09 12:10_

---

_Label `windows` added by @konstin on 2026-01-09 12:10_

---

_Comment by @samypr100 on 2026-01-10 18:03_

this change seems reasonable and correct to me

---

_@zanieb approved on 2026-01-10 18:11_

---

_Merged by @zanieb on 2026-01-10 18:11_

---

_Closed by @zanieb on 2026-01-10 18:11_

---

_Branch deleted on 2026-01-10 19:48_

---
