```yaml
number: 22541
title: "Misleading documentation for B904: raise-without-from-inside-except"
type: issue
state: open
author: shawnz
labels:
  - documentation
assignees: []
created_at: 2026-01-12T22:45:34Z
updated_at: 2026-01-13T19:45:22Z
url: https://github.com/astral-sh/ruff/issues/22541
synced_at: 2026-01-13T20:36:52Z
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

_Comment by @ntBre on 2026-01-12 23:36_

There are a couple of dunder attributes that are also set differently in implicit vs explicit chaining: https://docs.python.org/3/library/exceptions.html#exception-context, so it's not _just_ about the error message. But @amyreese may know better than me how common inspecting those would be. I think we could make the documentation more precise in any case and possibly include the link above and/or [PEP 3134](https://peps.python.org/pep-3134/), which has some additional context.

There was some previous discussion of this in https://github.com/astral-sh/ruff/issues/8736, which was closed in https://github.com/astral-sh/ruff/pull/9680, but it doesn't look like the docs were updated.

---

_Label `documentation` added by @ntBre on 2026-01-12 23:36_

---

_Comment by @shawnz on 2026-01-13 01:15_

I see, sorry for the dupe!

---

_Comment by @ntBre on 2026-01-13 15:30_

No need to apologize! The other issue was closed, so thank you for reopening the discussion. I think [this comment](https://github.com/astral-sh/ruff/issues/8736#issuecomment-1816525873) about updating the docs is still relevant.

---

_Comment by @amyreese on 2026-01-13 18:41_

IMO B904 is a bad lint rule, because `raise Foo from exc` actually puts *less* information in the resulting stack trace. That's fine if you as the developer have made a conscious choice that the original exception is not useful there, but in most cases, having that original context in the resulting stack trace is better, not worse.

---

_Comment by @shawnz-swiftly on 2026-01-13 18:44_

> IMO B904 is a bad lint rule, because `raise Foo from exc` actually puts *less* information in the resulting stack trace. That's fine if you as the developer have made a conscious choice that the original exception is not useful there, but in most cases, having that original context in the resulting stack trace is better, not worse.

Exactly, totally agree -- and with the way the docs are worded now, they might misleadingly have you believe that you're losing context if you *don't* do this!

---

_Comment by @ntBre on 2026-01-13 19:14_

Do you have an example of the explicit chaining putting less information in the stack trace? Based on my experiments in the REPL and Zanie's comment on the other issue, the trace looks basically the same except for the phrasing. Is the effect more pronounced for longer chains?

---

_Comment by @amyreese on 2026-01-13 19:45_

Hmm, you're right. The Python documentation you linked to is very misleading, and I feel like I've seen the behavior it describes, but I can't repro that. Looking at the [upstream commit for when B904 was added](https://github.com/PyCQA/flake8-bugbear/commit/b462a5e6926d12e344181d91cd9ba4f37773e45b), it looks like they reference Hypothesis and Trio as reasons for the rule. I've never used either, so 🤷🏻‍♀️.

I'm still in the mindset that B904 is at best not useful, but perhaps not as counter-productive as I expected. 😅

---
