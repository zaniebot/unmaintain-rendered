```yaml
number: 21744
title: "[ty] Fix non-determinism in `ConstraintSet.specialize_constrained`"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/nondeterminism
created_at: 2025-12-01T23:39:37Z
updated_at: 2025-12-03T15:19:40Z
url: https://github.com/astral-sh/ruff/pull/21744
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix non-determinism in `ConstraintSet.specialize_constrained`

---

_Pull request opened by @dcreager on 2025-12-01 23:39_

This fixes a non-determinism that we were seeing in the constraint set tests in https://github.com/astral-sh/ruff/pull/21715.

In this test, we create the following constraint set, and then try to create a specialization from it:

```
(T@constrained_by_gradual_list = list[Base])
  ∨
(Bottom[list[Any]] ≤ T@constrained_by_gradual_list ≤ Top[list[Any]])
```

That is, `T` is either specifically `list[Base]`, or it's any `list`. Our current heuristics say that, absent other restrictions, we should specialize `T` to the more specific type (`list[Base]`).

In the correct test output, we end up creating a BDD that looks like this:

```
(T@constrained_by_gradual_list = list[Base])
┡━₁ always
└─₀ (Bottom[list[Any]] ≤ T@constrained_by_gradual_list ≤ Top[list[Any]])
    ┡━₁ always
    └─₀ never
```

In the incorrect output, the BDD looks like this:

```
(Bottom[list[Any]] ≤ T@constrained_by_gradual_list ≤ Top[list[Any]])
┡━₁ always
└─₀ never
```

The difference is the ordering of the two individual constraints. Both constraints appear in the first BDD, but the second BDD only contains `T is any list`. If we were to force the second BDD to contain both constraints, it would look like this:

```
(Bottom[list[Any]] ≤ T@constrained_by_gradual_list ≤ Top[list[Any]])
┡━₁ always
└─₀ (T@constrained_by_gradual_list = list[Base])
    ┡━₁ always
    └─₀ never
```

This is the standard shape for an OR of two constraints. However! Those two constraints are not independent of each other! If `T` is specifically `list[Base]`, then it's definitely also "any `list`". From that, we can infer the contrapositive: that if `T` is not any list, then it cannot be `list[Base]` specifically. When we encounter impossible situations like that, we prune that path in the BDD, and treat it as `false`. That rewrites the second BDD to the following:

```
(Bottom[list[Any]] ≤ T@constrained_by_gradual_list ≤ Top[list[Any]])
┡━₁ always
└─₀ (T@constrained_by_gradual_list = list[Base])
    ┡━₁ never   <-- IMPOSSIBLE, rewritten to never
    └─₀ never
```

We then would see that that BDD node is redundant, since both of its outgoing edges point at the `never` node. Our BDDs are _reduced_, which means we have to remove that redundant node, resulting in the BDD we saw above:

```
(Bottom[list[Any]] ≤ T@constrained_by_gradual_list ≤ Top[list[Any]])
┡━₁ always
└─₀ never       <-- redundant node removed
```

The end result is that we were "forgetting" about the `T = list[Base]` constraint, but only for some BDD variable orderings.

To fix this, I'm leaning in to the fact that our BDDs really do need to "remember" all of the constraints that they were created with. Some combinations might not be possible, but we now have the sequent map, which is quite good at detecting and pruning those.

So now our BDDs are _quasi-reduced_, which just means that redundant nodes are allowed. (At first I was worried that allowing redundant nodes would be an unsound "fix the glitch". But it turns out they're real! [This](https://ieeexplore.ieee.org/abstract/document/130209) is the paper that introduces them, though it's very difficult to read. Knuth mentions them in §7.1.4 of [TAOCP](https://course.khoury.northeastern.edu/csu690/ssl/bdd-knuth.pdf), and [this paper](https://par.nsf.gov/servlets/purl/10128966) has a nice short summary of them in §2.)

While we're here, I've added a bunch of `debug` and `trace` level log messages to the constraint set implementation. I was getting tired of having to add these by hands over and over. To enable them, just set `TY_LOG` in your environment, e.g.

```sh
env TY_LOG=ty_python_semantic::types::constraints::SequentMap=trace ty check ...
```

[Note, this has an `internal` label because are still not using `specialize_constrained` in anything user-facing yet.]

---

_Review requested from @carljm by @dcreager on 2025-12-01 23:39_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-01 23:39_

---

_Review requested from @sharkdp by @dcreager on 2025-12-01 23:39_

---

_Label `internal` added by @dcreager on 2025-12-01 23:39_

---

_Label `ty` added by @dcreager on 2025-12-01 23:39_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 23:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 23:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 493 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 41 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:1041:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2052 diagnostics
+ Found 2050 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:45 on 2025-12-02 13:46_

I think this test (and the other changed one below) were other possible points of nondeterminism, since the result could depend on whether `T ≤ bool` was kept in the BDD or simplified away as described in the PR body.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:257 on 2025-12-02 13:47_

This test failed before these fixes, reproducing the test failure in the macOS CI job

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:785 on 2025-12-02 13:53_

This is the clause that used to remove redundant BDD nodes. Removing it changes our BDDs from reduced to quasi-reduced.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:956 on 2025-12-02 13:55_

This method skips calling `Node::new` for some inputs, so we have to change the base cases to not throw away redundant nodes. (ditto below)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3172 on 2025-12-02 13:57_

This was a bug in our display rendering that we couldn't trigger before, since `never` BDDs were always collapsed into the `AlwaysNever` terminal. Now we might have BDDs where all paths lead to the `never` terminal, which would hit this clause. (The intuition here is that we're displaying a DNF formula — a union of intersections — and so if the union contains no elements, the overall formula is `false`/`never`. And the new snippet just above handles where there's a single-element union of an empty intersection, which represents `true`/`always`.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3426 on 2025-12-02 14:00_

This looks like a big blow-up in the size of the BDD, but we still have structural sharing from the salsa interning. So for example, all of the copies of

```
(U = bool)
┡━₁ always
└─₀ never
```

are stored once as a single `InternalNode`. So the rendered size here is not a good proxy for how much memory is used internally.

---

_@dcreager reviewed on 2025-12-02 14:00_

---

_Comment by @codspeed-hq[bot] on 2025-12-02 14:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fnondeterminism?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21744 will **not alter performance**

<sub>Comparing <code>dcreager/nondeterminism</code> (948a9fb) with <code>main</code> (cd079bd)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fnondeterminism?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:45 on 2025-12-03 03:07_

I'm not sure I understand why we solve this to `bool`. The constraints are `T <= int | T <= bool`, which seems equivalent to `T <= int`. Why do we prefer solving to `bool` when there's the additional (seemingly redundant) constraint present?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:3426 on 2025-12-03 03:11_

Primer doesn't report any noticeable memory increases, that's good enough for me!

---

_@carljm approved on 2025-12-03 03:14_

It seems a bit sad that we have to do this. I guess ultimately the reason for it boils down to the fact that constraints can involve gradual types, and we don't staticify those via materialization, and this can lead to these path-dependent redundancy determinations. And we can't reduce constraints to fully static types without breaking the gradual guarantee / causing false positives.

But I guess the positive is that this fix isn't complex at all, fixes the issue, and doesn't seem to cause memory issues!

Thank you.

---

_@dcreager reviewed on 2025-12-03 14:37_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:45 on 2025-12-03 14:37_

This is because of our current heuristic for handling multiple paths in a BDD. `T ≤ int ∨ T ≤ bool` ends up looking like this now:

```
(T ≤ int)
┡━₁ (T ≤ bool)
│   ┡━₁ true
│   └─₀ true
└─₀ (T ≤ bool)
    ┡━₁ impossible
    └─₀ false
```

There are two paths to the `true` terminal, one representing `T ≤ bool` and one representing `bool < T ≤ int`. `T = bool` and `T = int` are the largest types that satisfy each respective path. Our current heuristic says that if there's a type that satisfies all paths, we choose that. (That is, if the intersection of the specializations is non-empty, use it.)

I'm very much open to changing that heuristic, but think that should be follow-on work. I'll mark this as a TODO that we'd rather produce `T = int` here.

---

_Merged by @dcreager on 2025-12-03 15:19_

---

_Closed by @dcreager on 2025-12-03 15:19_

---

_Branch deleted on 2025-12-03 15:19_

---
