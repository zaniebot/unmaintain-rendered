---
number: 14421
title: "Issue with new `PYI034` autofix when first param is annotated"
type: issue
state: closed
author: Avasam
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-11-18T03:14:26Z
updated_at: 2024-12-09T14:59:13Z
url: https://github.com/astral-sh/ruff/issues/14421
synced_at: 2026-01-10T01:22:55Z
---

# Issue with new `PYI034` autofix when first param is annotated

---

_Issue opened by @Avasam on 2024-11-18 03:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"




-->

Thanks @InSyncWithFoo for adding an autofix in https://github.com/astral-sh/ruff/pull/14217 .

* A minimal code snippet that reproduces the bug.

I did notice an issue that I think should be possible to handle. This case:
```py
class Container(tuple):
    def __new__(cls: Type[Container], *args, **kwargs) -> Container: ...
```

gets autofixed to
```py
class Container(tuple):
    def __new__(cls: Type[Container], *args, **kwargs) -> Self: ...
```

Which causes type error:
```
error: "Self" cannot be used in a function with a `self` or `cls` parameter that has a type annotation other than "Self" (reportGeneralTypeIssues)
```

I think it should be:
```py
class Container(tuple):
    def __new__(cls, *args, **kwargs) -> Self: ...
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

`ruff check --select=PYI034 --fix --preview --unsafe-fixes --isolated`

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

None

* The current Ruff version (`ruff --version`).

ruff 0.7.4


---

_Comment by @InSyncWithFoo on 2024-11-18 03:39_

Admittedly, the fix is unsafe here (and, indeed, it was marked as such), but I'm not sure if this falls within the scope of `PYI034`. I would say it is closer to [`PYI019`](https://docs.astral.sh/ruff/rules/custom-type-var-return-type/).

---

_Comment by @Avasam on 2024-11-18 04:00_

I don't think this falls under PYI019, there are no typevar used here.

Ruff probably doesn't need to try and fix code like `def __new__(cls: Type[Container], *args, **kwargs) -> Self: ...` when it's encountered in the wild, as it's invalid to begin with. So I'm not asking for a rule to fix it here, *but* it shouldn't produce it either if possible.

The autofix is indeed unsafe, as there are cases where you wouldn't want to use `Self`. But assuming this is not one of those cases, it's correct to attempt an autofix here, the current implementation just doesn't yet handle it.

---

However (and if) this case ends up being handled, both known reasons for this fix to be unsafe could be documented.
1. Intentionally returning a specific class from `__new__`
2. May introduce type issues, for instance if the `self`/`cls` param is annotated)

---

_Comment by @Avasam on 2024-11-18 04:06_

Maybe Ruff *should* have a rule to disallow (and remove) non-typevar annotations on `self`/`cls`? 🤔 
That'd solve the issue and this issue could be closed with only documenting the fix safety.

---

_Comment by @MichaReiser on 2024-11-18 07:06_

@InSyncWithFoo If I recall, then https://github.com/astral-sh/ruff/pull/14238 handles type annotations in `self`/`cls` positions correctly. Any concerns with detecting type annotation for `self`/`cls` that can't be replaced with `Self` and bailing out of the fix?

---

_Label `bug` added by @MichaReiser on 2024-11-18 07:06_

---

_Label `fixes` added by @MichaReiser on 2024-11-18 07:06_

---

_Comment by @InSyncWithFoo on 2024-11-18 08:28_

I think it would only be safe to remove the annotation (not just replace) when:

* `self` is annotated with `Container` and `cls` with `type[Container]`.
* `Container` is <em>not</em> generic.

Consider this contrived example:

```python
# Before
class Container[T]:
    def __init__(self, v: T, /) -> None: ...
    def map(self: Container, mapper: Callable, /) -> Container: ...
#                 ^^^^^^^^^     `Container[Any]`     ^^^^^^^^^

# After
class Container[T]:
    def __init__(self, v: T, /) -> None: ...
    def map(self, mapper: Callable, /) -> Self: ...
#                          `Container[T]` ^^^^
```

```python
c = Container(0)  # Container[int]

# Before
reveal_type(c.map(lambda _: ''))  # Container[Any]
# After
reveal_type(c.map(lambda _: ''))  # Container[int]
```

---

_Referenced in [astral-sh/ruff#14801](../../astral-sh/ruff/pulls/14801.md) on 2024-12-06 00:13_

---

_Closed by @AlexWaygood on 2024-12-09 14:59_

---
