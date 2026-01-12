```yaml
number: 11202
title: "Update bad shell assumptions from \"Shell autocompletion\" page"
type: pull_request
state: merged
author: Avasam
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-02-03T22:35:18Z
updated_at: 2025-02-05T23:52:24Z
url: https://github.com/astral-sh/uv/pull/11202
synced_at: 2026-01-12T16:09:43Z
```

# Update bad shell assumptions from "Shell autocompletion" page

---

_@Avasam_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- PowerShell isn't Windows-only
- Bash/Elvish isn't UNIX-only
- PowerShell profile won't necessarily exist
- Extracted the `echo $SHELL` tip to a tip tooltip

The new PowerShell lines are taken directly from https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#how-to-create-a-profile

Note that the "Standalone installer" commands are unaffected. Running the Linux one through PowerShell worked just fine.

## Test Plan

Look at the page generated from this PR
<!-- How was it tested? -->


---

_Renamed from "Remove bad shell assumptions from "Shell autocompletion" page" to "Update bad shell assumptions from "Shell autocompletion" page" by @Avasam on 2025-02-03 22:35_

---

_@zanieb approved on 2025-02-05 23:33_

---

_Merged by @zanieb on 2025-02-05 23:33_

---

_Closed by @zanieb on 2025-02-05 23:33_

---

_Comment by @zanieb on 2025-02-05 23:33_

Thanks!

---

_Label `documentation` added by @zanieb on 2025-02-05 23:33_

---

_Branch deleted on 2025-02-05 23:52_

---
