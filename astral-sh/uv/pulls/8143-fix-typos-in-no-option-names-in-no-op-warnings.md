```yaml
number: 8143
title: Fix typos in --no-... option names in no-op warnings
type: pull_request
state: merged
author: maxfriedrich
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-no-x-warnings
created_at: 2024-10-12T08:36:13Z
updated_at: 2024-10-12T12:53:19Z
url: https://github.com/astral-sh/uv/pull/8143
synced_at: 2026-01-10T12:54:03Z
```

# Fix typos in --no-... option names in no-op warnings

---

_Pull request opened by @maxfriedrich on 2024-10-12 08:36_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When trying out standalone scripts I noticed a warning that said `--no_readme` is a no-op when I provided `--no-readme`.

I searched for this "--\w+_" pattern in the codebase and found a similar typo in warnings in other places, so I fixed them here.

## Test Plan


no plan, since these commands are mainly for interactive use I would assume nobody parses out warnings about unnecessary options?

---

_@charliermarsh approved on 2024-10-12 12:53_

Thank you!

---

_Label `bug` added by @charliermarsh on 2024-10-12 12:53_

---

_Merged by @charliermarsh on 2024-10-12 12:53_

---

_Closed by @charliermarsh on 2024-10-12 12:53_

---
