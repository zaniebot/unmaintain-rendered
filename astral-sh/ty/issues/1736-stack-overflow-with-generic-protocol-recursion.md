```yaml
number: 1736
title: Stack overflow with generic-protocol recursion
type: issue
state: closed
author: correctmost
labels:
  - fatal
  - Protocols
  - fuzzer
assignees: []
created_at: 2025-12-03T08:08:25Z
updated_at: 2025-12-09T17:05:20Z
url: https://github.com/astral-sh/ty/issues/1736
synced_at: 2026-01-12T15:54:25Z
```

# Stack overflow with generic-protocol recursion

---

_@correctmost_

### Summary

ty crashes when checking this code:

```python
from typing import Protocol

class C[T](Protocol):
    a: 'C[set[T]]'

c: 'C[set[int]]' = C()
```

```
thread '<unknown>' (2402366) has overflowed its stack
fatal runtime error: stack overflow, aborting
```


### Version

ty 0.0.1-alpha.29

---

_Label `fatal` added by @AlexWaygood on 2025-12-03 08:11_

---

_Label `Protocols` added by @AlexWaygood on 2025-12-03 08:11_

---

_Added to milestone `Beta` by @MichaReiser on 2025-12-03 08:12_

---

_Comment by @AlexWaygood on 2025-12-03 13:50_

I ran ty under lldb on this snippet. The looping part of the stacktrace is:

```
    frame #3497: 0x0000000101179c3c ty`ty_python_semantic::types::protocol_class::ProtocolInterface::has_relation_to_impl::_$u7b$$u7b$closure$u7d$$u7d$::hf40cf461dc1096b3(other_member=ProtocolMember @ 0x00000001702ab430) at protocol_class.rs:283:18
    frame #3498: 0x000000010161f4e4 ty`_$LT$I$u20$as$u20$ty_python_semantic..types..constraints..IteratorConstraintsExtension$LT$T$GT$$GT$::when_all::hbb88b0d1f43c4714(self=Map<alloc::collections::btree::map::Iter<ruff_python_ast::name::Name, ty_python_semantic::types::protocol_class::ProtocolMemberData>, ty_python_semantic::types::protocol_class::{impl#6}::members::{closure_env#0}> @ 0x00000001702ab508, db=&dyn ty_python_semantic::db::Db @ 0x00000001702ab470, f={closure_env#0} @ 0x00000001702ab550) at constraints.rs:150:37
    frame #3499: 0x0000000100ce09bc ty`ty_python_semantic::types::protocol_class::ProtocolInterface::has_relation_to_impl::he9d90b44947bc1df(self=ProtocolInterface @ 0x00000001702ab500, db=&dyn ty_python_semantic::db::Db @ 0x00000001702ab588, other=ProtocolInterface @ 0x00000001702ab598, inferable=InferableTypeVars @ 0x00000001702ab780, relation=TypeRelation @ 0x00000001702ab7a0, relation_visitor=0x0000000170df70f0, disjointness_visitor=0x0000000170df7168) at protocol_class.rs:281:27
    frame #3500: 0x0000000100ee7a30 ty`ty_python_semantic::types::instance::_$LT$impl$u20$ty_python_semantic..types..Type$GT$::satisfies_protocol::h59328380805a265b(self=Type @ 0x00000001702ab7d0, db=&dyn ty_python_semantic::db::Db @ 0x00000001702ab740, protocol=ProtocolInstanceType @ 0x00000001702ab7f0, inferable=InferableTypeVars @ 0x00000001702ab800, relation=TypeRelation @ 0x00000001702ab820, relation_visitor=0x0000000170df70f0, disjointness_visitor=0x0000000170df7168) at instance.rs:136:41
    frame #3501: 0x0000000100b28ef8 ty`ty_python_semantic::types::Type::has_relation_to_impl::_$u7b$$u7b$closure$u7d$$u7d$::h020197c07eecf21f at types.rs:2501:26
    frame #3502: 0x0000000100fbdcf0 ty`ty_python_semantic::types::cyclic::CycleDetector$LT$Tag$C$T$C$R$GT$::visit::h0ebf05d0aadeb5e2(self=0x0000000170df70f0, item=(ty_python_semantic::types::Type, ty_python_semantic::types::Type, ty_python_semantic::types::TypeRelation) @ 0x00000001702ac850, func={closure_env#19} @ 0x00000001702ac8a0) at cyclic.rs:86:19
    frame #3503: 0x0000000100ed0eec ty`ty_python_semantic::types::Type::has_relation_to_impl::hf4c3659561405fc1(self=Type @ 0x00000001702ae200, db=&dyn ty_python_semantic::db::Db @ 0x00000001702ad170, target=Type @ 0x00000001702ae220, inferable=InferableTypeVars @ 0x00000001702ae180, relation=TypeRelation @ 0x00000001702ae198, relation_visitor=0x0000000170df70f0, disjointness_visitor=0x0000000170df7168) at types.rs:2500:34
    frame #3504: 0x000000010117a0e0 ty`ty_python_semantic::types::protocol_class::ProtocolInterface::has_relation_to_impl::_$u7b$$u7b$closure$u7d$$u7d$::_$u7b$$u7b$closure$u7d$$u7d$::h9fe2b0badb31130d(our_member=ProtocolMember @ 0x00000001702ae310) at protocol_class.rs:341:26
```

(etc. on infinite repeat)

---

_Comment by @carljm on 2025-12-05 01:10_

This is why the cycle-detection visitor doesn't catch this infinite recursion:

```
Protocol instance subtyping: C[Unknown] <: C[set[int]]
Protocol instance subtyping: C[set[Unknown]] <: C[set[set[int]]]
Protocol instance subtyping: C[set[set[Unknown]]] <: C[set[set[set[int]]]]
Protocol instance subtyping: C[set[set[set[Unknown]]]] <: C[set[set[set[set[int]]]]]
Protocol instance subtyping: C[set[set[set[set[Unknown]]]]] <: C[set[set[set[set[set[int]]]]]]
Protocol instance subtyping: C[set[set[set[set[set[Unknown]]]]]] <: C[set[set[set[set[set[set[int]]]]]]]
Protocol instance subtyping: C[set[set[set[set[set[set[Unknown]]]]]]] <: C[set[set[set[set[set[set[set[int]]]]]]]]
Protocol instance subtyping: C[set[set[set[set[set[set[set[Unknown]]]]]]]] <: C[set[set[set[set[set[set[set[set[int]]]]]]]]]
Protocol instance subtyping: C[set[set[set[set[set[set[set[set[Unknown]]]]]]]]] <: C[set[set[set[set[set[set[set[set[set[int]]]]]]]]]]
...
```

Each time we recurse to the next layer of recursive attribute, we also deepen the recursion of the type parameter type.

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:07_

---

_Assigned to @carljm by @carljm on 2025-12-05 15:38_

---

_Closed by @carljm on 2025-12-09 17:05_

---
