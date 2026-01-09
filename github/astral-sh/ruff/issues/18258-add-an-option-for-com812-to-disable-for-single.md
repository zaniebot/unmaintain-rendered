---
number: 18258
title: Add an option for COM812 to disable for single-argument functions?
type: issue
state: open
author: davidt
labels:
  - rule
  - configuration
  - needs-decision
assignees: []
created_at: 2025-05-22T17:25:02Z
updated_at: 2025-06-03T18:11:44Z
url: https://github.com/astral-sh/ruff/issues/18258
synced_at: 2026-01-07T13:12:16-06:00
---

# Add an option for COM812 to disable for single-argument functions?

---

_Issue opened by @davidt on 2025-05-22 17:25_

### Summary

I really want to be able to use the COM812 rule, but our codebase contains a ton of code like the following:

```python
some_string = gettext(
    'This is a long string which '
    'is marked for internationalization'
)

another_function(
    some_long +
    expression +
    gets_passed
)
```

While trailing commas do make a lot of sense for function calls that contain multiple arguments, for functions that take a single argument, requiring them muddies the syntax. I'd love to have an option for the flake8-commas plugin (something like `[lint.flake8-commas] allow-single-arg-function-calls = true`)

I'm happy to do the implementation for this, but I wanted to check in first to see if this is something that would be accepted.

---

_Comment by @MichaReiser on 2025-05-22 17:32_

This seems the same as https://github.com/astral-sh/ruff/issues/13410 and IMO, the same reasoning applies in the sense that this is the exact intention of the rule.

---

_Label `rule` added by @MichaReiser on 2025-05-22 17:32_

---

_Comment by @davidt on 2025-05-22 18:07_

Fundamentally, it does look like the same request.

I agree that attempting to do a full static analysis and determine if the callee's signature supports only a single argument is not a good approach, but I'd argue that for the sake of code style, I don't care if there are possible optional arguments (or indeed the potential for possible optional arguments in the future) on the definition side, I only care what's happening at the point of the call.

Adding another argument to such a call would mean needing to add the (now missing) comma, which I agree does affect the diff, but that happens rarely, if ever (gettext's API hasn't changed and I would bet money it never will). In contrast, for things like lists, dicts, or imports, those sorts of changes can happen frequently and optimizing for the diff by requiring the comma makes sense.

Extracting these sorts of args into variables works in some cases, but explicitly does not work with the `gettext()` example, because the scanner that extracts strings into .po files is simple and requires the string to be directly inside the call.

---

_Label `configuration` added by @MichaReiser on 2025-05-23 14:04_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-23 14:04_

---

_Comment by @davidt on 2025-06-03 18:11_

I have a working implementation of a setting for this. Should I spend the time to clean up the change and put it up for review?

Thanks!

---

_Referenced in [astral-sh/ruff#21567](../../astral-sh/ruff/pulls/21567.md) on 2025-11-21 18:14_

---
