```yaml
number: 19426
title: "[ty] highlight the relevant argument in type assert error messages"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: jack/known_function_lints
created_at: 2025-07-18T21:34:28Z
updated_at: 2025-07-23T15:24:13Z
url: https://github.com/astral-sh/ruff/pull/19426
synced_at: 2026-01-12T15:56:39Z
```

# [ty] highlight the relevant argument in type assert error messages

---

_@oconnor663_

Closes https://github.com/astral-sh/ty/issues/209.

Here's a quick before/after summary of the formatting changes (**updated** after some reverts below):

```py
from ty_extensions import static_assert
static_assert(3 > 4, "custom message")
```

Before:
```
error[static-assert-error]: Static assertion error: custom message
 --> test.py:2:1
  |
1 | from ty_extensions import static_assert
2 | static_assert(3 > 4, "custom message")
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
```

After:
```
error[static-assert-error]: Static assertion error: custom message
 --> test.py:2:1
  |
1 | from ty_extensions import static_assert
2 | static_assert(3 > 4, "custom message")
  | ^^^^^^^^^^^^^^-----^^^^^^^^^^^^^^^^^^^
  |               |
  |               Inferred type of argument is `Literal[False]`
  |
```

---

_Review requested from @carljm by @oconnor663 on 2025-07-18 21:34_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-18 21:34_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-18 21:34_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-18 21:34_

---

_@oconnor663 reviewed on 2025-07-18 21:34_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/directives/static_assert.md`:1 on 2025-07-18 21:34_

Is this the right place for this new mdtest file?

---

_Comment by @github-actions[bot] on 2025-07-18 21:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/directives/static_assert.md`:1 on 2025-07-18 22:55_

I think it could go into `type_api.md` with the existing tests for `static_assert`, or if we want to keep it separate because it's all about the diagnostics rather than the functionality, then it could go somewhere under `diagnostics`?

---

_@carljm approved on 2025-07-18 22:57_

Nice!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/cast.md_-_`cast`_-_The_redundant_`cast`…_(7746465d25c4280a).snap`:28 on 2025-07-19 07:03_

I think this means that for a multiline `cast()` call, you'd have to have a `type: ignore` comment very precisely located to suppress the `redundant-cast` diagnostic. E.g. this would suppress the diagnostic:

```py
cast(
    int,
    very_long_variable_name_splitting_the_call_over_several_lines,  # type: ignore
)
```

But this wouldn't, since the range of the suppression comment wouldn't overlap with the primary range of the diagnostic:

```py
cast(  # type: ignore
    int,
    very_long_variable_name_splitting_the_call_over_several_lines,
)
```

I think that would be very confusing for users, so for this reason, in #18050 (regarding the same issue, but for different diagnostics) I went with a slightly different approach of highlighting the call-arguments range with a secondary annotation but keeping the range of the entire call expression as the diagnostic's range. Some discussion in https://github.com/astral-sh/ruff/pull/18050#discussion_r2085260142

In the long term, I think it would be great if we could disentangle the concepts of "primary range to be highlighted in diagnostics" and "range in which suppression comments are deemed to apply" so that our diagnostic model doesn't require them to always be the same. But that's not something for this PR!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1199 on 2025-07-19 07:04_

Hmm, doesn't this just repeat information already presented in the summary line of the diagnostic?

---

_@AlexWaygood reviewed on 2025-07-19 07:05_

---

_@AlexWaygood reviewed on 2025-07-19 08:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/cast.md_-_`cast`_-_The_redundant_`cast`…_(7746465d25c4280a).snap`:28 on 2025-07-19 08:18_

For the same reasons, I'm not sure we should make any changes to `assert_type` diagnostics in this PR: we already discussed this issue w.r.t. `assert_type` diagnostics in #18050 and the changes made in that PR were basically the best we thought we could do without reworking the diagnostics model to disentangle the concepts of "diagnostic suppression range" and "range displayed when the diagnostic is rendered"

---

_Renamed from "highlight the relevant argument in type assert error messages" to "[ty] highlight the relevant argument in type assert error messages" by @AlexWaygood on 2025-07-19 18:04_

---

_Label `ty` added by @AlexWaygood on 2025-07-19 18:05_

---

_Label `diagnostics` added by @AlexWaygood on 2025-07-19 18:05_

---

_Comment by @oconnor663 on 2025-07-23 01:19_

@AlexWaygood thanks for the explanation there. I've reverted the changes to all the diagnostics other than for `static_assert`, and I've made that one use a secondary annotation for consistency with the others (though it's internal and I don't imagine we'll ever want to `# type: ignore` it). See the edited PR description at the top. I've kept some new snapshot tests for `cast`, but those are now testing the preexisting behavior. Do you have time to re-review?

---

_@AlexWaygood approved on 2025-07-23 10:35_

thank you!

---

_Merged by @oconnor663 on 2025-07-23 15:24_

---

_Closed by @oconnor663 on 2025-07-23 15:24_

---

_Branch deleted on 2025-07-23 15:24_

---
