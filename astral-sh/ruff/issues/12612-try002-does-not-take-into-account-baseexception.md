```yaml
number: 12612
title: "`TRY002` does not take into account `BaseException`"
type: issue
state: closed
author: epenet
labels: []
assignees: []
created_at: 2024-08-01T13:17:47Z
updated_at: 2024-08-05T08:45:50Z
url: https://github.com/astral-sh/ruff/issues/12612
synced_at: 2026-01-10T11:09:54Z
```

# `TRY002` does not take into account `BaseException`

---

_Issue opened by @epenet on 2024-08-01 13:17_

Spotted in https://github.com/home-assistant/core/pull/123021

It seems that pylint doesn't like Exception nor BaseException
https://pylint.pycqa.org/en/latest/user_guide/messages/warning/broad-exception-raised.html

But ruff is quite happy to let BaseException pass

I think both should trigger a warning, same as regular Exception

It should be an easy fix in https://github.com/astral-sh/ruff/blob/a3e67abf4ce7b519152c03ff441424c18e4c18ee/crates/ruff_linter/src/rules/tryceratops/rules/raise_vanilla_class.rs#L71

---

_Renamed from "TRY002 does not take into account BaseException" to "`TRY002` does not take into account `BaseException`" by @epenet on 2024-08-01 13:37_

---

_Comment by @dylwil3 on 2024-08-01 20:39_

I think Ruff is correctly reproducing the rule from [tryceratops](https://github.com/guilatrova/tryceratops), which is implemented [here](https://github.com/guilatrova/tryceratops/blob/0c6b8d5f4b0f447d5e59f804352020b233bdd4b9/src/tryceratops/analyzers/call.py#L27C1-L28C1).

The pylint rule [--overgeneral-exceptions](https://pylint.readthedocs.io/en/stable/user_guide/configuration/all-options.html#overgeneral-exceptions) used to coincide with this rule but has since changed in two ways:

- The rule is now configurable, so the user can specify which exceptions are too general.
- The default configuration includes `BaseException`.

So I think this needs a decision from the maintainers. Should we:

1. Add `BaseException` to the TRY002 violation and call it a day (this would disagree with tryceratops).
2. Create a new rule in Pylint (not sure how to find the code) where `Exception` and `BaseException` both give violations.
3. Same as (2) but make it configurable as in Pylint.
4. Same as (3) and then also remove/redirect TRY0002.
5. Something else.

---

_Closed by @AlexWaygood on 2024-08-05 08:45_

---
