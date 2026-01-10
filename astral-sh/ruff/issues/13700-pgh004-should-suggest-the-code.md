---
number: 13700
title: "(🎁) `PGH004` should suggest the code"
type: issue
state: open
author: KotlinIsland
labels:
  - fixes
  - diagnostics
assignees: []
created_at: 2024-10-10T02:01:46Z
updated_at: 2024-10-24T06:00:42Z
url: https://github.com/astral-sh/ruff/issues/13700
synced_at: 2026-01-10T01:22:54Z
---

# (🎁) `PGH004` should suggest the code

---

_Issue opened by @KotlinIsland on 2024-10-10 02:01_

```py
a  # noqa 
```
```
> ruff check
test.py:1:4: PGH004 Use specific rule codes when using `noqa`
```

it should suggest the required error codes, additionally, it could have an autofix

https://play.ruff.rs/e847f4d3-7278-482b-a334-a1073b101829

---

_Label `rule` added by @dhruvmanila on 2024-10-10 04:43_

---

_Label `rule` removed by @MichaReiser on 2024-10-10 05:34_

---

_Label `fixes` added by @MichaReiser on 2024-10-10 05:34_

---

_Comment by @MichaReiser on 2024-10-10 05:36_

A fix would definitely be useful. I'm not convinced that we should include the error codes in the message. It could make the message unnecessarily long. But we could include a hint when we have our revamped diagnostic system 

---

_Label `diagnostics` added by @MichaReiser on 2024-10-10 05:36_

---

_Comment by @AlexWaygood on 2024-10-10 11:13_

I don't think suggesting the code in the error message would make the message _very_ long:

```diff
-test.py:1:4: PGH004 Use specific rule codes when using `noqa`
+test.py:1:4: PGH004 Use `noqa: F811` rather than catch-all suppression comment
```

---

_Comment by @MichaReiser on 2024-10-10 15:37_

It could be if we use rule names instead of codes or if there are multiple codes. 

---

_Comment by @KotlinIsland on 2024-10-24 03:35_

couldn't it just be in the `help` part of the output?

---

_Comment by @MichaReiser on 2024-10-24 06:00_

> couldn't it just be in the `help` part of the output?

Yeah, that's what I would prefer. 

---
