---
number: 7709
title: Consider removing ANN101
type: issue
state: closed
author: charliermarsh
labels:
  - needs-decision
assignees: []
created_at: 2023-09-29T12:04:03Z
updated_at: 2024-02-01T19:35:10Z
url: https://github.com/astral-sh/ruff/issues/7709
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider removing ANN101

---

_Issue opened by @charliermarsh on 2023-09-29 12:04_

Note to self, to fill in when I’m back at my laptop.

---

_Label `needs-decision` added by @charliermarsh on 2023-09-29 12:04_

---

_Comment by @cosmojg on 2023-10-06 21:05_

Is ANN102 included in this discussion?

---

_Comment by @NeilGirdhar on 2023-10-20 21:27_

Why remove ANN101 and ANN102?

But if anything, at least don't trigger on the first argument of an ordinary or class method (typically called "self" or "cls").  These have implicit types: `typing.Self` and `type[typing.Self]`.

---

_Comment by @charliermarsh on 2023-10-20 21:35_

ANN101 and ANN102 only affect `self` and `cls` respectively, right?

---

_Comment by @NeilGirdhar on 2023-10-20 21:37_

I thought that they check all of the arguments?  "Checks that function arguments have type annotations."

---

_Comment by @charliermarsh on 2023-10-20 22:06_

It's broken into multiple rules, so ANN101 only checks `self`. ANN001 checks arbitrary arguments: https://docs.astral.sh/ruff/rules/missing-type-function-argument/.

---

_Comment by @NeilGirdhar on 2023-10-20 22:29_

Ohhh, my mistake.   I confused ANN001 with ANN101.  Yes, please get rid of ANN101 and 102 😄 

---

_Referenced in [astral-sh/ruff#9461](../../astral-sh/ruff/issues/9461.md) on 2024-01-11 01:22_

---

_Comment by @KotlinIsland on 2024-01-12 03:38_

> Note to self, to fill in when I’m back at my laptop.

Instead of a Note to `self`, could we add an annotation to `self`?

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Comment by @zanieb on 2024-01-30 01:23_

See #9689 which currently deprecate these rules

---

_Referenced in [astral-sh/ruff#9680](../../astral-sh/ruff/pulls/9680.md) on 2024-01-30 19:18_

---

_Closed by @zanieb on 2024-02-01 19:35_

---

_Referenced in [astral-sh/ruff#4396](../../astral-sh/ruff/issues/4396.md) on 2024-02-01 22:33_

---
