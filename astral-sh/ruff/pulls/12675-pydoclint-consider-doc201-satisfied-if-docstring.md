```yaml
number: 12675
title: "[`pydoclint`] Consider `DOC201` satisfied if docstring begins with \"Returns\""
type: pull_request
state: merged
author: augustelalande
labels:
  - docstring
  - preview
assignees: []
merged: true
base: main
head: doc201
created_at: 2024-08-05T02:54:34Z
updated_at: 2024-08-06T17:30:47Z
url: https://github.com/astral-sh/ruff/pull/12675
synced_at: 2026-01-10T21:47:02Z
```

# [`pydoclint`] Consider `DOC201` satisfied if docstring begins with "Returns"

---

_Pull request opened by @augustelalande on 2024-08-05 02:54_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #12636

Consider docstrings which begin with the word "Returns" as having satisfactorily documented they're returns. For example
```python
def f():
    """Returns 1."""
    return 1
```
is valid.

## Test Plan

Added example to test fixture.


---

_Label `docstring` added by @charliermarsh on 2024-08-05 02:54_

---

_Label `preview` added by @charliermarsh on 2024-08-05 02:54_

---

_Comment by @github-actions[bot] on 2024-08-05 03:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-08-05 03:19_

Should this only be applied to single-line docstrings?

---

_Comment by @augustelalande on 2024-08-05 03:22_

The [google document](https://google.github.io/styleguide/pyguide.html#doc-function-returns) is not specific. I leave it up to you.

---

_Comment by @augustelalande on 2024-08-05 03:23_

Also I guess this should also apply to yield. I'll add that.

---

_@dhruvmanila reviewed on 2024-08-05 03:25_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:656 on 2024-08-05 03:25_

We should also check for "Return" (no 's') according to the docs (https://google.github.io/styleguide/pyguide.html#doc-function-returns)

> It may also be omitted if the docstring starts with “Return”, “Returns”, “Yield”, or “Yields”

---

_@augustelalande reviewed on 2024-08-05 03:25_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:656 on 2024-08-05 03:25_

Ya was gonna do that as well

---

_Comment by @augustelalande on 2024-08-05 03:26_

I think technically
>and the opening sentence is sufficient to describe the return value.

implies that there may be more than one sentence.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-08-06 06:26_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC402_google.py`:79 on 2024-08-06 06:28_

```suggestion
    yield 1


# OK
def f(num: int):
    """
    Yields 1.

    Args:
        num (int): A number
    """
    yield 1
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC201_google.py`:100 on 2024-08-06 06:29_

```suggestion
    return 1


# OK
def f(num: int):
    """
    Returns 1.

    Args:
        num (int): A number
    """
    return 1
```

---

_@dhruvmanila reviewed on 2024-08-06 06:29_

---

_@dhruvmanila approved on 2024-08-06 06:30_

I added test cases for multiline docstring for both "Returns" and "Yields".

---

_Merged by @dhruvmanila on 2024-08-06 06:46_

---

_Closed by @dhruvmanila on 2024-08-06 06:46_

---

_Branch deleted on 2024-08-06 17:30_

---
