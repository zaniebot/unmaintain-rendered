```yaml
number: 17403
title: "[red-knot] optimize building large unions of literals"
type: pull_request
state: merged
author: carljm
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: cjm/bigunions3
created_at: 2025-04-15T01:28:59Z
updated_at: 2025-04-16T14:00:39Z
url: https://github.com/astral-sh/ruff/pull/17403
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] optimize building large unions of literals

---

_@carljm_

## Summary

Special-case literal types in `UnionBuilder` to speed up building large unions of literals.

This optimization is extremely effective at speeding up building even a very large union (it improves the large-unions benchmark by 41x!). The problem we can run into is that it is easy to then run into another operation on the very large union (for instance, narrowing may add it to an intersection, which then distributes it over the intersection) which is still slow.

I think it is possible to avoid this by extending this optimized "grouped" representation throughout not just `UnionBuilder`, but all of our union and intersection representations. I have some work in this direction, but rather than spending more time on it right now, I'd rather just land this much, along with a limit on the size of these unions (to avoid building really big unions quickly and then hitting issues where they are used.)

## Test Plan

Existing tests and benchmarks.


---

_Comment by @github-actions[bot] on 2025-04-15 01:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-04-15 01:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fbigunions3)

### Merging #17403 will **improve performances by ×41**

<sub>Comparing <code>cjm/bigunions3</code> (a99bdc3) with <code>main</code> (13ea4e5)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_micro[many_string_assignments] `` | 3,211.2 ms | 79 ms | ×41 |


---

_Label `performance` added by @AlexWaygood on 2025-04-15 09:25_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-15 09:25_

---

_Marked ready for review by @carljm on 2025-04-16 01:20_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-16 01:20_

---

_Review requested from @sharkdp by @carljm on 2025-04-16 01:20_

---

_Review requested from @dcreager by @carljm on 2025-04-16 01:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:191 on 2025-04-16 06:54_

Maybe
```suggestion
                            UnionElement::IntLiterals(literals) => {
                                Type::IntLiteral(*literals.first().unwrap())
                            }
                            UnionElement::StringLiterals(literals) => {
                                Type::StringLiteral(*literals.first().unwrap())
                            }
                            UnionElement::BytesLiterals(literals) => {
                                Type::BytesLiteral(*literals.first().unwrap())
                            }
```

---

_@sharkdp approved on 2025-04-16 07:04_

This looks great, thanks. The fact that we now show union elements grouped by literal type also seems like a UX improvement, even if we potentially disrupt the original source code order. It seems unlikely that someone wants to preserve the order of `Literal[1] | Literal["a"] | Literal[2]`.

---

_@sharkdp reviewed on 2025-04-16 07:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:113 on 2025-04-16 07:05_

Slightly disappointing that we can't use something like the following here (you might have tried as well). Doesn't work with the `FxHasher`, for some reason.
```suggestion
                    self.elements.push(UnionElement::StringLiterals([literal].into()));
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:113 on 2025-04-16 09:38_

This _does_ compile, however:

```suggestion
                    self.elements
                        .push(UnionElement::StringLiterals(FxOrderSet::from_iter([
                            literal,
                        ])));
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:135 on 2025-04-16 09:42_

```suggestion
                    self.elements
                        .push(UnionElement::BytesLiterals(FxOrderSet::from_iter([
                            literal,
                        ])));
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:157 on 2025-04-16 09:42_

```suggestion
                    self.elements
                        .push(UnionElement::IntLiterals(FxOrderSet::from_iter([
                            literal,
                        ])));
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:191 on 2025-04-16 09:44_

or even

```suggestion
                            UnionElement::IntLiterals(literals) => Type::IntLiteral(literals[0]),
                            UnionElement::StringLiterals(literals) => {
                                Type::StringLiteral(literals[0])
                            }
                            UnionElement::BytesLiterals(literals) => {
                                Type::BytesLiteral(literals[0])
                            }
```

---

_@AlexWaygood reviewed on 2025-04-16 09:45_

---

_Comment by @dcreager on 2025-04-16 13:05_

> will **improve performances by ×41**

I guess that's _pretty_ good...

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 13:21_

I think a lot of the logic below would become simpler if this was expanded out into four separate fields:

```suggestion
    elements: Vec<Type<'db>>,
    int_literals: FxOrderSet<i64>,
    string_literals: FxOrderSet<StringLiteralType<'db>>,
    bytes_literals: FxOrderSet<BytesLiteralType<'db>>,
```

That would come at the cost of `UnionBuilder` having to store the three `FxOrderSet`s even if the union being built doesn't contain any literals of that type — but that would only be temporary, since they would disappear as part of building the final `Type`.

It would also mean a fixed ordering for the elements of the resulting union (depending on which order you visit the fields in `build`). That is, e.g., all ints before all strings before all bytes before other types. With the current approach, literals of each type will be grouped together, and the ordering of the types will depend on the order when the first literal of each type is added to the builder. (I don't think either is necessarily better than the other, just calling out a ramification of this suggestion.)

---

A third alternative, which I mentioned briefly in Discord, would be to have a separate field for all literals, keyed by their supertype:

```rust
    elements: Vec<Type<'db>>,
    literals: FxOrderMap<Type<'db>, FxOrderSet<Type<'db>>>,
```

That would also simplify e.g. the `add` logic below, since instead of iterating through `elements` looking for an existing `StringLiterals`, you would use `entry` and `or_default` to create the string literal set if needed.

This version would retain the "types ordered by insertion" property of the current approach, and (I think) is the representation we would want to extend this logic to enum types.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/builder.rs`:38 on 2025-04-16 13:21_

Excellent writeup

---

_@dcreager reviewed on 2025-04-16 13:22_

---

_@carljm reviewed on 2025-04-16 13:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 13:48_

Yes, I considered both of those representations, and they would have some advantages, but I decided against them for the reason you noted: we aim to preserve order of user-provided unions (on a best-effort basis, assuming the union does not simplify). We preferably don't want the user to write `Literal[1] | None` and then we repeat back to them `None | Literal[1]`. The implementation I chose preserves this property (except for grouping same-kind literals, which I think is more acceptable and generally an improvement), and I think remains reasonably straightforward. I think this implementation can also be extended to enums via a new variant of `UnionElement` which holds both the enum super-type as well as a set of all the present enum members.

We could decide that preserving order of user-authored unions is not that important, and it's more important to gain the advantages of another implementation. But I didn't see a strong case for that at this point.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 13:51_

Will go ahead and merge this for now, happy to revisit in another PR if you see sufficiently strong reasons to prefer an alternative.

---

_@carljm reviewed on 2025-04-16 13:51_

---

_@dcreager reviewed on 2025-04-16 13:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 13:54_

> Will go ahead and merge this for now, happy to revisit in another PR if you see sufficiently strong reasons to prefer an alternative.

The third alternative has this property too (since it uses `FxOrderMap` for the outer map), but I'm also okay with merging this as-is

---

_@dcreager reviewed on 2025-04-16 13:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 13:54_

> The third alternative has this property too (since it uses `FxOrderMap` for the outer map), but I'm also okay with merging this as-is

Ohh, although then you don't have that property relative to the non-literal elements.  Never mind, merge away!

---

_@carljm reviewed on 2025-04-16 13:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 13:54_

In terms of linear-scan vs O(1) finding of the right hash-set for a certain literal kind, I'm essentially banking on the theory that this is fine because unions will remain small outside of literal types, and we have to do this for every other type anyway. If that theory is challenged in practice, we would need to adjust.

---

_Merged by @carljm on 2025-04-16 13:55_

---

_Closed by @carljm on 2025-04-16 13:55_

---

_Branch deleted on 2025-04-16 13:55_

---

_@dcreager reviewed on 2025-04-16 14:00_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2025-04-16 14:00_

> In terms of linear-scan vs O(1) finding of the right hash-set for a certain literal kind, I'm essentially banking on the theory that this is fine because unions will remain small outside of literal types, and we have to do this for every other type anyway. If that theory is challenged in practice, we would need to adjust.

Agreed! To clarify, I was less worried about the linear scan because of performance, and more because of code readability.

---
