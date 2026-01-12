```yaml
number: 22236
title: "PT006 doesn't allow any argnames besides tuple in pytest.mark.parametrize"
type: issue
state: closed
author: cgoldberg
labels:
  - question
assignees: []
created_at: 2025-12-28T16:39:35Z
updated_at: 2025-12-29T15:18:01Z
url: https://github.com/astral-sh/ruff/issues/22236
synced_at: 2026-01-12T15:54:58Z
```

# PT006 doesn't allow any argnames besides tuple in pytest.mark.parametrize

---

_@cgoldberg_

### Summary

If you use anything besides a tuple as the first argument to `pytest.mark.parametrize`, running ruff with `PT` enabled complains with:
 
```
PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
```

However, the first argument can be "A comma-separated string denoting one or more argument names, or a list/tuple of argument strings":

https://docs.pytest.org/en/stable/reference/reference.html#pytest.Metafunc.parametrize


For example, running `ruff check` against the following code will trigger `PT006` twice, even though both are valid: 

```
import pytest


@pytest.mark.parametrize(
    ["foo", "bar"],
    [
        ("a", "b"),
        ("c", "d"),
    ],
)
def test_list(foo, bar):
    pass


@pytest.mark.parametrize(
    "foo,bar",
    [
        ("a", "b"),
        ("c", "d"),
    ],
)
def test_string(foo, bar):
    pass
```

A comma-separated string or any sequence of strings should be allowed.

Playground link: https://play.ruff.rs/d0fa3c43-7fa2-4dee-ba48-ff9c96d721f8

### Version

ruff 0.14.10

---

_Comment by @ntBre on 2025-12-29 15:11_

Based on the [rule documentation](https://docs.astral.sh/ruff/rules/pytest-parametrize-names-wrong-type/), I believe this is the intention of the rule: to enforce a consistent style even if other forms are allowed. You can configure the style you want to enforce with [lint.flake8-pytest-style.parametrize-names-type](https://docs.astral.sh/ruff/settings/#lint_flake8-pytest-style_parametrize-names-type) or disable the rule if you prefer to vary the style.

---

_Label `question` added by @ntBre on 2025-12-29 15:11_

---

_Comment by @cgoldberg on 2025-12-29 15:18_

That sounds reasonable... I'll close this. 

---

_Closed by @cgoldberg on 2025-12-29 15:18_

---
