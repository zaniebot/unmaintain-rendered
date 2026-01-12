```yaml
number: 19474
title: highlight when a function cannot return all of its unioned types
type: issue
state: open
author: gpshead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-21T23:19:35Z
updated_at: 2025-07-24T15:13:42Z
url: https://github.com/astral-sh/ruff/issues/19474
synced_at: 2026-01-12T15:54:56Z
```

# highlight when a function cannot return all of its unioned types

---

_@gpshead_

### Summary

```python
def beans_api(...) -> Spam | None:
    if failsafe:
        return None

    spam = Spam(...)

    if spam.bad_condition:
        return None   
```

Spot the bug.  A wise analyzer knows that no code path returned Spam despite the type annotation saying it could.  `ruff` may not always be wise enough but probably has some simple whens for this simple footgun "obviously never returned anything other than literals" case?  But `ruff+ty` could be wise.

I can imagine cases where this might not be wanted, are they real?  or just worthy of a checking disable comment.  ex: abstract base class functions... but those are generally in the always-raises NotImplemented category where there is never a return value? a case which should not trigger this. ("never returns" being distinct from "returns X|Y but not Z")

Also filed as https://github.com/microsoft/pyright/issues/10731 - which was understandably closed - but this is a more general typing/linting boundary check that may fit a linter better than a type checker.

planting the seed in the back of your mind.  do with it as you will. :)

---

_Comment by @carljm on 2025-07-21 23:31_

One specific case where I think this would result in lots of false positives is inheritance. It's common (and pretty reasonable) to have a wider return type on a method in a class hierarchy where other implementations in the hierarchy might return that other type, but this particular implementation doesn't. There's no way to avoid these false positives in general, so I think at best this is a diagnostic that would need to be restricted to free functions. (Even with a method on a final class, the method might have to conform to a base class return type that is used by the base class implementation, or by other unknown subclasses of it.)

---

_Label `rule` added by @ntBre on 2025-07-22 01:01_

---

_Label `needs-decision` added by @ntBre on 2025-07-22 01:01_

---

_Comment by @jlopezpena on 2025-07-24 10:29_

Another common case where there would be a lot of false positives, without needing to resort to inheritance. Consider this type alias to represent the potential values of something that comes from deserialising JSON:

```python
JSONValue: TypeAlias = None | bool | int | float | str | list[JSONValue] | dict[str, JSONValue]
JSONObject: TypeAlias = dict[str, JSONValue]
```

Then any custom json serialisers would complain if they cannot have every single type of value



---

_Comment by @gpshead on 2025-07-24 15:13_

This might be most valuable in the `| None` case (as in the example) to ensure at least one of the non-None return types happens? There are probably other heuristics that'd have good signal to noise value.  agreed that there will always be some classes of function that do not explicitly return all of their types.  Just the fact that a TypeAlias was defined for the return type might be signal in and of itself?

---

for the _specific_ example I gave I thought i've seen other checkers that seek to ensure that if you have any `return` statements in a function that all exit paths have an explicit `return` along with seeking consistency between naked `return` and `return expression` within one function.  Somewhat related to https://docs.astral.sh/ruff/rules/implicit-return-value/ but going beyond that. Also including https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/inconsistent-return-statements.html but more than that, we want to flag any exit from a function that lacks an explicit return statement.

---
