```yaml
number: 21460
title: "[ty] Suppress completions when introducing names with `as`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/suppress-completions
created_at: 2025-11-14T17:48:37Z
updated_at: 2025-11-14T19:21:09Z
url: https://github.com/astral-sh/ruff/pull/21460
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Suppress completions when introducing names with `as`

---

_Pull request opened by @BurntSushi on 2025-11-14 17:48_

There are a few places in Python where it is known that new names are
being introduced and thus we probably shouldn't offer completions. We
already handle this today for things like `class <CURSOR>` and `def
<CURSOR>`. But we didn't handle `as <CURSOR>`, which can appear in
`import`, `with`, `except` and `match` statements. Indeed, these are
exactly the 4 cases where the `as` keyword can occur. So we look for the
presence of `as` and suppress completions based on that.

While we're here, we also make the implementation a bit more robust with
respect to suppressing completions when the user hasn't typed anything.
Namely, previously, we'd still offer completions in a `class <CURSOR>`
context. But it looks like LSP clients (at least, VS Code) doesn't ask
for completions here, so we were "saved" incidentally. This PR detects
this case and suppresses completions there so we don't rely on LSP
client behavior to handle that case correctly.

Fixes astral-sh/ty#1287


---

_Review requested from @carljm by @BurntSushi on 2025-11-14 17:48_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-11-14 17:48_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-11-14 17:48_

---

_Review requested from @sharkdp by @BurntSushi on 2025-11-14 17:48_

---

_Review requested from @dcreager by @BurntSushi on 2025-11-14 17:48_

---

_Review request for @dcreager removed by @BurntSushi on 2025-11-14 17:48_

---

_Review request for @carljm removed by @BurntSushi on 2025-11-14 17:48_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-11-14 17:48_

---

_Label `server` added by @BurntSushi on 2025-11-14 17:49_

---

_Label `ty` added by @BurntSushi on 2025-11-14 17:49_

---

_Comment by @BurntSushi on 2025-11-14 17:49_

Demo:


https://github.com/user-attachments/assets/b595bf9d-1bc7-4a87-b4ac-a45d694cc36a



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 17:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 17:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:935 on 2025-11-14 18:26_

(optional, obviously)

```suggestion
    if last.kind() != TokenKind::Name && !last.kind().is_keyword() {
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:3175 on 2025-11-14 18:29_

❤️

---

_@AlexWaygood approved on 2025-11-14 18:35_

Very nice, thank you!

The ony issue I spotted from manual testing is that in this location, `as` does not seem to be suggested (and in fact, `as` should probably be the _only_ suggestion in this context?). But that's probably a distinct issue to the ones being fixed in this PR.

(Screengrab is from a playground build using your PR branch)

<img width="1632" height="534" alt="image" src="https://github.com/user-attachments/assets/6d2e2616-0ab5-4449-a566-a8ac6b106fac" />


---

_@MichaReiser approved on 2025-11-14 18:37_

---

_Comment by @AlexWaygood on 2025-11-14 18:41_

> Fixes [astral-sh/ty#1287](https://github.com/astral-sh/ty/issues/1287)

The first three bullets in https://github.com/astral-sh/ty/issues/1287#issuecomment-3527860427 will not be fixed by this PR -- but we can open a followup issue(s) for those

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:935 on 2025-11-14 19:11_

I very weakly prefer `matches!` here. :)

---

_@BurntSushi reviewed on 2025-11-14 19:11_

---

_Comment by @BurntSushi on 2025-11-14 19:12_

> The ony issue I spotted from manual testing is that in this location, `as` does not seem to be suggested (and in fact, `as` should probably be the _only_ suggestion in this context?). But that's probably a distinct issue to the ones being fixed in this PR.

Yeah good catch. This is indeed a distinct issue. The `import` detection allows false positives, but not for any specifically compelling reason. I'll open a new issue for this.

---

_Comment by @BurntSushi on 2025-11-14 19:20_

I've filed https://github.com/astral-sh/ty/issues/1563 and https://github.com/astral-sh/ty/issues/1562 as additional work here.

---

_Merged by @BurntSushi on 2025-11-14 19:21_

---

_Closed by @BurntSushi on 2025-11-14 19:21_

---

_Branch deleted on 2025-11-14 19:21_

---
