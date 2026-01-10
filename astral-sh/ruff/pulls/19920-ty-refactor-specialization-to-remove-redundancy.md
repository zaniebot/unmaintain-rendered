```yaml
number: 19920
title: "[ty] Refactor `Specialization` to remove redundancy between `types` and `tuple_inner` fields"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
base: main
head: alex/thinner-spec
created_at: 2025-08-14T19:36:41Z
updated_at: 2025-08-15T13:53:49Z
url: https://github.com/astral-sh/ruff/pull/19920
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Refactor `Specialization` to remove redundancy between `types` and `tuple_inner` fields

---

_Pull request opened by @AlexWaygood on 2025-08-14 19:36_

## Summary

Fixes https://github.com/astral-sh/ty/issues/985.

## Test Plan

Existing tests.

Co-authored-by: Douglas Creager <dcreager@dcreager.net>


---

_Label `ty` added by @AlexWaygood on 2025-08-14 19:36_

---

_Comment by @github-actions[bot] on 2025-08-14 19:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-15 10:17:26.761238682 +0000
+++ new-output.txt	2025-08-15 10:17:26.829239179 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(3869)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1e049)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @AlexWaygood on 2025-08-14 19:40_

Aha! Good job we added those assertions @dcreager -- one of them is failing on a mypy_primer project 😆

---

_Comment by @AlexWaygood on 2025-08-14 20:14_

Minimized repro of the primer panic:

```py
def _(val):
    if type(val) is tuple:
        coerced_val = val
```

---

_@dcreager reviewed on 2025-08-14 20:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:360 on 2025-08-14 20:33_

For the record, this is where we had originally put a comment saying "Doug is convinced this will be okay, but Alex is skeptical" :smile: 

So I guess we do need to handle the tuple case here. For that case, we should assert that `types` has one element, and create a `SpecializationInner::Tuple` containing a homogeneous tuple of that type.

---

_Comment by @github-actions[bot] on 2025-08-14 20:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-14 21:18_

Here's the memory report on `main` when running ty on a large project locally:

<details>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=602.31MB fields=1204.62MB count=15057724
`Expression`                                       metadata=609.79MB fields=711.42MB count=12703891
`StringLiteralType`                                metadata=128.97MB fields=185.36MB count=2302978
`IntersectionType`                                 metadata=54.03MB  fields=138.57MB count=964778
`OverloadLiteral`                                  metadata=99.54MB  fields=101.00MB count=1777501
`UnionType`                                        metadata=55.05MB  fields=79.17MB  count=982992
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=68.22MB  fields=58.47MB  count=1218202
`File`                                             metadata=13.69MB  fields=49.39MB  count=213845
`FunctionType`                                     metadata=103.69MB fields=46.87MB  count=1851552
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=46.25MB  fields=39.64MB  count=825879
`place_by_id::interned_arguments`                  metadata=98.81MB  fields=38.01MB  count=1900253
`ScopeId`                                          metadata=74.14MB  fields=31.77MB  count=2647752
`FunctionLiteral`                                  metadata=99.56MB  fields=28.45MB  count=1777905
`TupleType`                                        metadata=13.47MB  fields=28.21MB  count=240455
`ClassLiteral`                                     metadata=23.50MB  fields=25.49MB  count=419732
`CallableType`                                     metadata=3.26MB   fields=25.18MB  count=58127
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=28.63MB  fields=20.45MB  count=511190
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=19.44MB  fields=16.66MB  count=347108
`Unpack`                                           metadata=8.60MB   fields=10.32MB  count=214971
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=28.19MB  fields=8.05MB   count=503346
`Specialization`                                   metadata=8.38MB   fields=7.66MB   count=149718
`Type < 'db >::apply_specialization_::interned_arguments` metadata=13.46MB  fields=5.77MB   count=240271
`ModuleNameIngredient`                             metadata=3.64MB   fields=4.92MB   count=64962
`PropertyInstanceType`                             metadata=7.29MB   fields=4.16MB   count=130095
`BoundMethodType`                                  metadata=9.67MB   fields=4.15MB   count=172739
`BytesLiteralType`                                 metadata=0.88MB   fields=2.63MB   count=15683
`GenericAlias`                                     metadata=7.72MB   fields=2.20MB   count=137772
`BoundSuperType`                                   metadata=3.98MB   fields=2.14MB   count=76584
`Project`                                          metadata=0.00MB   fields=1.84MB   count=1
`ModuleLiteralType`                                metadata=3.46MB   fields=1.33MB   count=66584
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=4.16MB   fields=1.19MB   count=74347
`TypeVarInstance`                                  metadata=0.65MB   fields=0.84MB   count=11674
`EnumLiteralType`                                  metadata=0.95MB   fields=0.76MB   count=17047
`ProtocolInterface`                                metadata=0.47MB   fields=0.75MB   count=8407
`GenericContext`                                   metadata=0.48MB   fields=0.69MB   count=8599
`BoundTypeVarInstance`                             metadata=1.47MB   fields=0.42MB   count=26294
`StarImportPlaceholderPredicate`                   metadata=0.49MB   fields=0.35MB   count=17375
`PatternPredicate`                                 metadata=0.11MB   fields=0.27MB   count=3374
`TypeIsType`                                       metadata=0.20MB   fields=0.12MB   count=3599
`FileModule`                                       metadata=0.01MB   fields=0.04MB   count=401
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`FieldInstance`                                    metadata=0.01MB   fields=0.00MB   count=161
`DeprecatedInstance`                               metadata=0.01MB   fields=0.00MB   count=184
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=1
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=752.08MB fields=15860.00MB count=196657
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=1178.94MB fields=4283.07MB count=2630806
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=2014.75MB fields=3351.43MB count=13857357
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=3462.86MB fields=2757.83MB count=12111828
`source_text -> ruff_db::source::SourceText`
    metadata=12.58MB  fields=1718.88MB count=196662
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap>`
    metadata=19.13MB  fields=1625.37MB count=367809
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=55.12MB  fields=1114.26MB count=196266
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=43.79MB  fields=823.69MB count=842120
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=181.32MB fields=139.20MB count=549686
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=187.53MB fields=124.12MB count=1566254
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=38.44MB  fields=87.61MB  count=385471
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=127.48MB fields=86.59MB  count=1047915
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=130.62MB fields=60.81MB  count=1900253
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=14.95MB  fields=60.01MB  count=196658
`infer_expression_type -> ty_python_semantic::types::Type`
    metadata=265.34MB fields=55.86MB  count=3491136
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=86.65MB  fields=42.64MB  count=503346
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=104.03MB fields=39.17MB  count=1612139
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=153.82MB fields=38.98MB  count=1218202
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=37.95MB  fields=38.98MB  count=212248
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=12.56MB  fields=29.16MB  count=196262
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=154.16MB fields=26.43MB  count=825879
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=83.48MB  fields=20.12MB  count=419246
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=50.10MB  fields=16.36MB  count=511190
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=58.45MB  fields=14.01MB  count=159245
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=31.42MB  fields=12.90MB  count=419502
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=159.04MB fields=8.33MB   count=347108
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=2.32MB   fields=7.34MB   count=44707
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=13.97MB  fields=3.84MB   count=240271
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=30.20MB  fields=3.36MB   count=419501
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=9.65MB   fields=3.08MB   count=141286
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=12.50MB  fields=2.38MB   count=74347
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=14.13MB  fields=1.57MB   count=196262
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=7.34MB   fields=1.13MB   count=141090
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=10.75MB  fields=0.78MB   count=64962
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=4.28MB   fields=0.56MB   count=46525
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind>`
    metadata=32.45MB  fields=0.42MB   count=419095
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=27.70MB  fields=0.42MB   count=418812
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=39.26MB  fields=0.41MB   count=411471
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature`
    metadata=0.14MB   fields=0.38MB   count=1459
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType`
    metadata=6.92MB   fields=0.27MB   count=22833
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.30MB   fields=0.24MB   count=2444
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.29MB   fields=0.23MB   count=2587
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.01MB   fields=0.12MB   count=110
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.01MB   fields=0.05MB   count=92
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.13MB   fields=0.00MB   count=559
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 47227.28MB
    struct metadata = 2246.61MB
    struct fields = 2889.33MB
    memo metadata = 9628.94MB
    memo fields = 32462.39MB
```

</details>

And here's the memory report with this PR:

<details>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=602.31MB fields=1204.62MB count=15057724
`Expression`                                       metadata=609.79MB fields=711.42MB count=12703891
`StringLiteralType`                                metadata=128.97MB fields=185.36MB count=2302978
`IntersectionType`                                 metadata=53.67MB  fields=137.74MB count=958359
`OverloadLiteral`                                  metadata=99.54MB  fields=101.00MB count=1777501
`UnionType`                                        metadata=54.43MB  fields=78.37MB  count=971911
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=68.22MB  fields=58.47MB  count=1218195
`File`                                             metadata=13.69MB  fields=49.39MB  count=213845
`FunctionType`                                     metadata=103.68MB fields=46.86MB  count=1851477
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=46.25MB  fields=39.64MB  count=825872
`place_by_id::interned_arguments`                  metadata=98.81MB  fields=38.01MB  count=1900253
`ScopeId`                                          metadata=74.14MB  fields=31.77MB  count=2647752
`FunctionLiteral`                                  metadata=99.56MB  fields=28.45MB  count=1777905
`TupleType`                                        metadata=13.47MB  fields=28.21MB  count=240465
`ClassLiteral`                                     metadata=23.50MB  fields=25.49MB  count=419732
`CallableType`                                     metadata=3.26MB   fields=25.18MB  count=58131
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=28.63MB  fields=20.45MB  count=511186
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=19.44MB  fields=16.66MB  count=347106
`Unpack`                                           metadata=8.60MB   fields=10.32MB  count=214971
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=28.17MB  fields=8.05MB   count=503112
`Specialization`                                   metadata=8.37MB   fields=6.08MB   count=149396
`Type < 'db >::apply_specialization_::interned_arguments` metadata=13.46MB  fields=5.77MB   count=240271
`ModuleNameIngredient`                             metadata=3.64MB   fields=4.92MB   count=64962
`PropertyInstanceType`                             metadata=7.29MB   fields=4.16MB   count=130095
`BoundMethodType`                                  metadata=9.67MB   fields=4.15MB   count=172740
`BytesLiteralType`                                 metadata=0.88MB   fields=2.63MB   count=15683
`GenericAlias`                                     metadata=7.70MB   fields=2.20MB   count=137527
`BoundSuperType`                                   metadata=3.98MB   fields=2.14MB   count=76584
`Project`                                          metadata=0.00MB   fields=1.84MB   count=1
`ModuleLiteralType`                                metadata=3.46MB   fields=1.33MB   count=66584
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=4.16MB   fields=1.19MB   count=74347
`TypeVarInstance`                                  metadata=0.65MB   fields=0.84MB   count=11673
`EnumLiteralType`                                  metadata=0.95MB   fields=0.76MB   count=17047
`ProtocolInterface`                                metadata=0.47MB   fields=0.75MB   count=8409
`GenericContext`                                   metadata=0.48MB   fields=0.69MB   count=8600
`BoundTypeVarInstance`                             metadata=1.47MB   fields=0.42MB   count=26292
`StarImportPlaceholderPredicate`                   metadata=0.49MB   fields=0.35MB   count=17375
`PatternPredicate`                                 metadata=0.11MB   fields=0.27MB   count=3374
`TypeIsType`                                       metadata=0.20MB   fields=0.12MB   count=3599
`FileModule`                                       metadata=0.01MB   fields=0.04MB   count=401
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`FieldInstance`                                    metadata=0.01MB   fields=0.00MB   count=161
`DeprecatedInstance`                               metadata=0.01MB   fields=0.00MB   count=184
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=1
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=752.08MB fields=15860.00MB count=196657
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=1178.94MB fields=4283.07MB count=2630806
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=2014.75MB fields=3351.43MB count=13857357
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=3462.84MB fields=2757.83MB count=12111828
`source_text -> ruff_db::source::SourceText`
    metadata=12.58MB  fields=1718.88MB count=196662
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap>`
    metadata=19.13MB  fields=1625.37MB count=367809
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=55.12MB  fields=1114.26MB count=196266
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=43.79MB  fields=823.69MB count=842120
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=181.32MB fields=139.20MB count=549686
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=187.47MB fields=124.12MB count=1566254
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=38.45MB  fields=87.61MB  count=385471
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=127.43MB fields=86.59MB  count=1047915
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=130.62MB fields=60.81MB  count=1900253
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=14.95MB  fields=60.01MB  count=196658
`infer_expression_type -> ty_python_semantic::types::Type`
    metadata=265.34MB fields=55.86MB  count=3491136
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=86.66MB  fields=42.62MB  count=503112
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=104.03MB fields=39.17MB  count=1612139
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=153.82MB fields=38.98MB  count=1218195
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=37.95MB  fields=38.98MB  count=212248
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=12.56MB  fields=29.16MB  count=196262
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=154.16MB fields=26.43MB  count=825872
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=83.50MB  fields=20.12MB  count=419246
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=50.10MB  fields=16.36MB  count=511186
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=58.45MB  fields=14.01MB  count=159245
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=31.42MB  fields=12.90MB  count=419502
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=159.04MB fields=8.33MB   count=347106
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=2.32MB   fields=7.34MB   count=44707
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=13.97MB  fields=3.84MB   count=240271
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=30.20MB  fields=3.36MB   count=419501
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=9.65MB   fields=3.08MB   count=141286
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=12.50MB  fields=2.38MB   count=74347
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=14.13MB  fields=1.57MB   count=196262
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=7.34MB   fields=1.13MB   count=141090
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=10.75MB  fields=0.78MB   count=64962
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=4.28MB   fields=0.56MB   count=46525
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind>`
    metadata=32.45MB  fields=0.42MB   count=419095
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=27.70MB  fields=0.42MB   count=418812
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=39.26MB  fields=0.41MB   count=411471
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature`
    metadata=0.14MB   fields=0.38MB   count=1459
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType`
    metadata=4.28MB   fields=0.27MB   count=22764
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.30MB   fields=0.24MB   count=2444
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.29MB   fields=0.23MB   count=2587
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.01MB   fields=0.12MB   count=110
`homogeneous_element_type -> ty_python_semantic::types::Type`
    metadata=1.20MB   fields=0.08MB   count=5261
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.01MB   fields=0.05MB   count=92
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.13MB   fields=0.00MB   count=559
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 47221.57MB
    struct metadata = 2245.58MB
    struct fields = 2886.12MB
    memo metadata = 9627.42MB
    memo fields = 32462.45MB
```

</details>

So... maybe a ~tiny reduction in memory usage, but not really one that's significant. And codspeed shows no perf improvement either.

Given that, I think the only remaining motivation for landing this would be if we found it a cleaner design conceptually. I do find it a cleaner design personally:
- I don't like that the same information is captured in both the `tuple_inner` and the `types` field for tuple specializations on `main`; it feels like there should be a single source of truth there
- I like that the new representation forces us to explicitly think about how these methods on `GenericContext` and `Specialization` should work for tuple contexts and tuple specializations.

The disadvantage is obviously that it's more code to maintain overall!

I could go either way; no strong opinion.

---

_Marked ready for review by @AlexWaygood on 2025-08-14 21:18_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-14 21:18_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-14 21:18_

---

_Label `internal` added by @AlexWaygood on 2025-08-14 21:18_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-14 21:22_

---

_Comment by @carljm on 2025-08-15 00:12_

I don't have strong feelings either way here either. I think one thing we should consider is how/whether this may all need to change when we add support for `TypeVarTuple`. 

---

_Comment by @carljm on 2025-08-15 00:21_

With `TypeVarTuple`, any generic type can be somewhat tuple-like (in that it can be generic over an arbitrary/unknown number of type parameters). But I guess what remains particularly weird about tuples is that their typeshed definition is in terms of a single type variable, so we have to do this mapping from the variadic generic to a single union typevar whenever we're falling back to typeshed. So maybe we'd still want this tuple/non-tuple distinction?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:168 on 2025-08-15 00:27_

This seems like dead code (assuming no custom typeshed), because this method is only used for PEP 695 type params, and the typeshed definition of `tuple` doesn't use PEP 695. Replacing this with `unreachable!(...)` doesn't fail any tests. Not sure how much we should account for custom typeshed with totally different `tuple` definition; I feel like we're already doing a lot of special-casing based on assumptions about the typeshed `tuple` (like, for instance, that it is generic over a single typevar, not a `TypeVarTuple`).

---

_@carljm approved on 2025-08-15 00:36_

I think this looks reasonable and am fine with it landing. I think in general I would encourage not spending too much time on refactors of this code (even if performance motivated) that don't add new functionality, when we have significant new functionality yet to be added. Refactors are more useful when driven by the needs of new functionality.

---

_@AlexWaygood reviewed on 2025-08-15 06:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:168 on 2025-08-15 06:45_

Yeah, Doug and I thought it was good to future-proof the code here nonetheless, since one day typeshed will likely switch to the new syntax everywhere. Can add a comment.

---

_Comment by @AlexWaygood on 2025-08-15 09:52_

I think @dcreager should probably have the final word on whether this change improves or hinders maintainability, since he's been the main architect of our generics machinery to date and will likely continue to lead this work for the foreseeable future!

---

_Assigned to @dcreager by @AlexWaygood on 2025-08-15 09:52_

---

_Comment by @dcreager on 2025-08-15 13:47_

> I think @dcreager should probably have the final word on whether this change improves or hinders maintainability, since he's been the main architect of our generics machinery to date and will likely continue to lead this work for the foreseeable future!

tbh I would say "not yet" — we know that this is doable, and can resurrect this branch in the future if we need to, but in the meantime it's not really hurting anything as it stands on `main`

---

_Comment by @AlexWaygood on 2025-08-15 13:53_

Thanks both! I'll make a standalone PR to add the test in https://github.com/astral-sh/ruff/pull/19920/commits/7c34ed337473a761ac09e02bd262b9170cf8a114, since it seems we're missing some coverage there

---

_Closed by @AlexWaygood on 2025-08-15 13:53_

---
