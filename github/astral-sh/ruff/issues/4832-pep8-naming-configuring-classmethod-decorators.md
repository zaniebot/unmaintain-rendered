---
number: 4832
title: "pep8-naming: configuring classmethod-decorators"
type: issue
state: closed
author: olliemath
labels: []
assignees: []
created_at: 2023-06-03T13:37:42Z
updated_at: 2023-06-04T02:03:38Z
url: https://github.com/astral-sh/ruff/issues/4832
synced_at: 2026-01-07T13:12:14-06:00
---

# pep8-naming: configuring classmethod-decorators

---

_Issue opened by @olliemath on 2023-06-03 13:37_

# Issue

Currently ruff (0.0.270) does not seem to respect the classmethod-decorators option for pep8-naming. However, in the past this setting was respected. The regression seems to have been introduced in the **0.0.220 -> 0.0.221** release.

# Reproducing  example

With the following snippet (see below for pyproject), ruff 0.0.220 correctly identifies an error only on the "bad" method, whereas current ruff identifies both methods as incorrect:

```python
def extra_classmethod(method):
    return classmethod(method)

class Foo:
    @extra_classmethod
    def good(cls):  # should be ok
        ...

    def bad(cls):  # should be flagged
        ...
```

Here's the pyproject.toml (options taken from docs at https://beta.ruff.rs/docs/settings/#classmethod-decorators):

```toml
[tool.ruff]
select = ['N']
[tool.ruff.pep8-naming]
classmethod-decorators = ['extra_classmethod']
```

---

_Comment by @charliermarsh on 2023-06-03 15:55_

We don't support decorators that are defined in the same file right now, because `classmethod-decorators` takes a list of fully-qualified symbols, not a list of string names or regular expressions.

That's why, e.g., you need to use `pydantic.validator`, so that we respect:

```py
import pydantic as foo

class Foo:
    @foo.validator
    def good(cls):
        ...
```

If you move your decorator out to a separate file, and then use the fully-qualified path instead of the string name, it should work. Ideally we'd let you use the fully-qualified path for symbols in the module they're defined (e.g., if your file above is `foo.py`, you would specify `["foo.extra_classmethod"]`), but I'd still want to use fully-qualified paths rather than string names.

---

_Comment by @olliemath on 2023-06-03 16:31_

OK, I understand - yeah it works if I move out to another file. 

It would be nice if we could write `foo.extra_classmethod`, but I guess it's quite a small edge case with the in-file method

---

_Comment by @charliermarsh on 2023-06-04 02:03_

I'm going to close for now since the same-file thing is a little bit of a bigger issue than this specific question.

---

_Closed by @charliermarsh on 2023-06-04 02:03_

---
