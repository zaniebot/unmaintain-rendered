---
number: 15774
title: "Add rule to forbid `asyncio.create_task`"
type: issue
state: open
author: Tinche
labels:
  - rule
  - wish
assignees: []
created_at: 2025-01-27T17:46:11Z
updated_at: 2025-01-27T18:34:39Z
url: https://github.com/astral-sh/ruff/issues/15774
synced_at: 2026-01-07T13:12:16-06:00
---

# Add rule to forbid `asyncio.create_task`

---

_Issue opened by @Tinche on 2025-01-27 17:46_

### Description

I'd like to enforce structured concurrency in my codebases, and a rule to forbid `asyncio.create_task` would be very helpful, since it would enforce all tasks are managed through TaskGroups.

This is likely to be controversial and spawn false positives so I think it should probably be disabled by default. Obviously it's possible to achieve structured concurrency even with `asyncio.create_task`, but I would argue you'd just be reimplementing TaskGroups.

Still, for some codebases I think a rule like this would be very useful.

---

_Comment by @AlexWaygood on 2025-01-27 18:34_

I can see the appeal. Unfortunately I think this is probably much too opinionated to be done until we recategorize our ruleset so that we can better distinguish highly opinionated, restrictive rules like this one from rules that nearly everybody would probably want enabled :( See https://github.com/astral-sh/ruff/issues/1774 for the recategorisation issue

---

_Label `rule` added by @AlexWaygood on 2025-01-27 18:34_

---

_Label `wish` added by @AlexWaygood on 2025-01-27 18:34_

---
