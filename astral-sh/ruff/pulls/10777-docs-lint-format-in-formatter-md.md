```yaml
number: 10777
title: "docs: `Lint` -> `Format` in formatter.md"
type: pull_request
state: merged
author: SnirBroshi
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-04-04T16:48:19Z
updated_at: 2024-04-04T16:50:55Z
url: https://github.com/astral-sh/ruff/pull/10777
synced_at: 2026-01-10T22:47:03Z
```

# docs: `Lint` -> `Format` in formatter.md

---

_Pull request opened by @SnirBroshi on 2024-04-04 16:48_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Since #10217 the [formatter docs](https://docs.astral.sh/ruff/formatter/) contained

```
ruff format                   # Format all files in the current directory.
ruff format path/to/code/     # Lint all files in `path/to/code` (and any subdirectories).
ruff format path/to/file.py   # Format a single file.
```

I believe the `Lint` here is a copy-paste typo from the [linter docs](https://docs.astral.sh/ruff/linter/).

## Test Plan

N/A


---

_@charliermarsh approved on 2024-04-04 16:49_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-04-04 16:49_

---

_Merged by @charliermarsh on 2024-04-04 16:50_

---

_Closed by @charliermarsh on 2024-04-04 16:50_

---
