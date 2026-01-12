```yaml
number: 18742
title: Fixing some rules with debug f-strings introduces a syntax error
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - fixes
assignees: []
created_at: 2025-06-18T03:17:42Z
updated_at: 2025-06-25T08:04:16Z
url: https://github.com/astral-sh/ruff/issues/18742
synced_at: 2026-01-12T15:54:56Z
```

# Fixing some rules with debug f-strings introduces a syntax error

---

_@MeGaGiGaGon_

### Summary

I discovered this while messing around with `B013`, but fixing some rules that contain debug f-strings will break the code:
```py
f"{1=
}"
```
Gets turned into
```py
f"{1=\r\n}"
```
Which is invalid code.

[playground example using `B013`](https://play.ruff.rs/7aaa414b-84ce-4ba2-8b54-1c398680a3b2)
[playground example using `RUF030`](https://play.ruff.rs/ad9ff4d1-6f44-451c-b366-d9954490ff6b)

There are some rules that don't break the f-string, for example `E713` [playground](https://play.ruff.rs/4290bc70-9ac4-46a1-bee6-724f46b13e88) or `PERF401` [playground](https://play.ruff.rs/96b137d2-f28c-4e6e-b8d2-07ff8ac1f546) or `PLC1802` [playground](https://play.ruff.rs/05d46da0-21d3-40af-9cb9-057a2ff9efda). I'm not sure why these are different. 

I did not extensively test every single fixable rule / every single rule category to see which ones have this bug.

### Version

playground

---

_Label `fixes` added by @MichaReiser on 2025-06-18 07:59_

---

_Comment by @MichaReiser on 2025-06-18 08:00_

Yeah, I think almost every fix could potentially break a debug f-string. It would be nice if we could prevent that somehow but I don't think it's worth the effort, especially considering that debug expressions are mainly intended for debugging.

---

_Comment by @MeGaGiGaGon on 2025-06-18 15:37_

I did a bunch more tests and found one that could more conceivably pop up in real code, `PLR1714` [playground](https://play.ruff.rs/2be939d6-ffb2-4ce3-ad9e-eaf18d2b431d). The issue here isn't that these change the behavior, I actually haven't found a fix that does that so far. The issue is that they make the code invalid/non-functional due to a syntax error:
```
PS D:\python_projects> Get-Content issue.py
```
```py
foo = "1"
if foo == "1" or foo == f"{1=
}":
    print(1)
```
```
PS D:\python_projects> py issue.py
```
```
1
```
```
PS D:\python_projects> uvx ruff check issue.py --select PLR --fix --unsafe-fixes
```
```
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes PLR1714, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

issue.py:2:4: PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
  |
1 |   foo = "1"
2 |   if foo == "1" or foo == f"{1=
  |  ____^
3 | | }":
  | |__^ PLR1714
4 |       print(1)
  |
  = help: Merge multiple comparisons

Found 1 error.
[*] 1 fixable with the --fix option.
```
And given that these all break the same way, my suspicion is that these rules all share the same code bug, and there is a chance it would be findable/fixable globally.

---

_Comment by @MeGaGiGaGon on 2025-06-18 19:34_

Found another one, `PIE810` [playground](https://play.ruff.rs/c3286971-7c13-4231-b0d8-a55ad24eeae0)

---

_Renamed from "Fixing B/RUF rules breaks debug f-strings" to "Fixing some rules with debug f-strings introduces a syntax error" by @MeGaGiGaGon on 2025-06-18 19:36_

---

_Comment by @MeGaGiGaGon on 2025-06-19 01:23_

I found a bunch more rules from `SIM` that do this: `SIM101`, `SIM103`, `SIM108`, `SIM110`, `SIM201`, `SIM202`, `SIM208`, `SIM210`, `SIM211`, `SIM212`, `SIM222`, `SIM401` [playground](https://play.ruff.rs/85dc2cc7-fc11-40c5-94b9-c003523620d8)

---

_Comment by @MeGaGiGaGon on 2025-06-19 01:44_

Looking through some of the `SIM` fixes that both do and don't trigger the bug, I think I found the common thread: They all use `Edit::range_replacement`. I'm not sure beyond that why this happens, but in all the broken fixes they seem to use `Edit::range_replacement`

---

_Closed by @MichaReiser on 2025-06-25 08:04_

---
