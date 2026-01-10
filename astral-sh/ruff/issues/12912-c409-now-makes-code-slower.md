```yaml
number: 12912
title: C409 now makes code slower
type: issue
state: open
author: hauntsaninja
labels:
  - rule
  - preview
assignees: []
created_at: 2024-08-15T23:04:11Z
updated_at: 2025-03-31T18:23:30Z
url: https://github.com/astral-sh/ruff/issues/12912
synced_at: 2026-01-10T11:09:54Z
```

# C409 now makes code slower

---

_Issue opened by @hauntsaninja on 2024-08-15 23:04_

Introduced in https://github.com/astral-sh/ruff/pull/12657

`tuple(i for i in range(10000))` is like 50% slower than `tuple([i for i in range(10000)])`. I think it's still fine to have this as a lint, but it's now impossible to configure ruff to distinguish between the two.

This is a thematically similar complaint to https://github.com/astral-sh/ruff/issues/8884

---

_Renamed from "C409 now makes my code slower" to "C409 now makes code slower" by @hauntsaninja on 2024-08-15 23:04_

---

_Comment by @hauntsaninja on 2024-08-15 23:05_

Oh this is also similar to https://github.com/astral-sh/ruff/issues/10838

---

_Comment by @charliermarsh on 2024-08-15 23:08_

These changes are all in preview and so we can still decide to change them before stabilizing. If we revert that change, though, we should do the same for the list rule variants.

I don’t feel strongly about it. I would also be open to making it configurable. Perhaps the setting should state whether the user prefers a comprehension or a generator, and enforce it one way or another? Or would the setting just ignore comprehensions?

---

_Label `rule` added by @AlexWaygood on 2024-08-15 23:16_

---

_Label `preview` added by @AlexWaygood on 2024-08-15 23:16_

---

_Comment by @hauntsaninja on 2024-08-15 23:23_

By list rule variant do you mean C410? I think that one is fine because it never introduces a generator comprehension.

It's maybe more dangerous to enforce list comprehensions over generator comprehensions. Also for probably mostly theoretical memory reasons, despite the perf impact, people typically seem more eager to go from list -> generator than generator -> list. So maybe I lean towards a setting to leave list comprehensions alone.

Or actually, maybe the slowening parts of C409 and C419 should be split into separate rules. The new rule that applies to comprehensions could then have a setting for whether you prefer list comprehensions or generator comprehensions (defaulting to generator). I think this would let everyone configure their ideal behaviour.

---

_Comment by @charliermarsh on 2024-08-16 01:51_

Sorry, I misspoke. I thought there was a rule that changed:

```python
list([f(x) for x in foo])
```

To:

```python
list(f(x) for x in foo)
```

But no, the fix there is `[f(x) for x in foo]`, so that's fine.


---

_Comment by @charliermarsh on 2024-08-16 01:51_

I generally agree that this behavior should be configurable (or even removed). What do you think, @AlexWaygood?


---

_Comment by @AlexWaygood on 2024-08-16 13:44_

I agree that these should either be two rules (one that makes your code slower and one that doesn't), or one configurable rule (where the default is to only emit the recommendations that do not make your code slower).

Some people prefer to always remove inner parentheses in calls like `tuple([x for x in foo])` because they find it annoying when they're not necessary for semantics, and/or they feel that the inner parentheses are a "micro-optimisation" that you should only worry about in very performance-sensitive code. Other people prefer to always include them, because they want their code to be as fast as possible. I think both are reasonable points of view, but we shouldn't mix recommendations that have no negative performance implications with ones that could seriously slow down your code in the same rule. (At least, not with our default settings.)

---

_Comment by @Avasam on 2024-08-17 23:58_

> `tuple(i for i in range(10000))` is like 50% slower than `tuple([i for i in range(10000)])`.
>
> Oh this is also similar to #10838

And similar to the exact kind of example I requested there: https://github.com/astral-sh/ruff/issues/11839
I wouldn't mind merging my Feature Request with an already open issue.

---

_Comment by @Skylion007 on 2025-03-04 15:11_

See this PR https://github.com/pytorch/pytorch/pull/148412#issue-2892876294 for timing that shows that removing the list comprehension here can be sub-optimal. At least, we need to add a knob to back out of the preview behavior.

See
```python
>>> timeit.timeit("tuple(x for x in range(10))")
0.39464114885777235
>>> timeit.timeit("tuple([x for x in range(10)])")
0.21258362499065697
>>>
```

At the very least, there should be a knob to turn this more controversial behavior off on this flake8 rule.

@charliermarsh was the preview of this rule removed recently? We really should make this behavior optin since it's worse.

---

_Comment by @MichaReiser on 2025-03-04 15:18_

> was the preview of this rule removed recently? We really should make this behavior optin since it's worse.

I didn't go back further but it is stable since before october 2023

---

_Comment by @Skylion007 on 2025-03-04 15:25_

@MichaReiser Found the issue, it's overlap with another rule that was flagging it in our linter system: reported in #16500

---

_Comment by @Skylion007 on 2025-03-04 15:31_

> > was the preview of this rule removed recently? We really should make this behavior optin since it's worse.
> 
> I didn't go back further but it is stable since before october 2023

Also if the preview behavior of this rule was removed, it still mentioned the preview behavior in the official docs: https://docs.astral.sh/ruff/rules/unnecessary-literal-within-tuple-call/#unnecessary-literal-within-tuple-call-c409

---

_Comment by @MichaReiser on 2025-03-07 09:40_

@Skylion007 sorry, I misread your question. This behavior is still under preview, but the rule was stabilized a long time ago

https://github.com/astral-sh/ruff/blob/a1a536b2c57ae1a7cc9e84b60db0e8216a34fc9d/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs#L100-L105

---

_Comment by @Avasam on 2025-03-31 18:21_

Looks like tuple call with a generator expression is getting optimized in Python 3.14 from https://github.com/astral-sh/ruff/issues/12912

I don't have actual numbers to compare though.

(thanks @davfsa for spotting this in https://github.com/astral-sh/ruff/issues/16904#issuecomment-2766646655 )

---
