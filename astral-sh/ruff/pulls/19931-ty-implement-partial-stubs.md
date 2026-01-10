```yaml
number: 19931
title: "[ty] Implement partial stubs"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
assignees: []
merged: true
base: main
head: gankra/stub-mod
created_at: 2025-08-15T17:40:25Z
updated_at: 2025-08-18T17:14:14Z
url: https://github.com/astral-sh/ruff/pull/19931
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Implement partial stubs

---

_Pull request opened by @Gankra on 2025-08-15 17:40_

Fixes https://github.com/astral-sh/ty/issues/184

---

_Review requested from @carljm by @Gankra on 2025-08-15 17:40_

---

_Label `ty` added by @Gankra on 2025-08-15 17:40_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-15 17:40_

---

_Review requested from @sharkdp by @Gankra on 2025-08-15 17:40_

---

_Review requested from @dcreager by @Gankra on 2025-08-15 17:40_

---

_Comment by @Gankra on 2025-08-15 17:41_

(I need to write some proper tests but this fixes `from authlib.integrations.requests_client import OAuth2Session`)

---

_Comment by @github-actions[bot] on 2025-08-15 17:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@Gankra reviewed on 2025-08-15 17:42_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:812 on 2025-08-15 17:42_

This is really gross and I considered factoring a proper type *but* literally only one function deals with this and immediately unpacks these so... eh?

---

_@Gankra reviewed on 2025-08-15 17:43_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1054 on 2025-08-15 17:43_

No idea if this is correct but i like it conceptually

---

_Comment by @github-actions[bot] on 2025-08-15 17:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1054 on 2025-08-15 19:53_

The spec isn't totally clear here, it just says "This marker applies recursively: if a top-level package includes it, all its sub-packages MUST support type checking as well." It doesn't say anything about a sub-package going to `partial` where its parent was "full", or whatever. But I think this behavior is sensible.

---

_@carljm reviewed on 2025-08-15 19:53_

Looks reasonable to me! But yeah, should add some tests before we land it.

---

_Comment by @Gankra on 2025-08-18 01:50_

Added tests

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1482 on 2025-08-18 07:30_

Could this be an mdtest similar to https://github.com/astral-sh/ruff/blob/1ba56b4bc65442c40d7d6f2873367380d37241a8/crates/ty_python_semantic/resources/mdtest/import/stub_packages.md#L35-L58

---

_@MichaReiser approved on 2025-08-18 10:25_

Nice. This looks good to me. 

Can you coordinate with @BurntSushi to make sure partial is also considered for completions (https://github.com/astral-sh/ruff/pull/19883)? 

It would be nice if some of those module resolver tests could be mdtests. I ported some module resolver tests but haven't had the time to migrate all, but it would be nice if we can avoid adding new once (unless it uses a mdtest feature that we aren't supporting yet)

---

_Comment by @BurntSushi on 2025-08-18 12:45_

> Can you coordinate with @BurntSushi to make sure partial is also considered for completions (#19883)?

I added https://github.com/astral-sh/ty/issues/1042 to track this.

---

_Comment by @Gankra on 2025-08-18 13:23_

Oh sure I can make these mdtests.

---

_Merged by @Gankra on 2025-08-18 17:14_

---

_Closed by @Gankra on 2025-08-18 17:14_

---

_Branch deleted on 2025-08-18 17:14_

---
