```yaml
number: 2360
title: Stack overflow on generic type with forward reference
type: issue
state: open
author: lsorber
labels:
  - fatal
  - Protocols
  - type aliases
assignees: []
created_at: 2026-01-06T09:54:35Z
updated_at: 2026-01-06T19:01:34Z
url: https://github.com/astral-sh/ty/issues/2360
synced_at: 2026-01-10T01:56:41Z
```

# Stack overflow on generic type with forward reference

---

_Issue opened by @lsorber on 2026-01-06 09:54_

### Summary

Ty crashes on this MRE ([ty playground](https://play.ty.dev/eb5974ff-c7ff-4f32-b690-db1a992d134e)):
```python
from collections.abc import Iterable, Mapping
from typing import Any

type AnyComponent = "Component[Any]"  # Ty does not crash without this forward reference
type Nested[T] = Iterable[Nested[T]] | Mapping[Any, Nested[T]]


class Component[T: Iterable[Nested[AnyComponent]] | Mapping[Any, Nested[AnyComponent]]]:
    pass


dict[str, list[AnyComponent]]().get("x", [])
```

Output:
```
thread '<unknown>' (17625271) has overflowed its stack
fatal runtime error: stack overflow, aborting
```

### Version

0.0.9

---

_Renamed from "Crash on generic type with forward reference" to "Stack overflow on generic type with forward reference" by @AlexWaygood on 2026-01-06 09:56_

---

_Label `fatal` added by @AlexWaygood on 2026-01-06 09:56_

---

_Label `type aliases` added by @AlexWaygood on 2026-01-06 09:56_

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-06 09:56_

---

_Assigned to @mtshiba by @mtshiba on 2026-01-06 15:04_

---

_Comment by @mtshiba on 2026-01-06 18:50_

The root cause of this appears to be recursive (synthesized) protocols rather than a recursive type alias, as this example still results in a panic.

```python
from collections.abc import Iterable, Mapping
from typing import Any

type AnyComponent = Component[Any]

class Component[T: Iterable[Iterable[AnyComponent] | Mapping[Any, Iterable[Any]]] | Mapping[Any, Iterable[AnyComponent] | Mapping[Any, Iterable[Any]]]]:
    pass


dict[str, list[AnyComponent]]().get("x", [])

```

<details>
<summary>backtrace (1 cycle)</summary>

```
ty!enum2$<ty_python_semantic::types::Type>::apply_type_mapping_impl+0x10ce
ty!ty_python_semantic::types::signatures::impl$8::apply_type_mapping_impl::closure$0+0xbe
ty!enum2$<core::option::Option<enum2$<ty_python_semantic::types::Type> > >::map<enum2$<ty_python_semantic::types::Type>,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::signatures::impl$8::apply_type_mapping_impl::closure_env$0>+0xc3
ty!ty_python_semantic::types::signatures::Parameter::apply_type_mapping_impl+0xda
ty!ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure$0+0xb9
ty!core::iter::adapters::map::map_fold::closure$0<ref$<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::Parameter,tuple$<>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0,core::iter::traits::iterator::Iterator::for_each::call::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::vec::impl$20::extend_trusted::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> > > >+0x59
ty!core::slice::iter::impl$166::fold<ty_python_semantic::types::signatures::Parameter,tuple$<>,core::iter::adapters::map::map_fold::closure_env$0<ref$<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::Parameter,tuple$<>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0,core::iter::traits::iterator::Iterator::for_each::call::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::vec::impl$20::extend_trusted::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> > > > >+0xaf
ty!core::iter::adapters::map::impl$2::fold<ty_python_semantic::types::signatures::Parameter,core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0,tuple$<>,core::iter::traits::iterator::Iterator::for_each::call::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::vec::impl$20::extend_trusted::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> > > >+0x80
ty!core::iter::traits::iterator::Iterator::for_each<core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0>,alloc::vec::impl$20::extend_trusted::closure_env$0<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> > >+0x28
ty!alloc::vec::Vec<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global>::extend_trusted<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> >+0x128
ty!alloc::vec::spec_extend::impl$1::spec_extend<ty_python_semantic::types::signatures::Parameter,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0>,alloc::alloc::Global>+0xe
ty!alloc::vec::spec_from_iter_nested::impl$1::from_iter<ty_python_semantic::types::signatures::Parameter,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> >+0xf8
ty!alloc::vec::spec_from_iter::impl$0::from_iter<ty_python_semantic::types::signatures::Parameter,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> >+0x11
ty!alloc::vec::impl$15::from_iter<ty_python_semantic::types::signatures::Parameter,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0> >+0x27
ty!core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Parameter>,ty_python_semantic::types::signatures::impl$5::apply_type_mapping_impl::closure_env$0>,alloc::vec::Vec<ty_python_semantic::types::signatures::Parameter,alloc::alloc::Global> >+0x11
ty!ty_python_semantic::types::signatures::Parameters::apply_type_mapping_impl+0x15b
ty!ty_python_semantic::types::signatures::Signature::apply_type_mapping_impl+0xe9
ty!ty_python_semantic::types::signatures::impl$0::apply_type_mapping_impl::closure$0+0xb9
ty!core::ops::function::impls::impl$4::call_once+0xa
ty!enum2$<core::option::Option<ref$<ty_python_semantic::types::signatures::Signature> > >::map+0x40
ty!core::iter::adapters::map::impl$2::next<ty_python_semantic::types::signatures::Signature,core::slice::iter::Iter<ty_python_semantic::types::signatures::Signature>,ty_python_semantic::types::signatures::impl$0::apply_type_mapping_impl::closure_env$0>+0x86
ty!smallvec::impl$31::extend<array$<ty_python_semantic::types::signatures::Signature,1>,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Signature>,ty_python_semantic::types::signatures::impl$0::apply_type_mapping_impl::closure_env$0> >+0x106
ty!smallvec::impl$30::from_iter<array$<ty_python_semantic::types::signatures::Signature,1>,core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Signature>,ty_python_semantic::types::signatures::impl$0::apply_type_mapping_impl::closure_env$0> >+0x6b
ty!core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Signature>,ty_python_semantic::types::signatures::impl$0::apply_type_mapping_impl::closure_env$0>,smallvec::SmallVec<array$<ty_python_semantic::types::signatures::Signature,1> > >+0x11
ty!ty_python_semantic::types::signatures::CallableSignature::from_overloads<core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Signature>,ty_python_semantic::types::signatures::impl$0::apply_type_mapping_impl::closure_env$0> >+0x30
ty!ty_python_semantic::types::signatures::CallableSignature::apply_type_mapping_impl+0x149
ty!ty_python_semantic::types::CallableType::apply_type_mapping_impl+0xaf
ty!enum2$<ty_python_semantic::types::protocol_class::ProtocolMemberKind>::apply_type_mapping_impl+0x117
ty!ty_python_semantic::types::protocol_class::ProtocolMemberData::apply_type_mapping_impl+0x90
ty!ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure$0+0x114
ty!core::ops::function::impls::impl$4::call_once+0xa
ty!enum2$<core::option::Option<tuple$<ref$<ruff_python_ast::name::Name>,ref$<ty_python_semantic::types::protocol_class::ProtocolMemberData> > > >::map+0x55
ty!core::iter::adapters::map::impl$2::next<tuple$<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0>+0xa6
ty!alloc::vec::spec_from_iter_nested::impl$0::from_iter<tuple$<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,core::iter::adapters::map::Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0> >+0x58
ty!alloc::vec::spec_from_iter::impl$0::from_iter<tuple$<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,core::iter::adapters::map::Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0> >+0x11
ty!alloc::vec::impl$15::from_iter<tuple$<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,core::iter::adapters::map::Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0> >+0x2a
ty!core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0>,alloc::vec::Vec<tuple$<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,alloc::alloc::Global> >+0x11
ty!alloc::collections::btree::map::impl$80::from_iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData,core::iter::adapters::map::Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0> >+0x39
ty!core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData>,ty_python_semantic::types::protocol_class::impl$6::specialized_and_normalized::closure_env$0>,alloc::collections::btree::map::BTreeMap<ruff_python_ast::name::Name,ty_python_semantic::types::protocol_class::ProtocolMemberData,alloc::alloc::Global> >+0x11
ty!ty_python_semantic::types::protocol_class::ProtocolInterface::specialized_and_normalized+0xc7
ty!ty_python_semantic::types::instance::synthesized_protocol::SynthesizedProtocolType::apply_type_mapping_impl+0x54
ty!ty_python_semantic::types::instance::ProtocolInstanceType::apply_type_mapping_impl+0xef
ty!enum2$<ty_python_semantic::types::Type>::apply_type_mapping_impl+0x679
ty!ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure$1+0x206
ty!core::ops::function::impls::impl$4::call_once+0x12
ty!enum2$<core::option::Option<tuple$<usize,tuple$<ref$<enum2$<ty_python_semantic::types::Type> >,ty_python_semantic::types::BoundTypeVarInstance> > > >::map+0x7c
ty!core::iter::adapters::map::impl$2::next<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1>+0xba
ty!alloc::vec::Vec<enum2$<ty_python_semantic::types::Type>,alloc::alloc::Global>::extend_desugared<enum2$<ty_python_semantic::types::Type>,alloc::alloc::Global,core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1> >+0x3c
ty!alloc::vec::spec_extend::impl$0::spec_extend<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1>,alloc::alloc::Global>+0xe
ty!alloc::vec::spec_from_iter_nested::impl$0::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1> >+0x212
ty!alloc::vec::spec_from_iter::impl$0::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1> >+0x11
ty!alloc::vec::impl$15::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1> >+0x2a
ty!core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1>,alloc::vec::Vec<enum2$<ty_python_semantic::types::Type>,alloc::alloc::Global> >+0x11
ty!alloc::boxed::iter::impl$13::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1> >+0x2d
ty!core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<core::iter::adapters::enumerate::Enumerate<core::iter::adapters::zip::Zip<core::slice::iter::Iter<enum2$<ty_python_semantic::types::Type> >,core::iter::adapters::copied::Copied<indexmap::map::iter::Values<ty_python_semantic::types::BoundTypeVarIdentity,ty_python_semantic::types::BoundTypeVarInstance> > > >,ty_python_semantic::types::generics::impl$7::apply_type_mapping_impl::closure_env$1>,alloc::boxed::Box<slice2$<enum2$<ty_python_semantic::types::Type> >,alloc::alloc::Global> >+0x9
ty!ty_python_semantic::types::generics::Specialization::apply_type_mapping_impl+0x266
ty!ty_python_semantic::types::class::GenericAlias::apply_type_mapping_impl+0x12b
ty!enum2$<ty_python_semantic::types::class::ClassType>::apply_type_mapping_impl+0xe0
ty!ty_python_semantic::types::instance::NominalInstanceType::apply_type_mapping_impl+0x1e3
ty!enum2$<ty_python_semantic::types::Type>::apply_type_mapping_impl+0x10ce
```

</details>

This means that this may be related to https://github.com/astral-sh/ty/issues/1736, and may be the kind of panic that https://github.com/astral-sh/ruff/pull/21858 did not remove.

---

_Label `Protocols` added by @mtshiba on 2026-01-06 18:50_

---
