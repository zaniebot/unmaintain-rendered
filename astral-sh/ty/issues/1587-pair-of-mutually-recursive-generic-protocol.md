```yaml
number: 1587
title: "pair of mutually recursive generic `Protocol` definitions"
type: issue
state: open
author: gertvdijk
labels:
  - Protocols
assignees: []
created_at: 2025-11-18T23:05:36Z
updated_at: 2025-12-19T12:07:21Z
url: https://github.com/astral-sh/ty/issues/1587
synced_at: 2026-01-12T15:54:25Z
```

# pair of mutually recursive generic `Protocol` definitions

---

_@gertvdijk_

I've got a small Python typing showcase (currently using mypy) up at https://github.com/gertvdijk/mypy-sibling-generics-demo. Was wondering how `ty` would handle/show this, tried it out, but sadly it triggers `ty` to panic (tested at [this current latest commit](https://github.com/gertvdijk/mypy-sibling-generics-demo/commit/18e84d223ffb2dfdd53e5ee362db47101e15a9f5)). It told me to create this bug report, so here I am. 😄 

Steps to reproduce: clone that project, and running `ty` triggers it basically. I believe the output below includes all necessary info, but please LMK if you need anything else or even a smaller MRE.

```console
$ git clone https://github.com/gertvdijk/mypy-sibling-generics-demo.git && cd mypy-sibling-generics-demo
$ uv add --dev ty
$ uv run ty check
error[panic]: Panicked at crates/ty_python_semantic/src/types/instance.rs:822:18 when checking `/path/to/mypy-sibling-generics-demo/foobar/client.py`: `Class wrapped by `Protocol` should be a protocol class`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.27
[...]
```

<details>

<summary>Full output</summary>

```console
error[panic]: Panicked at crates/ty_python_semantic/src/types/instance.rs:822:18 when checking `/path/to/mypy-sibling-generics-demo/foobar/client.py`: `Class wrapped by `Protocol` should be a protocol class`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.27
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: is_equivalent_to_object_inner(Id(25801))
             at crates/ty_python_semantic/src/types/instance.rs:663
             cycle heads: infer_definition_types(Id(300e)) -> iteration = 2
   1: infer_deferred_types(Id(300b))
             at crates/ty_python_semantic/src/types/infer.rs:141
             cycle heads: infer_definition_types(Id(300d)) -> iteration = 2, TypeVarInstance < 'db >::lazy_bound_(Id(1c801)) -> iteration = 2
   2: TypeVarInstance < 'db >::lazy_bound_(Id(1c800))
             at crates/ty_python_semantic/src/types.rs:8734
   3: infer_definition_types(Id(300e))
             at crates/ty_python_semantic/src/types/infer.rs:94
   4: infer_deferred_types(Id(300c))
             at crates/ty_python_semantic/src/types/infer.rs:141
   5: TypeVarInstance < 'db >::lazy_bound_(Id(1c801))
             at crates/ty_python_semantic/src/types.rs:8734
   6: infer_definition_types(Id(300d))
             at crates/ty_python_semantic/src/types/infer.rs:94
   7: infer_scope_types(Id(2400))
             at crates/ty_python_semantic/src/types/infer.rs:70
   8: check_file_impl(Id(c01))
             at crates/ty_project/src/lib.rs:535


error[panic]: Panicked at crates/ty_python_semantic/src/types/instance.rs:822:18 when checking `/path/to/mypy-sibling-generics-demo/foobar/server.py`: `Class wrapped by `Protocol` should be a protocol class`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.27
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: is_equivalent_to_object_inner(Id(26801))
             at crates/ty_python_semantic/src/types/instance.rs:663
             cycle heads: infer_definition_types(Id(380d)) -> iteration = 2
   1: infer_deferred_types(Id(3808))
             at crates/ty_python_semantic/src/types/infer.rs:141
             cycle heads: infer_definition_types(Id(380c)) -> iteration = 2, TypeVarInstance < 'db >::lazy_bound_(Id(1e801)) -> iteration = 2
   2: TypeVarInstance < 'db >::lazy_bound_(Id(1e800))
             at crates/ty_python_semantic/src/types.rs:8734
   3: infer_definition_types(Id(380d))
             at crates/ty_python_semantic/src/types/infer.rs:94
   4: infer_deferred_types(Id(3809))
             at crates/ty_python_semantic/src/types/infer.rs:141
   5: TypeVarInstance < 'db >::lazy_bound_(Id(1e801))
             at crates/ty_python_semantic/src/types.rs:8734
   6: infer_definition_types(Id(380c))
             at crates/ty_python_semantic/src/types/infer.rs:94
   7: infer_scope_types(Id(2800))
             at crates/ty_python_semantic/src/types/infer.rs:70
   8: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 2 diagnostics
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `fatal` added by @AlexWaygood on 2025-11-18 23:10_

---

_Label `Protocols` added by @AlexWaygood on 2025-11-18 23:10_

---

_Added to milestone `Beta` by @carljm on 2025-11-18 23:53_

---

_Renamed from "[panic] `Class wrapped by `Protocol` should be a protocol class`" to "[panic] Class wrapped by `Protocol` should be a protocol class" by @dhruvmanila on 2025-11-19 02:53_

---

_Comment by @MeGaGiGaGon on 2025-11-19 06:52_

I was able to minimize the code to this smaller/standalone repo:
```py
from typing import Any, Protocol, TypeVar

Req_t_co = TypeVar("Req_t_co", bound="BaseRequest[Any]")
Res_t_co = TypeVar("Res_t_co", bound="BaseResponse[Any]")

class BaseRequest(Protocol[Res_t_co]):
    def get_response_type() :
        ...

class BaseResponse(Protocol[Req_t_co]):
    def get_request_type() :
        ...

SCReq_t_co = TypeVar("SCReq_t_co", bound="ServerContextRequest[Any]")
SCResp_t_co = TypeVar("SCResp_t_co", bound="ServerContextResponse[Any]")

class ServerContextRequest(
    BaseRequest[SCResp_t_co], Protocol[SCResp_t_co]
):
    ...

class ServerContextResponse(
    BaseResponse[SCReq_t_co], Protocol[SCReq_t_co]
):
    ...
```

---

_Comment by @AlexWaygood on 2025-11-19 12:17_

Thanks @MeGaGiGaGon -- that's extremely helpful!!

---

_Comment by @AlexWaygood on 2025-11-19 12:47_

The class that's causing us to panic here is `ServerContextResponse`. The way we figure out if a class is a `Protocol` class or not is by iterating through its explicit bases in reverse order and seeing if any of them are either `Protocol` or `Protocol[T, S]` etc:

https://github.com/astral-sh/ruff/blob/18a14bfaf114460e77b0792d49e0824e5c8a43d5/crates/ty_python_semantic/src/types/class.rs#L1640-L1668

It looks like at an earlier stage of type-checking, we looked at `ServerContextResponse`'s explicit bases, saw `Protocol` in there, decided it was a `Protocol` class, and created a `ProtocolInstanceType`. But later on, when looking at the same class, we now think that the types of the class's explicit bases are `[Never, Never]` rather than `[<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>, typing.Protocol[SCReq_t_co]]` -- meaning that we no longer infer it as being a protocol class, and therefore causing us to panic when we retrieve the interface for a previously constructed `ProtocolInstanceType` that refers to this class.

I came to this conclusion by adding these debug prints:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 8ac9eca111..46494f73e7 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -1648,6 +1648,15 @@ impl<'db> ClassLiteral<'db> {
         self.known(db)
             .map(KnownClass::is_protocol)
             .unwrap_or_else(|| {
+                if self.name(db) == "ServerContextResponse" {
+                    println!(
+                        "Explicit bases: {:?}",
+                        self.explicit_bases(db)
+                            .iter()
+                            .map(|t| t.display(db).to_string())
+                            .collect::<Vec<_>>()
+                    );
+                }
                 // Iterate through the last three bases of the class
                 // searching for `Protocol` or `Protocol[]` in the bases list.
                 //
diff --git a/crates/ty_python_semantic/src/types/protocol_class.rs b/crates/ty_python_semantic/src/types/protocol_class.rs
index 6cb204231f..b5a143a06b 100644
--- a/crates/ty_python_semantic/src/types/protocol_class.rs
+++ b/crates/ty_python_semantic/src/types/protocol_class.rs
@@ -37,6 +37,9 @@ impl<'db> ClassLiteral<'db> {
 impl<'db> ClassType<'db> {
     /// Returns `Some` if this is a protocol class, `None` otherwise.
     pub(super) fn into_protocol_class(self, db: &'db dyn Db) -> Option<ProtocolClass<'db>> {
+        if self.name(db) == "ServerContextResponse" {
+            dbg!(self.is_protocol(db));
+        }
         self.is_protocol(db).then_some(ProtocolClass(self))
     }
 }
```

Which, for the minimal repro above, cause us to print:

```
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
[crates/ty_python_semantic/src/types/protocol_class.rs:41:13] self.is_protocol(db) = true
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
[crates/ty_python_semantic/src/types/protocol_class.rs:41:13] self.is_protocol(db) = true
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
Explicit bases: ["<class 'BaseResponse[SCReq_t_co@ServerContextResponse]'>", "typing.Protocol[SCReq_t_co]"]
Explicit bases: ["Never", "Never"]
[crates/ty_python_semantic/src/types/protocol_class.rs:41:13] self.is_protocol(db) = false
Explicit bases: ["Never", "Never"]
```

I don't understand how this can be possible, since `explicit_bases` is a Salsa query: surely after it's been called once, the result should be cached and it should return the same types every time?

---

_Comment by @MichaReiser on 2025-11-19 13:37_

> I don't understand how this can be possible, since explicit_bases is a Salsa query: surely after it's been called once, the result should be cached and it should return the same types every time?

Unless there's a cycle, in which case the query runs many times. But all other queries participating in the same cycle should re-run.

---

_Comment by @AlexWaygood on 2025-11-19 13:39_

> Unless there's a cycle, in which case the query runs many times

Right, but my debug print is printing the value _returned_ by `ClassLiteral::explicit_bases()`. It will only return a result after the cycle has converged, I think? And after it's returned a value once, that value should be cached, and it shouldn't ever run the query again?

---

_Comment by @MichaReiser on 2025-11-19 13:51_

> It will only return a result after the cycle has converged, I think? 

`is_protocol` might participate in the cycle and it can then see the provisional value (not converged value). This changed when I improved cycle handling performance. If there are multiple nested cycles, we no longer iterate the inner cycle to completion before iterating the outer cycle because this has exponential runtime. Instead, all cycles (the outermost and all nested cycles) iterate at once, all seeing the provisional values of the other cycle heads until they all converge. This has the benefit that the complexity is bound by how quickly the problem converges and not by the number of nested cycles + how quickly the problem converges

---

_Comment by @AlexWaygood on 2025-11-19 13:53_

OK, thanks. In that case this `.expect()` call seems definitely unsafe, as we can't rely on `is_protocol` returning a consistent thing for one class.

---

_Comment by @AlexWaygood on 2025-11-19 14:24_

Here's a patch that gets rid of the `.expect()` call:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types/display.rs b/crates/ty_python_semantic/src/types/display.rs
index b8a8a05ac4..f003b5d2cd 100644
--- a/crates/ty_python_semantic/src/types/display.rs
+++ b/crates/ty_python_semantic/src/types/display.rs
@@ -195,7 +195,7 @@ impl<'db> super::visitor::TypeVisitor<'db> for AmbiguousClassCollector<'db> {
             Type::ProtocolInstance(ProtocolInstanceType {
                 inner: Protocol::FromClass(class),
                 ..
-            }) => return self.visit_type(db, Type::from(class)),
+            }) => return self.visit_type(db, Type::from(*class)),
             _ => {}
         }
 
@@ -392,12 +392,14 @@ impl Display for DisplayRepresentation<'_> {
                 }
             }
             Type::ProtocolInstance(protocol) => match protocol.inner {
-                Protocol::FromClass(ClassType::NonGeneric(class)) => {
-                    class.display_with(self.db, self.settings.clone()).fmt(f)
-                }
-                Protocol::FromClass(ClassType::Generic(alias)) => {
-                    alias.display_with(self.db, self.settings.clone()).fmt(f)
-                }
+                Protocol::FromClass(class) => match *class {
+                    ClassType::NonGeneric(class) => {
+                        class.display_with(self.db, self.settings.clone()).fmt(f)
+                    }
+                    ClassType::Generic(alias) => {
+                        alias.display_with(self.db, self.settings.clone()).fmt(f)
+                    }
+                },
                 Protocol::Synthesized(synthetic) => {
                     f.write_str("<Protocol with members ")?;
                     let interface = synthetic.interface();
diff --git a/crates/ty_python_semantic/src/types/generics.rs b/crates/ty_python_semantic/src/types/generics.rs
index 169b69e496..45d1cea575 100644
--- a/crates/ty_python_semantic/src/types/generics.rs
+++ b/crates/ty_python_semantic/src/types/generics.rs
@@ -1565,9 +1565,9 @@ impl<'db> SpecializationBuilder<'db> {
                     // generic protocol, we will need to check the types of the protocol members to be
                     // able to infer the specialization of the protocol that the class implements.
                     Type::ProtocolInstance(ProtocolInstanceType {
-                        inner: Protocol::FromClass(ClassType::Generic(alias)),
+                        inner: Protocol::FromClass(class),
                         ..
-                    }) => Some(alias),
+                    }) => class.into_generic_alias(),
                     _ => None,
                 };
 
diff --git a/crates/ty_python_semantic/src/types/instance.rs b/crates/ty_python_semantic/src/types/instance.rs
index 523f7fc796..ac7e5294b9 100644
--- a/crates/ty_python_semantic/src/types/instance.rs
+++ b/crates/ty_python_semantic/src/types/instance.rs
@@ -10,7 +10,7 @@ use crate::semantic_index::definition::Definition;
 use crate::types::constraints::{ConstraintSet, IteratorConstraintsExtension};
 use crate::types::enums::is_single_member_enum;
 use crate::types::generics::{InferableTypeVars, walk_specialization};
-use crate::types::protocol_class::walk_protocol_interface;
+use crate::types::protocol_class::{ProtocolClass, walk_protocol_interface};
 use crate::types::tuple::{TupleSpec, TupleType};
 use crate::types::{
     ApplyTypeMappingVisitor, ClassBase, ClassLiteral, FindLegacyTypeVarsVisitor,
@@ -44,13 +44,17 @@ impl<'db> Type<'db> {
                     .as_ref(),
             )),
             Some(KnownClass::Object) => Type::object(),
-            _ if class_literal.is_protocol(db) => {
-                Self::ProtocolInstance(ProtocolInstanceType::from_class(class))
-            }
-            _ if class_literal.is_typed_dict(db) => Type::typed_dict(class),
-            // We don't call non_tuple_instance here because we've already checked that the class
-            // is not `object`
-            _ => Type::NominalInstance(NominalInstanceType(NominalInstanceInner::NonTuple(class))),
+            _ => class_literal
+                .is_typed_dict(db)
+                .then(|| Type::typed_dict(class))
+                .or_else(|| {
+                    class.into_protocol_class(db).map(|protocol_class| {
+                        Self::ProtocolInstance(ProtocolInstanceType::from_class(protocol_class))
+                    })
+                })
+                .unwrap_or(Type::NominalInstance(NominalInstanceType(
+                    NominalInstanceInner::NonTuple(class),
+                ))),
         }
     }
 
@@ -601,7 +605,7 @@ pub(super) fn walk_protocol_instance_type<'db, V: super::visitor::TypeVisitor<'d
 impl<'db> ProtocolInstanceType<'db> {
     // Keep this method private, so that the only way of constructing `ProtocolInstanceType`
     // instances is through the `Type::instance` constructor function.
-    fn from_class(class: ClassType<'db>) -> Self {
+    fn from_class(class: ProtocolClass<'db>) -> Self {
         Self {
             inner: Protocol::FromClass(class),
             _phantom: PhantomData,
@@ -625,7 +629,7 @@ impl<'db> ProtocolInstanceType<'db> {
     pub(super) fn as_nominal_type(self) -> Option<NominalInstanceType<'db>> {
         match self.inner {
             Protocol::FromClass(class) => {
-                Some(NominalInstanceType(NominalInstanceInner::NonTuple(class)))
+                Some(NominalInstanceType(NominalInstanceInner::NonTuple(*class)))
             }
             Protocol::Synthesized(_) => None,
         }
@@ -634,7 +638,7 @@ impl<'db> ProtocolInstanceType<'db> {
     /// Return the meta-type of this protocol-instance type.
     pub(super) fn to_meta_type(self, db: &'db dyn Db) -> Type<'db> {
         match self.inner {
-            Protocol::FromClass(class) => SubclassOfType::from(db, class),
+            Protocol::FromClass(class) => SubclassOfType::from(db, *class),
 
             // TODO: we can and should do better here.
             //
@@ -805,11 +809,17 @@ impl<'db> VarianceInferable<'db> for ProtocolInstanceType<'db> {
 
 /// An enumeration of the two kinds of protocol types: those that originate from a class
 /// definition in source code, and those that are synthesized from a set of members.
+///
+/// # Ordering
+///
+/// Ordering between variants is stable and should be the same between runs.
+/// Ordering within variants is based on the wrapped data's salsa-assigned id and not on its values.
+/// The id may change between runs, or when e.g. a `Protocol` was garbage-collected and recreated.
 #[derive(
     Copy, Clone, Debug, Eq, PartialEq, Hash, salsa::Update, PartialOrd, Ord, get_size2::GetSize,
 )]
 pub(super) enum Protocol<'db> {
-    FromClass(ClassType<'db>),
+    FromClass(ProtocolClass<'db>),
     Synthesized(SynthesizedProtocolType<'db>),
 }
 
@@ -817,10 +827,7 @@ impl<'db> Protocol<'db> {
     /// Return the members of this protocol type
     fn interface(self, db: &'db dyn Db) -> ProtocolInterface<'db> {
         match self {
-            Self::FromClass(class) => class
-                .into_protocol_class(db)
-                .expect("Class wrapped by `Protocol` should be a protocol class")
-                .interface(db),
+            Self::FromClass(class) => class.interface(db),
             Self::Synthesized(synthesized) => synthesized.interface(),
         }
     }
diff --git a/crates/ty_python_semantic/src/types/protocol_class.rs b/crates/ty_python_semantic/src/types/protocol_class.rs
index 6cb204231f..5e77bc5e99 100644
--- a/crates/ty_python_semantic/src/types/protocol_class.rs
+++ b/crates/ty_python_semantic/src/types/protocol_class.rs
@@ -42,7 +42,15 @@ impl<'db> ClassType<'db> {
 }
 
 /// Representation of a single `Protocol` class definition.
-#[derive(Debug, Copy, Clone, PartialEq, Eq)]
+///
+/// # Ordering
+///
+/// Ordering between variants is stable and should be the same between runs.
+/// Ordering within variants is based on the wrapped data's salsa-assigned id and not on its values.
+/// The id may change between runs, or when e.g. a `ProtocolClass` was garbage-collected and recreated.
+#[derive(
+    Debug, Copy, Clone, PartialEq, Eq, Hash, salsa::Update, get_size2::GetSize, PartialOrd, Ord,
+)]
 pub(super) struct ProtocolClass<'db>(ClassType<'db>);
 
 impl<'db> ProtocolClass<'db> {
@@ -124,6 +132,19 @@ impl<'db> ProtocolClass<'db> {
             report_undeclared_protocol_member(context, first_definition, self, class_place_table);
         }
     }
+
+    pub(super) fn apply_type_mapping_impl<'a>(
+        self,
+        db: &'db dyn Db,
+        type_mapping: &TypeMapping<'a, 'db>,
+        tcx: TypeContext<'db>,
+        visitor: &ApplyTypeMappingVisitor<'db>,
+    ) -> Self {
+        Self(
+            self.0
+                .apply_type_mapping_impl(db, type_mapping, tcx, visitor),
+        )
+    }
 }
 
 impl<'db> Deref for ProtocolClass<'db> {
```

</details>

But with that patch applied, we still panic on the repro in https://github.com/astral-sh/ty/issues/1587#issuecomment-3551075375, just in a different place:

```
error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:321:21 when checking `/Users/alexw/dev/ruff/foo.py`: `ClassLiteral < 'db >::explicit_bases_(Id(4c09)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.5+60 (18a14bfaf 2025-11-19)
info: Args: ["target/debug/ty", "check", "foo.py", "--python-version=3.14"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: cached_protocol_interface(Id(6805))
             at crates/ty_python_semantic/src/types/protocol_class.rs:790
   1: is_equivalent_to_object_inner(Id(8003))
             at crates/ty_python_semantic/src/types/instance.rs:667
   2: infer_deferred_types(Id(1409))
             at crates/ty_python_semantic/src/types/infer.rs:141
             cycle heads: infer_definition_types(Id(140b)) -> iteration = 200, TypeVarInstance < 'db >::lazy_bound_(Id(5803)) -> iteration = 200
   3: TypeVarInstance < 'db >::lazy_bound_(Id(5802))
             at crates/ty_python_semantic/src/types.rs:8734
   4: infer_definition_types(Id(140c))
             at crates/ty_python_semantic/src/types/infer.rs:94
   5: infer_deferred_types(Id(140a))
             at crates/ty_python_semantic/src/types/infer.rs:141
   6: TypeVarInstance < 'db >::lazy_bound_(Id(5803))
             at crates/ty_python_semantic/src/types.rs:8734
   7: infer_definition_types(Id(140b))
             at crates/ty_python_semantic/src/types/infer.rs:94
   8: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
   9: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

I think this confirms that the reason why we were not considering the class a `Protocol` class later on was because we were trying to converge on a cycle for the `explicit_bases` query.

---

_Comment by @AlexWaygood on 2025-11-19 14:25_

That patch might be worth applying anyway. It's probably slightly more efficient, and it's nice to get rid of `.expect()` calls where possible. It also makes it clear that the underlying bug here isn't really in our `Protocol` machinery (edit: or, at least not the bit in `instance.rs`...? Not sure...).

I'll make a PR.

---

_Comment by @MichaReiser on 2025-11-19 15:14_

> OK, thanks. In that case this .expect() call seems definitely unsafe, as we can't rely on is_protocol returning a consistent thing for one class.

I have to read the code more carefully but I've to catch a train. Overall, all queries that participate in a cycle should be rerun, and the results of a query are consistent within an iteration. 

---

_Comment by @AlexWaygood on 2025-11-24 20:26_

An x-failing mdtest for this was added in https://github.com/astral-sh/ruff/pull/21594.

---

_Renamed from "[panic] Class wrapped by `Protocol` should be a protocol class" to "[panic] `ClassLiteral::explicit_bases` fails to converge on a pair of mutually recursive `Protocol` definitions" by @AlexWaygood on 2025-11-24 20:26_

---

_Renamed from "[panic] `ClassLiteral::explicit_bases` fails to converge on a pair of mutually recursive `Protocol` definitions" to "[panic] `ClassLiteral::explicit_bases` fails to converge on a pair of mutually recursive generic `Protocol` definitions" by @AlexWaygood on 2025-11-24 20:26_

---

_Comment by @MichaReiser on 2025-11-28 15:52_

@carljm is this still something you plan to look into next week?

---

_Comment by @carljm on 2025-12-02 00:44_

I don't remember saying that I planned to, but sure, if it needs an owner I'm happy to look into it.

---

_Assigned to @carljm by @carljm on 2025-12-02 00:44_

---

_Closed by @carljm on 2025-12-04 23:17_

---

_Renamed from "[panic] `ClassLiteral::explicit_bases` fails to converge on a pair of mutually recursive generic `Protocol` definitions" to "pair of mutually recursive generic `Protocol` definitions" by @carljm on 2025-12-04 23:18_

---

_Label `fatal` removed by @carljm on 2025-12-04 23:18_

---

_Label `cycles` added by @carljm on 2025-12-04 23:21_

---

_Removed from milestone `Beta` by @carljm on 2025-12-04 23:21_

---

_Added to milestone `Stable` by @carljm on 2025-12-04 23:21_

---

_Reopened by @carljm on 2025-12-04 23:21_

---

_Comment by @carljm on 2025-12-04 23:22_

This no longer panics, but we can have false negatives on one of the upper bounds; there are TODOs in the tests to capture this, should also be fixable.

---

_Closed by @AlexWaygood on 2025-12-05 11:26_

---

_Reopened by @carljm on 2025-12-05 17:40_

---

_Label `cycles` removed by @AlexWaygood on 2025-12-19 12:07_

---
