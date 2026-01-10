---
number: 14926
title: "[ruff] Hex code formatting restrictions around debug f-strings are too strict on preview"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - formatter
assignees: []
created_at: 2024-12-11T21:22:05Z
updated_at: 2024-12-12T07:06:12Z
url: https://github.com/astral-sh/ruff/issues/14926
synced_at: 2026-01-10T01:22:55Z
---

# [ruff] Hex code formatting restrictions around debug f-strings are too strict on preview

---

_Issue opened by @MeGaGiGaGon on 2024-12-11 21:22_

With preview disabled [playground link](https://play.ruff.rs/68e68e62-750b-411f-b997-d1948e35a661): 
```py
# input:
f"{"\xFF\N{space}"=}"
# output:
f"{"\xff\N{SPACE}"=}"

```
With preview enabled [playground link](https://play.ruff.rs/b6cb8fdb-bdd4-414e-bb49-f2de6c05b90a): 
```py
# input:
f"{"\xFF\N{space}"=}"
# output:
f"{"\xFF\N{space}"=}"

```
This change isn't observable, since the replacement of escapes happens before the debug applies:
```py
>>> f"{"\xFF\N{space}"=}"
'"ÿ "=\'ÿ \''
>>> f"{"\xff\N{SPACE}"=}"
'"ÿ "=\'ÿ \''
```
The issue with #14766 is that the change in formatting happens inside an r-string, not specifically the f-string debug. It should be fine to apply this formatting with the usual rules on string types, regardless of where the string is.
For completeness I have also previously opened the [same issue in black](https://github.com/psf/black/issues/4523), however it looks unlikely for black to have this fixed by the 2025 release, or anytime soon, as it would require resolving outstanding questions around how to handle f-string formatting.
Also cc @MichaReiser 

---

_Label `formatter` added by @MichaReiser on 2024-12-12 07:02_

---

_Comment by @MichaReiser on 2024-12-12 07:06_

Thanks for reporting this and the nice write up. This is interesting. While this is a missed opportunity, I don't think it's important for us to fix it because debug expressions are rare (especially in production code), and escapes are rare, too. It's also somewhat complicated to fix because we currently don't apply any formatting for debug expressions. We just preserve them as is and I think that's a good enough default.

---

_Closed by @MichaReiser on 2024-12-12 07:06_

---

_Referenced in [astral-sh/ruff#14929](../../astral-sh/ruff/pulls/14929.md) on 2024-12-12 09:28_

---
