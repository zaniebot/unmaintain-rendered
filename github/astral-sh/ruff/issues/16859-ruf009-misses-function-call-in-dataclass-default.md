---
number: 16859
title: RUF009 misses function call in dataclass default
type: issue
state: open
author: bmyers-ozette
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-19T21:26:59Z
updated_at: 2025-04-16T11:00:13Z
url: https://github.com/astral-sh/ruff/issues/16859
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF009 misses function call in dataclass default

---

_Issue opened by @bmyers-ozette on 2025-03-19 21:26_

### Summary

The following code does not seem to trigger the linter rule RUF009.

```
@dataclasses.dataclass
class A:
    timestamp: str = dataclasses.field(default=datetime.datetime.utcnow().isoformat())
```

Based on the description of the rule, I was expecting this to get flagged to use `default_factory`. 

### Version

ruff 0.11.0

---

_Comment by @ntBre on 2025-03-19 21:42_

Thanks, this looks like another mismatch between RUF009 and B008 ([previous issue](https://github.com/astral-sh/ruff/issues/15772)). B008 is [flagged](https://play.ruff.rs/be9f1759-da92-4546-9808-2193141e0af7) (twice) for a similar function definition.

---

_Label `bug` added by @ntBre on 2025-03-19 21:42_

---

_Label `help wanted` added by @ntBre on 2025-03-19 21:42_

---

_Comment by @InSyncWithFoo on 2025-03-20 06:24_

@ntBre This is not a mismatch: if you added `str` as an annotation for the parameter, `B008` would not emit anything either. `RUF009` <em>would</em> have reported the snippet in question had it not been for #15772/#16048.

---

_Comment by @MichaReiser on 2025-03-20 07:22_

Yeah, I think there's an argument whether the changes introduced in #15772 were correct. 

The rule summary states

> Checks for function calls in dataclass attribute defaults.

That would suggest that all function calls are forbidden. 

The *Why is this bad* section goes into more detail

> Function calls are only performed once, at definition time. The returned value is then reused by all instances of the dataclass. This can lead to unexpected behavior when the function call returns a mutable object, as changes to the object will be shared across all instances

It narrows the function calls to those that return mutable values, which doesn't apply to `datetime.utcnow().isoformat()`.

However, there's an argument that the function must be pure and its return value should be immutable to be allowed in a default value position. The first is no longer guaranteed. The only way I see to fix the *pure* is to revert the changes introduced in #15772, but that comes at the cost of increasing the false positives again.


---

_Label `bug` removed by @MichaReiser on 2025-03-20 07:22_

---

_Label `help wanted` removed by @MichaReiser on 2025-03-20 07:22_

---

_Label `rule` added by @MichaReiser on 2025-03-20 07:22_

---

_Label `needs-decision` added by @MichaReiser on 2025-03-20 07:22_

---

_Comment by @bmyers-ozette on 2025-03-20 16:23_

>Function calls are only performed once, at definition time. The returned value is then reused by all instances of the dataclass. 

From my perspective, this is the behavior that caused our issue. Specifically, we had a bug where timestamps in our logging were all set to the dataclass definition time, rather than when each instance was created.

I could see an argument that our bug was a misunderstanding of intended behavior in Python, but in practice it would be helpful (for us at least) to flag _any_ function call used as a default. That said I could definitely see this being a matter of preference and up for debate. Perhaps a stricter variant could live as a separate rule?


---

_Comment by @CarliJoy on 2025-04-15 16:19_

I believe we should differentiate between *mutable* and *dynamic* default values. These should be handled by two (or with B009, rather four) distinct sets of rules.

An **mutable default** can lead to hard-to-detect bugs when instances use the default and then modify it.

**Dynamic defaults**, on the other hand, *could* introduce subtle bugs but are more likely to result in a high number of false positives. For each function call, Ruff would need to determine whether it is static for the given set of arguments — which is difficult, if not impossible.

I would also argue that the current rule name [`function-call-in-dataclass-default-argument` for RUF009](https://docs.astral.sh/ruff/rules/function-call-in-dataclass-default-argument/#function-call-in-dataclass-default-argument-ruf009) is just as misleading as [`function-call-in-default-argument`](https://docs.astral.sh/ruff/rules/function-call-in-default-argument/#function-call-in-default-argument-b008) for B008.

A more accurate name would be something like `mutable-generator-as-default-argument`.

Likewise, the name [`extend-immutable-calls`](https://docs.astral.sh/ruff/settings/#lint_flake8-bugbear_extend-immutable-calls) is misleading. Constructs like `attrs.Factory` are not immutable — they simply ensure that a new value is generated for each instance. The same applies to something like `fastapi.Query(None)`, which isn’t really a default value but rather a placeholder.

So, combining the same setting for B008 and RUF009 seems inappropriate in this context.  
A construct that is valid in a dataclass/attrs context may be invalid in a function — and vice versa.

Additionally, the current configuration doesn’t allow for aliasing constructs like `attrs.field`, such as [`typed_settings.option`](https://typed-settings.readthedocs.io/en/latest/apiref.html#typed_settings.option).  
Using `typed_settings.option(default=[])` is problematic, whereas `typed_settings.option(factory=list)` would be acceptable.

**TL;DR**:  
Don't modify RUF009 or B008 to account for *dynamic* defaults — instead, rename the existing rules and introduce new ones if wanted.  
Also, consider improved configuration options for allowing known-safe constructs.


---
