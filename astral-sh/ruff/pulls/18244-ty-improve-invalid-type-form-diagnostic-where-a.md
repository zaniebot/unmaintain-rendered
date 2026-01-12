```yaml
number: 18244
title: "[ty] Improve `invalid-type-form` diagnostic where a module-literal type is used in a type expression and the module has a member which would be valid in a type expression"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/datetime-dot-datetime-diag
created_at: 2025-05-21T16:57:52Z
updated_at: 2025-05-21T19:38:58Z
url: https://github.com/astral-sh/ruff/pull/18244
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Improve `invalid-type-form` diagnostic where a module-literal type is used in a type expression and the module has a member which would be valid in a type expression

---

_@AlexWaygood_

## Summary

It's fairly common in Python do to something like this:

```py
import datetime

x: datetime
```

when you actually meant to do this:

```py
import datetime

x: datetime.datetime
```

This PR adds subdiagnostics to hint to the user what the cause of the `invalid-type-form` error might be in cases like this.

Closes https://github.com/astral-sh/ty/issues/373

## Test Plan

Snapshots


---

_Review requested from @carljm by @AlexWaygood on 2025-05-21 16:57_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-21 16:57_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-21 16:57_

---

_Label `ty` added by @AlexWaygood on 2025-05-21 16:58_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-21 16:58_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:46 on 2025-05-21 17:00_

Nit: I'd remove the `Hint`. I think try is enough and it avoids the double-colon

---

_@MichaReiser reviewed on 2025-05-21 17:00_

---

_Comment by @github-actions[bot] on 2025-05-21 17:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-21 17:02_

you have to leave some diagnostic improvements to contributors ;)

---

_@AlexWaygood reviewed on 2025-05-21 17:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:46 on 2025-05-21 17:03_

I think what I'd _actually_ like here is

```suggestion
hint: try `datetime.datetime`
```

but we obviously don't have that right now :P

---

_@MichaReiser reviewed on 2025-05-21 17:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:46 on 2025-05-21 17:11_

I think this has come up before with the *rule is enabled by default*. It could make sense for us to have different sub-diagnostic severities. CC: @BurntSushi 

---

_@InSyncWithFoo reviewed on 2025-05-21 17:24_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:62 on 2025-05-21 17:24_

Not something directly related to this PR, but these `info` lines are inconsistent in their capitalizations and punctuations.

---

_@BurntSushi reviewed on 2025-05-21 17:29_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:46 on 2025-05-21 17:29_

Yeah I'd be in favor of adding a `hint` and/or even `note` severity.

---

_@AlexWaygood reviewed on 2025-05-21 17:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:62 on 2025-05-21 17:29_

yes

---

_@InSyncWithFoo reviewed on 2025-05-21 17:31_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:62 on 2025-05-21 17:31_

And I'd argue that the message is a bit awkwardly phrased. How about this?

> Did you mean to use its member `Image.Image` instead?

---

_@AlexWaygood reviewed on 2025-05-21 17:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:62 on 2025-05-21 17:32_

That's a nice shortening, thanks!

---

_@AlexWaygood reviewed on 2025-05-21 17:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:62 on 2025-05-21 17:40_

done

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/invalid.md_-_Tests_for_invalid_ty…_-_Diagnostics_for_comm…_-_Module-literal_used_…_(652fec4fd4a6c63a).snap`:62 on 2025-05-21 18:13_

> Not something directly related to this PR, but these `info` lines are inconsistent in their capitalizations and punctuations.

(I agree that this is inconsistent, but I don't plan to address it in this PR since it predates this PR)

---

_@AlexWaygood reviewed on 2025-05-21 18:13_

---

_@carljm approved on 2025-05-21 19:38_

---

_Merged by @AlexWaygood on 2025-05-21 19:38_

---

_Closed by @AlexWaygood on 2025-05-21 19:38_

---

_Branch deleted on 2025-05-21 19:38_

---
