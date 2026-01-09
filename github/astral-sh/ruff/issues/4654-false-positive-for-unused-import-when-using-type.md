---
number: 4654
title: False positive for unused import when using type as string with generic class behind TYPE_CHECKING flag
type: issue
state: closed
author: last-partizan
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-05-25T11:36:32Z
updated_at: 2023-06-01T22:19:39Z
url: https://github.com/astral-sh/ruff/issues/4654
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive for unused import when using type as string with generic class behind TYPE_CHECKING flag

---

_Issue opened by @last-partizan on 2023-05-25 11:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello, this happens when using django models with `django-types` and `ForeignKey["SomeModel"]`, but here's minimal example without django:

```python
from typing import TYPE_CHECKING, Generic, TypeVar


if TYPE_CHECKING:
    from pathlib import Path

T = TypeVar("T")


class ForeignKey(Generic[T]):
    ...


class Foo:
    var = ForeignKey["Path"]()
```

```sh
> ruff --version
ruff 0.0.270
> ruff --isolated /tmp/test.py
/tmp/test.py:5:25: F401 [*] `pathlib.Path` imported but unused
Found 1 error.
```

---

_Label `bug` added by @charliermarsh on 2023-05-25 14:12_

---

_Label `type-inference` added by @charliermarsh on 2023-05-25 14:12_

---

_Comment by @charliermarsh on 2023-05-25 14:14_

Ruff isn't able to do that level of reasoning right now (i.e., to know that `ForeignKey` is a generic, and so `"Path"` should be treated as a forward reference). We _could_ special-case members of `django-types` to support this.

---

_Comment by @last-partizan on 2023-05-27 07:18_

It would be nice.

If i understand correctly, `ruff` does not know about `django-types` either, so it would be django-specific special case.

How would it looks like?

I think it can be some option like `treat-as-generics = []` with default pointing to django `models.ForeignKey`, and maybe some others.

---

_Comment by @JonathanPlasse on 2023-05-27 08:07_

You can use the [typing-modules](https://beta.ruff.rs/docs/settings/#typing-modules) setting.
You will have to use a separate file to define your generics.

---

_Comment by @last-partizan on 2023-05-27 08:14_

@JonathanPlasse can you give an example?

In django-types ForeignKey is defined here:

https://github.com/sbdchd/django-types/blob/6e63b611329e5a8fc7b9e04e4a52e8c980734c9b/django-stubs/db/models/fields/related.pyi#L173



---

_Comment by @JonathanPlasse on 2023-05-27 08:48_

Sorry, I got it wrong, a setting needs to be added to extend the annotated subscripts.

---

_Comment by @JonathanPlasse on 2023-05-27 09:08_

I am working on adding the setting `extend-annotated-subscripts`.

---

_Referenced in [astral-sh/ruff#4677](../../astral-sh/ruff/pulls/4677.md) on 2023-05-27 09:49_

---

_Closed by @charliermarsh on 2023-06-01 22:19_

---

_Referenced in [astral-sh/ruff#15860](../../astral-sh/ruff/issues/15860.md) on 2025-02-01 17:05_

---
