```yaml
number: 10979
title: "pydocstyle: args/returns/raises should be sentences"
type: issue
state: open
author: adamjstewart
labels:
  - rule
  - docstring
assignees: []
created_at: 2024-04-16T16:36:19Z
updated_at: 2024-08-25T12:27:44Z
url: https://github.com/astral-sh/ruff/issues/10979
synced_at: 2026-01-10T11:09:53Z
```

# pydocstyle: args/returns/raises should be sentences

---

_Issue opened by @adamjstewart on 2024-04-16 16:36_

### Feature Request

I would like to propose a new pydocstyle rule that converts:
```python
"""My fancy function.

Args:
    foo: an arg

Returns:
    an output
"""
```
to:
```python
"""My fancy function.

Args:
    foo: An arg.

Returns:
    An output.
"""
```

### Rationale

All args/returns/raises/etc. in the Google style guide are sentences (they start with a capital letter and end with a period), but the style guide does not explicitly say whether or not this is required. Whether or not we include it in the "google" convention (this would be a change in behavior from pydocstyle) is less important to me. I just want an easy way to make all my docstrings consistent.

### Additional Information

This was briefly discussed in https://github.com/PyCQA/pydocstyle/issues/547 but a final decision was never reached. 

If this is something ruff would be willing to accept, I can try my hand at following https://docs.astral.sh/ruff/contributing/#example-adding-a-new-lint-rule and submit a PR!

---

_Label `docstring` added by @AlexWaygood on 2024-04-16 21:15_

---

_Label `rule` added by @AlexWaygood on 2024-04-16 21:15_

---

_Comment by @RubenVanEldik on 2024-08-25 12:27_

I ran into inconsistent formatting on this topic in our codebase as well! I think it would make sense to add this rule, even though Pydocstyle itself has never implemented it. Could one of the maintainers let us know if this is something that they agree with, or do we want Ruff to keep following Pydocstyle and not expanding it?

---
