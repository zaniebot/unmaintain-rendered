```yaml
number: 20021
title: "[ty] Fix incorrect docstring in call signature completion"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-wrong-function-in-call-signatures
created_at: 2025-08-21T12:20:46Z
updated_at: 2025-08-21T20:36:42Z
url: https://github.com/astral-sh/ruff/pull/20021
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Fix incorrect docstring in call signature completion

---

_Pull request opened by @MichaReiser on 2025-08-21 12:20_

## Summary

This PR fixes https://github.com/astral-sh/ty/issues/1071

The core issue is that `CallableType` is a salsa interned but `Signature` (which `CallableType` stores) ignores the `Definition` in its `Eq` and `Hash` implementation.

This PR tries to simplest fix by removing the custom `Eq` and `Hash` implementation. The main downside of this fix is that it can increase memory usage because `CallableType`s that are equal except for their `Definition` are now interned separately.

The alternative is to remove `Definition` from `CallableType` and instead, call `bindings` directly on the callee (call_expression.func). However, this would require 
addressing the TODO 

here https://github.com/astral-sh/ruff/blob/39ee71c2a57bd6c51482430ba8bfe3728b4ca443/crates/ty_python_semantic/src/types.rs#L4582-L4586 

This might probably be worth addressing anyway, but is the more involved fix. That's why I opted for removing the custom `Eq` implementation.

We already "ignore" the definition during normalization, thank's to Alex's work in https://github.com/astral-sh/ruff/pull/19615

## Test Plan


https://github.com/user-attachments/assets/248d1cb1-12fd-4441-adab-b7e0866d23eb



---

_Label `bug` added by @MichaReiser on 2025-08-21 12:20_

---

_Review requested from @carljm by @MichaReiser on 2025-08-21 12:20_

---

_Label `server` added by @MichaReiser on 2025-08-21 12:20_

---

_Label `ty` added by @MichaReiser on 2025-08-21 12:20_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-21 12:20_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-21 12:20_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-21 12:20_

---

_Review requested from @Gankra by @MichaReiser on 2025-08-21 12:21_

---

_Review requested from @UnboundVariable by @MichaReiser on 2025-08-21 12:21_

---

_Review request for @dcreager removed by @MichaReiser on 2025-08-21 12:21_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-08-21 12:21_

---

_Comment by @github-actions[bot] on 2025-08-21 12:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 12:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-08-21 12:33_

Makes sense to me!

I agree that we need to do some work cleaning up `Type::bindings()`, but I think both @sharkdp and I have tried several times at this point and every time we found it surprisingly hard 🙃 so this is definitely the easier fix for now.

---

_Renamed from "[ty] Fix wrong docstring in call signature completion" to "[ty] Fix incorrect docstring in call signature completion" by @MichaReiser on 2025-08-21 12:50_

---

_@Gankra approved on 2025-08-21 14:54_

Nice!

---

_Comment by @MichaReiser on 2025-08-21 15:00_

I'll give @carljm a chance to chime in, as he reasoned that we should remove `Definition` from `CallableType`.

---

_Comment by @carljm on 2025-08-21 17:50_

> I'll give @carljm a chance to chime in, as he reasoned that we should remove `Definition` from `CallableType`.

I do think that's what we should do eventually, but it doesn't have to be done right now -- this fix improves the behavior in the short term and I don't think makes anything more difficult for removing the definition in future, so I'm in favor of it.

(Also it removes code, and all else equal I'm always in favor of that 😆 )

---

_Comment by @Gankra on 2025-08-21 20:36_

I'm gonna land this since it's a important/pervasive bugfix, makes it easier to test/debug other function handling features.

---

_Merged by @Gankra on 2025-08-21 20:36_

---

_Closed by @Gankra on 2025-08-21 20:36_

---

_Branch deleted on 2025-08-21 20:36_

---
