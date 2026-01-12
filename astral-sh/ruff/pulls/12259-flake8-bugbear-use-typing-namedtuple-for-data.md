```yaml
number: 12259
title: "[`flake8-bugbear`] Use typing.NamedTuple for data classes that only set attributes in an __init__ method (`B903`)"
type: pull_request
state: closed
author: jkauerl
labels: []
assignees: []
base: main
head: simple-init-classes
created_at: 2024-07-09T15:46:19Z
updated_at: 2025-01-07T17:35:21Z
url: https://github.com/astral-sh/ruff/pull/12259
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-bugbear`] Use typing.NamedTuple for data classes that only set attributes in an __init__ method (`B903`)

---

_@jkauerl_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
It semi-implements rule B903 from flake8-bugbear. The implementation is working but I need some clarifications regarding what is considerd a class that only set attributes in an  `__init__` method. This was implemented because it is rule to be implemented in #3758.

## Test Plan

<!-- How was it tested? -->
I implementeed the same test suite as the one located in flake8-bugbear, as such the should behave the same way.

Test suite:
```python
class NoWarningsMoreMethods:
    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar

    def other_function(self): ...


class NoWarningsClassAttributes:
    spam = "ham"

    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar


class NoWarningsComplicatedAssignment:
    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar
        self.spam = " - ".join([foo, bar])


class NoWarningsMoreStatements:
    def __init__(self, foo, bar):
        foo = " - ".join([foo, bar])
        self.foo = foo
        self.bar = bar


class Warnings:
    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar


class WarningsWithDocstring:
    """A docstring should not be an impediment to a warning"""

    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar
```

I want to add more test cases because the current implementation is a generalization.


---

_Marked ready for review by @jkauerl on 2024-07-16 09:01_

---

_Renamed from "[`flake8-bugbear`] Use typing.NamedTuple for data classes that only set attributes in an __init__ method (`B902`)" to "[`flake8-bugbear`] Use typing.NamedTuple for data classes that only set attributes in an __init__ method (`B903`)" by @AlexWaygood on 2024-07-18 12:32_

---

_Comment by @dylwil3 on 2025-01-07 17:35_

The earlier PR #9601 ended up implementing this rule, which unfortunately makes this a duplicate. (You couldn't have known, since that PR was originally implementing a closely related `pylint` rule).

Apologies for the outcome and the long delay in resolving this, but thank you for your contribution and interest in Ruff - I hope this doesn't discourage you from contributing in the future!

---

_Closed by @dylwil3 on 2025-01-07 17:35_

---
