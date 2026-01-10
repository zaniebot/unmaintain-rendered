```yaml
number: 950
title: Print fully qualified type names in type checker errors
type: issue
state: closed
author: missingdays
labels: []
assignees: []
created_at: 2025-08-07T09:25:11Z
updated_at: 2025-08-07T09:47:00Z
url: https://github.com/astral-sh/ty/issues/950
synced_at: 2026-01-10T02:06:24Z
```

# Print fully qualified type names in type checker errors

---

_Issue opened by @missingdays on 2025-08-07 09:25_

Consider this example

```python
class A:
    class B:
        pass

class C:
    class B:
        pass


a: A.B = C.B()
```

Here, `ty` reports `Object of type B is not assignable to B`. This message does not help fixing the error.

This this example, it's easy to see the problem. However, consider if two `B` classes were coming from different packages. In that case, it would be much harder to understand why the error is happening.

Printing the fully qualified name, with packages, module and inner classes would improve the error message while keeping the error message clear.

---

_Comment by @missingdays on 2025-08-07 09:25_

I will be happy to send a merge request for this. However, I couldn't find a proper way to get the fully qualified name of a type, with packages and all.

---

_Comment by @MichaReiser on 2025-08-07 09:35_

Makes sense.

We should probably avoid doing this all the time and only print the full qualified name if the name is ambiguous in the current file (e.g. there are multiple symbols with the same name or the type isn't even imported)

---

_Comment by @AlexWaygood on 2025-08-07 09:47_

Thanks for the report! I think we can fold this into #848, but this is a great additional example of why we need to improve our diagnostics

---

_Closed by @AlexWaygood on 2025-08-07 09:47_

---
