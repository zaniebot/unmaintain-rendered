```yaml
number: 22201
title: "Warn or autofix \"return <Exception>\""
type: issue
state: open
author: andrewwillowen
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-26T06:05:41Z
updated_at: 2025-12-30T11:43:48Z
url: https://github.com/astral-sh/ruff/issues/22201
synced_at: 2026-01-10T11:10:00Z
```

# Warn or autofix "return <Exception>"

---

_Issue opened by @andrewwillowen on 2025-12-26 06:05_

### Summary

I spent an hour today debugging a test with pytest because I didn't realize that I had used `return TypeError('blah')` when I should have used `raise TypeError('blah')`. 

I'm not sure (but willing to be corrected) if there is a case where one would want to `return` the Exception instead of `raise` -ing the exception. If there is, then would be good to throw a warning. 
If not, then would be better to autofix so that `return` becomes `raise` instead.

Closest existing issue I found was https://github.com/astral-sh/ruff/pull/21571 .

Looks like I'm using ruff 0.14.10. Happy to provide more info if helpful.

---

_Comment by @ntBre on 2025-12-26 16:11_

I could have sworn we had a rule for this, but I was probably thinking of [useless-exception-statement (PLW0133)](https://docs.astral.sh/ruff/rules/useless-exception-statement/#useless-exception-statement-plw0133). I'll put `needs-decision` on this for now to give others a chance to weigh in (cc @amyreese), but this makes sense to me as a new rule!

---

_Label `rule` added by @ntBre on 2025-12-26 16:11_

---

_Label `needs-decision` added by @ntBre on 2025-12-26 16:11_

---

_Comment by @MichaReiser on 2025-12-29 09:46_

One use case where one might want to return an error are error factories like:


```py
def error_for_status(code: int) -> HTTPError:
    match code:
        case 404:
            return NotFoundError("Not found")
        case 403:
            return ForbiddenError("Forbidden")
        case _:
            return HTTPError(f"HTTP {code}")
```


This is also something a type checker can easily detect, though it might still make sense to treat it as a separate rule for functions without a return type annotation, that don't override any parent method.

---

_Comment by @ntBre on 2025-12-29 13:44_

We could also limit the check to functions that return both an exception and a non-exception. That's similar to what [implicit-return-value (RET502)](https://docs.astral.sh/ruff/rules/implicit-return-value/#implicit-return-value-ret502) and [implicit-return (RET503)](https://docs.astral.sh/ruff/rules/implicit-return/#implicit-return-ret503) do for `None`.

---

_Comment by @pjonsson on 2025-12-30 11:43_

I know I have written code in the last couple of years that returns an exception for the failure cases, and something else for success (morally an `Either` type). That put the complicated "decoding" logic in one place, and callers could then raise the exception where it made sense.

I don't see why the factory example or my example would be bad code, but if it's enough to add a type signature to avoid warnings I guess it doesn't really matter if Ruff warns on everything that looks even vaguely unintentional.

---
