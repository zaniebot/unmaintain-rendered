---
number: 17579
title: Daily property test run failed on Wed Apr 23 2025
type: issue
state: closed
author: github-actions
labels:
  - bug
  - testing
  - ty
assignees: []
created_at: 2025-04-23T12:13:43Z
updated_at: 2025-04-23T13:11:21Z
url: https://github.com/astral-sh/ruff/issues/17579
synced_at: 2026-01-07T13:12:16-06:00
---

# Daily property test run failed on Wed Apr 23 2025

---

_Issue opened by @github-actions on 2025-04-23 12:13_

Run listed here: https://github.com/astral-sh/ruff/actions/runs/14617781267

---

_Label `bug` added by @github-actions[bot] on 2025-04-23 12:13_

---

_Label `red-knot` added by @github-actions[bot] on 2025-04-23 12:13_

---

_Label `testing` added by @github-actions[bot] on 2025-04-23 12:13_

---

_Comment by @AlexWaygood on 2025-04-23 12:15_

Hmmmm

```
test types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union ... FAILED
test types::property_tests::stable::all_types_assignable_to_object ... FAILED
test types::property_tests::stable::all_type_pairs_are_assignable_to_their_union ... FAILED
test types::property_tests::stable::disjoint_from_is_irreflexive ... FAILED
test types::property_tests::stable::assignable_to_is_reflexive ... FAILED
test types::property_tests::stable::equivalent_to_is_reflexive ... FAILED
test types::property_tests::stable::equivalent_to_is_symmetric ... FAILED
test types::property_tests::stable::equivalent_to_is_transitive ... FAILED
test types::property_tests::stable::all_fully_static_types_subtype_of_object ... FAILED
test types::property_tests::stable::gradual_equivalent_to_is_symmetric ... FAILED
test types::property_tests::stable::never_assignable_to_every_type ... FAILED
test types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping ... FAILED
test types::property_tests::stable::never_subtype_of_every_fully_static_type ... FAILED
test types::property_tests::stable::disjoint_from_is_symmetric ... FAILED
test types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence ... FAILED
test types::property_tests::stable::subtype_of_is_antisymmetric ... FAILED
test types::property_tests::stable::subtype_of_implies_assignable_to ... FAILED
test types::property_tests::stable::subtype_of_is_reflexive ... FAILED
test types::property_tests::stable::singleton_implies_single_valued ... FAILED
test types::property_tests::stable::subtype_of_is_transitive ... FAILED
test types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent ... FAILED
test types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent ... FAILED
test types::property_tests::stable::subtype_of_implies_not_disjoint_from ... FAILED

failures:

---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n7")), annotated_ty: Some(Union([StringLiteral(""), BuiltinsBoundMethod { class: "str", method: "isascii" }, AlwaysTruthy])), default_ty: None }]), returns: Some(BuiltinsBoundMethod { class: "int", method: "bit_length" }) }, BuiltinClassLiteral("object")]), AlwaysTruthy)
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::all_types_assignable_to_object stdout ----

thread 'types::property_tests::stable::all_types_assignable_to_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_types_assignable_to_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_types_assignable_to_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_types_assignable_to_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_types_assignable_to_object' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Intersection { pos: [], neg: [Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n9")), annotated_ty: Some(KnownClassInstance(TypeAliasType)), default_ty: Some(Tuple([SubclassOfBuiltinClass("str"), BuiltinClassLiteral("int")])) }, Param { kind: KeywordVariadic, name: Some(Name("n7")), annotated_ty: Some(Tuple([SubclassOfAbcClass("ABC")])), default_ty: None }]), returns: Some(StringLiteral("a")) }, BuiltinClassLiteral("int")] })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Callable { params: List([]), returns: Some(None) }, BuiltinClassLiteral("int"))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::disjoint_from_is_irreflexive stdout ----

thread 'types::property_tests::stable::disjoint_from_is_irreflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::disjoint_from_is_irreflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::disjoint_from_is_irreflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::disjoint_from_is_irreflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([BuiltinClassLiteral("str"), Callable { params: List([]), returns: Some(BuiltinsFunction("chr")) }]))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::assignable_to_is_reflexive stdout ----

thread 'types::property_tests::stable::assignable_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::assignable_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function
error: test failed, to rerun pass `-p red_knot_python_semantic --lib`

thread 'types::property_tests::stable::assignable_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::assignable_to_is_reflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Intersection { pos: [BuiltinClassLiteral("int")], neg: [Callable { params: List([Param { kind: Variadic, name: Some(Name("n5")), annotated_ty: Some(Callable { params: List([]), returns: Some(KnownClassInstance(FunctionType)) }), default_ty: None }, Param { kind: KeywordVariadic, name: Some(Name("n0")), annotated_ty: Some(KnownClassInstance(Tuple)), default_ty: None }]), returns: Some(SubclassOfBuiltinClass("object")) }] })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::equivalent_to_is_reflexive stdout ----

thread 'types::property_tests::stable::equivalent_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_reflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([Callable { params: List([Param { kind: Variadic, name: Some(Name("n8")), annotated_ty: Some(Union([KnownClassInstance(TypeVar), AlwaysTruthy, BuiltinsFunction("ascii")])), default_ty: None }, Param { kind: KeywordVariadic, name: Some(Name("n2")), annotated_ty: Some(BuiltinsBoundMethod { class: "str", method: "isascii" }), default_ty: None }]), returns: Some(AbcInstance("ABCMeta")) }, BuiltinClassLiteral("str")]))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::equivalent_to_is_symmetric stdout ----

thread 'types::property_tests::stable::equivalent_to_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_symmetric' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Tuple([AbcClassLiteral("ABCMeta")]), Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n7")), annotated_ty: Some(Intersection { pos: [Tuple([AbcClassLiteral("ABCMeta")])], neg: [Union([BuiltinClassLiteral("bool"), StringLiteral(""), KnownClassInstance(TypeVar)])] }), default_ty: Some(Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n0")), annotated_ty: Some(Intersection { pos: [Union([BuiltinClassLiteral("int"), BytesLiteral("\0")])], neg: [Callable { params: List([Param { kind: PositionalOrKeyword, name: Some(Name("n1")), annotated_ty: Some(Callable { params: List([Param { kind: KeywordVariadic, name: Some(Name("n3")), annotated_ty: Some(Callable { params: List([]), returns: Some(BytesLiteral("")) }), default_ty: None }]), returns: Some(SubclassOfAbcClass("ABCMeta")) }), default_ty: None }, Param { kind: KeywordOnly, name: Some(Name("n0")), annotated_ty: Some(KnownClassInstance(Tuple)), default_ty: Some(Callable { params: GradualForm, returns: Some(BuiltinClassLiteral("object")) }) }, Param { kind: KeywordVariadic, name: Some(Name("n2")), annotated_ty: Some(Tuple([None])), default_ty: None }]), returns: Some(KnownClassInstance(Tuple)) }] }), default_ty: None }, Param { kind: KeywordOnly, name: Some(Name("n4")), annotated_ty: Some(Callable { params: GradualForm, returns: None }), default_ty: None }, Param { kind: KeywordVariadic, name: Some(Name("n5")), annotated_ty: Some(Union([Union([SubclassOfBuiltinClass("str"), IntLiteral(-1)]), Union([TypingLiteral, KnownClassInstance(List), KnownClassInstance(FunctionType)]), BuiltinsFunction("ascii")])), default_ty: None }]), returns: None }) }]), returns: None })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::equivalent_to_is_transitive stdout ----

thread 'types::property_tests::stable::equivalent_to_is_transitive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_transitive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_transitive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_transitive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::equivalent_to_is_transitive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([Intersection { pos: [], neg: [BuiltinClassLiteral("int")] }, Callable { params: List([Param { kind: PositionalOrKeyword, name: Some(Name("n9")), annotated_ty: Some(Tuple([BuiltinsBoundMethod { class: "int", method: "bit_length" }])), default_ty: Some(SubclassOfBuiltinClass("str")) }]), returns: Some(AbcInstance("ABCMeta")) }]), Tuple([StringLiteral("")]), IntLiteral(-1))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::all_fully_static_types_subtype_of_object stdout ----

thread 'types::property_tests::stable::all_fully_static_types_subtype_of_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_fully_static_types_subtype_of_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_fully_static_types_subtype_of_object' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::all_fully_static_types_subtype_of_object' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([BuiltinClassLiteral("object"), Callable { params: List([Param { kind: KeywordVariadic, name: Some(Name("n6")), annotated_ty: Some(Union([LiteralString, AbcInstance("ABCMeta")])), default_ty: None }]), returns: Some(AlwaysTruthy) }]))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::gradual_equivalent_to_is_symmetric stdout ----

thread 'types::property_tests::stable::gradual_equivalent_to_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::gradual_equivalent_to_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::gradual_equivalent_to_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::gradual_equivalent_to_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::gradual_equivalent_to_is_symmetric' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([BuiltinClassLiteral("int"), Callable { params: List([Param { kind: KeywordVariadic, name: Some(Name("n2")), annotated_ty: Some(KnownClassInstance(FunctionType)), default_ty: None }]), returns: Some(KnownClassInstance(NoDefaultType)) }]), Callable { params: GradualForm, returns: None })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::never_assignable_to_every_type stdout ----

thread 'types::property_tests::stable::never_assignable_to_every_type' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::never_assignable_to_every_type' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::never_assignable_to_every_type' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::never_assignable_to_every_type' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::never_assignable_to_every_type' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Intersection { pos: [BuiltinClassLiteral("bool"), Callable { params: List([]), returns: Some(AlwaysTruthy) }], neg: [] })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping stdout ----

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n4")), annotated_ty: Some(Union([Tuple([]), Tuple([BuiltinInstance("type")])])), default_ty: Some(Union([Union([LiteralString, SubclassOfAbcClass("ABCMeta")]), Union([SubclassOfBuiltinClass("type"), BuiltinClassLiteral("str")])])) }, Param { kind: KeywordOnly, name: Some(Name("n8")), annotated_ty: None, default_ty: Some(IntLiteral(1)) }, Param { kind: KeywordOnly, name: Some(Name("n7")), annotated_ty: None, default_ty: Some(Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n1")), annotated_ty: Some(Tuple([BuiltinsBoundMethod { class: "int", method: "bit_length" }])), default_ty: Some(StringLiteral("")) }, Param { kind: KeywordOnly, name: Some(Name("n2")), annotated_ty: None, default_ty: Some(KnownClassInstance(Str)) }, Param { kind: KeywordVariadic, name: Some(Name("n3")), annotated_ty: Some(BuiltinClassLiteral("str")), default_ty: None }]), returns: None }) }, Param { kind: KeywordOnly, name: Some(Name("n1")), annotated_ty: None, default_ty: Some(Intersection { pos: [Union([SubclassOfAbcClass("ABCMeta"), StringLiteral("a"), AbcInstance("ABC")]), Tuple([AbcInstance("ABC")])], neg: [Tuple([])] }) }, Param { kind: KeywordVariadic, name: Some(Name("n0")), annotated_ty: Some(Union([Intersection { pos: [BuiltinClassLiteral("str"), BuiltinClassLiteral("str")], neg: [BytesLiteral("")] }, Tuple([]), Callable { params: List([]), returns: Some(KnownClassInstance(Object)) }])), default_ty: None }]), returns: Some(Tuple([])) }, Callable { params: GradualForm, returns: Some(Never) })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::never_subtype_of_every_fully_static_type stdout ----

thread 'types::property_tests::stable::never_subtype_of_every_fully_static_type' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::never_subtype_of_every_fully_static_type' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::never_subtype_of_every_fully_static_type' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([Callable { params: List([]), returns: Some(SubclassOfBuiltinClass("type")) }, BuiltinClassLiteral("bool")]))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::disjoint_from_is_symmetric stdout ----

thread 'types::property_tests::stable::disjoint_from_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::disjoint_from_is_symmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::disjoint_from_is_symmetric' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([Callable { params: List([Param { kind: Variadic, name: Some(Name("n6")), annotated_ty: Some(Tuple([BuiltinClassLiteral("str")])), default_ty: None }]), returns: Some(StringLiteral("")) }, BuiltinClassLiteral("str")]), SubclassOfAbcClass("ABC"))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence stdout ----

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([BuiltinClassLiteral("int"), Callable { params: List([Param { kind: KeywordVariadic, name: Some(Name("n2")), annotated_ty: Some(Intersection { pos: [], neg: [LiteralString, BuiltinClassLiteral("bool")] }), default_ty: None }]), returns: Some(KnownClassInstance(NoDefaultType)) }]), KnownClassInstance(NoDefaultType))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::subtype_of_is_antisymmetric stdout ----

thread 'types::property_tests::stable::subtype_of_is_antisymmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_is_antisymmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_is_antisymmetric' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_is_antisymmetric' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Intersection { pos: [BuiltinClassLiteral("bool")], neg: [Callable { params: List([Param { kind: PositionalOrKeyword, name: Some(Name("n5")), annotated_ty: Some(Callable { params: List([]), returns: Some(BuiltinsBoundMethod { class: "int", method: "bit_length" }) }), default_ty: None }, Param { kind: Variadic, name: Some(Name("n6")), annotated_ty: Some(Intersection { pos: [BytesLiteral(""), BuiltinInstance("type")], neg: [AbcClassLiteral("ABCMeta"), BuiltinsBoundMethod { class: "int", method: "bit_length" }] }), default_ty: None }]), returns: Some(SubclassOfBuiltinClass("object")) }] }, Intersection { pos: [Callable { params: GradualForm, returns: None }], neg: [] })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::subtype_of_implies_assignable_to stdout ----

thread 'types::property_tests::stable::subtype_of_implies_assignable_to' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_implies_assignable_to' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n5")), annotated_ty: Some(Union([Callable { params: List([Param { kind: KeywordVariadic, name: Some(Name("n1")), annotated_ty: Some(BuiltinClassLiteral("bool")), default_ty: None }]), returns: Some(None) }, Union([SubclassOfAbcClass("ABCMeta"), BuiltinClassLiteral("object")]), AbcClassLiteral("ABC")])), default_ty: None }, Param { kind: KeywordOnly, name: Some(Name("n1")), annotated_ty: None, default_ty: Some(Union([Tuple([BuiltinClassLiteral("int")]), BuiltinsBoundMethod { class: "int", method: "bit_length" }])) }]), returns: Some(Tuple([Never])) }, KnownClassInstance(Int))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::subtype_of_is_reflexive stdout ----

thread 'types::property_tests::stable::subtype_of_is_reflexive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_is_reflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Intersection { pos: [BuiltinClassLiteral("bool")], neg: [Callable { params: List([Param { kind: Variadic, name: Some(Name("n8")), annotated_ty: Some(BuiltinsFunction("ascii")), default_ty: None }]), returns: Some(LiteralString) }] })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::singleton_implies_single_valued stdout ----

thread 'types::property_tests::stable::singleton_implies_single_valued' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::singleton_implies_single_valued' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([Intersection { pos: [BuiltinClassLiteral("str")], neg: [] }, Callable { params: List([Param { kind: PositionalOrKeyword, name: Some(Name("n6")), annotated_ty: Some(Tuple([KnownClassInstance(Object), AbcClassLiteral("ABCMeta")])), default_ty: None }]), returns: Some(KnownClassInstance(Str)) }]))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::subtype_of_is_transitive stdout ----

thread 'types::property_tests::stable::subtype_of_is_transitive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_is_transitive' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_is_transitive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (BuiltinClassLiteral("str"), Callable { params: List([]), returns: Some(Union([SubclassOfBuiltinClass("str"), BuiltinsFunction("chr")])) }, Intersection { pos: [Callable { params: GradualForm, returns: None }], neg: [] })
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent stdout ----

thread 'types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Tuple([BuiltinsFunction("ascii")]), Union([Callable { params: List([Param { kind: KeywordOnly, name: Some(Name("n3")), annotated_ty: Some(KnownClassInstance(TypeVar)), default_ty: Some(Intersection { pos: [BuiltinClassLiteral("object")], neg: [SubclassOfBuiltinClass("str")] }) }]), returns: Some(StringLiteral("a")) }, BuiltinClassLiteral("bool")]))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent stdout ----

thread 'types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Callable { params: List([Param { kind: Variadic, name: Some(Name("n7")), annotated_ty: Some(Intersection { pos: [], neg: [Callable { params: List([Param { kind: PositionalOrKeyword, name: Some(Name("n0")), annotated_ty: Some(BuiltinClassLiteral("bool")), default_ty: Some(SubclassOfAny) }]), returns: Some(KnownClassInstance(SpecialForm)) }, Union([BuiltinsFunction("ascii"), BuiltinClassLiteral("str"), AbcInstance("ABC")])] }), default_ty: None }, Param { kind: KeywordOnly, name: Some(Name("n2")), annotated_ty: Some(Callable { params: GradualForm, returns: Some(Any) }), default_ty: None }, Param { kind: KeywordVariadic, name: Some(Name("n8")), annotated_ty: None, default_ty: None }]), returns: Some(Tuple([AbcInstance("ABC"), AbcInstance("ABC")])) }, BuiltinClassLiteral("object"))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"

---- types::property_tests::stable::subtype_of_implies_not_disjoint_from stdout ----

thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/zalsa_local.rs:237:46:
cannot create a tracked struct disambiguator outside of a tracked function

thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED (runtime error). Arguments: (Union([BuiltinClassLiteral("bool"), Callable { params: List([Param { kind: KeywordVariadic, name: Some(Name("n3")), annotated_ty: Some(Union([KnownClassInstance(TypeAliasType), KnownClassInstance(Bool)])), default_ty: None }]), returns: Some(BuiltinClassLiteral("bool")) }]), SubclassOfBuiltinClass("type"))
Error: "cannot create a tracked struct disambiguator outside of a tracked function"


failures:
    types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union
    types::property_tests::stable::all_fully_static_types_subtype_of_object
    types::property_tests::stable::all_type_pairs_are_assignable_to_their_union
    types::property_tests::stable::all_types_assignable_to_object
    types::property_tests::stable::assignable_to_is_reflexive
    types::property_tests::stable::disjoint_from_is_irreflexive
    types::property_tests::stable::disjoint_from_is_symmetric
    types::property_tests::stable::equivalent_to_is_reflexive
    types::property_tests::stable::equivalent_to_is_symmetric
    types::property_tests::stable::equivalent_to_is_transitive
    types::property_tests::stable::gradual_equivalent_to_is_symmetric
    types::property_tests::stable::never_assignable_to_every_type
    types::property_tests::stable::never_subtype_of_every_fully_static_type
    types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence
    types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping
    types::property_tests::stable::singleton_implies_single_valued
    types::property_tests::stable::subtype_of_implies_assignable_to
    types::property_tests::stable::subtype_of_implies_not_disjoint_from
    types::property_tests::stable::subtype_of_is_antisymmetric
    types::property_tests::stable::subtype_of_is_reflexive
    types::property_tests::stable::subtype_of_is_transitive
    types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent
    types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent

test result: FAILED. 0 passed; 23 failed; 0 ignored; 0 measured; 210 filtered out; finished in 0.09s
```

---

_Comment by @AlexWaygood on 2025-04-23 12:16_

Ugh. I can reproduce this locally.

---

_Comment by @MichaReiser on 2025-04-23 12:17_

This looks like we call a function that creates tracked struct but it isn't enclosed by a salsa query (somehwere, doesn't have to be the function itself)

---

_Comment by @AlexWaygood on 2025-04-23 12:18_

I'm bisecting it now

---

_Comment by @AlexWaygood on 2025-04-23 12:23_

`git bisect` points to e45f23b0ec1efd093c6588f9b7d56bf87c4eebae:

```
e45f23b0ec1efd093c6588f9b7d56bf87c4eebae is the first bad commit
commit e45f23b0ec1efd093c6588f9b7d56bf87c4eebae
Author: Matthew Mckee <matthewmckee04@yahoo.co.uk>
Date:   Wed Apr 23 06:40:33 2025 +0100

    [red-knot] Class literal `__new__` function callable subtyping (#17533)
    
    ## Summary
    
    From
    https://typing.python.org/en/latest/spec/constructors.html#converting-a-constructor-to-callable
    
    this covers step 2 and partially step 3 (always respecting the
    `__new__`)
    
    ## Test Plan
    
    Update is_subtype_of.md
    
    ---------
    
    Co-authored-by: Carl Meyer <carl@astral.sh>

 .../mdtest/type_properties/is_subtype_of.md        | 50 ++++++++++++++++++++++
 crates/red_knot_python_semantic/src/types.rs       | 31 ++++++--------
 crates/red_knot_python_semantic/src/types/class.rs | 37 ++++++++++++++++
 3 files changed, 99 insertions(+), 19 deletions(-)
```

And I double-checked that the property tests all pass prior to that commit, but all fail after it. I don't see anything obviously wrong with the changes that commit made, though?

---

_Comment by @MichaReiser on 2025-04-23 12:37_

I think the problem is that `is_subtype_of` now calls `Class::into_callable` which calls `into_bound_method_type` which creates a salsa tracked `BoundMethodType`

We'll need to introduce an intermediate salsa query in the property tests somewhere... 

---

_Referenced in [astral-sh/ruff#17581](../../astral-sh/ruff/pulls/17581.md) on 2025-04-23 12:57_

---

_Closed by @MichaReiser on 2025-04-23 13:11_

---
