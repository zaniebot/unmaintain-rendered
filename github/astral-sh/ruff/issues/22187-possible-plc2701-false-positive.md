---
number: 22187
title: possible PLC2701 false positive
type: issue
state: open
author: dotysan
labels:
  - bug
  - rule
  - preview
assignees: []
created_at: 2025-12-24T23:48:53Z
updated_at: 2025-12-25T15:07:03Z
url: https://github.com/astral-sh/ruff/issues/22187
synced_at: 2026-01-07T13:12:16-06:00
---

# possible PLC2701 false positive

---

_Issue opened by @dotysan on 2025-12-24 23:48_

### Summary

Apolgies if I'm misunderstanding. But this:
```py
from mirror_syno.__main__ import main
```
is throwing PLC2701 on the last word 'main':
```
Private name import `__main__` from external module `mirror_syno`
```

There is no dunder in `main`. And I don't believe `__main__` as a module is a violation.

---

_Comment by @ntBre on 2025-12-25 15:06_

Thanks for the report! Based on the rule test suite, I think flagging private sub-modules is intentional:

https://github.com/astral-sh/ruff/blob/139149f87b4ad3afb1fe2f4b97af280449fdd741/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC2701_import_private_name__submodule____main__.py.snap#L14-L20

but I think it would make sense both to have an exception for dunder names, which we have for the root module [here](https://github.com/astral-sh/ruff/blob/139149f87b4ad3afb1fe2f4b97af280449fdd741/crates/ruff_linter/src/rules/pylint/rules/import_private_name.rs#L99), and also to make the diagnostic range on the actual private name, as you said.

---

_Label `bug` added by @ntBre on 2025-12-25 15:07_

---

_Label `preview` added by @ntBre on 2025-12-25 15:07_

---

_Label `rule` added by @ntBre on 2025-12-25 15:07_

---
