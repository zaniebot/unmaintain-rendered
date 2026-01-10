```yaml
number: 20148
title: "[ty] add more lsp tests for overloads"
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/test-over
created_at: 2025-08-29T03:22:12Z
updated_at: 2025-09-02T14:38:28Z
url: https://github.com/astral-sh/ruff/pull/20148
synced_at: 2026-01-10T17:46:21Z
```

# [ty] add more lsp tests for overloads

---

_Pull request opened by @Gankra on 2025-08-29 03:22_

I decided to split out the addition of these tests from other PRs so that it's easier to follow changes to the LSP's function call handling. I'm not particularly concerned with whether the results produced by these tests are "good" or "bad" in this PR, I'm just establishing a baseline.


---

_Review requested from @carljm by @Gankra on 2025-08-29 03:22_

---

_Label `internal` added by @Gankra on 2025-08-29 03:22_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-29 03:22_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-29 03:22_

---

_Review requested from @sharkdp by @Gankra on 2025-08-29 03:22_

---

_Label `server` added by @Gankra on 2025-08-29 03:22_

---

_Review requested from @dcreager by @Gankra on 2025-08-29 03:22_

---

_Label `ty` added by @Gankra on 2025-08-29 03:22_

---

_Comment by @Gankra on 2025-08-29 03:23_

(Each of the 4 files received a copy of the same 6 tests)

---

_Comment by @github-actions[bot] on 2025-08-29 03:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 03:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto_definition.rs`:822 on 2025-08-29 13:57_

Should this have the overloads defined so that the test asserts that goto definition goes to the correct overload? I'm fine with it not being present, I mainly want to understand the reasoning behind the test cases in this file as they all have defined only the implementation.

---

_@dhruvmanila approved on 2025-08-29 14:00_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-29 14:01_

---

_@Gankra reviewed on 2025-08-29 14:32_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:822 on 2025-08-29 14:32_

Hmm interesting, would you expect a .pyi *and* a .py to declare overloads? (they're purely type annotations right? there isn't an implementation to go to for an overload? or did I get myself mixed up)

---

_@dhruvmanila reviewed on 2025-08-29 16:51_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto_definition.rs`:822 on 2025-08-29 16:51_

Hmm, yeah that's a good point. It's not required for the overload definitions to be present in both runtime and stub files as they're mainly for typing purposes (as you've stated as well). So, I think my main question would be do we want to test the goto definition functionality by defining the overloads in the runtime file?

---

_@Gankra reviewed on 2025-09-02 14:38_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_definition.rs`:822 on 2025-09-02 14:38_

I think ultimately goto-definition just has boring answers here, but it's worth making sure stub mapping doesn't get confused by the situation (that said it's possible we want like, 3 different variations of every one of these tests but... for now let's not).

---

_Merged by @Gankra on 2025-09-02 14:38_

---

_Closed by @Gankra on 2025-09-02 14:38_

---

_Branch deleted on 2025-09-02 14:38_

---
