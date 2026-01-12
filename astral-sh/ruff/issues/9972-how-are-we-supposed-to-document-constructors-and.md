```yaml
number: 9972
title: How are we supposed to document constructors and how does this affect the class documentation?
type: issue
state: open
author: nbro10
labels: []
assignees: []
created_at: 2024-02-13T10:56:37Z
updated_at: 2024-06-28T09:10:21Z
url: https://github.com/astral-sh/ruff/issues/9972
synced_at: 2026-01-12T15:54:49Z
```

# How are we supposed to document constructors and how does this affect the class documentation?

---

_@nbro10_

This is my first time trying ruff. I'm interested in `ruff` mostly because I'm interested in rust. Our codebases aren't so big that using ruff would lead to a significant improvement in development efficiency, so we could also stick with PyLint, flake8, black and isort, in case ruff does not satisfy our usual needs. Anyway, I have a few questions, all related to docstrings.

Ruff version: 0.2.1

Ruff settings in `pyproject.toml`

```
[tool.ruff]
line-length = 100
show-fixes = true

[tool.ruff.lint]
select = ["D"]
ignore = ["D104"]

[tool.ruff.lint.pydocstyle]
convention = "google"

[tool.ruff.lint.per-file-ignores]
# Ignore some rules for specific files

[tool.ruff.format]
quote-style = "double"
```

Example:

This example actually illustrates more than one problem that I'd like to ask about.

```
# example.py

"""
A module.
"""


class A:
    """
    A class.
    """

    def __init__(self, a):
        """

        Args:
            a: The parameter.
        """

```

Now, if we run `ruff check example.py`, we will get these warnings

```
example.py:3:1: D200 One-line docstring should fit on one line
example.py:3:1: D212 [*] Multi-line docstring summary should start at the first line
example.py:9:5: D200 One-line docstring should fit on one line
example.py:9:5: D212 [*] Multi-line docstring summary should start at the first line
example.py:14:9: D205 1 blank line required between summary line and description
example.py:14:9: D212 [*] Multi-line docstring summary should start at the first line
Found 6 errors.
```

Btw, I don't get any warnings if I run pylint, which is strange, but I know that `ruff` and `pylint` are not really compatible right now. Here, with pylint, I'm only suppressing `missing-module-docstring` and `too-few-public-methods`.

Now, I understand the D200 warnings, because [PEP257][1] suggests the following

> The closing quotes are on the same line as the opening quotes. This looks better for one-liners.

However, if I run `ruff` with the option `--fix`, it will produce this ugliness (which is inconsistent with the suggestion above):

```
# example.py

"""A module.
"""


class A:
    """A class.
    """

    def __init__(self, a):
        """Args:
        a: The parameter.
        """
```

Clearly, I was not expecting e.g. 

```
"""
A module.
"""
```

to turn into this ugliness

```
"""A module.
"""
```

but 

```
"""A module."""
```

**First of all, why is ruff acting like this? Is this because of a rule, or is this a bug? If this is not a bug, is there a way to tell ruff to format this properly, i.e. have one-liners be one-liners?** In any case, this is inconsistent with [PEP257](https://peps.python.org/pep-0257/)

Second, you can see that the docstrings of the `__init__` method also turned into another ugliness

```
        """Args:
        a: The parameter.
        """
```

**Is this also bug? Can we avoid this?** I suppose yes, by ignoring the initial warning, but this feels like a bug because nobody will ever want this format by default - it looks horrible.

Another question - **How are we supposed to document `__init__` methods, according to ruff and according to the Python conventions?** PyLint never complained that I didn't give a description to the `__init__` method, as far as I remember. This PyLint behavior is desirable, assuming that we already provide a description for the class, which is the case here.

This leads me to another question - **how are we supposed to document the class when the `__init__` method also has parameters, according to ruff and the Python conventions?** I think documenting the `__init__` method when it has no parameters is completely useless. Everyone knows that the `__init__` method is supposed to initialise. But when it has parameters, we very often want to document those parameters somewhere, which can be in the `__init__` method or in the class docstring.

Note: I'd like my docstrings to be compatible with Sphinx, which leads me to my last question - **is ruff compatible with sphinx in general? If not, what are the known incompatibilities?**

 [1]: https://peps.python.org/pep-0257/

---

_Comment by @tmke8 on 2024-02-13 20:59_

The `D` warnings come from [pydocstyle](https://github.com/PyCQA/pydocstyle), not pylint.

Sphinx can take the documentation both from the class docstring or the `__init__` docstring. You can control which with this setting: https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autoclass_content By default Sphinx only looks at the class docstring.

The fixes do look like they might be bugs.

---

_Comment by @butterlyn on 2024-06-28 09:10_

Seconding this as an issue, it's very unusual to require a `__init__` docstring description if the class already has one.

---
