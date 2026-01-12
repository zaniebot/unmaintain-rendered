```yaml
number: 14387
title: "Rule proposal: require explicit `strict=` argument for `itertools.batched`"
type: issue
state: closed
author: tjkuson
labels:
  - rule
assignees: []
created_at: 2024-11-16T20:29:01Z
updated_at: 2024-12-10T08:39:48Z
url: https://github.com/astral-sh/ruff/issues/14387
synced_at: 2026-01-12T15:54:53Z
```

# Rule proposal: require explicit `strict=` argument for `itertools.batched`

---

_@tjkuson_

[As of Python 3.13](https://docs.python.org/3/library/itertools.html#itertools.batched), `itertools.batched` has a `strict` parameter that defaults to `False`.  By default, the batches might not be of the same size, which may cause subtle bugs. When`strict=True`, it raises `ValueError` if the final batch is not the same size as the rest.

This seems analogous to the existing rule `B905` which requires an explicit `strict=` parameter for `zip`. Hence, I think it makes sense if there is a similar rule for `itertools.batched`.

I also created an [issue](https://github.com/PyCQA/flake8-bugbear/issues/498) `flake8-bugbear` which seems like a natural place for the rule given the similarity to `B905`.

Searched keywords: `itertools.batched`, `itertools`, `batched`

---

_Label `rule` added by @AlexWaygood on 2024-11-16 22:41_

---

_Comment by @InSyncWithFoo on 2024-11-17 02:25_

I'll take this on.

---

_Comment by @MichaReiser on 2024-11-18 07:35_

Thanks for creating the upstream issue. Let's see what the outcome is upstream before adding this rule to avoid recoding in the near future.

---

_Comment by @tjkuson on 2024-12-08 15:02_

Merged as `B911` upstream.

---

_Comment by @MichaReiser on 2024-12-08 17:38_

Great. @InSyncWithFoo do you want to update your PR and recode the rule to B911?

---

_Comment by @InSyncWithFoo on 2024-12-08 18:18_

Done, and thanks @tjkuson for the heads-up.

---

_Closed by @MichaReiser on 2024-12-10 08:39_

---
