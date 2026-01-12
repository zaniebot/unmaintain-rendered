```yaml
number: 8868
title: Exact reasons for fix unsafety should be given (docs or CLI)
type: issue
state: closed
author: sh-at-cs
labels: []
assignees: []
created_at: 2023-11-28T14:03:02Z
updated_at: 2023-11-28T14:40:49Z
url: https://github.com/astral-sh/ruff/issues/8868
synced_at: 2026-01-12T15:54:48Z
```

# Exact reasons for fix unsafety should be given (docs or CLI)

---

_@sh-at-cs_

It would be cool if Ruff gave an explanation for *why* certain fixes are considered unsafe, either in the docs or (even better, but probably requiring much more work) in the CLI output.

E.g. in the case of the fix for [UP007](https://docs.astral.sh/ruff/rules/non-pep604-annotation/), one can guess that it's because some libraries read annotations at runtime and might behave differently for `Union[X | Y]` and `X | Y` (just a guess, maybe that's wrong), but AFAICT, nothing like that is mentioned anywhere in the docs nor in the CLI output.

Maybe there could be a mandatory section explaining the relevant fixes' safety status in the docs page of each rule that has any.

---

_Renamed from "What exactly makes certain fixes unsafe should be explained" to "Exact reasons for fix unsafety should be given (docs or CLI)" by @sh-at-cs on 2023-11-28 14:03_

---

_Comment by @dhruvmanila on 2023-11-28 14:39_

Yeah, I agree. This is related to https://github.com/astral-sh/ruff/issues/8482. Perhaps for the CLI, we could implement it using https://github.com/astral-sh/ruff/issues/4361, which requires the reason to be mentioned in the code. Subsequently, the documentation could extract the message from there, as is currently done for the diagnostic message.

---

_Comment by @dhruvmanila on 2023-11-28 14:40_

I'll close this favor of #8482 

---

_Closed by @dhruvmanila on 2023-11-28 14:40_

---
