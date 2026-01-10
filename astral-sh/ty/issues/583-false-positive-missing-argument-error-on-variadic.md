```yaml
number: 583
title: "False positive \"missing-argument\" error on variadic function calls with unpacked arguments"
type: issue
state: closed
author: Ragug
labels: []
assignees: []
created_at: 2025-06-05T03:44:27Z
updated_at: 2025-06-05T06:20:48Z
url: https://github.com/astral-sh/ty/issues/583
synced_at: 2026-01-10T02:34:10Z
```

# False positive "missing-argument" error on variadic function calls with unpacked arguments

---

_Issue opened by @Ragug on 2025-06-05 03:44_

### Summary

**Description:**

I encountered this error when using the `ty` checker with a variadic function call pattern similar to the `and_` function from SQLAlchemy's source code [link here](https://github.com/sqlalchemy/sqlalchemy/blob/742a3a6f9b320514e51e8f882143665a8edf31b9/lib/sqlalchemy/sql/_elements_constructors.py#L117)

```python
def and_(initial_condition, *conditions):
    return " AND ".join((initial_condition,) + conditions)

filters = ["age > 18", "status = 'active'"]
if filters:
    print(and_(*filters))
if len(filters) > 0:
    print(and_(*filters))
```

The `ty` tool reports a `missing-argument` error on the lines calling `and_(*filters)`, even though `filters` is guaranteed to have at least one element in the current context.

**Error message:**

```
error[missing-argument]: No argument provided for required parameter `initial_condition` of function `and_`
```

**Why this seems to be a false positive:**

* The argument list is unpacked from `filters` which is checked for non-empty before the call.
* The function requires one positional argument and additional variadic arguments.
* The unpacked `filters` list provides all required arguments.
* The tool does not seem to consider the runtime length check or unpacking when validating arguments.

**Expected behavior:**

The `ty` checker should recognize that the unpacked arguments from `filters` satisfy the required `initial_condition` parameter, given that `filters` is verified to be non-empty.

**Additional info:**

* `ty` version: `ty 0.0.1-alpha.8`
* Python version: `3.12.3`
* Minimal repro code provided above.

Thanks for the great tool! Let me know if you need any more details.

### Version

ty 0.0.1-alpha.8

---

_Comment by @dhruvmanila on 2025-06-05 04:41_

Thank you for providing all the details! I think this might be due to missing support for starred arguments (https://github.com/astral-sh/ty/issues/247) which is mentioned as point 3 in #445.

---

_Comment by @Ragug on 2025-06-05 05:06_

Thanks for the quick response! That makes sense—looks like it's related to the starred arguments support, as you mentioned. I’ll keep an eye on (https://github.com/astral-sh/ty/issues/247)  and (https://github.com/astral-sh/ty/issues/445) for updates. Appreciate the great work on this tool!

---

_Closed by @MichaReiser on 2025-06-05 06:20_

---
