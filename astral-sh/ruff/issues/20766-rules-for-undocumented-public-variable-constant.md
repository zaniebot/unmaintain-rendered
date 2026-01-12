```yaml
number: 20766
title: Rules for Undocumented Public Variable / Constant and Class Member (attribute docstrings)
type: issue
state: open
author: phiresky
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-08T13:29:54Z
updated_at: 2025-10-08T17:47:28Z
url: https://github.com/astral-sh/ruff/issues/20766
synced_at: 2026-01-12T15:54:57Z
```

# Rules for Undocumented Public Variable / Constant and Class Member (attribute docstrings)

---

_@phiresky_

### Summary

[PEP 257 – Docstring Conventions (attribute docstrings)](https://peps.python.org/pep-0257/) defines a syntax for variable/member docstrings. The syntax is controversial, but it is supported by tools (e.g. VSCode).

Currently there is no lint to enforce global variables / constants to either be made private (underscore) or be documented.

So I'm imagining two new rules, very similar to the existing pydocstyle rules. The first one I think would be great, the second one is a bit difficult due to the various other ways that exist to document class members.


# 1. undocumented-public-module-variable
 ## What it does
 Checks for undocumented public module-level variable definitions.

 ## Why is this bad?
 Public module-level variables should be documented via docstrings to
 describe their purpose and expected values.

 According to PEP 257, docstrings for module-level variables should be
 placed immediately after the variable assignment.

 If the codebase adheres to a standard format for module-level variable
 docstrings, follow that format for consistency.

 ## Example
 ```python
 DEFAULT_TIMEOUT: int = 30
 MAX_CONNECTIONS: int = 100
 ```

 Use instead:
 ```python
 DEFAULT_TIMEOUT: int = 30
 """The default timeout duration in seconds."""

 MAX_CONNECTIONS: int = 100
 """The maximum number of concurrent connections allowed."""
 ```

# 2. undocumented-public-class-attribute
 ## What it does
 Checks for undocumented public class attribute definitions.

 ## Why is this bad?
 Public class attributes should be documented via docstrings to describe
 their purpose and expected values.

 According to PEP 257, docstrings for class attributes should be placed
 immediately after the attribute assignment.

 If the codebase adheres to a standard format for attribute docstrings,
 follow that format for consistency.

 ## Example
 ```python
 class Config:
     timeout: int = 30
     max_retries: int = 3
 ```

 Use instead:
 ```python
 class Config:
     timeout: int = 30
     """The timeout duration in seconds."""

     max_retries: int = 3
     """The maximum number of retry attempts."""
 ```



---

_Label `rule` added by @ntBre on 2025-10-08 15:17_

---

_Label `needs-decision` added by @ntBre on 2025-10-08 15:17_

---

_Comment by @my1e5 on 2025-10-08 15:20_

> [PEP 257 – Docstring Conventions (attribute docstrings)](https://peps.python.org/pep-0257/) defines a syntax for variable/member docstrings.

I believe the relevant PEP here is (the rejected) [PEP 224 – Attribute Docstrings](https://peps.python.org/pep-0224/)? 

---

_Comment by @phiresky on 2025-10-08 15:25_

PEP 224 was the first to mention it, but this text is included in PEP 257:

> String literals occurring elsewhere in Python code may also act as documentation. They are not recognized by the Python bytecode compiler and are not accessible as runtime object attributes (i.e. not assigned to `__doc__`), but two types of extra docstrings may be extracted by software tools:
> -    **String literals occurring immediately after a simple assignment at the top level of a module, class, or `__init__` method are called “attribute docstrings”.**
> -    String literals occurring immediately after another docstring are called “additional docstrings”.


ruff already (partially) models this:

https://github.com/astral-sh/ruff/blob/3771f1567c778776777679674b8cffdc90aa51aa/crates/ruff_python_semantic/src/model.rs#L2519-L2542

---

_Renamed from "Rules for Undocumented Public Variable / Constant and Class Member (attribute" to "Rules for Undocumented Public Variable / Constant and Class Member (attribute docstrings)" by @phiresky on 2025-10-08 17:43_

---

_Comment by @phiresky on 2025-10-08 17:47_

Added an implementation here: https://github.com/astral-sh/ruff/pull/20772

---
