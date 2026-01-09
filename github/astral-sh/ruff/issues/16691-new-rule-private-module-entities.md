---
number: 16691
title: "[new-rule] Private module entities"
type: issue
state: open
author: danmur97
labels:
  - rule
assignees: []
created_at: 2025-03-12T19:11:55Z
updated_at: 2025-03-13T20:26:04Z
url: https://github.com/astral-sh/ruff/issues/16691
synced_at: 2026-01-07T13:12:16-06:00
---

# [new-rule] Private module entities

---

_Issue opened by @danmur97 on 2025-03-12 19:11_

### Summary

# Description

- Almost any entity that is private (underscore prefixed) and is imported from a module should be denied
e.g. `from foo import _foo_function, _FooClass, _FOO_CONSTANT  # denied`
- This should not apply to modules themselves because a private module should be importable from its direct parent.
e.g. `from . import _direct_child_module   # allowed`
- Ensuring correct imports over modules is out of the scope of this issue.
e.g. `from .foo import _internal_module   # allowed but denied by other rule`

# Motivation

Underscore prefix on names commonly means private/internal definitions that are not intended to be used outside.
This is widely known for members of a class and enforced by the [SLF001](https://docs.astral.sh/ruff/rules/private-member-access/#private-member-access-slf001) lint rule. However, this private convention is not enforced at module level.

Both modules and classes can be used to group entities. Constants, variables, functions, and other classes can be defined within a module, some of which can also be private. The lack of a private-module-entity-access lint rule forces to replace a module with a wrapper class to enforce the private convention.

---

_Comment by @ntBre on 2025-03-12 20:34_

Does the [preview](https://docs.astral.sh/ruff/preview/) rule [`import-private-name`](https://docs.astral.sh/ruff/rules/import-private-name/#import-private-name-plc2701) (`PLC2701`) do what you want?

---

_Comment by @danmur97 on 2025-03-12 22:30_

Testing the PLC2701 rule, I only got `Private name import '_foo' from external module 'foo'` that only checks for illegal imports, taking `foo` as an external package.

The rule has to be adjusted or extended by this one to also check internal importations.
Also, by the description, it seems that all private entities are denied, when modules should be allowed (even if private).

---

_Label `rule` added by @ntBre on 2025-03-12 23:20_

---

_Comment by @Avasam on 2025-03-13 03:03_

> Also, by the description, it seems that all private entities are denied, when modules should be allowed (even if private).

I just happened to opened an issue for `PLC2701` flagging first-party private modules: https://github.com/astral-sh/ruff/issues/16694

---

_Referenced in [astral-sh/ruff#16694](../../astral-sh/ruff/issues/16694.md) on 2025-03-13 14:52_

---
