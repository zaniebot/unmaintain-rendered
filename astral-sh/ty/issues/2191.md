```yaml
number: 2191
title: "narrow tagged `TypedDict` unions with `not in`"
type: issue
state: closed
author: oconnor663
labels:
  - narrowing
  - typeddict
assignees: []
created_at: 2025-12-23T19:34:34Z
updated_at: 2026-01-03T13:12:59Z
url: https://github.com/astral-sh/ty/issues/2191
synced_at: 2026-01-10T01:56:41Z
```

# narrow tagged `TypedDict` unions with `not in`

---

_Issue opened by @oconnor663 on 2025-12-23 19:34_

Related to this TODO: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/typed_dict.md?plain=1#L2139

Which was inspired by @AlexWaygood's comment here: https://github.com/astral-sh/ruff/pull/22104#discussion_r2643082896

In short:

```py
class Foo(TypedDict):
    foo: int

class Bar(TypedDict):
    bar: int

def _(u: Foo | Bar):
    if "foo" not in u:
        reveal_type(u)  # currently `Foo | Bar`, should be `Bar`
```

Note that, as mentioned in the mdtest comments, we actually *shouldn't* narrow to `Foo` when we have `"foo" in u`, because `Bar` could have "extra items".

---

_Label `narrowing` added by @oconnor663 on 2025-12-23 19:34_

---

_Added to milestone `Stable` by @carljm on 2025-12-23 19:45_

---

_Label `typeddict` added by @oconnor663 on 2025-12-23 21:15_

---

_Closed by @AlexWaygood on 2026-01-03 13:12_

---
