```yaml
number: 17006
title: "[red-knot] Add property tests for callable types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-property-tests
created_at: 2025-03-27T02:09:58Z
updated_at: 2025-04-01T21:09:16Z
url: https://github.com/astral-sh/ruff/pull/17006
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Add property tests for callable types

---

_Pull request opened by @dhruvmanila on 2025-03-27 02:09_

## Summary

Part of #15382, this PR adds property tests for callable types.

Specifically, this PR updates the property tests to generate an arbitrary signature for a general callable type which includes:
* Arbitrary combination of parameter kinds in the correct order
* Arbitrary number of parameters
* Arbitrary optional types for annotation and return type
* Arbitrary parameter names (no duplicate names), optional for positional-only parameters

## Test Plan

```
QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable
```

Also, the commands in CI:

https://github.com/astral-sh/ruff/blob/d72b4100a33ccb35047f86e900bede6310c7c119/.github/workflows/daily_property_tests.yaml#L47-L52

---

_Label `red-knot` added by @dhruvmanila on 2025-03-27 02:09_

---

_Comment by @github-actions[bot] on 2025-03-27 02:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @dhruvmanila on 2025-03-27 18:16_

This is blocked on adding support for `is_disjoint_from` for callable types otherwise there are failures:

```
---- types::property_tests::stable::subtype_of_implies_not_disjoint_from stdout ----
thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /Users/dhruv/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [Callable { params: List([]), returns: Some(AbcInstance("ABCMeta")) }, None], neg: [] }, AlwaysTruthy)

---- types::property_tests::stable::subtype_of_implies_not_disjoint_from stdout ----
thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /Users/dhruv/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [Callable { params: List([]), returns: Some(SubclassOfBuiltinClass("object")) }, StringLiteral("a")], neg: [] }, LiteralString)
```

---

_Comment by @dhruvmanila on 2025-03-31 22:25_

Ok, this seems to be discovering multiple failures which, by the looks of it, is probably related to the fact that we still need to handle relationship between the general `Callable` and other types:

`subtype_of_implies_not_disjoint_from`:

```
Arguments: (
    Intersection {
        pos: [
            Callable { 
                params: List([
                    Param { 
                        kind: KeywordVariadic, 
                        name: Some(Name("n5")), 
                        annotated_ty: Some(Tuple([])), 
                        default_ty: None
                    }
                ]), 
                returns: Some(AlwaysTruthy) 
            }, 
            BytesLiteral("")
        ],
        neg: []
    },
    AlwaysTruthy
)
```

`subtype_of_is_transitive`:

```
Arguments: (
    None,
    AlwaysFalsy,
    Intersection {
        pos: [],
        neg: [Callable {
            params: List([]),
            returns: Some(KnownClassInstance(Str)),
        }],
    },
)
```

`subtype_of_is_transitive`:

```
Arguments: (
    Callable {
        params: List([
            Param {
                kind: KeywordOnly,
                name: Some(Name("n6")),
                annotated_ty: Some(Union([
                    Intersection { pos: [], neg: [] },
                    Intersection {
                        pos: [],
                        neg: [KnownClassInstance(List), KnownClassInstance(List)],
                    },
                    Tuple([SubclassOfAny, KnownClassInstance(TypeAliasType)]),
                ])),
                default_ty: Some(Tuple([IntLiteral(0)])),
            },
            Param {
                kind: KeywordVariadic,
                name: Some(Name("n5")),
                annotated_ty: Some(Union([
                    KnownClassInstance(Tuple),
                    Intersection {
                        pos: [BuiltinClassLiteral("str")],
                        neg: [IntLiteral(-1), AlwaysTruthy],
                    },
                ])),
                default_ty: None,
            },
        ]),
        returns: Some(Intersection {
            pos: [AbcInstance("ABC")],
            neg: [SubclassOfAbcClass("ABC")],
        }),
    },
    Intersection {
        pos: [],
        neg: [LiteralString],
    },
    Intersection {
        pos: [],
        neg: [StringLiteral("")],
    },
)
```

---

_Marked ready for review by @dhruvmanila on 2025-04-01 19:06_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-01 19:06_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-01 19:06_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-01 19:06_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-01 19:06_

---

_@carljm approved on 2025-04-01 19:32_

Excellent!

---

_Merged by @dhruvmanila on 2025-04-01 19:37_

---

_Closed by @dhruvmanila on 2025-04-01 19:37_

---

_Branch deleted on 2025-04-01 19:37_

---

_@sharkdp reviewed on 2025-04-01 21:09_

Very nice!

---
