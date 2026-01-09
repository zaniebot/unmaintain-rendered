---
number: 5736
title: "[FEATURE] Ruff support for removing obsolete and not needed noqa in the code"
type: issue
state: closed
author: matejsp
labels: []
assignees: []
created_at: 2023-07-13T11:41:21Z
updated_at: 2023-07-13T13:48:31Z
url: https://github.com/astral-sh/ruff/issues/5736
synced_at: 2026-01-07T13:12:15-06:00
---

# [FEATURE] Ruff support for removing obsolete and not needed noqa in the code

---

_Issue opened by @matejsp on 2023-07-13 11:41_

We have quite large codebase and have a lot of noqa in the code for various reasons.
When we refactor or format the code some noqa statements are no longer required.

I really like that you have better validation for noqa comments with latest ruff. We fixed a lot of invalid noqa statements in the code.

I propose a new feature (or cmd flag) that would run all the rules in the configuration and display warning if specific rule ignore was not needed.

for example:
```
@django_cache_decorators.never_cache  # noqa: C901
def some_function():
    return "a"
```

Since rule "Function is too complex (C901)" is no longer needed because this function does not need, I would like a warning that this particular noqa was not used. This should work only for RUFF know rules and should skip unknown (for example if they are not already implemented by ruff).

Display something along the lines:
```
warning: Invalid `# noqa` directive on line 459: this rule C901 is not needed in this line.
```

I understand that some rules are maybe 100% in sync with other tools and could pose a problem (just a warning is fine).
Alternative could be to include this in statistics somehow.


---

_Renamed from "[FEATURE] Ruff support for removing obsolete and not needed noqa" to "[FEATURE] Ruff support for removing obsolete and not needed noqa in the code" by @matejsp on 2023-07-13 11:41_

---

_Comment by @zanieb on 2023-07-13 13:35_

Hey @matejsp could you briefly clarify how this feature request is different from https://beta.ruff.rs/docs/rules/unused-noqa/?

---

_Comment by @matejsp on 2023-07-13 13:48_

Ha! that's exactly what I was looking for, but was searching at the wrong place. Ruff is awesome.

---

_Closed by @matejsp on 2023-07-13 13:48_

---
