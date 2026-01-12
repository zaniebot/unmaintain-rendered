```yaml
number: 1707
title: High memory usage
type: issue
state: closed
author: klonuo
labels:
  - needs-info
  - memory
assignees: []
created_at: 2025-12-01T13:59:54Z
updated_at: 2025-12-03T11:16:20Z
url: https://github.com/astral-sh/ty/issues/1707
synced_at: 2026-01-12T15:54:25Z
```

# High memory usage

---

_@klonuo_

Hi,

I have small project with just one script and set it to use my system python:

```toml
[project]
name = "air-pollution"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[tool.ty.environment]
python = "C:/Python312"
```

These are all imports in the script:
```python
from datetime import datetime, date
import json

from dateutil.relativedelta import relativedelta
import numpy as np
import pandas as pd
import requests
from retry import retry
from sqlalchemy import create_engine, text
```

When I noticed that `ty` uses too much memory (7GB):

<img width="482" height="624" alt="Image" src="https://github.com/user-attachments/assets/4359a65b-bf78-4125-abee-48d5204a84f4" />

This should probably be because I didn't created venv, but even with venv standard backend apps commits to 2-3GB.

Just wanted to report this.

---

_Label `memory` added by @AlexWaygood on 2025-12-01 14:00_

---

_Comment by @MichaReiser on 2025-12-01 15:12_

That's certainly a lot. Are you running ty as CLI tool or in VS code? 

Any change you have the experimental auto import feature enable? 

Ideally, we'd do better, but these are certainly large libraries that ty has to analyze. Can you share the full list of dependencies that you've installed?

---

_Label `needs-info` added by @MichaReiser on 2025-12-01 15:12_

---

_Comment by @klonuo on 2025-12-01 15:25_

I configured LSP server to use `ty server` in both Zed and Sublime Text, with same outcome
`autoImport` is explicitly set to false

I suspect it accumulates memory over time, as I had the project open for several hours

Not sure about which dependencies you refer to, I shared all imports I use in the project

---

_Comment by @daltunay on 2025-12-01 17:25_

I have the same issue there, using ty as a VS Code extension (language server). I am working on a very big legacy codebase with 100+ 3rd party libraries in pyproject.toml, as well as a lot of bloated modules. I know this is not an optimal setup, but is there a possibility to specify a whitelist of paths/libs to analyze maybe?

---

_Comment by @MichaReiser on 2025-12-01 17:44_

> I have the same issue there, using ty as a VS Code extension (language server). I am working on a very big legacy codebase with 100+ 3rd party libraries in pyproject.toml, as well as a lot of bloated modules. I know this is not an optimal setup, but is there a possibility to specify a whitelist of paths/libs to analyze maybe?

Is the memory usage high from the beginning or is it just growing over time?



---

_Comment by @MichaReiser on 2025-12-01 18:04_

If you're using VS Code, could you share the memory report here (Cmd+P, ty: Open debug information, at the very end)

It looks like this:

```
Memory report:
=======SALSA STRUCTS=======
`Definition`                                       metadata=0.24MB   fields=0.58MB   count=6059
`Expression`                                       metadata=0.02MB   fields=0.03MB   count=512
`ScopeId`                                          metadata=0.05MB   fields=0.02MB   count=1903
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`File`                                             metadata=0.01MB   fields=0.01MB   count=86
...
```

---

_Comment by @klonuo on 2025-12-01 21:40_

It's reproducible, I opened the same project ~2 hours ago, and now checking it grow to 6GB

I can track `ty` process memory usage on some interval, but that wont help I guess...

---

_Comment by @daltunay on 2025-12-01 23:44_

Sure, here it is @MichaReiser:

<details><summary>Memory report</summary>
<p>

```txt
=======SALSA STRUCTS=======
`Definition`                                       metadata=19.56MB  fields=27.38MB  count=488976
`File`                                             metadata=3.65MB   fields=10.86MB  count=56992
`FunctionType`                                     metadata=1.76MB   fields=7.59MB   count=31498
`IntersectionType`                                 metadata=3.08MB   fields=7.23MB   count=54924
`Expression`                                       metadata=12.31MB  fields=6.15MB   count=256378
`Type < 'db >::is_redundant_with_::interned_arguments` metadata=7.80MB   fields=4.46MB   count=139287
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=4.98MB   fields=4.27MB   count=88906
`CallableType`                                     metadata=0.84MB   fields=3.84MB   count=15086
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=4.35MB   fields=3.73MB   count=77635
`UnionType`                                        metadata=2.68MB   fields=3.12MB   count=47859
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=3.58MB   fields=2.56MB   count=63912
`FileModule`                                       metadata=1.84MB   fields=2.51MB   count=32805
`StringLiteralType`                                metadata=2.46MB   fields=2.43MB   count=43989
`Specialization`                                   metadata=1.63MB   fields=1.76MB   count=29195
`OverloadLiteral`                                  metadata=1.23MB   fields=1.52MB   count=21906
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=1.72MB   fields=1.47MB   count=30673
`Type < 'db >::apply_specialization_::interned_arguments` metadata=3.19MB   fields=1.37MB   count=57014
`TupleType`                                        metadata=0.76MB   fields=1.14MB   count=13631
`ScopeId`                                          metadata=2.48MB   fields=1.06MB   count=88702
`place_by_id::interned_arguments`                  metadata=2.63MB   fields=1.01MB   count=50648
`ClassLiteral`                                     metadata=0.47MB   fields=0.59MB   count=8307
`BoundMethodType`                                  metadata=1.05MB   fields=0.45MB   count=18712
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=1.50MB   fields=0.43MB   count=26779
`GenericAlias`                                     metadata=1.25MB   fields=0.36MB   count=22397
`ModuleLiteralType`                                metadata=0.70MB   fields=0.27MB   count=13523
`Unpack`                                           metadata=0.30MB   fields=0.24MB   count=7543
`ProtocolInterface`                                metadata=0.11MB   fields=0.22MB   count=2004
`BoundTypeVarInstance`                             metadata=0.78MB   fields=0.22MB   count=13852
`code_generator_of_class::interned_arguments`      metadata=0.69MB   fields=0.20MB   count=12325
`StarImportPlaceholderPredicate`                   metadata=0.27MB   fields=0.20MB   count=9808
`FunctionLiteral`                                  metadata=1.24MB   fields=0.18MB   count=22185
`TypeVarInstance`                                  metadata=0.19MB   fields=0.16MB   count=3411
`GenericContext`                                   metadata=0.09MB   fields=0.16MB   count=1674
`ModuleNameIngredient`                             metadata=0.17MB   fields=0.16MB   count=2974
`TypeVarIdentity`                                  metadata=0.17MB   fields=0.12MB   count=3105
`EnumLiteralType`                                  metadata=0.12MB   fields=0.07MB   count=2105
`ExpressionWithContext`                            metadata=0.11MB   fields=0.05MB   count=2036
`PatternPredicate`                                 metadata=0.02MB   fields=0.05MB   count=568
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=0.11MB   fields=0.03MB   count=1931
`PropertyInstanceType`                             metadata=0.05MB   fields=0.03MB   count=887
`BoundSuperType`                                   metadata=0.04MB   fields=0.02MB   count=697
`Program`                                          metadata=0.00MB   fields=0.01MB   count=1
`is_equivalent_to_object_inner::interned_arguments` metadata=0.05MB   fields=0.01MB   count=904
`Project`                                          metadata=0.00MB   fields=0.00MB   count=1
`BytesLiteralType`                                 metadata=0.00MB   fields=0.00MB   count=50
`FieldInstance`                                    metadata=0.00MB   fields=0.00MB   count=46
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=36
`DataclassParams`                                  metadata=0.00MB   fields=0.00MB   count=17
`TypeIsType`                                       metadata=0.00MB   fields=0.00MB   count=21
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=2
`DataclassTransformerParams`                       metadata=0.00MB   fields=0.00MB   count=5
`ManualPEP695TypeAliasType`                        metadata=0.00MB   fields=0.00MB   count=5
`DeprecatedInstance`                               metadata=0.00MB   fields=0.00MB   count=13
`SearchPathIngredient`                             metadata=0.00MB   fields=0.00MB   count=4
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=2
`list_modules::interned_arguments`                 metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=2.62MB   fields=4274.44MB count=34428
`source_text -> ruff_db::source::SourceText`
    metadata=2.19MB   fields=276.93MB count=34428
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=20.87MB  fields=71.94MB  count=5230
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=65.50MB  fields=52.68MB  count=188379
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=18.92MB  fields=51.62MB  count=31850
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=63.61MB  fields=30.37MB  count=136473
`symbols_for_file_global_only -> ty_ide::symbols::FlatSymbols`
    metadata=1.75MB   fields=19.91MB  count=32202
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=2.79MB   fields=6.30MB   count=23525
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=6.01MB   fields=3.67MB   count=26779
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=19.77MB  fields=2.84MB   count=88903
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=21.71MB  fields=2.48MB   count=77631
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member`
    metadata=7.28MB   fields=2.05MB   count=63911
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=15.46MB  fields=1.98MB   count=24653
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature`
    metadata=1.03MB   fields=1.74MB   count=8419
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=3.60MB   fields=1.62MB   count=50647
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=1.54MB   fields=1.34MB   count=3304
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=2.63MB   fields=0.95MB   count=10924
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=5.23MB   fields=0.91MB   count=57014
`all_submodule_names_for_package -> core::option::Option<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module>>`
    metadata=2.26MB   fields=0.78MB   count=32380
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature`
    metadata=0.50MB   fields=0.78MB   count=3337
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=16.45MB  fields=0.74MB   count=30664
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.25MB   fields=0.58MB   count=3907
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=0.89MB   fields=0.57MB   count=3179
`infer_expression_type_impl -> ty_python_semantic::types::Type`
    metadata=2.56MB   fields=0.54MB   count=33683
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=1.40MB   fields=0.53MB   count=21630
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=1.89MB   fields=0.40MB   count=4504
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=1.57MB   fields=0.37MB   count=7615
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.60MB   fields=0.24MB   count=8057
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.02MB   fields=0.24MB   count=283
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind>`
    metadata=1.58MB   fields=0.15MB   count=12325
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.85MB   fields=0.14MB   count=3915
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.82MB   fields=0.14MB   count=15813
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=9.22MB   fields=0.14MB   count=139287
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap>`
    metadata=0.46MB   fields=0.14MB   count=8829
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity, rustc_hash::FxBuildHasher>`
    metadata=0.05MB   fields=0.09MB   count=911
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.27MB   fields=0.08MB   count=4077
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.02MB   fields=0.07MB   count=189
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.06MB   fields=0.07MB   count=1243
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=0.58MB   fields=0.06MB   count=8056
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=0.57MB   fields=0.06MB   count=7841
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=0.56MB   fields=0.06MB   count=1931
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType`
    metadata=0.65MB   fields=0.05MB   count=5924
`symbols_for_file -> ty_ide::symbols::FlatSymbols`
    metadata=0.00MB   fields=0.04MB   count=10
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.08MB   fields=0.04MB   count=398
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=1.48MB   fields=0.04MB   count=2974
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.29MB   fields=0.03MB   count=394
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.28MB   fields=0.03MB   count=3910
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.18MB   fields=0.03MB   count=3554
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=0.20MB   fields=0.03MB   count=3797
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=2.54MB   fields=0.03MB   count=29043
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType`
    metadata=0.47MB   fields=0.02MB   count=1310
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.48MB   fields=0.01MB   count=1636
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.85MB   fields=0.01MB   count=7901
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.65MB   fields=0.01MB   count=7121
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.05MB   fields=0.01MB   count=453
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints>`
    metadata=0.01MB   fields=0.00MB   count=101
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::Type`
    metadata=0.02MB   fields=0.00MB   count=84
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type>`
    metadata=0.01MB   fields=0.00MB   count=59
`is_equivalent_to_object_inner -> bool`
    metadata=0.25MB   fields=0.00MB   count=904
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints>`
    metadata=0.00MB   fields=0.00MB   count=14
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=2
`list_modules_in -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.03MB   fields=0.00MB   count=4
`list_modules -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.00MB   fields=0.00MB   count=1
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 5317.30MB
    struct metadata = 92.03MB
    struct fields = 99.70MB
    memo metadata = 314.46MB
    memo fields = 4811.11MB
```

</p>
</details> 

Thanks!

---

_Comment by @MichaReiser on 2025-12-02 07:24_

Thank you @daltunay. That's very useful. Do you have auto imports enabled? Are there many diagnostics in your project?

I think the solution here is to do https://github.com/astral-sh/ty/issues/789

Edit: Yes, I can see that you enabled auto imports. The linked PR should help a lot by clearing out the majority of `parsed_module`:

```
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=2.62MB   fields=4274.44MB count=34428
```

---

_Closed by @MichaReiser on 2025-12-03 11:16_

---
