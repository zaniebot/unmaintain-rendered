```yaml
number: 7401
title: "pydocstyle: Ignore matching functions"
type: issue
state: open
author: JP-Ellis
labels:
  - docstring
  - needs-decision
assignees: []
created_at: 2023-09-15T06:25:12Z
updated_at: 2024-02-27T16:27:16Z
url: https://github.com/astral-sh/ruff/issues/7401
synced_at: 2026-01-12T15:54:47Z
```

# pydocstyle: Ignore matching functions

---

_@JP-Ellis_

## Summary

I would like the ability to ignore functions matching a pattern for `pydocstyle`. This is primarily motivated by the definition of test functions which, due to their number and straightforwardness, need not be documented in general. This would be especially useful in cases where some other functions within the same file need to be documented.

I'm thinking this could be achieved as follows:

```toml
[tool.ruff.pydocstyle]
ignore = [
  "test_*",
  "*_test"
]
```

## Motivation

Requiring all publicly facing functions to have documentation I think can be a very good; however, I am finding myself writing a test suite and this requirement is becoming a hinderance in cases where I want some functions documented, and some are just test functions.

For example, when defining [pytest fixtures](https://docs.pytest.org/en/latest/explanation/fixtures.html), I would prefer to require these functions to be documented properly; while simultaneously allowing test functions to remain undocumented.

## Alternatives

### Ignore Rule in directory / files

One option is to simply adjust the ignored rules for the tests directory,

```toml
extend = "../pyproject.toml"
ignore = ["D103"]
```

or making use of `per-file-ignores`.

This option is not ideal in cases were some code within these files/directories should be documented (as explained above).

### Per-line Ignore

It is of course possible to add `# noqa: D103` after each test function. This achieves the desired results at the cost of being very verbose.



---

_Label `docstring` added by @charliermarsh on 2023-09-19 03:30_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-19 03:30_

---

_Comment by @charliermarsh on 2023-09-19 03:45_

It seems reasonable to me, though if others would find this useful, I'd love for them to chime in. Every configuration option adds some burden on us as maintainers and as users, so it's important that said options are broadly useful.

---

_Comment by @volodymyrkir on 2024-01-21 12:49_

I'd agree with the thread author: such functionality would be great for Python testing lingering and supporting.

---

_Comment by @johhnry on 2024-02-27 16:27_

Hi, agree on this one. I have a codebase with hundred of test functions and I don't want them to be documented because the function name is descriptive enough and the test document itself.

---
