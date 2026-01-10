```yaml
number: 10347
title: Add support for attribute docstrings
type: issue
state: open
author: tibdex
labels:
  - docstring
assignees: []
created_at: 2024-03-11T22:07:22Z
updated_at: 2025-12-05T07:55:45Z
url: https://github.com/astral-sh/ruff/issues/10347
synced_at: 2026-01-10T11:09:52Z
```

# Add support for attribute docstrings

---

_Issue opened by @tibdex on 2024-03-11 22:07_

The goal of this issue is to get Ruff to lint and format attribute docstrings as already done for class/method/function docstrings.

[PEP 224](https://peps.python.org/pep-0224) is about attribute docstrings. It was rejected but its proposed syntax (where a triple quote string can be placed under a class attribute to document it) has become a de facto standard:

- Pyright/Pylance/VS Code supports it (added in https://github.com/microsoft/pyright/issues/2106 and https://github.com/microsoft/pyright/commit/ea60aafef03d10a7f28c543b10941fcc4cd08a09).
- Sphinx supports it with the [`:autoattribute: directive`](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#directive-autoattribute).

For example, with this code:

```python
from dataclasses import dataclass

@dataclass
class Demo:
    """Demo."""

    value: int
    """The value."""
```

Sphinx outputs the expected HTML:

<img width="408" alt="Screenshot 2024-03-11 at 17 29 48" src="https://github.com/astral-sh/ruff/assets/6574550/f53e5313-1c6b-4322-a15d-5a0b6af8eed0">

and [`shpinx.ext.doctest`](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html) would even detect interactive Python sessions in `value`'s docstring and check them.

VS Code also shows the attribute docstring on hover:

<img width="250" alt="Screenshot 2024-03-11 at 17 33 38" src="https://github.com/astral-sh/ruff/assets/6574550/e31e4769-b0cb-4715-a598-24c5905b9db8">

On the other hand, with this code trying to document the attribute in a Google style docstring:

```python
from dataclasses import dataclass

@dataclass
class Demo:
    """Demo.

    Attributes:
        value: The value.
    """

    value: int
```

Sphinx, even with [sphinx.ext.napoleon](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html) correctly configured, duplicates the attribute:

<img width="421" alt="Screenshot 2024-03-11 at 17 29 03" src="https://github.com/astral-sh/ruff/assets/6574550/54ccc1a1-eb17-497c-908d-b8df3432b678">

and VS Code does not show the docstring on hover:

<img width="247" alt="Screenshot 2024-03-11 at 17 33 59" src="https://github.com/astral-sh/ruff/assets/6574550/7e815230-1948-4bd9-bfc8-8df7adfc3050">

Would you consider detecting this syntax as attribute docstrings and handling them like the other docstrings (e.g. formatting code examples in them)?

> [!NOTE]
> [PEP 727](https://peps.python.org/pep-0727) is a draft suggesting to document attributes like this:
> 
> ```python
> from typing import Annotated, Doc
> 
> class Demo:
>     value: Annotated[int, Doc("The value.")]
> ```
>
> [`typing-extensions` already provides it](https://typing-extensions.readthedocs.io/en/stable/#Doc) but Sphinx and VS Code do not support it.

---

_Label `docstring` added by @charliermarsh on 2024-03-11 22:30_

---

_Comment by @charliermarsh on 2024-03-11 22:30_

I'm generally supportive of this.

---

_Comment by @dhruvmanila on 2024-06-07 03:40_

Ruff can now detect this via https://github.com/astral-sh/ruff/pull/11315 although it's currently not being used anywhere (linter or formatter).

---

_Comment by @danielcjacobs on 2025-11-06 15:56_

> Ruff can now detect this via https://github.com/astral-sh/ruff/pull/11315 although it's currently not being used anywhere (linter or formatter).

Note, it seems ruff only handles attribute docstrings that are directly under the attribute.

It would be very nice to also have support for attribute docstrings placed in a google-style `Attributes:` section, as requested in the issue description.

---

_Comment by @DetachHead on 2025-12-05 07:55_

from #21805, this should also work on global variables (not just attributes in classes) as well as the new syntax for type aliases:

```py
foo = 1
"""docstring"""

type Foo = int
"""docstring"""
```

---
