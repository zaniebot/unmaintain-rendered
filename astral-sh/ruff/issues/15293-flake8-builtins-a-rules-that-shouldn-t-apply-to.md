```yaml
number: 15293
title: "`flake8-builtins (A)` rules that shouldn't apply to stubs"
type: issue
state: open
author: Avasam
labels:
  - rule
assignees: []
created_at: 2025-01-06T03:50:24Z
updated_at: 2025-01-08T17:13:24Z
url: https://github.com/astral-sh/ruff/issues/15293
synced_at: 2026-01-12T15:54:54Z
```

# `flake8-builtins (A)` rules that shouldn't apply to stubs

---

_@Avasam_

The following I'm pretty sure should be disabled in stubs, as they are out of a stub author's control:
- [builtin-variable-shadowing (A001)](https://docs.astral.sh/ruff/rules/builtin-variable-shadowing/#builtin-variable-shadowing-a001) (344 hits in typeshed)
  This happens with aliases and other top-level variables. Non-runtime type aliases should be private in the first place. Devs also shouldn't try to import builtins from a module that happens to import said builtin. And finally this rule mostly tries to protect against *using* the shadowed variable. In a typing-only context, you should mostly be guarded by type tests anyway.
- [builtin-argument-shadowing (A002)](https://docs.astral.sh/ruff/rules/builtin-argument-shadowing/#builtin-argument-shadowing-a002) (2085 hits on typeshed)
- [builtin-attribute-shadowing (A003)](https://docs.astral.sh/ruff/rules/builtin-attribute-shadowing/#builtin-attribute-shadowing-a003)
- [builtin-module-shadowing (A005)](https://docs.astral.sh/ruff/rules/builtin-module-shadowing/#builtin-module-shadowing-a005) (done in https://github.com/astral-sh/ruff/pull/15350)

As for the rest:

-  [builtin-import-shadowing (A004)](https://docs.astral.sh/ruff/rules/builtin-import-shadowing/#builtin-import-shadowing-a004) (53 hits in typeshed, nearly all re-exports)
  Very similar as `A001` but can actually be worked around with import aliases and re-export aliases. (which would trigger `A001` instead)
  Could maybe be ignored in stubs if re-exported ? (`import print as print`, `from tensorflow.math import abs as abs`, `import foo as print; print = print` and  `import foo as print; __all__= ["print"]`)
- [builtin-lambda-argument-shadowing (A006)](https://docs.astral.sh/ruff/rules/builtin-lambda-argument-shadowing/#builtin-lambda-argument-shadowing-a006)
  I don't think lambdas apply to type stubs anyway. Probably doesn't need to be ignored.

Ruff: 0.8.5

(this report is extracted from https://github.com/astral-sh/ruff/issues/14535#issuecomment-2496160152 and https://github.com/astral-sh/ruff/issues/14535#issuecomment-2561461038 for ease of tracking and discussion)

---

_Label `rule` added by @dhruvmanila on 2025-01-06 06:05_

---

_Comment by @AlexWaygood on 2025-01-08 12:59_

> [builtin-module-shadowing (A005)](https://docs.astral.sh/ruff/rules/builtin-module-shadowing/#builtin-module-shadowing-a005)

This has been tackled in https://github.com/astral-sh/ruff/pull/15350

---
