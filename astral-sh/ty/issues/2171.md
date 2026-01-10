```yaml
number: 2171
title: emit a diagnostic on unsafe dunder method overrides on tuple subclasses
type: issue
state: open
author: carljm
labels:
  - help wanted
  - unsoundness
assignees: []
created_at: 2025-12-22T19:50:43Z
updated_at: 2025-12-22T23:40:18Z
url: https://github.com/astral-sh/ty/issues/2171
synced_at: 2026-01-10T01:56:41Z
```

# emit a diagnostic on unsafe dunder method overrides on tuple subclasses

---

_Issue opened by @carljm on 2025-12-22 19:50_

We do (or want to do) various kinds of narrowing on tuples (see #215, #560, #2140), but the `tuple` type includes tuple subclasses, so making these narrowings sound requires assuming that tuple subclasses don't override `__len__`, `__bool__`, or `__eq__`. We should emit a diagnostic if they do.

---

_Label `help wanted` added by @carljm on 2025-12-22 19:50_

---

_Label `unsoundness` added by @carljm on 2025-12-22 19:50_

---

_Comment by @MatthewMckee4 on 2025-12-22 22:41_

Would this fall under `invalid-method-override`, or do you think a new rule should be proposed?

I would think that due to the working of why we emit that diagnostic, we would emit a different one here.

---

_Comment by @carljm on 2025-12-22 22:45_

Yes, I think a different rule specific to this tuple case.

---

_Comment by @MatthewMckee4 on 2025-12-22 22:47_

Should the rule be constrained to just tuples, something like `unsafe-dunder-method-override`.

Would we want this rule to cover other types too?

---

_Comment by @carljm on 2025-12-22 23:40_

I don't at the moment have ideas of other cases where we'd want to apply such a rule. I think it's probably better if the rule is clear that it's specific to tuples, something like `unsafe-tuple-subclass`. And probably just a warning?

---
