```yaml
number: 17547
title: Simplify test cases by removing unnecessary version pins
type: issue
state: open
author: gaardhus
labels:
  - enhancement
assignees: []
created_at: 2026-01-17T09:10:17Z
updated_at: 2026-01-17T09:10:17Z
url: https://github.com/astral-sh/uv/issues/17547
synced_at: 2026-01-17T10:08:04Z
```

# Simplify test cases by removing unnecessary version pins

---

_@gaardhus_

### Summary

## Description

Many test cases in the `pip_freeze` test suite (and potentially other test suites) explicitly pin package versions when installing dependencies. Since the test suite uses `exclude-newer`, these version pins are unnecessary and could be removed to simplify the tests.

## Context

This was identified during review of #17045. The original test case used version pins (e.g., `MarkupSafe==2.1.3`, `tomli==2.0.1`), but the reviewer noted that these aren't necessary because the test suite already uses `exclude-newer` to ensure reproducible builds.

## Proposed Solution

- Audit test cases in `crates/uv/tests/it/pip_freeze.rs` (and potentially other test files)
- Remove explicit version pins from `pip install` and `pip sync` commands where they're not specifically testing version behavior
- Rely on `exclude-newer` for test reproducibility instead

## Benefits

- Simpler, more maintainable test code
- Tests are less brittle to upstream package updates
- Makes it clearer when version pins are intentional vs. unnecessary

## References

- PR #17045 (comment thread: https://github.com/astral-sh/uv/pull/17045/files#r2614920808)
- Related comment: https://github.com/astral-sh/uv/pull/17045/files#r2606801916

### Example

_No response_

---

_Label `enhancement` added by @gaardhus on 2026-01-17 09:10_

---
