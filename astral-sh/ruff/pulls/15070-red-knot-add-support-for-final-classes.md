```yaml
number: 15070
title: "[red-knot] Add support for `@final` classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex-carl/final-classes
created_at: 2024-12-19T18:38:58Z
updated_at: 2024-12-19T21:04:10Z
url: https://github.com/astral-sh/ruff/pull/15070
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Add support for `@final` classes

---

_Pull request opened by @AlexWaygood on 2024-12-19 18:38_

## Summary

This PR adds support for the `@final` decorator on classes. The `@final` decorator can also be applied to methods; this PR does not add support for that yet.

`@final` is specified here: https://typing.readthedocs.io/en/latest/spec/qualifiers.html#final. When applied to a class, it declares to the type checker that the class cannot be subclassed, and that the type checker should emit an error if it sees the class being subclassed. This information is extremely valuable to a type checker, as it rules out certain multiple inheritance possibilities, allows for more precise inference in some cases, and improves analysis of whether or not certain blocks of code are reachable.

This PR doesn't yet make use of these extra capabilities that an understanding of `@final` opens up to us; it just adds a basic understanding of finality, and adds a rule such that we emit a diagnostic if we see a final class being subclassed.

## Test Plan

New mdtests added to `final.md`.

Co-authored-by: Carl Meyer <carl@astral.sh>

---

_Label `red-knot` added by @AlexWaygood on 2024-12-19 18:38_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-19 18:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-19 18:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-19 18:38_

---

_Comment by @github-actions[bot] on 2024-12-19 18:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/d31e3ea11e2090afdb639324bf7638891695d1ad/stdlib/dataclasses.pyi#L259'>stdlib/dataclasses.pyi:259:19:</a> W292 No newline at end of file
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| W292 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/final.md`:14 on 2024-12-19 20:11_

I suppose since we put some work into this part of the implementation, we could also use the column-number assertion here to show that the error is on the specific base (I could be off-by-one here, not sure)
```suggestion
class B(A): ...  # error: 9 [subclass-of-final-class] "Class `B` cannot inherit from final class `A`"
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3136 on 2024-12-19 20:12_

We are going to be re-doing this filter/any anytime we check whether a class is final. I suspect this is fine, it's still cheap and we won't do it often enough to matter, so it's not worth another query to avoid it.

---

_@carljm approved on 2024-12-19 20:13_

Nice!

---

_Merged by @AlexWaygood on 2024-12-19 21:02_

---

_Closed by @AlexWaygood on 2024-12-19 21:02_

---

_Branch deleted on 2024-12-19 21:02_

---
