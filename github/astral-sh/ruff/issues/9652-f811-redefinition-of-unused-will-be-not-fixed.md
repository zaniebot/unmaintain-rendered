---
number: 9652
title: "\"F811 redefinition of unused\" will be not fixed"
type: issue
state: closed
author: jedie
labels:
  - question
  - preview
assignees: []
created_at: 2024-01-26T15:56:37Z
updated_at: 2024-01-26T16:19:07Z
url: https://github.com/astral-sh/ruff/issues/9652
synced_at: 2026-01-07T13:12:15-06:00
---

# "F811 redefinition of unused" will be not fixed

---

_Issue opened by @jedie on 2024-01-26 15:56_

ruff 0.1.14

e.g.:
```
from sys import version_info
from sys import version_info
from sys import version_info
from sys import version_info
from sys import version_info


print(version_info)
```
is valid for ruff and `ruff format` will not fix: **F811 redefinition of unused** but F ist active.

---

_Comment by @charliermarsh on 2024-01-26 15:58_

This exists in preview mode. If you run `ruff check --fix --preview`, it will remove redefined imports.

(`ruff format` doesn't remove redefined imports, since the formatter intends not to change the syntax tree / meaning of the code.)

---

_Closed by @charliermarsh on 2024-01-26 15:58_

---

_Label `question` added by @charliermarsh on 2024-01-26 15:58_

---

_Label `preview` added by @charliermarsh on 2024-01-26 15:58_

---

_Comment by @jedie on 2024-01-26 16:00_

Thanks for the fast feedback.

I'm confused ;)

I have select `ALL` and for me it seems that F811 is enabled:

![grafik](https://github.com/astral-sh/ruff/assets/71315/52d07a8d-349b-42ad-a89e-a0b9caff0902)

Where can i find the rules that are only available in preview mode?!?

---

_Comment by @charliermarsh on 2024-01-26 16:05_

Yeah, it'll be enabled on stable in a later release. (Hopefully in the next few weeks.)

---

_Comment by @jedie on 2024-01-26 16:07_

I use v0.1.14 and the screenshot is made from this version.

---

_Comment by @charliermarsh on 2024-01-26 16:09_

Ah yeah. The rule exists in preview and on stable, but the automatic _fix_ is only available in preview right now. The docs don't reflect that specific nuance (that the rule exists, but the fix requires preview).

---

_Comment by @jedie on 2024-01-26 16:10_

OK. Thanks.

---

_Comment by @jedie on 2024-01-26 16:10_

Sorry... Even with `--preview` it's not fixed...
Hm. Maybe my test setup is wrong. I will doublecheck.

EDIT: OK it worked with: `ruff check --isolated --fix --preview`

---
