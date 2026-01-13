```yaml
number: 18119
title: Tests using fixtures prefixed with underscore (_) are flagged as violating PT019
type: issue
state: open
author: ckutlu
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-05-15T13:28:36Z
updated_at: 2026-01-13T16:35:49Z
url: https://github.com/astral-sh/ruff/issues/18119
synced_at: 2026-01-13T17:25:29Z
```

# Tests using fixtures prefixed with underscore (_) are flagged as violating PT019

---

_@ckutlu_

### Summary

I would expect

``` python
import pytest


@pytest.fixture
def _bar():
    return 42


def test_bar_by_foo(_bar):
    assert _bar / 7 == 6
```

to pass PT019 because it's using `_bar` inside `test_bar_by_foo`, but

``` sh
ruff --version && ruff check foo.py
```
says

```
ruff 0.11.8
foo.py:9:21: PT019 Fixture `_bar` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
   |
 9 | def test_bar_by_foo(_bar):
   |                     ^^^^ PT019
10 |     assert _bar / 7 == 6
   |

Found 1 error.
```

### Version

ruff 0.11.8

---

_Closed by @MichaReiser on 2025-05-15 13:37_

---

_Reopened by @MichaReiser on 2025-05-15 13:37_

---

_Comment by @ntBre on 2025-05-15 18:50_

This does feel like a false positive since the rule docs say:

> If the test function depends on the fixture being activated, **but does not use it in the test body** or otherwise rely on its return value

but it's also consistent with the upstream linter:

```shell
> uvx --with flake8-pytest-style flake8 try.py
try.py:5:1: PT005 fixture '_bar' returns a value, remove leading underscore
try.py:9:1: PT019 fixture _bar without value is injected as parameter, use @pytest.mark.usefixtures instead
```

We don't currently check if the argument is actually used in the function body.

---

_Label `rule` added by @ntBre on 2025-05-15 18:50_

---

_Label `needs-decision` added by @ntBre on 2025-05-15 18:50_

---

_Comment by @manueljacob on 2026-01-13 11:38_

I was very confused by the error message because it was unclear to me what a “fixture without value” might be, especially in a case when the parameter gets passed a value and it’s actually used inside the test.

## Current implementation is based on obsoleted convention

With the upstream linter, rules PT004 and PT005 check that a fixture’s name begins with an underscore if and only if it doesn’t return a value. These rules were implemented in ruff, but deprecated in 7defc0d136792ee52b24da0077f50cb25e809938 and removed in c4007257132a4be0ea09040acd1a8c86e7a09109 because it “isn't a practice recommended by the pytest community”. The decision was made in #8796.

If I understand #8796 correctly, the point was that the leading underscore should be used for showing whether the fixture is private or not rather than whether it returns a value or not (because these two aspects are orthogonal, it can only be used for either). When we dropped rules PT004 and PT005 for that reason, we should also have dropped rule PT019 as implemented currently.

## Rationale of rule and partial overlap with other rules

The rationale stated in the [upstream rule documentation](https://github.com/m-burst/flake8-pytest-style/blob/v2.2.0/docs/rules/PT019.md#rationale) is “to avoid unused parameters in test functions”. There are other ruff rules that check for unused parameters. In the context of these rules, the leading underscore has a different meaning to the two discussed above: it means that the parameter was intentionally defined _although_ unused. That means that, if a fixture whose name has a leading underscore (whether or not that means it returns a value or is private) is used, these rules won’t trigger. Another issue is that, when these rules trigger, the user is not made aware of the option to use `@pytest.mark.usefixtures`; therefore, they’ll probably silence these rules for that line.

So it might be valuable to have a rule which is similar to PT019 but checks whether the fixture value is used inside the test function or not instead of whether its name begins with an underscore or not. I am unsure however whether it should be a modification of the existing rule PT019 or a new rule.

---

_Comment by @ntBre on 2026-01-13 16:35_

Thanks for your analysis and comments! I think I agree with you overall that PT019 is outdated. A rule like ARG001 could catch this more reliably because it actually checks for usage of the parameter, but it will give a less specific recommendation, or even a misleading recommendation, if it suggests to prefix the parameter name with an underscore.

I see a couple of options here:
- retain the PT019 rule code but reuse the ARG001 implementation and choose which of the two rules to emit based on a heuristic (the PT variant in a module that starts with `test_`, in a function that starts with `test_`, etc)
- deprecate PT019 and customize the ARG001 diagnostic (and docs) based on similar heuristics

It seems that we have an existing implementation of this heuristic in [`is_likely_pytest_test`](https://github.com/astral-sh/ruff/blob/55a335165ab126d50edb89e775c0c5c340cb8ae3/crates/ruff_linter/src/rules/flake8_pytest_style/helpers.rs#L66).

But I'm curious what others think!

---
