```yaml
number: 20285
title: "[ty] equality narrowing on enums that don't override `__eq__` or `__ne__`"
type: pull_request
state: merged
author: Renkai
labels: []
assignees: []
merged: true
base: main
head: renkai-narrow-enum-2
created_at: 2025-09-07T13:25:26Z
updated_at: 2025-09-08T23:56:28Z
url: https://github.com/astral-sh/ruff/pull/20285
synced_at: 2026-01-10T17:46:22Z
```

# [ty] equality narrowing on enums that don't override `__eq__` or `__ne__`

---

_Pull request opened by @Renkai on 2025-09-07 13:25_

It's a TODO clean of PR https://github.com/astral-sh/ruff/pull/20164 that solved issue https://github.com/astral-sh/ty/issues/939

---

_Comment by @sharkdp on 2025-09-08 07:23_

Thank you for your contribution. It looks like a better format here would be a GitHub issue on the https://github.com/astral-sh/ty repo? Or is there a particular reason that you are opening a PR at this stage?

---

_Comment by @Renkai on 2025-09-08 07:34_

Hi, @sharkdp I want to clean some todos added in this PR:https://github.com/astral-sh/ruff/pull/20164
It's not finished yet so I mark this as `draft` in github. do you think we shall create an issue first? I'm not quite familiar with the best practice yet.

---

_Comment by @sharkdp on 2025-09-08 07:49_

If you plan to do more changes in this draft PR, that's totally fine. I just thought you were looking for feedback on that document you added here. In that case, I'll assume that we wait for this to move to in-review.

---

_Comment by @github-actions[bot] on 2025-09-08 11:25_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @Renkai on 2025-09-08 11:28_

---

_Review requested from @carljm by @Renkai on 2025-09-08 11:28_

---

_Review requested from @AlexWaygood by @Renkai on 2025-09-08 11:28_

---

_Review requested from @sharkdp by @Renkai on 2025-09-08 11:28_

---

_Review requested from @dcreager by @Renkai on 2025-09-08 11:28_

---

_Renamed from "[ty][WIP]Create narrow_enum_pan.md" to "[ty]reate narrow_enum_pan.md" by @Renkai on 2025-09-08 11:29_

---

_Comment by @github-actions[bot] on 2025-09-08 11:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
- isort/api.py:573:12: warning[possibly-unresolved-reference] Name `key` used when possibly not defined
- isort/api.py:573:20: warning[possibly-unresolved-reference] Name `key` used when possibly not defined
- isort/api.py:574:22: warning[possibly-unresolved-reference] Name `key` used when possibly not defined
- Found 37 diagnostics
+ Found 34 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/exchanges/kraken.py:960:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/exchanges/utils.py:201:37: warning[possibly-unresolved-reference] Name `url` used when possibly not defined
+ rotkehlchen/history/events/structures/swap.py:170:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1589 diagnostics
+ Found 1590 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-09-08 11:37_

Thanks! Could you possibly update your PR description to describe this change a little better?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:774 on 2025-09-08 21:14_

We shouldn't duplicate all this code from `is_single_valued`; we should extract it as a separate method, e.g. `Type::overrides_equality`. (It doesn't even have to be specific to enums, even if we only call it for enums currently.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:746 on 2025-09-08 21:18_

No mdtests fail if we remove this arm. I think that's because we only use this method as an additional check beyond `is_single_valued`, and enum literals are already always single-valued.

I think we can probably remove this `is_simple_enum` method altogether in favor of an `is_enum` method, and use `ty.is_enum() && !ty.overrides_equality()` everywhere the PR currently uses `ty.is_simple_enum()`. This isn't quite as concise but I think it's clearer, as its meaning is more self-evident, where the meaning of "simple enum" is not.

---

_@carljm reviewed on 2025-09-08 21:19_

This is looking really good! I think it gets the correct semantics. A few comments on the implementation.

---

_Renamed from "[ty]reate narrow_enum_pan.md" to "[ty] equality narrowing on enums that don't override `__eq__` or `__ne__`" by @carljm on 2025-09-08 21:20_

---

_@Renkai reviewed on 2025-09-08 23:33_

---

_Review comment by @Renkai on `crates/ty_python_semantic/src/types.rs`:746 on 2025-09-08 23:33_

Thanks for the review! I fixed it followed by your instruction, but cargo clippy forced me to use `!element.is_enum(self.db) || element.overrides_equality(self.db)` instead, hope it's still good to you.

---

_Review requested from @carljm by @Renkai on 2025-09-08 23:40_

---

_@carljm approved on 2025-09-08 23:55_

---

_Merged by @carljm on 2025-09-08 23:56_

---

_Closed by @carljm on 2025-09-08 23:56_

---
