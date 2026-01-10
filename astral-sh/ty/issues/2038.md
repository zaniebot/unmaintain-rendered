```yaml
number: 2038
title: "error[unresolved-attribute]: Object of type `(...) -> Any` has no attribute `__name__`"
type: issue
state: closed
author: kracekumar
labels: []
assignees: []
created_at: 2025-12-17T21:04:54Z
updated_at: 2025-12-18T23:18:23Z
url: https://github.com/astral-sh/ty/issues/2038
synced_at: 2026-01-10T01:53:59Z
```

# error[unresolved-attribute]: Object of type `(...) -> Any` has no attribute `__name__`

---

_Issue opened by @kracekumar on 2025-12-17 21:04_

### Summary

The Python callable has attribute `__name__`but ty reports as unresolved-attribute.

Snippet

```python
def export(defn: Callable[..., Any]) -> Callable[..., Any]:
    """Decorator to explicitly mark functions that are exposed in a lib."""
    globals()[defn.__name__] = defn
    __all__.append(defn.__name__)
    return defn

```

Diagnostics

```
error[unresolved-attribute]: Object of type `(...) -> Any` has no attribute `__name__`
  --> litecli/packages/special/__init__.py:12:15
   |
10 | def export(defn: Callable[..., Any]) -> Callable[..., Any]:
11 |     """Decorator to explicitly mark functions that are exposed in a lib."""
12 |     globals()[defn.__name__] = defn
   |               ^^^^^^^^^^^^^
13 |     __all__.append(defn.__name__)
14 |     return defn
   |
info: rule `unresolved-attribute` is enabled by default
```

mypy: https://mypy-play.net/?mypy=latest&python=3.9&gist=8fca5c11fe239843ba39dc08aa0132ab
ty: https://play.ty.dev/01eb2208-8168-444c-a0b7-81bc20be124d

### Version

ty 0.0.2 (4283557 2025-12-16) Python: 3.9

---

_Comment by @carljm on 2025-12-17 21:12_

Thanks for the report! ty is alerting you to a real problem in your code here: not all callables have a `__name__` attribute. Functions and lambdas do, but there are other ways to create a callable in Python:

```pycon
>>> class C:
...     def __call__(self):
...         return None
...
>>> C().__name__
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    C().__name__
AttributeError: 'C' object has no attribute '__name__'. Did you mean: '__ne__'?
```

You can make your code robust against this by checking `isinstance(defn, types.FunctionType)`, and ty will then be happy: https://play.ty.dev/725cbad9-14e7-4804-bf57-310cd118c217

There's further discussion in https://github.com/astral-sh/ty/issues/1495 of ways we could improve this experience in ty.

---

_Closed by @carljm on 2025-12-17 21:12_

---

_Comment by @kracekumar on 2025-12-18 00:49_

Thank you for the link and suggestion. I think the `if` check wouldn't hurt. 

---

_Comment by @kracekumar on 2025-12-18 13:49_

I ended up using `isinstance check of callabale and hasattr(defn, "__name__")`. 

```python
def export(defn: Callable[..., Any]) -> Callable[..., Any]:
    """Decorator to explicitly mark functions that are exposed in a lib."""
    # ty, not all callable has __name__, so check it's has attr __name__
    if isinstance(defn, Callable) and hasattr(defn, "__name__")::
        # ty, suspects some time the __name__ can refer to an object, so explicitly converting to string to prevent it, error[invalid-argument-type].
        globals()[str(defn.__name__)] = defn
        __all__.append(str(defn.__name__))
    return defn

```

### Example declarations

1. Class decorator

```python

@export
class ArgumentMissing(Exception):
    pass
```

2. function decorator

```python
@export
def parse_special_command(sql: str) -> tuple[str, "Verbosity", str]:
    """
    Parse a special command, extracting the base command name, verbosity
    (normal, verbose (+), or succinct (-)), and the remaining argument.
    Mirrors the behavior used in similar CLI tools.
    """
    command, _, arg = sql.partition(" ")
    verbosity = Verbosity.NORMAL
    if "+" in command:
        verbosity = Verbosity.VERBOSE
    elif "-" in command:
        verbosity = Verbosity.SUCCINCT
    command = command.strip().strip("+-")
    return (command, verbosity, arg.strip())

```
3. nested decorator

```python
@export
@special_command(
    "nopager",
    "\\n",
    "Disable pager, print to stdout.",
    arg_type=NO_QUERY,
    aliases=("\\n",),
    case_sensitive=True,
)
def disable_pager() -> list[tuple]:
    set_pager_enabled(False)
    return [(None, None, None, "Pager disabled.")]

```

---

_Comment by @sharkdp on 2025-12-18 18:52_

> I ended up using `isinstance check of callabale and hasattr(defn, "__name__")`.

Some alternative solutions to this are discussed here: https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-say-callable-has-no-attribute-__name__

In particular, I don't think you need the `isinstance(defn, Callable)` check? If you require `.__name__` to be of type `str`, you should consider using `if isinstance(defn, types.FunctionType):`

> Example declarations

I'm not sure what you are trying to show with those examples?

---

_Comment by @carljm on 2025-12-18 19:00_

> I'm not sure what you are trying to show with those examples?

I think the relevant thing we can take from the examples is that this decorator is also used on classes, which is valid because a class is Callable, but would not pass the `types.FunctionType` isinstance check. But classes do also have `__name__`. I think the nicest solution in that case is `if isinstance(defn, (type, types.FunctionType)):`, which also [works for ty](https://play.ty.dev/45677d0a-6500-4370-92e3-6aa970f02662).

But if you want to support any arbitrary callable object which happens to have a `__name__` attribute, then the `hasattr` solution makes sense. (Though I agree checking `isinstance(defn, Callable)` is redundant.)

---

_Comment by @kracekumar on 2025-12-18 23:18_

@sharkdp apologies for not being verbose. The intention of the examples were to show that there are other types compared to `FunctionType` that contains `__name__`. So in the examples, all of these fall into the types: `FunctionType or type`. So agreed, checking the callable is redundant and `isinstance(defn, (type, FunctionType)):`is sufficient!

Than you both!

---
