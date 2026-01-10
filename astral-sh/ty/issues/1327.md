```yaml
number: 1327
title: "`dataclass_transform`: feature overview"
type: issue
state: open
author: sharkdp
labels:
  - dataclasses
assignees: []
created_at: 2025-10-09T13:37:25Z
updated_at: 2025-12-19T11:20:38Z
url: https://github.com/astral-sh/ty/issues/1327
synced_at: 2026-01-10T01:53:59Z
```

# `dataclass_transform`: feature overview

---

_Issue opened by @sharkdp on 2025-10-09 13:37_

Link to the specification: https://typing.python.org/en/latest/spec/dataclasses.html

* Support all "flavors" of dataclass-transformers
  * [x] Support `@dataclass_transform`-decorated functions (decorators), including overloaded ones
  * [x] Support `@dataclass_transform`-decorated metaclasses
  * [x] Support `@dataclass_transform`-decorated classes
* [x] Support all parameters to `dataclass_transform`
  * [x] `eq_default`
  * [x] `order_default`
  * [x] `kw_only_default`
  * [x] `frozen_default`
  * [x] `field_specifiers` (https://github.com/astral-sh/ty/issues/1068)
* [x] #1386
  * [x] … using a function-like transformer
  * [x] … using a metaclass-based transformer
  * [x] … using a class-based transformer
* [ ] Support field specifier parameters
  * [x] `init`
  * [x] `default`
  * [x] `default_factory`
  * [x] `factory`
  * [x] `kw_only`
  * [x] #1385
  * [ ] `converter`
* [ ] Implement the semantics of classes which are neither frozen nor non-frozen, see this [comment and TODO](https://github.com/astral-sh/ruff/pull/21457#discussion_r2529812486).

---

_Label `dataclasses` added by @sharkdp on 2025-10-09 13:37_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-09 13:37_

---

_Added to milestone `GA` by @sharkdp on 2025-10-09 13:37_

---

_Unassigned @sharkdp by @sharkdp on 2025-12-19 11:20_

---
