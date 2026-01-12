```yaml
number: 22541
title: "Misleading documentation for B904: raise-without-from-inside-except"
type: issue
state: open
author: shawnz
labels: []
assignees: []
created_at: 2026-01-12T22:45:34Z
updated_at: 2026-01-12T22:47:13Z
url: https://github.com/astral-sh/ruff/issues/22541
synced_at: 2026-01-12T23:24:03Z
```

# Misleading documentation for B904: raise-without-from-inside-except

---

_@shawnz_

### Summary

Rule B904 has docs which I find to be misleading. In the docs, [they claim](https://github.com/astral-sh/ruff/blob/3ae4db3ccd227ec58870c4309ab46e24a4c5cdf2/crates/ruff_linter/src/rules/flake8_bugbear/rules/raise_without_from_inside_except.rs#L21):

> When raising an exception from within an except clause, always include a from clause to facilitate exception chaining. If the exception is not chained, it will be difficult to trace the exception back to its root cause.

But actually, exceptions get implicitly chained by default if there's no `from` clause. So this rule never helps facilitate chaining: it can only either A) reduce chaining, by encouraging people to specify `from None`, or B) just make the default chaining which was already happening more explicit.

I prefer to chain exceptions wherever possible for the reason indicated: I want to have the most possible context to be able to trace errors to their root cause. And having this rule active just creates needless boilerplate to capture what the default behaviour already would have done. If the docs were more accurate about this, I think it would be clearer that this is a rule which should be deselected for most people (IMO).

There's an argument that explicit chaining produces a slightly nicer message in the traceback than implicit chaining (`The above exception was the direct cause of the following exception` vs `During handling of the above exception, another exception occurred`). But if that small difference is the whole reason for this rule's existence, I think that is a lot harder to justify all the boilerplate created as a result of applying this rule and I think the docs could be more clear about the limited benefit.

Thanks, Shawn

---
