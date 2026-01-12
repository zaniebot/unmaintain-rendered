```yaml
number: 12277
title: Consider adding a rule that enforces using TypeIs instead of TypeGuard
type: issue
state: closed
author: ItsDrike
labels:
  - rule
assignees: []
created_at: 2024-07-10T14:50:31Z
updated_at: 2024-08-10T13:18:46Z
url: https://github.com/astral-sh/ruff/issues/12277
synced_at: 2026-01-12T15:54:51Z
```

# Consider adding a rule that enforces using TypeIs instead of TypeGuard

---

_@ItsDrike_

In [PEP 742](https://peps.python.org/pep-0742/), a new `TypeIs` annotation has been added. This provides an alternative to `TypeGuard` , which supports proper type narrowing. In vast majority of cases, people should use `TypeIs` as opposed to `TypeGuard` and the use of `TypeGuard` in code is likely a mistake.

It would therefore be nice to have a rule that bans the use of `TypeGuard` completely, just like the `ANN401` rule, that bans `typing.Any`.

---

_Label `rule` added by @charliermarsh on 2024-07-12 12:39_

---

_Comment by @JelleZijlstra on 2024-07-30 16:13_

I don't think TypeGuard is problematic enough that linters should warn against it. In fact a major part of the reason why I wrote PEP 742 as an alternative to PEP 724 (which proposed changing TypeGuard to behave similarly to how TypeIs now works) was that I looked at a number of real-world usages of TypeGuard and many worked with the existing semantics of TypeGuard, but not with TypeIs.

Writing a correct TypeIs function is tricky (https://typing.readthedocs.io/en/latest/guides/type_narrowing.html#writing-a-correct-typeis-function), and a lint rule that steers people to blindly replace TypeGuard with TypeIs is likely to cause incorrect TypeIs functions that lead to bad type narrowing.

---

_Comment by @ItsDrike on 2024-08-10 13:18_

Closing this, as JellieZiljstra pointed out, there are still a lot of valid use cases for TypeGuard that can't be changed to TypeIs, which makes a rule like this potentially pretty annoying for developers, as they'd need noqas for valid code there.

---

_Closed by @ItsDrike on 2024-08-10 13:18_

---
