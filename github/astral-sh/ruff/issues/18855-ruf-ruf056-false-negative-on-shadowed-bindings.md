---
number: 18855
title: "[`RUF`] `RUF056` false negative on shadowed bindings"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-06-21T20:35:36Z
updated_at: 2025-06-24T06:16:09Z
url: https://github.com/astral-sh/ruff/issues/18855
synced_at: 2026-01-07T13:12:16-06:00
---

# [`RUF`] `RUF056` false negative on shadowed bindings

---

_Issue opened by @MeGaGiGaGon on 2025-06-21 20:35_

### Summary

Like #18777, [falsy-dict-get-fallback (RUF056)](https://docs.astral.sh/ruff/rules/falsy-dict-get-fallback/#falsy-dict-get-fallback-ruf056) has a false negative when the name is a shadowed binding. [playground](https://play.ruff.rs/393aed06-5ef5-420d-9152-12f92affe66a)
```py
dict = {}
if dict.get(key, False):  # No error
    ...
```

### Version

playground

---

_Label `bug` added by @ntBre on 2025-06-22 03:25_

---

_Referenced in [astral-sh/ruff#18861](../../astral-sh/ruff/pulls/18861.md) on 2025-06-22 13:46_

---

_Renamed from "[`Ruff-specific rules`] `RUF056` false negative on shadowed bindings" to "[`RUF`] `RUF056` false negative on shadowed bindings" by @MichaReiser on 2025-06-23 06:51_

---

_Comment by @MichaReiser on 2025-06-24 06:16_

Using `resolve_name` over `only_binding` can lead to new false positives when control flow is involved:

```py
def demonstrate_false_positive():
    if True:
        a= set()
    else:
        a = {}


    # dict is now a string, should NOT trigger RUF056
    if a.get(key, False):  # But this DOES trigger - FALSE POSITIVE
        pass
```

That's why the fix is more complicated than replacing one with the other. I think the proper fix is to change `only_binding` to return `Some` if the last visible binding is visible regardless of the control flow graph taken. This would still leave some false positives (e.g the example above) but it would be an improvement over what we have today. 

The most comprehensive solution would be to enhance our type inference to verify that all potentially visible bindings resolve to a dictionary. 

---

_Referenced in [astral-sh/ruff#18818](../../astral-sh/ruff/issues/18818.md) on 2025-06-24 06:24_

---
