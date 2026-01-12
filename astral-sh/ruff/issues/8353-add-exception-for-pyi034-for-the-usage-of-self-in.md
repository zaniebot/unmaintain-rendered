```yaml
number: 8353
title: Add exception for PYI034 for the usage of Self in metaclasses
type: issue
state: closed
author: ZeeD
labels:
  - bug
assignees: []
created_at: 2023-10-30T10:51:16Z
updated_at: 2023-11-13T00:47:49Z
url: https://github.com/astral-sh/ruff/issues/8353
synced_at: 2026-01-12T15:54:48Z
```

# Add exception for PYI034 for the usage of Self in metaclasses

---

_@ZeeD_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

minimal example:

```python
from __future__ import annotations
from typing import Any, Self
class Meta(type):
    def __new__(cls,
                name: str,
                bases: tuple[type, ...],
                classdict: dict[str, Any]) -> Meta:
        return super().__new__(cls, name, bases, classdict)
```

in this dummy metaclass - if I run `ruff` enabling the rule `PYI034` I got

```
PYI034 `__new__` methods in classes like `Meta` usually return `self` at runtime
```

Unfortunately I think this is one of the exceptions in the usual cases the error talks about :)

If I annotate `__new__` with `Self` as return type, in fact, `mypy` tells me

```
error: Self type cannot be used in a metaclass  [misc]
```

(as you can see in https://github.com/python/mypy/blob/master/mypy/typeanal.py#L688-L689 )

Could you refine `PYI034` to ignore the `Self` requirements for metaclasses?

---

_Label `bug` added by @charliermarsh on 2023-10-30 13:24_

---

_Comment by @charliermarsh on 2023-10-30 23:42_

Interesting, flake8-pyi reports the same thing here. (\cc @AlexWaygood, you may be interested in this.)

---

_Comment by @AlexWaygood on 2023-10-31 00:12_

> Interesting, flake8-pyi reports the same thing here. (\cc @AlexWaygood, you may be interested in this.)

Yup. Personally I think it's somewhat unfortunate that [PEP 673](https://peps.python.org/pep-0673/) mandates this behaviour for metaclasses — but it does, and it's pretty unlikely that that's going to change any time soon. So mypy is definitely correct here; this is indeed a false positive from flake8-pyi.

The somewhat-hacky workaround we use in typeshed that keeps both flake8-pyi and mypy happy here is to just use a normal TypeVar that happens to be called Self(!): https://github.com/python/typeshed/blob/f7aa7b709a826ed34f52b1de3f7095f90f349a9c/stdlib/abc.pyi#L14-L19

Possibly flake8-pyi could add some heuristics here to try to detect if a class is a metaclass, and avoid emitting this error if so. I'm not sure if it would be worth it for us, though, since:

1. Metaclasses in general are pretty rare anyway, especially in stubs
2. I think it might significantly complicate our implementation of the check
3. Since we're a linter, not a type checker, it's pretty hard for us to reliably detect whether a class is a metaclass or not :(

Ruff isn't necessarily bound by the same constraints, though, so if you can see a way of avoiding this false positive, then I'd say go ahead!

---

_Comment by @dhruvmanila on 2023-10-31 01:50_

Thanks @AlexWaygood for taking the time to look at the issue!

> Ruff isn't necessarily bound by the same constraints, though, so if you can see a way of avoiding this false positive, then I'd say go ahead!

I think one solution would be to update the semantic model to include a new `META_CLASS` flag to be set when we encounter a metaclass and use that while checking for the rule violation. But, @charliermarsh will know more about this.

---

_Comment by @AlexWaygood on 2023-10-31 10:58_

Here's my best attempt at fixing this for flake8-pyi: https://github.com/PyCQA/flake8-pyi/pull/436. The check's already pretty complicated, so it didn't end up significantly complicating the implementation after all :)

But it would be great if ruff could fix this without the hacky heuristics that I'm using there ;)

---

_Comment by @JelleZijlstra on 2023-10-31 15:08_

Does Ruff run this check in `.py` files? `flake8-pyi` is designed only for pyi files, and I'm not sure this kind of check should be applied to `.py` files, where the type checker can see the body and therefore can provide more feedback.

---

_Comment by @AlexWaygood on 2023-10-31 15:51_

I actually think this rule could still be useful for `.py` files. If you have a class like this, a type checker won't necessarily emit an error on any of the `-> Foo` return annotations, since none of them are, strictly speaking, _incorrect_. But the annotations could also perhaps be _better_, and fixing them as soon as the class is written could save the author of the code some refactoring later on:

```py
from collections.abc import Iterator

class Foo(Iterator[int]):
    def __new__(cls) -> Foo:
        return cls()

    def __iter__(self) -> Foo:
        return self

    def __next__(self) -> int:
        return 42

    def __enter__(self) -> Foo:
        return self
```

---

_Comment by @charliermarsh on 2023-10-31 15:54_

(We do run this over `.py` files. We don't run _all_ `flake8-pyi` rules over `.py` files, but we do run a subset, and this one is included.)

---

_Comment by @AlexWaygood on 2023-11-09 18:44_

> Here's my best attempt at fixing this for flake8-pyi: [PyCQA/flake8-pyi#436](https://github.com/PyCQA/flake8-pyi/pull/436).

This is included in the [latest flake8-pyi release](https://github.com/PyCQA/flake8-pyi/releases/tag/23.11.0) FYI! Thanks for the ping @charliermarsh :D

---

_Comment by @charliermarsh on 2023-11-09 19:30_

Awesome, thanks @AlexWaygood! You rock.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-12 21:50_

---

_Closed by @charliermarsh on 2023-11-13 00:47_

---
