---
number: 8054
title: Incorrect/Confusing resolution failure message when constraint conflicts with dependency
type: issue
state: open
author: notatallshaw-gts
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-10-09T19:26:00Z
updated_at: 2024-10-09T21:31:40Z
url: https://github.com/astral-sh/uv/issues/8054
synced_at: 2026-01-10T01:24:23Z
---

# Incorrect/Confusing resolution failure message when constraint conflicts with dependency

---

_Issue opened by @notatallshaw-gts on 2024-10-09 19:26_

**Environment**: uv 0.4.20 / Linux

**Example message**:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because click-extra==4.11.0 depends on tabulate>=0.9,<1.dev0 and tabulate==0.8.10, we can conclude that click-extra==4.11.0 cannot be used.
      And because only click-extra<=4.11.0 is available and you require click-extra>=4.11.0, we can conclude that your requirements are unsatisfiable.
```

From this message it appears that "click-extra==4.11.0 depends on tabulate>=0.9,<1.dev0 and tabulate==0.8.10", but this isn't true, `tabulate==0.8.10` was a constraint, click-extra does not have that requirement.

**Steps to reproduce**:
1. Create a `constraints.txt` file with the following content:
```
tabulate==0.8.10
```
2. Run: `uv pip install "click-extra>=4.11.0" -c constraints.txt`


---

_Label `error messages` added by @zanieb on 2024-10-09 19:28_

---

_Comment by @zanieb on 2024-10-09 19:28_

Thanks!

---

_Label `bug` added by @zanieb on 2024-10-09 19:28_

---

_Comment by @zanieb on 2024-10-09 19:29_

The derivation tree looks like this

```
Resolver derivation tree before reduction
  root==0a0.dev0 depends on click-extra>=4.11.0
      click-extra==4.11.0 depends on tabulate==0.8.10
      click-extra==4.11.0 depends on tabulate>=0.9, <1.dev0
    no versions of click-extra>4.11.0
Resolver derivation tree after reduction
  root==0a0.dev0 depends on click-extra>=4.11.0
      click-extra==4.11.0 depends on tabulate==0.8.10
      click-extra==4.11.0 depends on tabulate>=0.9, <1.dev0
    no versions of click-extra>4.11.0
```

Looks like we're tracking this wrong in resolver, not reporting it incorrectly in the formatter.

---

_Comment by @zanieb on 2024-10-09 19:38_

I think this is just how constraints are modeled in the resolver. I'm not sure how hard that would be to change.

@BurntSushi or @charliermarsh would one of you be able to weigh in on this?

---

_Comment by @charliermarsh on 2024-10-09 19:51_

Yeah this is sort of inherent to how we model constraints. Perhaps possible to recognize this in our error reporter but seems hard.

---

_Comment by @notatallshaw-gts on 2024-10-09 20:02_

Ah, well, obviously not high priority by the way I never noticed or reported this before, even though  I have giant constraints files and this error isn't too unusual but I somehow missed this (actually I thought this was a regression when I saw this today, but I just grabbed a fairly old version of uv and same message).

But pip gives a more useful error here:

```
ERROR: Cannot install click-extra==4.11.0 because these package versions have conflicting dependencies.

The conflict is caused by:
    click-extra 4.11.0 depends on tabulate~=0.9
    The user requested (constraint) tabulate==0.8.10
```

---

_Comment by @notatallshaw-gts on 2024-10-09 20:10_

Similarly, I guess the fact the error says "click-extra==4.11.0 depends on tabulate>=0.9,<1.dev0" is also slightly confusing, as the verbatim dependency is "tabulate~=0.9".

I guess it would be nice if the error to the user mapped back to the real requirements given and said where they came from. But I can imagine that might take a lot of extra work for a fairly minor benefit.

---

_Comment by @zanieb on 2024-10-09 21:31_

We can definitely fix display of `~=` specifier bounds

---
