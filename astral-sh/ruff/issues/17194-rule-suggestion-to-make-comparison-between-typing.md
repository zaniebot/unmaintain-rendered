```yaml
number: 17194
title: Rule suggestion to make comparison between typing_extensions vs. typing more future proof
type: issue
state: open
author: Daraan
labels:
  - rule
assignees: []
created_at: 2025-04-04T07:47:59Z
updated_at: 2025-11-18T18:40:14Z
url: https://github.com/astral-sh/ruff/issues/17194
synced_at: 2026-01-12T15:54:55Z
```

# Rule suggestion to make comparison between typing_extensions vs. typing more future proof

---

_@Daraan_

### Summary

Using backported objects from `typing_extensions` as comparison can lead to false negatives e.g.:

```python
from typing_extensions import TypeAliasType

type Foo = int  # a typing.TypeAliasType
print(isinstance(Foo, TypeAliasType))  # False on 3.12
```

There are not many such cases in `typing_extensions` but for example the introduction the `TypeAliasType` backport in `typing_extensions` 4.13 caused some problems in major libraries;
e.g.,

- https://github.com/pydantic/pydantic/issues/10711
- https://github.com/sqlalchemy/sqlalchemy/issues/12473

`typing_extensions` does some neat tricks for certain objects like `ParamSpec` to be equivalent to their typing variant in `isinstance` checks - for other cases it cannot do more than warn *in their documentation*:

> Always check for both the [typing](https://docs.python.org/3/library/typing.html#module-typing) and typing_extensions versions of objects, even if they are currently the same on some Python version. Future typing_extensions releases may re-export a separate version of the object to backport some new feature or bugfix.

# Rule Proposal:

To write more future proof code and to avoid such errors. A rule could warn when using a `typing_extensions` object together with `isinstance`, `issubclass` or `is` and suggest to use the `typing` variant for these tests if available; given that one does not *explicitly* want to check for the backport variant.

## How to avoid to many errors, false positives?

I am thinking of keeping a whitelist and a possible candidate list. Whitelist objects are safe to use for comparison, e.g. `typing_extensions.ParamSpec`. Candidates are classes where a future incompatible backport is likely or are already known to be problematic. These lists would need some active maintenance looking at the current state of `typing_extensions` and possible upcoming PEPs. 

---

# Possible extensions, more related rules

Some backports, like https://github.com/python/typing_extensions/issues/103, do not work on all python versions. Another rule could scan for specific open issues when using certain python versions.

---

_Label `rule` added by @ntBre on 2025-04-04 18:07_

---

_Comment by @nstarman on 2025-11-18 18:40_

I think this would be great.

On a related note, I'd love an easy way to forbid the use of any imports from `typing_extensions` that aren't in a real version of Python. E.g. `typing_extensions.Doc` is for a rejected PEP and `typing_extensions.Sentinel` is in a deferred PEP. 



---
