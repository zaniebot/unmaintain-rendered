```yaml
number: 11457
title: Update README.md to use WinGet as the default installation method
type: pull_request
state: closed
author: localden
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-02-12T18:08:41Z
updated_at: 2025-02-12T18:16:05Z
url: https://github.com/astral-sh/uv/pull/11457
synced_at: 2026-01-10T11:10:38Z
```

# Update README.md to use WinGet as the default installation method

---

_Pull request opened by @localden on 2025-02-12 18:08_

## Summary

Addresses #11456 - helps avoid the scenarios where users are conditioned to run arbitrary scripts.

## Test Plan

N/A - change to README/documentation.


---

_Comment by @zanieb on 2025-02-12 18:13_

Sorry, we won't recommend alternative installers on the README. We have https://docs.astral.sh/uv/getting-started/installation/#winget

I understand the concerns about conditioning users, but the WinGet package is not Astral maintained and we cannot vouch for it. Similarly, without control of the installer we cannot improve the user experience.

---

_Closed by @zanieb on 2025-02-12 18:13_

---

_Comment by @localden on 2025-02-12 18:16_

@zanieb how can I help maintain an official `winget` package? For Windows, `winget` should now be the default package manager, and behind the scenes it can effectively do what your PowerShell script does.

---
