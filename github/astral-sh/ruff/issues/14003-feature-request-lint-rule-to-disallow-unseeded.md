---
number: 14003
title: "Feature request: Lint rule to disallow unseeded RNGs (niche?)"
type: issue
state: open
author: dmcc
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-10-30T17:35:27Z
updated_at: 2024-11-07T14:38:53Z
url: https://github.com/astral-sh/ruff/issues/14003
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature request: Lint rule to disallow unseeded RNGs (niche?)

---

_Issue opened by @dmcc on 2024-10-30 17:35_

I'll preface that this lint rule would not be intended for many (maybe even most) use cases. I wouldn't expect it to be on by default or anything like that. I recognize this could be considered too niche so won't be offended if this isn't a good fit.

The use case
-------------
In some [AI/ML training work](https://datascience.stackexchange.com/questions/78109/should-you-use-random-state-or-random-seed-in-machine-learning-models), seeding your RNG is strongly encouraged to improve reproducibility. For cases where RNGs are used for ML and have no security implications, it would be nice to enable this.

Examples
----------
This would be an error:

    import random
    # [...]
    rng = random.Random()

as would:

    import random
    # [...]
    x = random.random()

while this would be fine:

    import random
    # [...]
    rng = random.Random(x=7)

(non-exhaustive list as we'd ideally cover popular library uses too)

---

_Comment by @dhruvmanila on 2024-11-07 04:57_

> seeding your RNG is strongly encouraged to improve reproducibility

I'm not sure I understand this. Should the rule enforce seeding if there's any random value being used? I'm not experienced in this area so can't really say how useful this rule would be. Regardless, I don't think this is possible until #1774 is completed.

---

_Label `rule` added by @dhruvmanila on 2024-11-07 04:57_

---

_Label `needs-decision` added by @dhruvmanila on 2024-11-07 04:57_

---

_Comment by @dmcc on 2024-11-07 14:38_

> Should the rule enforce seeding if there's any random value being used?

Yes, exactly. Somewhat counterintuitively, in ML development (but not ML production), it's helpful for numbers to _not_ be random for more repeatable experiments, easier debugging, etc. There are a good number of articles for ML development best practices suggesting folks seed their RNGs. Here's one which is part of the official [PyTorch documentation](https://pytorch.org/docs/stable/notes/randomness.html#pytorch-random-number-generator), the leading deep learning framework.

> Regardless, I don't think this is possible until https://github.com/astral-sh/ruff/issues/1774 is completed.

Yeah, makes total sense. I think most folks would be annoyed/confused if they enabled this rule unintentionally.

(and yes, I've considered writing this as a Python linter, but prefer to have it running faster in ruff...)

---
