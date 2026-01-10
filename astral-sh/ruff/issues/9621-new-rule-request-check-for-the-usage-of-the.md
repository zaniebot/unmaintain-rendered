```yaml
number: 9621
title: "[New rule request] Check for the usage of the `@pytest.mark.only` decorator"
type: issue
state: open
author: vdusek
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-01-23T07:47:57Z
updated_at: 2024-01-23T10:06:17Z
url: https://github.com/astral-sh/ruff/issues/9621
synced_at: 2026-01-10T11:09:51Z
```

# [New rule request] Check for the usage of the `@pytest.mark.only` decorator

---

_Issue opened by @vdusek on 2024-01-23 07:47_


There is such a rule for Flake8 - [flake8-no-pytest-mark-only](https://github.com/fnesveda/flake8-no-pytest-mark-only).

Motivation (copied from [fnesveda/flake8-no-pytest-mark-only](https://github.com/fnesveda/flake8-no-pytest-mark-only?tab=readme-ov-file#rationale)):

> The @pytest.mark.only decorator, coming from the [pytest-only](https://pypi.org/project/pytest-only/) plugin, is great. It's super useful when developing your tests, so that you can run just the test (or set of tests) that you're working on, without having to run your whole test suite.
> 
> The only problem with it is that when you forget to remove it after you've finished developing your tests, and you accidentally commit it to your repository, it's hard to notice that only a subset of tests are running in CI instead of the whole test suite.
> 
> This Flake8 plugin prevents that, by raising a lint error on usages of the @pytest.mark.only decorator, so that you notice that you've committed changes with the decorator in them.

cc @fnesveda

---

_Label `rule` added by @dhruvmanila on 2024-01-23 10:06_

---

_Label `needs-decision` added by @dhruvmanila on 2024-01-23 10:06_

---
