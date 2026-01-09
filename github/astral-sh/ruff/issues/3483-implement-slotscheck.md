---
number: 3483
title: Implement slotscheck
type: issue
state: open
author: Goldziher
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-03-13T16:11:06Z
updated_at: 2025-06-12T17:09:24Z
url: https://github.com/astral-sh/ruff/issues/3483
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement slotscheck

---

_Issue opened by @Goldziher on 2023-03-13 16:11_

The [https://github.com/ariebovenberg/slotscheck](slotscheck) tool is pretty useful. We use it at Starlite to ensure all subclasses implement slots correctly. 

It would be great to have this functionality in Ruff directly. 

Additionally it would be great to have a linters rule that check if an attribute is not declared in slots when it should be declared, or should be removed from slots because it has an initializer (default value). 

---

_Label `plugin` added by @charliermarsh on 2023-03-13 16:38_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @Goldziher on 2025-06-12 16:57_

I'll re-ping on this one. 

It's a very useful linter whenever slots are used, which is often in sophisticated code bases. 

@charliermarsh 

---
