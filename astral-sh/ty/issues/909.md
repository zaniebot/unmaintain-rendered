```yaml
number: 909
title: "[panic] try_iterate() should not fail on a type assignable to Iterable"
type: issue
state: closed
author: mermoldy
labels:
  - fatal
assignees: []
created_at: 2025-07-28T11:15:30Z
updated_at: 2025-07-28T16:18:37Z
url: https://github.com/astral-sh/ty/issues/909
synced_at: 2026-01-10T02:06:24Z
```

# [panic] try_iterate() should not fail on a type assignable to Iterable

---

_Issue opened by @mermoldy on 2025-07-28 11:15_

Hi, I found an issue while analyzing the [Peewee](https://github.com/coleifer/peewee) ORM code. I’ve reduced it to a minimal example:

```python
import peewee
from playhouse import mysql_ext


class Model(peewee.Model):
    field = mysql_ext.JSONField()


m = Model()
tuple(m.field)
```

<details>

Peewee version:
```
> pip show peewee
Name: peewee
Version: 3.16.2
Summary: a little orm
Home-page: https://github.com/coleifer/peewee/
Author: Charles Leifer
Author-email: coleifer@gmail.com
License: MIT License
Location: /Users/mermoldy/Projects/fatmouse/.venv/lib/python3.13/site-packages
Requires:
Required-by:
```

Ty traceback:
```
❯ RUST_BACKTRACE=1 ty check /Users/mermoldy/Projects/fatmouse/taco/app/workspace/service/configuration_version.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at crates/ty_python_semantic/src/types/call/bind.rs:1010:75 when checking `/Users/mermoldy/Projects/fatmouse/taco/app/workspace/service/configuration_version.py`: `try_iterate() should not fail on a type assignable to `Iterable`: IterCallError(PossiblyNotCallable, Bindings { callable_type: Union(UnionType { elements: [Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("NoneType"), body_scope: ScopeId { [salsa id]: Id(1143), file: File(Vendored(VendoredPathBuf("stdlib/types.pyi"))), file_scope_id: FileScopeId(215) }, known: Some(NoneType), deprecated: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })] }), elements: [CallableBinding { callable_type: Dynamic(Unknown), signature_type: Dynamic(Unknown), dunder_call_is_possibly_unbound: false, bound_type: None, overload_call_return_type: None, matching_overload_index: Some(0), overloads: [Binding { signature: Signature { generic_context: None, inherited_generic_context: None, definition: None, parameters: Parameters { value: [Parameter { annotated_type: Some(Dynamic(Any)), kind: Variadic { name: Name("args") }, form: Value }, Parameter { annotated_type: Some(Dynamic(Any)), kind: KeywordVariadic { name: Name("kwargs") }, form: Value }], is_gradual: true }, return_ty: Some(Dynamic(Unknown)) }, callable_type: Dynamic(Unknown), signature_type: Dynamic(Unknown), return_ty: Dynamic(Unknown), specialization: None, inherited_specialization: None, argument_matches: [], parameter_tys: [None, None], errors: [] }] }, CallableBinding { callable_type: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("NoneType"), body_scope: ScopeId { [salsa id]: Id(1143), file: File(Vendored(VendoredPathBuf("stdlib/types.pyi"))), file_scope_id: FileScopeId(215) }, known: Some(NoneType), deprecated: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }), signature_type: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("NoneType"), body_scope: ScopeId { [salsa id]: Id(1143), file: File(Vendored(VendoredPathBuf("stdlib/types.pyi"))), file_scope_id: FileScopeId(215) }, known: Some(NoneType), deprecated: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }), dunder_call_is_possibly_unbound: false, bound_type: None, overload_call_return_type: None, matching_overload_index: None, overloads: [] }], argument_forms: [], conflicting_forms: [] })`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.16 (c452e53a0 2025-07-25)
info: Args: ["ty", "check", "/Users/mermoldy/Projects/fatmouse/taco/app/workspace/service/configuration_version.py"]
info: Backtrace:
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
  27: __mh_execute_header
  28: __mh_execute_header
  29: __mh_execute_header
  30: __mh_execute_header
  31: __mh_execute_header
  32: __mh_execute_header
  33: __mh_execute_header
  34: __mh_execute_header
  35: __mh_execute_header
  36: __mh_execute_header
  37: __mh_execute_header
  38: __mh_execute_header
  39: __mh_execute_header
  40: __mh_execute_header
  41: __mh_execute_header
  42: __mh_execute_header
  43: __mh_execute_header
  44: __mh_execute_header
  45: __pthread_cond_wait

info: query stacktrace:
   0: infer_scope_types(Id(1008))
             at crates/ty_python_semantic/src/types/infer.rs:134
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:514


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `fatal` added by @AlexWaygood on 2025-07-28 11:19_

---

_Assigned to @dcreager by @dcreager on 2025-07-28 14:05_

---

_Comment by @dcreager on 2025-07-28 14:29_

I thought this might be because of https://github.com/astral-sh/ruff/pull/19496, which touched the iterator protocol machinery recently. But it actually looks like it might be an issue with checking protocol assignability [[playground](https://play.ty.dev/428128f8-8979-4af4-8eb8-1cfbb9b7a078)]:

```py
from typing import Iterable
from typing_extensions import reveal_type
from ty_extensions import is_assignable_to

class NotIterable:
    __iter__ = None

reveal_type(is_assignable_to(NotIterable, Iterable))
```

@AlexWaygood, does that make this a duplicate of #889? I think this will be fixed once we consider the types of protocol members when checking assignability.

In the meantime I can fix the panic by removing the `expect` call and falling back on `Unknown`.

---

_Comment by @AlexWaygood on 2025-07-28 14:32_

> [@AlexWaygood](https://github.com/AlexWaygood), does that make this a duplicate of [#889](https://github.com/astral-sh/ty/issues/889)? I think this will be fixed once we consider the types of protocol members when checking assignability.

yup

> In the meantime I can fix the panic by removing the `expect` call and falling back on `Unknown`.

yes, I think that makes sense in the meantime. But it would be good if we could at least emit a logging message, because this shouldn't be happening! Definitely a bug in our internals that it is.

---

_Comment by @dcreager on 2025-07-28 16:18_

https://github.com/astral-sh/ruff/pull/19602 replaces the panic with a debug log message. For the fix of the underlying issue, closing this as a dup of #889

---

_Closed by @dcreager on 2025-07-28 16:18_

---
