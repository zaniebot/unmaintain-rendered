---
number: 13702
title: "Rule idea: pass regex flags by keyword"
type: issue
state: closed
author: xmo-odoo
labels:
  - rule
  - needs-info
assignees: []
created_at: 2024-10-10T08:27:48Z
updated_at: 2024-10-11T11:33:23Z
url: https://github.com/astral-sh/ruff/issues/13702
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule idea: pass regex flags by keyword

---

_Issue opened by @xmo-odoo on 2024-10-10 08:27_

In my experiece this is a relatively common issue for re.sub / re.subn: they take both a `count` and a `flags` parameter, and `count` is first. So

    re.sub(pattern, repl, text, re.MULTILINE)

will not apply `re.MULTILINE` but will limit the number of matches to 8. This typechecks because both parameters are typed as `int` and `re.RegexFlag` is an `IntFlag`. It can also happen with `re.split` (where the first parameter after the text is `maxsplit`) though I've less experience with that one.

Strictly speaking it's not needed for the rest (compile, search, match, iterfind, ...) as `flags` is their only integer parameter. I don't think it would *hurt* too keep things uniform but it can go either way.

Alternatively: more specifically flag passing an `re` flag as count or maxsplit, but I'm not sure that's possible, easier, or more useful.

---

_Comment by @AlexWaygood on 2024-10-10 14:26_

Hi, thanks for the issue! Does https://docs.astral.sh/ruff/rules/re-sub-positional-args/ already catch this error?

---

_Label `rule` added by @AlexWaygood on 2024-10-10 14:26_

---

_Label `needs-info` added by @AlexWaygood on 2024-10-10 14:26_

---

_Comment by @xmo-odoo on 2024-10-11 11:30_

> Hi, thanks for the issue! Does https://docs.astral.sh/ruff/rules/re-sub-positional-args/ already catch this error?

It does, clearly I missed it sorry about that.

---

_Closed by @xmo-odoo on 2024-10-11 11:30_

---

_Comment by @AlexWaygood on 2024-10-11 11:33_

No worries! Glad we could help :-)

---
