```yaml
number: 11233
title: "[`flake8-pyi`] Implement `PYI059` (`generic-not-last-base-class`)"
type: pull_request
state: merged
author: tusharsadhwani
labels: []
assignees: []
merged: true
base: main
head: pyi-059
created_at: 2024-05-01T18:24:10Z
updated_at: 2024-05-07T18:30:07Z
url: https://github.com/astral-sh/ruff/pull/11233
synced_at: 2026-01-10T22:05:26Z
```

# [`flake8-pyi`] Implement `PYI059` (`generic-not-last-base-class`)

---

_Pull request opened by @tusharsadhwani on 2024-05-01 18:24_

## Summary

Implements `Y059` from `flake8-pyi`: `typing.Generic[]` should be the last base in a class' bases list.

If Multiple `Generic[]` classes are found, we don't generate a fix for it.

## Test Plan

`cargo test` and `cargo insta review`.


---
