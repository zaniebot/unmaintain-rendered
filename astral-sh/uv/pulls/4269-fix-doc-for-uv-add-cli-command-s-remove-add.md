```yaml
number: 4269
title: "Fix doc for `uv add` cli command s/remove/add/"
type: pull_request
state: merged
author: ticosax
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-typo-docstring
created_at: 2024-06-12T13:35:41Z
updated_at: 2024-06-12T14:35:34Z
url: https://github.com/astral-sh/uv/pull/4269
synced_at: 2026-01-12T16:06:07Z
```

# Fix doc for `uv add` cli command s/remove/add/

---

_@ticosax_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix the docsting where `remove` was used instead of `add` in the context of `uv add` command.

## Test Plan

```
cargo run -- add --help
```
```
Add one or more packages to the project requirements

Usage: uv add [OPTIONS] <REQUIREMENTS>...

Arguments:
  <REQUIREMENTS>...
          The packages to add, as PEP 508 requirements (e.g., `flask==2.2.3`)

```


---

_@charliermarsh approved on 2024-06-12 13:36_

Thank you.

---

_Label `documentation` added by @charliermarsh on 2024-06-12 13:36_

---

_Merged by @charliermarsh on 2024-06-12 13:44_

---

_Closed by @charliermarsh on 2024-06-12 13:44_

---

_Branch deleted on 2024-06-12 14:35_

---
