```yaml
number: 10803
title: "[`refurb`] Do not allow any keyword arguments for `read-whole-file` in `rb` mode (`FURB101`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: read-whole-file-minor-bug
created_at: 2024-04-06T16:33:33Z
updated_at: 2024-04-22T18:01:54Z
url: https://github.com/astral-sh/ruff/pull/10803
synced_at: 2026-01-12T15:55:33Z
```

# [`refurb`] Do not allow any keyword arguments for `read-whole-file` in `rb` mode (`FURB101`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

`Path.read_bytes()` does not support any keyword arguments, so `FURB101` should not be triggered if the file is opened in `rb` mode with any keyword arguments.

## Test Plan

Move erroneous test to "Non-error" section of fixture.

---

_Renamed from "[`refurb`] Do not allow any keyword arguments for `read-whole-file` in `rb` mode" to "[`refurb`] Do not allow any keyword arguments for `read-whole-file` in `rb` mode (`FURB101`)" by @augustelalande on 2024-04-06 16:34_

---

_@charliermarsh approved on 2024-04-06 16:34_

---

_Label `bug` added by @charliermarsh on 2024-04-06 16:41_

---

_Merged by @charliermarsh on 2024-04-06 16:41_

---

_Closed by @charliermarsh on 2024-04-06 16:41_

---

_Comment by @github-actions[bot] on 2024-04-06 16:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2024-04-22 18:01_

---
