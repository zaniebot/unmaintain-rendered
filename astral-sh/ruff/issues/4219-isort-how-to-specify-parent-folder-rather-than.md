---
number: 4219
title: "isort: how to specify parent folder rather than all children folders in \"known-x\" rule?"
type: issue
state: open
author: ddahan
labels:
  - isort
  - wish
assignees: []
created_at: 2023-05-04T09:45:00Z
updated_at: 2023-07-10T01:21:23Z
url: https://github.com/astral-sh/ruff/issues/4219
synced_at: 2026-01-10T01:22:43Z
---

# isort: how to specify parent folder rather than all children folders in "known-x" rule?

---

_Issue opened by @ddahan on 2023-05-04 09:45_

Hi, I'm trying to organise my imports using ruff.

my `dj_apps` folder has multiple apps like `app1`, `app2`, etc.
How can I specify the `known-first-party` setting to tell  "all folders inside dj_apps"?

I tried:
- `known-first-party = ["dj_apps"]` -> no effect
- `known-first-party = ["dj_apps/*"]` -> no effect

The only thing which currently work is `known-first-party = ["app1", "app2"]`
This is not very handy as I should remember to edit my Ruff config as soon as I get a new django app.
Thanks! 

---

_Comment by @charliermarsh on 2023-05-04 15:52_

Unfortunately I don't think there's a way to do this right now, since `dj_apps` isn't actually part of the module path (I'm guessing?). E.g., when you import these apps, I'm assuming you're doing `from app1 import foo` rather than `from dj_apps.app1 import foo`, is that right?


---

_Label `question` added by @charliermarsh on 2023-05-04 15:52_

---

_Label `isort` added by @charliermarsh on 2023-05-04 15:52_

---

_Comment by @ddahan on 2023-05-04 16:17_

Yes exactly!

---

_Comment by @wjzhou-ep on 2023-06-07 15:40_

I have a similar problem.

is it ok to add an extra parameter or special process when there are stars?

e.g. 
if known-first-party="dj_apps/*", when ruff starts, it check the sub folder of dj_apps/* and add them as known-first-party
 

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:21_

---

_Label `wish` added by @charliermarsh on 2023-07-10 01:21_

---
