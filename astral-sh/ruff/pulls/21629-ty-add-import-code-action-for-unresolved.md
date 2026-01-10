```yaml
number: 21629
title: "[ty] Add \"import ...\" code-action for unresolved references"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/import-act2
created_at: 2025-11-25T16:18:30Z
updated_at: 2025-12-01T16:15:50Z
url: https://github.com/astral-sh/ruff/pull/21629
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add "import ..." code-action for unresolved references

---

_Pull request opened by @Gankra on 2025-11-25 16:18_

## Summary

Originally I planned to feed this in as a `fix` but I realized that we probably don't want to be trying to resolve import suggestions while we're doing type inference. Thus I implemented this as a fallback when there's no fixes on a diagnostic, which can use the full lsp machinery.

Fixes https://github.com/astral-sh/ty/issues/1552

## Test Plan

Works in the IDE, added some e2e tests.

---

_Label `server` added by @Gankra on 2025-11-25 16:18_

---

_Label `ty` added by @Gankra on 2025-11-25 16:18_

---

_@Gankra reviewed on 2025-11-25 16:19_

---

_Review comment by @Gankra on `crates/ty_ide/src/symbols.rs`:93 on 2025-11-25 16:19_

Extremely rude bug in this extremely pedantic codepath Andrew was very clear in the comments really should never ever happen, which I have now hijacked.

---

_Comment by @astral-sh-bot[bot] on 2025-11-25 16:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-25 16:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 505 diagnostics
+ Found 507 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @Gankra on 2025-11-26 19:51_

---

_Review requested from @carljm by @Gankra on 2025-11-26 19:51_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-26 19:51_

---

_Review requested from @sharkdp by @Gankra on 2025-11-26 19:51_

---

_Review requested from @dcreager by @Gankra on 2025-11-26 19:51_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-26 19:51_

---

_Comment by @Gankra on 2025-11-26 19:52_

I added a few speculative e2e tests for cases which don't work (yet) as well. Future work for someone else to implement those :)

---

_Renamed from "[ty] Add import-unresolved-reference code-action" to "[ty] Add "import ..." code-action for unresolved references" by @Gankra on 2025-11-26 20:02_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-27 07:36_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-27 07:36_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-27 07:36_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-27 07:36_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/code_action.rs`:18 on 2025-11-27 07:39_

Nit: Maybe rename to `diagnostic_range` as it wasn't clear to me what range this is

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/code_action.rs`:94 on 2025-11-27 07:41_

I think we want to add a `preferred: bool` field to `CodeAction` because, unlike `Diagnostic` fixes, `code_actions` can return multiple results.

---

_@MichaReiser approved on 2025-11-27 07:45_

This is great. I'd prefer for @BurntSushi to have a look at the completion changes

---

_Comment by @MichaReiser on 2025-11-27 08:30_

https://github.com/astral-sh/ruff/pull/21655 adds code action support to the playground. It would be great if you could integrate your action too

---

_Comment by @Gankra on 2025-11-27 13:31_

Added playground support (booted it up, works great).

---

_Merged by @Gankra on 2025-11-27 15:06_

---

_Closed by @Gankra on 2025-11-27 15:06_

---

_Branch deleted on 2025-11-27 15:06_

---

_@BurntSushi reviewed on 2025-12-01 16:15_

LGTM!

---
