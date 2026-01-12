```yaml
number: 12140
title: "[`pycodestyle`] Whitespace after decorator (`E204`)"
type: pull_request
state: merged
author: jkauerl
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: whitespace-after-decorator
created_at: 2024-07-01T13:44:52Z
updated_at: 2024-07-04T23:31:04Z
url: https://github.com/astral-sh/ruff/pull/12140
synced_at: 2026-01-12T15:55:40Z
```

# [`pycodestyle`] Whitespace after decorator (`E204`)

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
This is the implementation for the new rule of `pycodestyle (E204)`. It follows the guidlines described in the contributing site, and as such it has a new file named `whitespace_after_decorator.rs`, a new test file called `E204.py`, and as such invokes the `function` in the `AST statement checker` for functions and functions in classes. Linking #2402 because it has all the pycodestyle rules.

## Test Plan

<!-- How was it tested? -->
The file E204.py, has a `decorator` defined called wrapper, and this decorator is used for 2 cases. The first one is when a `function` which has a `decorator` is called in the file, and the second one is when there is a `class` and 2 `methods` are defined for the `class` with a `decorator` attached it.

Test file:

``` python
def foo(fun):
    def wrapper():
        print('before')
        fun()
        print('after')
    return wrapper

# No error
@foo
def bar():
    print('bar')

# E204
@ foo
def baz():
    print('baz')

class Test:
    # No error
    @foo
    def bar(self):
        print('bar')

    # E204
    @ foo
    def baz(self):
        print('baz')
```

I am still new to rust and any suggestion is appreciated. Specially with the way im using native ruff utilities.

---

_Marked ready for review by @jkauerl on 2024-07-03 10:37_

---

_Renamed from "[pycodestyle] Whitespace after decorator (E204)" to "[`pycodestyle`] Whitespace after decorator (`E204`)" by @jkauerl on 2024-07-04 10:48_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-07-04 23:11_

---

_Label `rule` added by @charliermarsh on 2024-07-04 23:11_

---

_Label `preview` added by @charliermarsh on 2024-07-04 23:11_

---

_@charliermarsh approved on 2024-07-04 23:21_

Thanks! I added a fix too.

---

_Merged by @charliermarsh on 2024-07-04 23:31_

---

_Closed by @charliermarsh on 2024-07-04 23:31_

---
