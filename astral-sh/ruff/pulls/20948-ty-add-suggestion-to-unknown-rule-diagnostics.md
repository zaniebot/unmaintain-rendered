```yaml
number: 20948
title: "[ty] Add suggestion to unknown rule diagnostics, rename `unknown-rule` lint to `ignore-comment-unknown-rule`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - ty
assignees: []
merged: true
base: main
head: micha/unknown-rule-suggestion
created_at: 2025-10-17T19:22:08Z
updated_at: 2025-10-18T10:44:23Z
url: https://github.com/astral-sh/ruff/pull/20948
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Add suggestion to unknown rule diagnostics, rename `unknown-rule` lint to `ignore-comment-unknown-rule`

---

_@MichaReiser_

## Summary

Show suggestions for misspelled rule names. 

Rename the `unknown-rule` lint because it overlaps with the non-suppressable `unknown-rule` error code used in configurations.

## Test Plan

Updated mdtests/CLI tests


---

_Label `ty` added by @MichaReiser on 2025-10-17 19:22_

---

_Review requested from @carljm by @MichaReiser on 2025-10-17 19:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-17 19:22_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-17 19:22_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-17 19:22_

---

_Label `breaking` added by @MichaReiser on 2025-10-17 19:23_

---

_Comment by @github-actions[bot] on 2025-10-17 19:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood reviewed on 2025-10-17 19:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/diagnostic.rs`:1 on 2025-10-17 19:24_

Should this module be merged with https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/util/diagnostics.rs ? I don't have a strong opinion about whether a top-level `diagnostic` module or a `util::diagnostic` module is better 😄

---

_Comment by @github-actions[bot] on 2025-10-17 19:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- pytest_robotframework/_internal/pytest/plugin.py:341:32: warning[unknown-rule] Unknown rule `possibly-unbound-import`
+ pytest_robotframework/_internal/pytest/plugin.py:341:32: warning[ignore-comment-unknown-rule] Unknown rule `possibly-unbound-import`
- pytest_robotframework/_internal/robot/library.py:86:99: warning[unknown-rule] Unknown rule `possibly-unbound-attribute`
+ pytest_robotframework/_internal/robot/library.py:86:99: warning[ignore-comment-unknown-rule] Unknown rule `possibly-unbound-attribute`

svcs (https://github.com/hynek/svcs)
- pyproject.toml:232:1: warning[unknown-rule] Unknown lint rule `possibly-unbound-attribute`
+ pyproject.toml:232:1: warning[unknown-rule] Unknown rule `possibly-unbound-attribute`

```
</details>
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-10-17 19:26_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/diagnostic.rs`:1 on 2025-10-17 19:26_

I searched like 2 minutes for where to put it and didn't notice the module in util (probably because I have an aversion to util modules)

---

_@AlexWaygood reviewed on 2025-10-17 19:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md`:131 on 2025-10-17 19:26_

judging by the comment, I think the trailing whitespace here was deliberate :-)

---

_@AlexWaygood reviewed on 2025-10-17 19:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md`:91 on 2025-10-17 19:32_

Is it worth this even being a separate error code to `invalid-ignore-comment`? Is there any situation where you'd want to globally ignore one but not the other? (Is there any situation where you'd want to globally ignore either?!)

---

_@AlexWaygood approved on 2025-10-17 19:33_

The hint is an awesome feature

---

_@MichaReiser reviewed on 2025-10-17 19:36_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md`:91 on 2025-10-17 19:36_

I guess that depends on if we ever want to support `type: ignore[code]`. I could then see cases where you would want to disable `ignore-comment-unknown-rule` but not `invalid-ignore-comment` when using ty alongside mypy and pyright.

But you know that I'm almost always on-board with removing rules ;) 

---

_@AlexWaygood reviewed on 2025-10-17 19:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md`:91 on 2025-10-17 19:39_

hmm, that's a reasonable point. I guess the current division keeps our options open!

---

_Merged by @MichaReiser on 2025-10-18 10:44_

---

_Closed by @MichaReiser on 2025-10-18 10:44_

---

_Branch deleted on 2025-10-18 10:44_

---
