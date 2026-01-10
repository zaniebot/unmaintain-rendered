```yaml
number: 21749
title: "[ty] Enable LRU collection for parsed module"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/lru-parsed-semantic
created_at: 2025-12-02T07:45:14Z
updated_at: 2025-12-03T11:16:21Z
url: https://github.com/astral-sh/ruff/pull/21749
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Enable LRU collection for parsed module

---

_Pull request opened by @MichaReiser on 2025-12-02 07:45_

## Summary

Enable LRU collection for the parsed module ~~and semantic index~~ so that ty only caches the results of a limited set of files, preventing ever-growing memory usage. 

Aggressively clear ASTs of third-party files during auto-import scanning.

The main downside of LRU collection is that we may end up doing more recomputations if we're too aggressive about removing ASTs and semantic indices. Ultimately, I think we have to experiment with and fine-tune this based on user feedback to find the right balance between compute/memory usage.

The other downside is that tracking the most recent values does add some overhead which shows up as a ~2% perf regression on the walltime benchmarks. I don't think there's much we can do about it other than, in the future, extend Salsa to make LRU a runtime feature that's only enabled in the LSP (although we have ideas to enable within-revision LRU which would also be useful for the CLI)

I also replaced the global tracker used in `heap_size` to ensure subsequent calls to `salsa_memory_dump` return accurate results (e.g. source text suddenly reported that it has a size of 0 when the memory report was created more than once)

Fixes https://github.com/astral-sh/ty/issues/1707

## Test Plan

**Main, after start**

```
=======SALSA QUERIES=======
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=0.01MB   fields=23.62MB  count=91
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=0.86MB   fields=16.92MB  count=88
`source_text -> ruff_db::source::SourceText`
    metadata=0.01MB   fields=1.74MB   count=91
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.44MB   fields=0.60MB   count=1990
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=0.13MB   fields=0.37MB   count=804
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=0.24MB   fields=0.34MB   count=221
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.21MB   fields=0.22MB   count=514
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=0.34MB   fields=0.21MB   count=689
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=0.22MB   fields=0.16MB   count=926
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.00MB   fields=0.10MB   count=11
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.41MB   fields=0.09MB   count=1969
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.23MB   fields=0.07MB   count=2164
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.05MB   fields=0.07MB   count=210
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.25MB   fields=0.06MB   count=1300
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=0.14MB   fields=0.06MB   count=1230
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.08MB   fields=0.05MB   count=1000
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.00MB   fields=0.03MB   count=1
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.02MB   fields=0.03MB   count=94
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.01MB   fields=0.02MB   count=225
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=0.32MB   fields=0.02MB   count=515
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.02MB   count=23
`symbols_for_file -> ty_ide::symbols::FlatSymbols`
    metadata=0.00MB   fields=0.02MB   count=1
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.02MB   fields=0.01MB   count=126
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=0.03MB   fields=0.01MB   count=443
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.01MB   fields=0.01MB   count=87
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=0.03MB   fields=0.01MB   count=102
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.01MB   fields=0.01MB   count=169
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=0.03MB   fields=0.01MB   count=127
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=35
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=21
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=131
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=0.05MB   fields=0.00MB   count=349
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.01MB   fields=0.00MB   count=111
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=0.00MB   fields=0.00MB   count=5
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.01MB   fields=0.00MB   count=264
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=0.02MB   fields=0.00MB   count=43
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=0.01MB   fields=0.00MB   count=215
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=0.18MB   fields=0.00MB   count=1706
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.01MB   fields=0.00MB   count=169
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=0.03MB   fields=0.00MB   count=145
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.03MB   fields=0.00MB   count=92
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.01MB   fields=0.00MB   count=137
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=0.03MB   fields=0.00MB   count=94
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=0.00MB   fields=0.00MB   count=77
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=0.02MB   fields=0.00MB   count=45
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.00MB   fields=0.00MB   count=1
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=32
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=10
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=9
`Type < 'db >::expand_eagerly__ -> ty_python_semantic::types::Type<'_>`
    metadata=0.00MB   fields=0.00MB   count=8
`PEP695TypeAliasType < 'db >::raw_value_type_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=0.02MB   fields=0.00MB   count=205
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.00MB   fields=0.00MB   count=6
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.02MB   fields=0.00MB   count=164
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.01MB   fields=0.00MB   count=84
`is_equivalent_to_object_inner -> bool`
    metadata=0.02MB   fields=0.00MB   count=74
`PEP695TypeAliasType < 'db >::generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.00MB   fields=0.00MB   count=7
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.00MB   fields=0.00MB   count=2
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=15
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=6
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 60.77MB
    struct metadata = 3.58MB
    struct fields = 7.66MB
    memo metadata = 4.61MB
    memo fields = 44.92MB

```


**Main, after auto completion**

```
=======SALSA QUERIES=======
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=3.55MB   fields=5325.71MB count=46737
`source_text -> ruff_db::source::SourceText`
    metadata=2.98MB   fields=382.08MB count=46737
`symbols_for_file_global_only -> ty_ide::symbols::FlatSymbols`
    metadata=2.43MB   fields=31.33MB  count=46726
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=1.00MB   fields=6.97MB   count=102
`all_submodule_names_for_package -> core::option::Option<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>>`
    metadata=3.28MB   fields=1.12MB   count=46744
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.49MB   fields=0.71MB   count=2258
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=0.15MB   fields=0.45MB   count=957
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=0.24MB   fields=0.34MB   count=221
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.22MB   fields=0.23MB   count=566
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=0.36MB   fields=0.21MB   count=716
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=0.27MB   fields=0.20MB   count=1121
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.53MB   fields=0.12MB   count=2558
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.33MB   fields=0.09MB   count=2957
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=0.18MB   fields=0.08MB   count=1641
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.32MB   fields=0.08MB   count=1585
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.05MB   fields=0.07MB   count=230
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.11MB   fields=0.06MB   count=1318
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.00MB   fields=0.03MB   count=1
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.02MB   fields=0.03MB   count=106
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.01MB   fields=0.03MB   count=265
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=0.41MB   fields=0.03MB   count=663
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.00MB   fields=0.02MB   count=11
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.02MB   count=24
`symbols_for_file -> ty_ide::symbols::FlatSymbols`
    metadata=0.00MB   fields=0.02MB   count=1
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=0.04MB   fields=0.02MB   count=518
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.02MB   fields=0.01MB   count=130
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.01MB   fields=0.01MB   count=87
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=0.04MB   fields=0.01MB   count=105
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.02MB   fields=0.01MB   count=192
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=0.03MB   fields=0.01MB   count=129
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=0.06MB   fields=0.00MB   count=416
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=23
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=131
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.01MB   fields=0.00MB   count=114
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=0.00MB   fields=0.00MB   count=6
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=0.02MB   fields=0.00MB   count=49
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.01MB   fields=0.00MB   count=285
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=0.20MB   fields=0.00MB   count=1881
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=0.01MB   fields=0.00MB   count=235
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.02MB   fields=0.00MB   count=187
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=0.03MB   fields=0.00MB   count=177
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.03MB   fields=0.00MB   count=111
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.01MB   fields=0.00MB   count=145
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=0.04MB   fields=0.00MB   count=115
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=0.00MB   fields=0.00MB   count=91
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=35
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=0.02MB   fields=0.00MB   count=53
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.00MB   fields=0.00MB   count=1
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=40
`Type < 'db >::expand_eagerly__ -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=11
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=10
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=9
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.00MB   fields=0.00MB   count=9
`PEP695TypeAliasType < 'db >::raw_value_type_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=0.02MB   fields=0.00MB   count=219
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.02MB   fields=0.00MB   count=181
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=3
`is_equivalent_to_object_inner -> bool`
    metadata=0.03MB   fields=0.00MB   count=87
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.01MB   fields=0.00MB   count=84
`list_modules_in -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.07MB   fields=0.00MB   count=3
`PEP695TypeAliasType < 'db >::generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.00MB   fields=0.00MB   count=7
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=25
`list_modules -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.00MB   fields=0.00MB   count=2
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=13
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 5797.30MB
    struct metadata = 9.40MB
    struct fields = 20.00MB
    memo metadata = 17.75MB
    memo fields = 5750.15MB

```

**This PR, after startup**

```
=======SALSA QUERIES=======
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=0.01MB   fields=23.62MB  count=91
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=0.86MB   fields=16.83MB  count=88
`source_text -> ruff_db::source::SourceText`
    metadata=0.01MB   fields=1.74MB   count=91
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.44MB   fields=0.60MB   count=1990
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=0.13MB   fields=0.40MB   count=861
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=0.24MB   fields=0.34MB   count=221
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.21MB   fields=0.22MB   count=521
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=0.35MB   fields=0.21MB   count=714
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=0.24MB   fields=0.18MB   count=1027
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.00MB   fields=0.11MB   count=12
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.47MB   fields=0.11MB   count=2241
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.30MB   fields=0.09MB   count=2707
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.29MB   fields=0.07MB   count=1460
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.05MB   fields=0.07MB   count=211
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=0.14MB   fields=0.06MB   count=1230
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.08MB   fields=0.05MB   count=1007
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.00MB   fields=0.03MB   count=1
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.02MB   fields=0.03MB   count=94
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.01MB   fields=0.03MB   count=252
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=0.36MB   fields=0.02MB   count=586
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.02MB   count=23
`symbols_for_file -> ty_ide::symbols::FlatSymbols`
    metadata=0.00MB   fields=0.02MB   count=1
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.02MB   fields=0.01MB   count=126
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=0.03MB   fields=0.01MB   count=443
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.01MB   fields=0.01MB   count=87
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=0.03MB   fields=0.01MB   count=102
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.01MB   fields=0.01MB   count=169
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=0.03MB   fields=0.01MB   count=127
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=35
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=22
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=0.05MB   fields=0.00MB   count=373
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=131
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.01MB   fields=0.00MB   count=111
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=0.00MB   fields=0.00MB   count=6
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=0.02MB   fields=0.00MB   count=47
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.01MB   fields=0.00MB   count=264
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=0.19MB   fields=0.00MB   count=1844
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=0.01MB   fields=0.00MB   count=215
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=0.04MB   fields=0.00MB   count=171
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.01MB   fields=0.00MB   count=169
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.01MB   fields=0.00MB   count=137
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.02MB   fields=0.00MB   count=91
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=0.03MB   fields=0.00MB   count=95
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=0.00MB   fields=0.00MB   count=77
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=0.02MB   fields=0.00MB   count=48
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.00MB   fields=0.00MB   count=1
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=32
`Type < 'db >::expand_eagerly__ -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=10
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=10
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=9
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.00MB   fields=0.00MB   count=9
`PEP695TypeAliasType < 'db >::raw_value_type_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=0.02MB   fields=0.00MB   count=205
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.02MB   fields=0.00MB   count=164
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.01MB   fields=0.00MB   count=84
`is_equivalent_to_object_inner -> bool`
    metadata=0.02MB   fields=0.00MB   count=74
`PEP695TypeAliasType < 'db >::generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.00MB   fields=0.00MB   count=7
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=22
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.00MB   fields=0.00MB   count=2
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=9
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 60.94MB
    struct metadata = 3.70MB
    struct fields = 7.40MB
    memo metadata = 4.90MB
    memo fields = 44.94MB

```

**This PR, after auto complete**

```
=======SALSA QUERIES=======
`source_text -> ruff_db::source::SourceText`
    metadata=2.98MB   fields=381.67MB count=46737
`symbols_for_file_global_only -> ty_ide::symbols::FlatSymbols`
    metadata=2.99MB   fields=31.33MB  count=46726
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=3.55MB   fields=22.92MB  count=46737
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=1.00MB   fields=4.24MB   count=102
`all_submodule_names_for_package -> core::option::Option<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>>`
    metadata=3.28MB   fields=1.12MB   count=46744
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.49MB   fields=0.71MB   count=2258
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=0.15MB   fields=0.44MB   count=939
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=0.24MB   fields=0.34MB   count=221
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=0.22MB   fields=0.23MB   count=566
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=0.36MB   fields=0.21MB   count=715
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=0.26MB   fields=0.20MB   count=1113
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.51MB   fields=0.12MB   count=2500
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.30MB   fields=0.09MB   count=2756
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=0.18MB   fields=0.08MB   count=1641
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.32MB   fields=0.08MB   count=1591
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.05MB   fields=0.07MB   count=232
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=0.11MB   fields=0.06MB   count=1318
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.00MB   fields=0.03MB   count=1
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.02MB   fields=0.03MB   count=105
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.01MB   fields=0.03MB   count=268
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=0.40MB   fields=0.03MB   count=646
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.00MB   fields=0.02MB   count=12
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.02MB   count=24
`symbols_for_file -> ty_ide::symbols::FlatSymbols`
    metadata=0.00MB   fields=0.02MB   count=1
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=0.04MB   fields=0.01MB   count=509
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.02MB   fields=0.01MB   count=130
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.01MB   fields=0.01MB   count=87
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=0.04MB   fields=0.01MB   count=105
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.02MB   fields=0.01MB   count=192
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=0.03MB   fields=0.01MB   count=129
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=0.06MB   fields=0.00MB   count=408
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=23
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=131
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.01MB   fields=0.00MB   count=114
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=0.00MB   fields=0.00MB   count=6
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=0.02MB   fields=0.00MB   count=49
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.01MB   fields=0.00MB   count=285
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=0.01MB   fields=0.00MB   count=235
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=0.20MB   fields=0.00MB   count=1868
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.02MB   fields=0.00MB   count=187
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=0.03MB   fields=0.00MB   count=175
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.03MB   fields=0.00MB   count=111
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.01MB   fields=0.00MB   count=145
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=0.04MB   fields=0.00MB   count=114
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=0.00MB   fields=0.00MB   count=91
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=35
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=0.02MB   fields=0.00MB   count=51
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.00MB   fields=0.00MB   count=1
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=40
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=10
`Type < 'db >::expand_eagerly__ -> ty_python_semantic::types::Type<'_>`
    metadata=0.01MB   fields=0.00MB   count=10
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=9
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.00MB   fields=0.00MB   count=9
`PEP695TypeAliasType < 'db >::raw_value_type_ -> ty_python_semantic::types::Type<'_>`
    metadata=0.00MB   fields=0.00MB   count=7
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=0.02MB   fields=0.00MB   count=219
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.02MB   fields=0.00MB   count=181
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=3
`is_equivalent_to_object_inner -> bool`
    metadata=0.03MB   fields=0.00MB   count=87
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.01MB   fields=0.00MB   count=84
`list_modules_in -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.07MB   fields=0.00MB   count=3
`PEP695TypeAliasType < 'db >::generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.00MB   fields=0.00MB   count=7
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=25
`list_modules -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.00MB   fields=0.00MB   count=2
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=13
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 491.24MB
    struct metadata = 9.37MB
    struct fields = 19.41MB
    memo metadata = 18.26MB
    memo fields = 444.19MB

```

and parsed modules drops down to less than 200 after making a few more changes (LRU is deferred in that it requires a few changes before it's safe for salsa to delete it)

```
=======SALSA QUERIES=======
`symbols_for_file_global_only -> ty_ide::symbols::FlatSymbols`
    metadata=2.99MB   fields=31.33MB  count=46726
`source_text -> ruff_db::source::SourceText`
    metadata=2.98MB   fields=19.40MB  count=46737
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=3.55MB   fields=12.72MB  count=46737
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=1.00MB   fields=4.24MB   count=102
`all_submodule_names_for_package -> core::option::Option<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>>`
    metadata=3.28MB   fields=1.12MB   count=46744
`infer_definition_types -> ty_python_semantic::types::infer::Definition
```


@ibraheemdev I'm not sure if the counts are correctly updated after LRU. Do we filter out memos without a value, or is it intentional that we don't, so that we still account for the metadata overhead?


**Note**: ty will still use more memory than reported by salsa because it currently doesn't release no-longer-used pages. But homeassistant now takes ~1.2 GB after LRU collection compared to 5.6 GB before, which is a substantial improvement

---

_Label `ty` added by @MichaReiser on 2025-12-02 07:45_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 07:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 07:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 493 diagnostics
+ Found 494 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5505 diagnostics
+ Found 5504 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 08:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser reviewed on 2025-12-02 10:12_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:28 on 2025-12-02 10:12_

The number here is rather random. We never evict a module within the same revision. The assumption here is that subsequent changes should rarely invalidate more than 200 files at once. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:90 on 2025-12-02 10:13_

I had to remove this assertion because the AST can now be collected so that it has a different module ref when dereferencing the node

---

_@MichaReiser reviewed on 2025-12-02 10:13_

---

_Marked ready for review by @MichaReiser on 2025-12-02 10:32_

---

_Review requested from @carljm by @MichaReiser on 2025-12-02 10:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-02 10:32_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-02 10:32_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-02 10:32_

---

_Comment by @MichaReiser on 2025-12-02 11:47_

Huh, it looks like Salsa also ends up reclaiming some memoized `source_text` calls. While this might be what we want in this particular instance, I don't know how salsa would know that `parsed_module` was the only query depending on `source_text` (salsa doesn't maintain a inreverse dependency tree)

---

_Comment by @MichaReiser on 2025-12-02 12:02_

> Huh, it looks like Salsa also ends up reclaiming some memoized `source_text` calls. While this might be what we want in this particular instance, I don't know how salsa would know that `parsed_module` was the only query depending on `source_text` (salsa doesn't maintain an inverse dependency tree)

I'm not sure why this would happen. All that Salsa does during LRU eviction is to set `memo.value = None`. It doesn't delete the `Memo` itself. Therefore, it shouldn't change any other queries

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-12-02 12:27_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-02 13:03_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-02 13:03_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-12-02 13:03_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-02 13:03_

---

_Review comment by @Gankra on `crates/ty_project/src/db.rs`:134 on 2025-12-02 14:35_

Ominous truncated comment

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/ast_node_ref.rs`:90 on 2025-12-02 14:37_

Dang, this kind of assertion is really handy, can we maybe make the lru stuff release-only...? I guess probably inadvisable to have that big of a behaviour change between debug and release.

---

_Review comment by @Gankra on `crates/ruff_memory_usage/src/lib.rs`:8 on 2025-12-02 14:39_

Would appreciate a comment explaining what on earth is going on here. Also, interesting that this no longer needs to be multi-threaded..?

---

_@Gankra approved on 2025-12-02 14:39_

Incredible win

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:390 on 2025-12-02 14:52_

Does this mean that each time completions are requested, every file will need to be re-parsed?

I checked out this PR and tried it out. I used a single Python source file containing just `array`. I opened it in VS Code and requested completions at the end of `array`. On `main`, I get:

```
2025-12-02 09:48:31.234246045 DEBUG request{id=13 method="textDocument/completion"}: Completions request returned 326 suggestions in 149.088978ms
2025-12-02 09:48:34.461071428 DEBUG request{id=15 method="textDocument/completion"}: Completions request returned 326 suggestions in 17.225306ms
2025-12-02 09:48:36.677139939 DEBUG request{id=17 method="textDocument/completion"}: Completions request returned 326 suggestions in 19.559503ms
2025-12-02 09:48:40.656242428 DEBUG request{id=19 method="textDocument/completion"}: Completions request returned 326 suggestions in 18.719669ms
```

And with your branch I get:

```
2025-12-02 09:50:26.152804396 DEBUG request{id=14 method="textDocument/completion"}: Completions request returned 326 suggestions in 166.780705ms
2025-12-02 09:50:29.762248593 DEBUG request{id=16 method="textDocument/completion"}: Completions request returned 326 suggestions in 19.985813ms
2025-12-02 09:50:33.345613225 DEBUG request{id=18 method="textDocument/completion"}: Completions request returned 326 suggestions in 19.429199ms
2025-12-02 09:50:37.048968976 DEBUG request{id=20 method="textDocument/completion"}: Completions request returned 326 suggestions in 18.763721ms
```

So there still seems to be significant caching happening?

---

_@BurntSushi approved on 2025-12-02 14:52_

Wow

---

_@MichaReiser reviewed on 2025-12-02 15:08_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:134 on 2025-12-02 15:08_

Lol yes. It's because I decided it's a bad idea and we should fix `heap_size` instead 

---

_@MichaReiser reviewed on 2025-12-02 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_memory_usage/src/lib.rs`:8 on 2025-12-02 15:08_

This was never multi-threaded. It's just that it was a `static` before, so we had to use a `Mutex`

---

_@MichaReiser reviewed on 2025-12-02 15:09_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:390 on 2025-12-02 15:09_

> Does this mean that each time completions are requested, every file will need to be re-parsed?

We still cache `symbols_for_file_global_only`. But yes, we'll have to reparse the AST if a third party file changes, but that should be very unlikely and not something we can't really avoid because we obviously have to recompute `symbols_for_file_global_only` in that case.

The main drawback of this is if we had any other query that iterates over all third-party modules, parses them, and does stuff. But I don't think there's any such query?

---

_@BurntSushi reviewed on 2025-12-02 15:14_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:390 on 2025-12-02 15:14_

Ah I see now. Yeah this is great. And no, I'm not aware of any other queries that do this. I agree we'd have to be careful introducing one. Maybe add a comment here warning of that eventuality?

---

_Renamed from "[ty] Enable LRU collection for parsed module and semantix index" to "[ty] Enable LRU collection for parsed module and semantic index" by @AlexWaygood on 2025-12-02 15:16_

---

_Comment by @ibraheemdev on 2025-12-02 16:12_

> I'm not sure if the counts are correctly updated after LRU. Do we filter out memos without a value, or is it intentional that we don't, so that we still account for the metadata overhead?

We should still count the metadata. Maybe we want separate counts for memos with/without values?

---

_@AlexWaygood reviewed on 2025-12-02 16:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/ast_node_ref.rs`:90 on 2025-12-02 16:14_

Yeah, this specific assertion has caught a bunch of bugs in my PRs prior to me filing them :/

---

_@ibraheemdev approved on 2025-12-02 16:54_

We talked a bit about better LRU heuristics, and potentially changing completions to attempt to load the semantic index if it has already been created instead of reparsing the file, but this seems great for now!

---

_Comment by @MichaReiser on 2025-12-02 17:51_

For tomorrow, I should test that this doesn't regress multithreading performance in a meaningful way, because the LRU uses a single `Mutex` per query :(

---

_@MichaReiser reviewed on 2025-12-02 20:42_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:90 on 2025-12-02 20:42_

I think we can bring it back for the first salsa revision. It allows us to catch at least some violations in mdtests. It's always disabled in mypy primer because we use a release build

---

_Review comment by @carljm on `crates/ruff_db/src/parsed.rs`:28 on 2025-12-03 01:18_

I think a comment in the code, saying exactly what you said in this github comment, would be useful to future-us.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index.rs`:54 on 2025-12-03 01:24_

Similarly here, let's comment on what assumptions the 200 is based on.

---

_@carljm approved on 2025-12-03 01:24_

The memory impact here is amazing.

---

_Comment by @MichaReiser on 2025-12-03 08:58_

Hmm, yeah. We can't land this. This is a huge perf regression. Checking a large project goes from 30s to 40s, and it's very clear that this comes from the locks because the involuntary context switches increase from 2'113'066  to 8'769'635, that's 4 times as many threads that block on each other.

Fixing this requires changes in Salsa. Ideally, Salsa would shard locks similarly to how we handle interned structs. This should remove most congestion because checking a file is, most of the time, local to a single thread. But I don't think this is an entirely trivial change and @ibraheemdev is probably the best person to work on it.

For now, I'll land the change that enables LRU for `parsed_module` only. This doesn't seem to be as big of a regression and is the main driver in https://github.com/astral-sh/ty/issues/1707

I still think it's important for us to enable LRU for semantic indexing because it's wasteful to keep all of them in memory when using workspace diagnostics.

---

_Renamed from "[ty] Enable LRU collection for parsed module and semantic index" to "[ty] Enable LRU collection for parsed module" by @MichaReiser on 2025-12-03 09:03_

---

_Merged by @MichaReiser on 2025-12-03 11:16_

---

_Closed by @MichaReiser on 2025-12-03 11:16_

---

_Branch deleted on 2025-12-03 11:16_

---
