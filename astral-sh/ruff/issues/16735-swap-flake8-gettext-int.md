```yaml
number: 16735
title: SWAP flake8-gettext (INT)
type: issue
state: closed
author: p1-dta
labels:
  - documentation
assignees: []
created_at: 2025-03-14T10:18:44Z
updated_at: 2025-03-19T17:02:30Z
url: https://github.com/astral-sh/ruff/issues/16735
synced_at: 2026-01-12T15:54:55Z
```

# SWAP flake8-gettext (INT)

---

_@p1-dta_

### Summary

Two rules documentation seems to be swapped:

- [INT002: Checks for str.format calls in gettext function calls.
](https://docs.astral.sh/ruff/rules/format-in-get-text-func-call/)
- [INT003: Checks for printf-style formatted strings in gettext function calls.](https://docs.astral.sh/ruff/rules/printf-in-get-text-func-call/)

Expected detection: 
- INT002: `_("Hello, {}!".format(name))  # Looks for "Hello, Maria!".`
- INT003: `_("Hello, %s!" % name)  # Looks for "Hello, Maria!".`

Actual documentation:
- INT002: `_("Hello, %s!" % name)  # Looks for "Hello, Maria!".`
- INT003: `_("Hello, {}!".format(name))  # Looks for "Hello, Maria!".`

I wonder if this is intended.

I didn't checked if the rule INT002 actually detect str.format or printf style, but in any case, there is an inconsistency somewhere.

---

_Comment by @ntBre on 2025-03-14 12:32_

Nice catch! If I'm parsing the results on the [playground](https://play.ruff.rs/24cc67c8-57bb-48b0-9e1b-105a3b60766e) right, it looks like the examples in the documentation are swapped and the implementations are correct.

---

_Label `documentation` added by @ntBre on 2025-03-14 12:32_

---

_Comment by @MichaReiser on 2025-03-14 12:34_

@p1-dta are you interested in PRing a fix or @ntBre, are you already working on it?

---

_Comment by @ntBre on 2025-03-14 12:44_

I haven't started working on it. @p1-dta feel free to go ahead if you're interested, or we can put the help wanted label on.

I am happy to work on it if nobody else does, though.

---

_Comment by @MichaReiser on 2025-03-14 12:57_

I think this is *bad enough* that we should fix it ourselves, unless @p1-dta is interested

---

_Comment by @ntBre on 2025-03-14 13:03_

I'll keep it in my inbox and check back later today.

---

_Assigned to @ntBre by @ntBre on 2025-03-14 13:04_

---

_Closed by @ntBre on 2025-03-17 14:37_

---

_Comment by @p1-dta on 2025-03-19 17:02_

Hi, I'm back, sorry for being late, I don't use GH on a daily basis, so I missed the notification.
I not looking for PRing if someone else have the time to tackle it.
Thanks for fixing it.

---
