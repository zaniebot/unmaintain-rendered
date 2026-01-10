```yaml
number: 13131
title: "[red-knot] support deferred evaluation of type expressions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: lazy-bases
created_at: 2024-08-28T00:52:32Z
updated_at: 2024-08-28T18:41:03Z
url: https://github.com/astral-sh/ruff/pull/13131
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] support deferred evaluation of type expressions

---

_Pull request opened by @carljm on 2024-08-28 00:52_

Prototype deferred evaluation of type expressions by deferring evaluation of class bases in a stub file. This allows self-referential class definitions, as occur with the definition of `str` in typeshed (which inherits `Sequence[str]`).


---

_Label `red-knot` added by @carljm on 2024-08-28 00:52_

---

_@carljm reviewed on 2024-08-28 01:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-08-28 01:09_

I don't love that this knowledge of whether the bases are deferred or not is spread across both type inference and here. But this explicit "look in the right place first" logic is more efficient than either an implicit "first check one, then the other" or merging/copying the two inferences. I'll think more tomorrow about whether there's a better way to consolidate this logic in one place.

---

_Comment by @codspeed-hq[bot] on 2024-08-28 01:14_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/lazy-bases)

### Merging #13131 will **not alter performance**

<sub>Comparing <code>lazy-bases</code> (b9c1dd3) with <code>main</code> (c6023c0)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-28 01:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @carljm on 2024-08-28 01:33_

---

_Review requested from @MichaReiser by @carljm on 2024-08-28 01:33_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-28 01:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-08-28 10:24_

Won't we also have to support deferred bases in `.py` files? Pyright and mypy both support recursive definitions like this in `.py` files:

```py
class Foo(list["Foo | None"]): pass
```

- https://mypy-play.net/?mypy=latest&python=3.12&gist=e9e1e5ebb6a0d34f80b190a395f19f7b
- https://pyright-play.net/?pyrightVersion=1.1.368&code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFCMDGFZAzm1AGJhgAUFJGxgBtAEQ8wUAD5QAcmBSExAXQCUALigJ2bRkVKUaiQn0l81IgAzr6QA

Though I suppose we don't support stringified annotations of any kind yet. Worth a TODO comment, at least?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1 on 2024-08-28 10:27_

https://people.csail.mit.edu/paulfitz/spanish/t2.html

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1763 on 2024-08-28 10:33_

```suggestion
            let symbol = symbols
                .symbol_id_by_name(id)
                .expect("Expected the symbol table to create a symbol for every Name node");
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files.rs`:432 on 2024-08-28 10:39_

I'd use `ruff_python_ast::PySourceType` here:

```suggestion
            .is_some_and(|extension| PySourceType::from_extension(extension).is_stub())
```

if nothing else, for a bit more symmetry with the `is_notebook` function here:

https://github.com/astral-sh/ruff/blob/cfafaa7637ef3f568d62d39654e0498f52a07b77/crates/ruff_db/src/source.rs#L54-L66

---

_@AlexWaygood reviewed on 2024-08-28 10:40_

Overall this looks good!

---

_@carljm reviewed on 2024-08-28 12:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-08-28 12:42_

Ah yeah, good call. I did have a TODO about stringified annotations (and the future import) in an earlier draft here, but that method got removed and I forgot to re-add the TODO comment.

I'm tempted to add support for deferred partially-stringified bases in `.py` files in this PR, so as to clarify how it will impact logic like this. The awkward thing is just that stringified types in bases in `.py` are only valid as generic type parameters, so I'll need to see how much of the subscripting logic needs to be added along with that, and if that starts to feel like scope creep.

In any case, I think you're right that this means we can't know in advance whether the type of a particular base is deferred or not, so I will need to provide some facility for finding an expression type that's part of a definition without knowing that in advance.

---

_@AlexWaygood reviewed on 2024-08-28 12:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-08-28 12:46_

> I'm tempted to add support for deferred partially-stringified bases in `.py` files in this PR, so as to clarify how it will impact logic like this. The awkward thing is just that stringified types in bases in `.py` are only valid as generic type parameters, so I'll need to see how much of the subscripting logic needs to be added along with that, and if that starts to feel like scope creep.

If it makes things easier -- in "pyflakes-as-monkey-patched-by-`flake8-pyi`", we only allow forward references in class bases when they're inside generic type parameters. We run "pyflakes-as-monkey-patched-by-`flake8-pyi`" over the whole of typeshed with no false positives, so I think it would be fine if we decided in red-knot that we only wanted to permit forward references inside class bases if they're inside generic type parameters, even if the source file is a `.pyi` file. Since there's no known use case for forward references in class bases that are outside generic type parameters, this seems like it would make sense if it reduces complexity for our implementation by reducing the differences between `.py` files and `.pyi` files.

---

_@AlexWaygood reviewed on 2024-08-28 12:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-08-28 12:50_

(I'm referring here to the [original flake8-pyi plugin for flake8](https://github.com/PyCQA/flake8-pyi), not Ruff's reimplementation of those lints. Ruff, I think, is slightly more permissive when it comes to the forward references it allows in `.pyi` files? I can't remember exactly where we landed on all of that... anyway, we're creating a semantic model "from scratch" here; there's no need to follow what Ruff does precisely.)

---

_Comment by @carljm on 2024-08-28 17:29_

I looked into the regression here, and it seems like it's increased Salsa overhead. This is consistent with the regression only showing up in the incremental benchmark, not the cold benchmark. I guess this is because we now double the number of tracked function memos for definitions, which are our highest-cardinality tracked struct.

---

_@carljm reviewed on 2024-08-28 17:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-08-28 17:54_

Adding support for stringified generic type params really felt like it would require adding generics support. Which I'd like to do, but not in this PR. So I didn't do that. But I did make the lookup generic in such a way that `ClassType::bases` no longer makes assumptions about whether a given base is deferred or not. And I added a TODO for the stringified generic type parameters.

---

_Comment by @carljm on 2024-08-28 17:57_

I added tracking of whether a given definition has any deferred expressions, which allows us to avoid ever calling `infer_deferred_types` query on a definition that doesn't have any. Locally this seems to make the regression go away, will see if CodSpeed agrees.

---

_@AlexWaygood approved on 2024-08-28 17:58_

---

_Comment by @carljm on 2024-08-28 17:59_

Yay, CodSpeed agrees!

---

_Merged by @carljm on 2024-08-28 18:41_

---

_Closed by @carljm on 2024-08-28 18:41_

---

_Branch deleted on 2024-08-28 18:41_

---
