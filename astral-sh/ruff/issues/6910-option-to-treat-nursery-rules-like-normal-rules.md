---
number: 6910
title: option to treat nursery rules like normal rules
type: issue
state: closed
author: DetachHead
labels:
  - cli
assignees: []
created_at: 2023-08-27T01:22:46Z
updated_at: 2023-09-13T02:23:07Z
url: https://github.com/astral-sh/ruff/issues/6910
synced_at: 2026-01-10T01:22:46Z
---

# option to treat nursery rules like normal rules

---

_Issue opened by @DetachHead on 2023-08-27 01:22_

i just found out that there are a bunch of rules that i thought i was using because i enabled the category they were in, and i didn't know about the nursery.

it would be nice if there was an option to treat these like regular rules so i don't have to explicitly enable them all. (i prefer when linters have everything enabled by default, then i can just easily disable ones that i don't like)

---

_Comment by @DetachHead on 2023-08-27 02:00_

leaving this here as a quick and easy way to get a list of all the nursery rules:
```js
Array.from($x("//tr[.//span[.='🌅' and @style='opacity: 1']]/td[@id]")).map(element => element.innerText)
```

run this in the devtools console on https://beta.ruff.rs/docs/rules/

---

_Comment by @charliermarsh on 2023-08-27 13:45_

\cc @zanieb - would be solved by a preview-like mode.

---

_Label `cli` added by @charliermarsh on 2023-08-27 13:45_

---

_Comment by @zanieb on 2023-08-27 15:28_

Yep this is on our roadmap, thanks for raising though!

---

_Assigned to @zanieb by @zanieb on 2023-08-27 15:28_

---

_Comment by @DetachHead on 2023-09-12 23:14_

looks like the new `--preview` flag does this https://github.com/astral-sh/ruff/releases/tag/v0.0.289

---

_Closed by @DetachHead on 2023-09-12 23:14_

---

_Comment by @zanieb on 2023-09-13 02:23_

Thanks for the bookkeeping! We're going to continue working on `--preview`, it got released early with some formatter fixes :)

---
