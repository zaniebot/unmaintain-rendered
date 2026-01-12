```yaml
number: 12256
title: ruff will Ignore target version when  work on pyi file
type: issue
state: closed
author: rainzee
labels:
  - documentation
  - question
assignees: []
created_at: 2024-07-09T09:01:48Z
updated_at: 2024-08-27T17:28:24Z
url: https://github.com/astral-sh/ruff/issues/12256
synced_at: 2026-01-12T15:54:51Z
```

# ruff will Ignore target version when  work on pyi file

---

_@rainzee_

The target version is specified as 3.8, and on the py file, everything works fine, but on the pyi file, it looks like the versioning rules are ignored, giving a not-yet-supported operation symbol

![image](https://github.com/astral-sh/ruff/assets/4585440/d531084a-8f7e-4a65-94cb-4f8ebef16573)


---

_Comment by @rainzee on 2024-07-09 09:05_

Or does pyi files have nothing to do with python version, only pyright?

---

_Comment by @MichaReiser on 2024-07-09 10:27_

Ruff's behavior that it ignores the target version for stub files is intentional and was previously discussed in https://github.com/astral-sh/ruff/issues/9285. @AlexWaygood's [comment](https://github.com/astral-sh/ruff/issues/9285#issuecomment-1872932728) explains the motivation well.

---

_Label `question` added by @MichaReiser on 2024-07-09 10:27_

---

_Comment by @AlexWaygood on 2024-07-09 10:47_

Possibly we should document this better, since it seems to have come up several times :)

---

_Label `documentation` added by @MichaReiser on 2024-07-09 11:42_

---

_Comment by @rainzee on 2024-07-09 16:36_

> Possibly we should document this better, since it seems to have come up several times :)

yes, pls. i can't find anything on doc

---

_Comment by @dhruvmanila on 2024-07-10 05:48_

The [`target-version` section](https://docs.astral.sh/ruff/settings/#target-version) might be a good place to put this information, what do you think @AlexWaygood ?

---

_Comment by @AlexWaygood on 2024-07-10 10:20_

> The [`target-version` section](https://docs.astral.sh/ruff/settings/#target-version) might be a good place to put this information, what do you think @AlexWaygood ?

Yeah, that looks like a good place to me too!

---

_Comment by @Avasam on 2024-07-18 17:18_

This is something I ended up having to explain a handful of times as well when contributing to stubs of projects where they're not as familiar with the typing system.

Would happily direct to Ruff's doc for explanation !

---

_Comment by @Avasam on 2024-07-25 15:03_

I was just made aware of https://typing.readthedocs.io/en/latest/guides/writing_stubs.html
I would've recommended linking to https://typing.readthedocs.io/en/latest/reference/stubs.html#syntax, ~~but it looks like it's still recommending to not use any new syntax past 3.8.~~ (it does mention special annotation syntax lower down, the initial wording is just a bit confusing about matching 3.8 features and syntax).

---

_Comment by @FichteFoll on 2024-08-19 11:22_

I also fell into the wording trap of https://typing.readthedocs.io/en/latest/reference/stubs.html#syntax and only later (after re-reading it a couple times and studying various issues on this tracker) that the "syntax" section only refers to the "syntax" and not … language features. 

My concerns about type aliases with forward references, pipe unions and getattr support for builtin types were not really addressed by the section or at least not in a fashion that gave me confidence in using them without trying them out first. 
I believe the primary problem I had was that type aliases, i.e. situations where these type features were used in an evaluated expression (not covered by `from __future__ import annotations`), were not mentioned at all. Thus, I assumed they weren't covered by the section and consequently not allowed.

https://typing.readthedocs.io/en/latest/guides/writing_stubs.html#language-features does not cover this either.

---

_Closed by @AlexWaygood on 2024-08-27 17:28_

---
