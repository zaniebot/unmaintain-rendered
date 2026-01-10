---
number: 21959
title: Slow startup
type: issue
state: open
author: lkpzxc
labels:
  - question
assignees: []
created_at: 2025-12-13T08:12:27Z
updated_at: 2025-12-15T07:11:20Z
url: https://github.com/astral-sh/ruff/issues/21959
synced_at: 2026-01-10T01:23:02Z
---

# Slow startup

---

_Issue opened by @lkpzxc on 2025-12-13 08:12_

### Question

Windows startup takes more than 2 seconds
ruff --version

### Version

_No response_

---

_Label `question` added by @lkpzxc on 2025-12-13 08:12_

---

_Comment by @MichaReiser on 2025-12-15 07:10_

Hmm, curious. Can you say a little more about your setup:

* What ruff version are you using
* How did you install ruff?
* What specific command takes 2s to start up? 
* Can you try running with the verbose flag `ruff check -v` 
* Do you use any antivirus software that might take a long time to scan ruff?

---
