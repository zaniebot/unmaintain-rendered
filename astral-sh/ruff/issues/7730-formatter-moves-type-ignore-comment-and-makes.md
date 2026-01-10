```yaml
number: 7730
title: "formatter moves `type:ignore` comment and makes line too long"
type: issue
state: closed
author: DetachHead
labels:
  - formatter
assignees: []
created_at: 2023-10-01T00:45:48Z
updated_at: 2023-12-04T06:05:08Z
url: https://github.com/astral-sh/ruff/issues/7730
synced_at: 2026-01-10T11:09:50Z
```

# formatter moves `type:ignore` comment and makes line too long

---

_Issue opened by @DetachHead on 2023-10-01 00:45_

# before
```py
def run_pytest(pytester, subprocess):
    result = (
        pytester.runpytest_subprocess 
        if subprocess else pytester.runpytest  # type:ignore[no-any-expr]
    )(*(pytest_args or []))
```
# after
```py
def run_pytest(pytester, subprocess):
    result = (
        pytester.runpytest_subprocess if subprocess else pytester.runpytest  # type:ignore[no-any-expr]
    )(*(pytest_args or []))

```
```
E501 Line too long (103 > 100 characters)
```
# playground
https://play.ruff.rs/720b0769-e4d7-430f-8e14-5c7295607d6d

---

_Comment by @charliermarsh on 2023-10-01 00:49_

This is an intentional behavior as we exclude pragmas when computing line length in the formatter: https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/README.md#pragma-comments-are-ignored-when-computing-line-width.

In the next release, we're planning to ignore them when enforcing E501 violations to make the linter consistent with this behavior.

---

_Comment by @charliermarsh on 2023-10-01 03:28_

I'm gonna close as fixed by https://github.com/astral-sh/ruff/pull/7692, but won't actually be fixed until we cut the next release.

---

_Closed by @charliermarsh on 2023-10-01 03:28_

---

_Label `formatter` added by @charliermarsh on 2023-10-01 03:28_

---

_Comment by @jonyscathe on 2023-12-04 06:05_

> This is an intentional behavior as we exclude pragmas when computing line length in the formatter: https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/README.md#pragma-comments-are-ignored-when-computing-line-width.

The example given there says that "Ruff will also avoid moving pragma comments" essentially to avoid suppressing more errors than intended.
However in the example above Ruff not only exceeds the line length, but also applies the type ignore to more code than was originally intended (benign here, but could suppress genuine errors in some cases)



---
