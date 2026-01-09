---
number: 8023
title: Pandas boolean if statement correction bug 
type: issue
state: closed
author: Stas1beat
labels: []
assignees: []
created_at: 2023-10-17T19:58:13Z
updated_at: 2023-10-17T20:30:11Z
url: https://github.com/astral-sh/ruff/issues/8023
synced_at: 2026-01-07T13:12:15-06:00
---

# Pandas boolean if statement correction bug 

---

_Issue opened by @Stas1beat on 2023-10-17 19:58_

Currently Ruff changes the boolean if statement from the format `if <field> == True:` (or False). to the format `if <field> is True:`
This format doesn't work in Pandas, `if <field> is True:` always returns a False result.
Thank you!


---

_Comment by @zanieb on 2023-10-17 20:30_

Hi thanks for the report. I believe this is a duplicate of https://github.com/astral-sh/ruff/issues/4560 — let me know if your problem is not captured there!

---

_Closed by @zanieb on 2023-10-17 20:30_

---
