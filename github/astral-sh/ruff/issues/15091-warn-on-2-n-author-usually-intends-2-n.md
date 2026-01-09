---
number: 15091
title: "Warn on `2 ^ n` - author usually intends `2 ** n`"
type: issue
state: open
author: Zac-HD
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-12-21T08:17:30Z
updated_at: 2024-12-21T13:40:20Z
url: https://github.com/astral-sh/ruff/issues/15091
synced_at: 2026-01-07T13:12:16-06:00
---

# Warn on `2 ^ n` - author usually intends `2 ** n`

---

_Issue opened by @Zac-HD on 2024-12-21 08:17_

It's pretty rare for people to *actually intend* to do bitwise-or with constants; making this a lint error when the "base" is either 2 or 10 would likely save quite a few people some frustrating debugging (or silent errors)

---

_Label `rule` added by @MichaReiser on 2024-12-21 13:38_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-21 13:39_

---

_Comment by @MichaReiser on 2024-12-21 13:40_

Thanks for opening this rule proposal. 

I think we have to wait before #1774 is complete to add this rule because it is a "restriction" lint and somewhat opinionated. 

---
