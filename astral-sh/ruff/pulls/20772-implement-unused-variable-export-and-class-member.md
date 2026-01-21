```yaml
number: 20772
title: "implement unused variable export and class member (fixes #20766)"
type: pull_request
state: closed
author: phiresky
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: unused-extras
created_at: 2025-10-08T17:47:11Z
updated_at: 2026-01-21T10:27:20Z
url: https://github.com/astral-sh/ruff/pull/20772
synced_at: 2026-01-21T10:59:59Z
```

# implement unused variable export and class member (fixes #20766)

---

_@phiresky_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements two new rules as described in #20766.

Main questions are:
1. where to put visibility check code and __all__ parsing
2. where to put rules (new group or the existing group where it very clearly would fit in)

Example:

```python
# Should trigger D1002 - undocumented public module variable (annotated)
public_var: int = 1

# Should NOT trigger - has docstring
documented_var: str = "hello"
"""This variable is documented."""

# Should NOT trigger - private variable
_private_var: int = 2

class MyClass:
    # Should trigger D1001 - undocumented public class attribute (annotated)
    class_var: int = 10

    # Should NOT trigger - has docstring
    documented_class_var: str = "test"
    """This class variable is documented."""
```

## Test Plan

Tested with snapshots.




---

_Comment by @ntBre on 2025-10-08 19:52_

Thank you for working on this! However, I think we should try to reach a decision on whether we want to include this rule in Ruff before moving onto an implementation. I probably should have mentioned this explicitly on the issue, but that's why I added the `needs-decision` label (and typically add that label to any new rule request).

Every rule adds some maintenance burden, and in my experience the docstring rules are especially error prone. At a minimum we should wait a little while and see if others are interested in this rule. I'll also approve the workflow run so we can get a sense for the ecosystem impact, but that can be another useful factor.

I searched a bit for previous issues and only really found https://github.com/astral-sh/ruff/issues/12434. I think there could be some overlap with the attribute-related pydoclint rules that haven't been implemented yet.

---

_Label `rule` added by @ntBre on 2025-10-08 19:52_

---

_Label `needs-decision` added by @ntBre on 2025-10-08 19:52_

---

_Closed by @MichaReiser on 2026-01-21 10:27_

---
