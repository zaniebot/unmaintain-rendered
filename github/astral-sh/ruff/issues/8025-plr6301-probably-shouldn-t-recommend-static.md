---
number: 8025
title: "PLR6301 probably shouldn't recommend static methods"
type: issue
state: closed
author: NeilGirdhar
labels:
  - documentation
assignees: []
created_at: 2023-10-17T21:16:59Z
updated_at: 2023-10-26T19:03:54Z
url: https://github.com/astral-sh/ruff/issues/8025
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR6301 probably shouldn't recommend static methods

---

_Issue opened by @NeilGirdhar on 2023-10-17 21:16_

If I remember correctly, Guido once suggested that adding static methods to Python was a mistake.  Whether or not that's apocryphal, I do think that Ruff shouldn't be recommending converting methods to static methods, which this rule does.

Static methods are often preferred to bare functions because they enclose the name in the class namespace.  And they're preferred to ordinary methods when you want to be able to call them without a reference to self.  The problem with static methods is that they _do not respect polymorphism when they are overridden in subclasses_.  Thus, I think they should usually be avoided and classmethods should be preferred.

Suggestion for rule PLR6301: Change the message from "could be a function or static method" to either:

* "could be a function or class method"
* "could be a function, static method, or class method"

In Ruff's documentation, it might be helpful to provide guidance.  (After all, Ruff makes us all better programmers with its gentle nudges, why not provide guidance too?)  We can highlight general principles to pick each solution:

* class methods should be preferred if they may be overridden in subclasses (since bare and static methods do not respect polymophic calls),
* otherwise bare functions are preferable for most functions,
* in rare cases where the function belongs in the class namespace, then static methods are acceptable.

I will probably disable the rule regardless since I tend to prefer ordinary methods for maximum flexibility when inheriting, and use classmethods mainly for factories and for methods called by factories.

---

_Referenced in [astral-sh/ruff#7993](../../astral-sh/ruff/issues/7993.md) on 2023-10-17 21:28_

---

_Label `documentation` added by @charliermarsh on 2023-10-20 23:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 23:07_

---

_Referenced in [astral-sh/ruff#8258](../../astral-sh/ruff/pulls/8258.md) on 2023-10-26 17:16_

---

_Closed by @charliermarsh on 2023-10-26 19:03_

---

_Referenced in [wildlife-dynamics/ecoscope#467](../../wildlife-dynamics/ecoscope/pulls/467.md) on 2025-04-25 17:19_

---
