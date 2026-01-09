---
number: 12831
title: Use main branch instead of master when init project
type: issue
state: closed
author: DamyanBG
labels:
  - question
assignees: []
created_at: 2025-04-11T09:41:50Z
updated_at: 2025-04-11T10:11:53Z
url: https://github.com/astral-sh/uv/issues/12831
synced_at: 2026-01-07T13:12:18-06:00
---

# Use main branch instead of master when init project

---

_Issue opened by @DamyanBG on 2025-04-11 09:41_

### Summary

I think it would be good practice to use main branch, instead of master branch, when creating new project with uv.

### Example

_No response_

---

_Label `enhancement` added by @DamyanBG on 2025-04-11 09:41_

---

_Renamed from "User main branch instead of master when init project" to "Use main branch instead of master when init project" by @DamyanBG on 2025-04-11 09:42_

---

_Comment by @ptc-swalk on 2025-04-11 10:09_

I'm pretty sure it does whatever `git init` is configured to do - for me it does use `main` because my ~/.gitconfig has 
```
[init]
	defaultBranch = main

```

---

_Label `enhancement` removed by @konstin on 2025-04-11 10:09_

---

_Label `question` added by @konstin on 2025-04-11 10:09_

---

_Comment by @DamyanBG on 2025-04-11 10:11_

I agree with you, I thought it is from uv.

---

_Closed by @DamyanBG on 2025-04-11 10:11_

---
