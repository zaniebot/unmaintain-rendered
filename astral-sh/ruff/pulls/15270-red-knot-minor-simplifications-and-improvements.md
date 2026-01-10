```yaml
number: 15270
title: "[red-knot] Minor simplifications and improvements to constraint narrowing logic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-constraint-fn
created_at: 2025-01-05T15:21:46Z
updated_at: 2025-01-05T21:52:49Z
url: https://github.com/astral-sh/ruff/pull/15270
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Minor simplifications and improvements to constraint narrowing logic

---

_Pull request opened by @AlexWaygood on 2025-01-05 15:21_

## Summary

On its own this is a pretty minor refactor, but it makes some other improvements I'm working on easier to implement. It nonetheless seems worth doing on its own, however, and reduces the size of the diff on the other branch I have locally, so I'm submitting it as its own PR.

## Test Plan

No new tests added; this is a pure refactor, which shouldn't change any functionality.


---

_Label `red-knot` added by @AlexWaygood on 2025-01-05 15:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-05 15:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-05 15:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-05 15:21_

---

_Comment by @github-actions[bot] on 2025-01-05 15:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -4 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L637'>tools/lib/template_parser.py:637:29:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L637'>tools/lib/template_parser.py:637:29:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L645'>tools/lib/template_parser.py:645:29:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L645'>tools/lib/template_parser.py:645:29:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB171 | 4 | 0 | 0 | 0 | 4 |

</p>
</details>




---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-05 16:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3087 on 2025-01-05 19:05_

Nit: this method doesn't actually "apply" a constraint, it just returns a type. Maybe `constraint_ty`?

I also wonder if this might be even clearer if it accepts an `argument: Type` instead of `class: Class` and encapsulates all of the logic that is currently in `generate_classinfo_constraint` (that is, match tuples and class literals, recursively build a union for tuples)? Don't feel strongly, though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:103 on 2025-01-05 19:07_

Orthogonal to this PR, but I think we should probably also match `Type::SubclassOf` here? I would expect code like this to narrow:

```py
def f(instance: A | None, ty: type[A]):
    if isinstance(instance, ty):
        reveal_type(instance)  # revealed: A
```

---

_@carljm approved on 2025-01-05 19:07_

---

_@AlexWaygood reviewed on 2025-01-05 19:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:103 on 2025-01-05 19:24_

Nice catch -- yes, I think you're right! We probably added this logic before we had the `Type::SubclassOf` variant. I won't fix it in this PR, though, as it _is_ orthogonal to what this PR is doing, and it'll probably complicate rebasing #15272 onto this branch

---

_@AlexWaygood reviewed on 2025-01-05 19:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:103 on 2025-01-05 19:25_

Oh, actually, addressing your other review comment already throws away any chance I have of a clean rebase of that PR branch onto this one, so I may as well fix it here 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:93 on 2025-01-05 19:46_

I moved this enum here from `types.rs` so that all the narrowing logic remained encapsulated in `narrow.rs`

---

_@AlexWaygood reviewed on 2025-01-05 19:46_

---

_Renamed from "[red-knot] Minor simplification to constraint narrowing logic" to "[red-knot] Minor simplifications and improvements to constraint narrowing logic" by @AlexWaygood on 2025-01-05 21:47_

---

_Merged by @AlexWaygood on 2025-01-05 21:51_

---

_Closed by @AlexWaygood on 2025-01-05 21:51_

---

_Branch deleted on 2025-01-05 21:51_

---
