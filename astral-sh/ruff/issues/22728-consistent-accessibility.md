```yaml
number: 22728
title: Consistent accessibility
type: issue
state: open
author: AndreuCodina
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2026-01-19T17:23:50Z
updated_at: 2026-01-21T19:36:59Z
url: https://github.com/astral-sh/ruff/issues/22728
synced_at: 2026-01-21T20:04:00Z
```

# Consistent accessibility

---

_@AndreuCodina_

### Summary

A rule should prevent public functions that return **private classes**.

For example, the following code has a public class with a public method returning a private class.

```python
class _Array:
    pass


class Numpy:
    def create_array(self) -> _Array:
        return _Array()
```

That forces you to import a private class.

In C#:

<img width="779" height="298" alt="Image" src="https://github.com/user-attachments/assets/55a0e424-8cc3-48e9-af84-00073f069d46" />

**Private modules and folders** should also be covered. For example, in my libraries, I have lots of files/folders prefixed with an underscore (`_async_concurrent_dictionary.py`, `_service_lookup/_service_provider_engine.py`...), containing classes without an underscore. This can be considered the equivalent of `internal` of C#.
This is important because I import those classes from `/src` and `/tests`, but I don't want user libraries to see or import the code.
This issue is related to this topic: https://github.com/astral-sh/ruff/issues/16892

**Aliases** should be covered too (https://github.com/astral-sh/ruff/issues/20916).

---

_Label `rule` added by @ntBre on 2026-01-19 17:40_

---

_Label `needs-decision` added by @ntBre on 2026-01-19 17:40_

---

_Comment by @ntBre on 2026-01-19 17:43_

Thanks! I would probably lean toward adding a single rule that handles both this and #20916, unless there's a desire to keep them separate. I think I was picturing that we would flag any private type in a public interface, like the rustc lint in my other [comment](https://github.com/astral-sh/ruff/issues/20916#issuecomment-3415533744).

---

_Comment by @MichaReiser on 2026-01-19 17:52_

The main challenge here is probably going to decide on what's considered a public type. Is it all types that aren't prefixed or only those that are accessible from outide the module/package?

---

_Comment by @AndreuCodina on 2026-01-19 17:59_

> The main challenge here is probably going to decide on what's considered a public type. Is it all types that aren't prefixed or only those that are accessible from outide the module/package?

For me:
- If a folder has an underscore, all inside is private.
- If a file has an underscore, all inside is private.
- And so on.

It's easier to think about it as the `internal` modifier of C# instead of `private`: https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/internal

Exception: Generics, given they have more capabilities with TypeVar/TypeVarTuple/... than using the short syntax.

---

_Comment by @flying-sheep on 2026-01-19 21:15_

We also have a lot of instances where we do

```python
# reused private aliases
type _Foo = Literal["a", "b"]
type _Bar = int | str

# a public function
def do_it(foo: _Foo) -> _Bar: ...
```

which then renders in our docs as intended:

> **do_it**(foo: Literal["a", "b"]) -> int | str

So while it would make sense to avoid exposing private *classes*, types starting with an underscore aren’t all that private in many cases.

---

_Comment by @MichaReiser on 2026-01-20 07:09_

> So while it would make sense to avoid exposing private classes, types starting with an underscore aren’t all that private in many cases.

Although this may require a configuration knob. I don't know if this is also an issue in the Python community but I always found it very frustrating when I used a TypeScript library, where a function takes an argument or returns a value that is typed as a union, and the library even defines a type alias for it, but I can't reuse it. Instead, I have to copy paste their definition. 

---
