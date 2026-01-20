```yaml
number: 22762
title: "[pylint] Implement unused-private-member (PLW0238)"
type: pull_request
state: open
author: mostaszewski
labels: []
assignees: []
base: main
head: feat/plw0238-unused-private-member
created_at: 2026-01-20T13:23:54Z
updated_at: 2026-01-20T13:23:54Z
url: https://github.com/astral-sh/ruff/pull/22762
synced_at: 2026-01-20T13:37:58Z
```

# [pylint] Implement unused-private-member (PLW0238)

---

_@mostaszewski_

## Summary

Implements Pylint rule W0238 (`unused-private-member`) which detects unused private class members (methods and class variables starting with `__` but not ending with `__`).

## Test Plan

Added comprehensive test fixture with 38 test cases covering:
- Unused private methods and class variables
- Dunder methods (correctly excluded)
- Static/class methods, async methods, generators
- Nested classes, multiple inheritance
- Property descriptors, dataclasses
- Known limitations documented (self/cls attribute access)

## Known Limitations

This rule does not detect usage via `self.__member` or `cls.__member` attribute access, as these are tracked as attribute accesses rather than references to the class-level binding. This is documented in the rule's docstring.

---
