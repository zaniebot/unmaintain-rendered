---
number: 12542
title: Rules pertaining to PEP 695
type: issue
state: closed
author: Goldziher
labels:
  - rule
assignees: []
created_at: 2024-07-27T09:04:33Z
updated_at: 2025-01-22T16:42:35Z
url: https://github.com/astral-sh/ruff/issues/12542
synced_at: 2026-01-10T01:22:52Z
---

# Rules pertaining to PEP 695

---

_Issue opened by @Goldziher on 2024-07-27 09:04_

MyPy released support for PEP 695 generics: https://github.com/python/mypy/issues/15238

It would be awesome to do any of the following:

1. get linting errors for code that uses older style generics (TypeVar, ParamSpec etc.) for Python 3.12+ 
2. get linting errors for code that uses newer style generics and should be backward compatible with lower Python versions
3. automagically upgrade older style generics to use the new syntax 

I see that for point 3 there is an [open related issue](https://github.com/astral-sh/ruff/issues/4617). This issue is about the underlying rust implementation for point 3, but does not cover points 1 and 2. 

---

_Comment by @charliermarsh on 2024-07-27 12:37_

We have some support for this already in https://docs.astral.sh/ruff/rules/non-pep695-type-alias/. I believe that rule does (1) and (3)?


---

_Comment by @Goldziher on 2024-07-27 19:39_

> We have some support for this already in https://docs.astral.sh/ruff/rules/non-pep695-type-alias/. I believe that rule does (1) and (3)?

I don't think so. I might be wrong but it seems to me like this rule is concerned with the `TypeAlias` type. 

I am talking about the new generics style: https://peps.python.org/pep-0695/#summary-examples

---

_Label `rule` added by @charliermarsh on 2024-07-27 21:34_

---

_Comment by @charliermarsh on 2024-07-27 21:34_

Thanks, you're right, that's my mistake. We may want to fold this into #4617, and just flesh out that issue a bit more.

---

_Referenced in [astral-sh/ruff#4617](../../astral-sh/ruff/issues/4617.md) on 2024-07-28 11:45_

---

_Referenced in [astral-sh/ruff#15642](../../astral-sh/ruff/issues/15642.md) on 2025-01-21 13:58_

---

_Referenced in [astral-sh/ruff#15565](../../astral-sh/ruff/pulls/15565.md) on 2025-01-22 16:29_

---

_Closed by @ntBre on 2025-01-22 16:35_

---

_Closed by @ntBre on 2025-01-22 16:35_

---

_Comment by @ntBre on 2025-01-22 16:42_

Cases (1) and (3) should now be handled by the rules UP046 and UP047 added in https://github.com/astral-sh/ruff/pull/15565, with a few caveats being tracked in https://github.com/astral-sh/ruff/issues/15642.

I think case (2) would be a good fit for the "pydowngrade" suggestion from #2501, so I'm closing this and making a comment there.

---

_Referenced in [astral-sh/ruff#2501](../../astral-sh/ruff/issues/2501.md) on 2025-01-22 16:45_

---
