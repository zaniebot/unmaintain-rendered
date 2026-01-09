---
number: 10251
title: "DTZ005 false postitive for `tz=None`"
type: issue
state: closed
author: adriangb
labels:
  - documentation
  - good first issue
  - rule
assignees: []
created_at: 2024-03-06T18:16:32Z
updated_at: 2024-03-27T19:42:14Z
url: https://github.com/astral-sh/ruff/issues/10251
synced_at: 2026-01-07T13:12:15-06:00
---

# DTZ005 false postitive for `tz=None`

---

_Issue opened by @adriangb on 2024-03-06 18:16_

```python
import datetime

datetime.datetime.now(tz=None) 
# The use of `datetime.datetime.now()` without `tz` argument is not allowed Ruff DTZ005
```

ruff 0.1.9

---

_Comment by @charliermarsh on 2024-03-06 19:29_

I think this is arguably a true positive, since `tz=None` is equivalent to omitting it, in that it won't produce a timezone-aware object. Perhaps the documentation and message need to be tweaked to reflect that? 

---

_Label `documentation` added by @charliermarsh on 2024-03-06 19:29_

---

_Comment by @adriangb on 2024-03-06 23:02_

I think it’s arguable. No arguments could be a mistake (and is a common one). Explicitly passing None is acknowledging that you actually want None. In my case I want to use UTC everywhere except in a helper script.

But yes at least an error message tweak would be nice.

---

_Comment by @charliermarsh on 2024-03-06 23:26_

Yeah I also view it as borderline. I could definitely be convinced to ignore it. May need more opinions. @AlexWaygood, what do you think?

---

_Label `documentation` removed by @charliermarsh on 2024-03-06 23:26_

---

_Label `needs-decision` added by @charliermarsh on 2024-03-06 23:26_

---

_Comment by @AlexWaygood on 2024-03-07 19:24_

It seems like we're following what the original flake8 rule does here, which is explicitly going out of its way to make sure that calls are still flagged when `None` is passed to this argument: https://github.com/pjknkda/flake8-datetimez/blob/1bcda18963ef61861a57c86f83741172884aadd1/flake8_datetimez.py#L96-L110. (But that's obviously not decisive -- if this is bad behaviour, we shouldn't do it!)

I... am sort-of inclined to leave this rule as it is, though. It's simple to explain currently: "aware datetimes are good; naive datetimes are bad". Keeping a rule simple to explain to users and easy to describe in documentation is valuable. Moreover, if we changed the behaviour here, it's possible people who don't understand the rationale behind the rule might think that they're "fixing" the issue by passing `None` (since the linter stops complaining at them). That wouldn't necessarily be true.

I feel like if you're working in a domain where you don't need aware timezones, you can probably just not enable this rule on your project. And if you have a specific script in a larger project where it's just that script that doesn't need aware timezones, you can probably just put `# ruff: noqa: DTZ005` somewhere near the top of the file.

---

_Comment by @adriangb on 2024-03-07 19:29_

> it's possible people who don't understand the rationale behind the ruule might think that they're "fixing" the issue by passing None (since linter stops complaining at them)

That's a fair point, I hadn't thought of that.

I guess the other recommendation is to do:

```python
datetime.now(tz=timezone.utc).astimezone()
```

Which I think gives you the local timezone, although it's not as simple as `tz=None`

Either way, whatever you choose is fine by my, I just wanted to bring it to attention.

---

_Comment by @zanieb on 2024-03-11 17:13_

I think we should just improve the rule documentation and recommendation here

---

_Label `needs-decision` removed by @zanieb on 2024-03-11 17:13_

---

_Label `documentation` added by @zanieb on 2024-03-11 17:13_

---

_Label `good first issue` added by @zanieb on 2024-03-11 17:13_

---

_Label `rule` added by @zanieb on 2024-03-11 17:13_

---

_Referenced in [astral-sh/ruff#10590](../../astral-sh/ruff/pulls/10590.md) on 2024-03-25 21:21_

---

_Referenced in [astral-sh/ruff#10621](../../astral-sh/ruff/pulls/10621.md) on 2024-03-26 18:53_

---

_Closed by @AlexWaygood on 2024-03-27 19:42_

---
