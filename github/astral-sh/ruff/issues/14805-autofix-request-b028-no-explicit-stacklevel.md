---
number: 14805
title: "[Autofix request] B028: no-explicit-stacklevel"
type: issue
state: closed
author: DanielYang59
labels:
  - fixes
  - accepted
assignees: []
created_at: 2024-12-06T05:07:20Z
updated_at: 2024-12-07T13:51:23Z
url: https://github.com/astral-sh/ruff/issues/14805
synced_at: 2026-01-07T13:12:16-06:00
---

# [Autofix request] B028: no-explicit-stacklevel

---

_Issue opened by @DanielYang59 on 2024-12-06 05:07_

Can I request an autofix for B028: no-explicit-stacklevel?

I understand `stacklevel=2` may not be the one-size-fits-all solution (some implementation might need higher stack level), but I guess it's a good default value for `ruff` autofix?

---

_Label `fixes` added by @dylwil3 on 2024-12-06 05:31_

---

_Label `needs-decision` added by @dylwil3 on 2024-12-06 05:31_

---

_Comment by @AlexWaygood on 2024-12-06 08:35_

Yeah, I agree that an unsafe autofix to `stacklevel=2` is reasonable here.

---

_Label `needs-decision` removed by @AlexWaygood on 2024-12-06 08:35_

---

_Label `accepted` added by @AlexWaygood on 2024-12-06 08:35_

---

_Comment by @DanielYang59 on 2024-12-06 08:55_

Thanks for the confirm that would be really helpful :)

---

_Referenced in [astral-sh/ruff#14829](../../astral-sh/ruff/pulls/14829.md) on 2024-12-07 03:50_

---

_Comment by @DanielYang59 on 2024-12-07 07:18_

@dylwil3 Really appreciate the quick fix, I noticed some minor relevant issues while cleaning up our code base: 
1. Some people might notice this warning, and thought it's "no explicit stacklevel" that is bad, and ended up giving `stacklevel=1`, would it be good to extend B028 to cover `stacklevel=1` as well? Or perhaps we should keep it as is in case people intentionally use `stacklevel=1` for whatever reason (to point to the implementation maybe?)
2. I also noticed people explicitly give the default `UserWarning` like `warnings.warn(msg, category=UserWarning)`, this is not really doing anything bad, but seems redundant

I'm happy to open a separate issue to track this if you believe it's worth the effort and feel free to reject them :)  

---

_Comment by @dylwil3 on 2024-12-07 13:23_

Sure! Thanks for submitting the issue!

> Some people might notice this warning, and thought it's "no explicit stacklevel" that is bad, and ended up giving stacklevel=1, would it be good to extend B028 to cover stacklevel=1 as well? Or perhaps we should keep it as is in case people intentionally use stacklevel=1 for whatever reason (to point to the implementation maybe?)

If a developer explicitly specifies the stacklevel it feels a little rude to contradict them 😄 We would also have to change the rule name and diagnostic message in that case. But I agree that the documentation around "Why this is bad" together with the autofix suggestion makes it sound like the rule is a little stricter than it is. So maybe there is some editing to be done to the message and/or documentation?

> I also noticed people explicitly give the default UserWarning like warnings.warn(msg, category=UserWarning), this is not really doing anything bad, but seems redundant

I don't think I know enough about the usage here to gauge whether this would be common/helpful, but please feel free to suggest this as a new lint rule in a separate issue and folks can weigh in.

---

_Closed by @charliermarsh on 2024-12-07 13:24_

---

_Closed by @charliermarsh on 2024-12-07 13:24_

---

_Comment by @DanielYang59 on 2024-12-07 13:48_

Thanks for the quick implementation again.

> If a developer explicitly specifies the stacklevel it feels a little rude to contradict them

Yep I agree, they might know well what they're doing, so perhaps don't warn for such case. Also manual fixing for such cases is much easier, just do a regular expression search and replace.

> So maybe there is some editing to be done to the message and/or documentation? 

I'm personally happy with the state of the docs. Our code base is built around chemistry so some people don't come from CS background to understand "stack", and they just don't bother to read the documentation and ended up passing a safe default `stacklevel=1` to mute the warning I guess.

> I don't think I know enough about the usage here to gauge whether this would be common/helpful, but please feel free to suggest this as a new lint rule in a separate issue and folks can weigh in.

Thanks for the comment. I guess it's not worthy a issue as it's just a unnecessary default argument given (looks not so tidy but don't have any other side effect), there's no way for us to warn again every default value usage I guess :)

Thanks again and all the best



---
