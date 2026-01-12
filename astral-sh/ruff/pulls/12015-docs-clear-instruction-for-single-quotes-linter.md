```yaml
number: 12015
title: "[Docs] Clear instruction for single quotes (linter and formatter)"
type: pull_request
state: merged
author: jackdesert
labels:
  - documentation
assignees: []
merged: true
base: main
head: single-quote-docs
created_at: 2024-06-24T15:44:42Z
updated_at: 2024-07-10T16:29:30Z
url: https://github.com/astral-sh/ruff/pull/12015
synced_at: 2026-01-12T15:55:40Z
```

# [Docs] Clear instruction for single quotes (linter and formatter)

---

_@jackdesert_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In order to use single quotes with both the ruff linter and the ruff formatter, 
two different rules must be applied. This was not clear to me when 
internet searching "configure ruff single quotes" and it eventually
I filed this issue:

https://github.com/astral-sh/ruff/issues/12003



## Test Plan

Please advise how to preview this documentation change.





---

_@charliermarsh reviewed on 2024-06-26 01:21_

---

_Review comment by @charliermarsh on `docs/configuration.md`:200 on 2024-06-26 01:21_

I'd prefer not to introduce this complexity in the example... What if we change "5. Use single quotes for non-triple-quoted strings." to something like "5. Use single quotes in `ruff format`."?

---

_@charliermarsh reviewed on 2024-06-26 01:22_

---

_Review comment by @charliermarsh on `docs/configuration.md`:200 on 2024-06-26 01:22_

(Most users won't / shouldn't need the `flake8-quotes` rules at all, especially if they're using the formatter.)

---

_Label `documentation` added by @charliermarsh on 2024-07-10 16:27_

---

_Merged by @charliermarsh on 2024-07-10 16:29_

---

_Closed by @charliermarsh on 2024-07-10 16:29_

---
