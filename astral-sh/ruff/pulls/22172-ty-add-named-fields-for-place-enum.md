```yaml
number: 22172
title: "[ty] Add named fields for `Place` enum"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: charlie/place
created_at: 2025-12-24T13:57:40Z
updated_at: 2026-01-06T17:24:53Z
url: https://github.com/astral-sh/ruff/pull/22172
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Add named fields for `Place` enum

---

_Pull request opened by @charliermarsh on 2025-12-24 13:57_

## Summary

Mechanical refactor to migrate this enum to named fields. No functional changes.

See: https://github.com/astral-sh/ruff/pull/22093#discussion_r2636050127.


---

_Label `internal` added by @charliermarsh on 2025-12-24 13:57_

---

_Label `ty` added by @charliermarsh on 2025-12-24 13:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 13:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-24 14:00_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1841 diagnostics
+ Found 1842 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5155 diagnostics
+ Found 5154 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14474 diagnostics
+ Found 14475 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @carljm by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-05 21:02_

---

_Comment by @AlexWaygood on 2026-01-06 14:47_

Looking at this, I wonder if a more elegant design might be to have `Place::Defined` wrap a struct that has the named fields?

```rs
enum Place<'db> {
    Defined(DefinedPlace<'db>),
    Undefined,
}

struct DefinedPlace<'db> {
    ty: Type<'db>,
    origin: TypeOrigin,
    definedness: Definedness,
    widening: Widening,
}
```

it might make matching on `Place` a bit easier in places -- it would enable patterns like

```rs
match place {
    Place::Defined(defined_place) if defined_place.is_definitely_defined() => {...}
    Place::Undefined => {...}
```

etc.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:263 on 2026-01-06 16:52_

```suggestion
            Place::Defined(
                place @ DefinedPlace {
                    ty: Type::Union(union),
                    ..
                },
            ) => union.map_with_boundness(db, |elem| {
                Place::Defined(DefinedPlace { ty: *elem, ..place }).try_call_dunder_get(db, owner)
            }),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:278 on 2026-01-06 16:52_

Can't add a suggestion here because the range spans deleted lines, but:

```rs
            Place::Defined(
                place @ DefinedPlace {
                    ty: Type::Intersection(intersection),
                    ..
                },
            ) => intersection.map_with_boundness(db, |elem| {
                Place::Defined(DefinedPlace { ty: *elem, ..place }).try_call_dunder_get(db, owner)
            }),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:790 on 2026-01-06 16:55_

I would do this as:

```diff
diff --git a/crates/ty_python_semantic/src/place.rs b/crates/ty_python_semantic/src/place.rs
index 6b94d5e27e..faeffac47a 100644
--- a/crates/ty_python_semantic/src/place.rs
+++ b/crates/ty_python_semantic/src/place.rs
@@ -765,33 +765,22 @@ impl<'db> PlaceAndQualifiers<'db> {
     pub(crate) fn into_lookup_result(self, db: &'db dyn Db) -> LookupResult<'db> {
         match self {
             PlaceAndQualifiers {
-                place:
-                    Place::Defined(DefinedPlace {
-                        ty,
-                        origin,
-                        definedness: Definedness::AlwaysDefined,
-                        widening,
-                    }),
+                place: Place::Defined(place),
                 qualifiers,
-            } => {
-                let ty = widening.apply_if_needed(db, ty);
-                Ok(TypeAndQualifiers::new(ty, origin, qualifiers))
-            }
-            PlaceAndQualifiers {
-                place:
-                    Place::Defined(DefinedPlace {
+            } => match place.definedness {
+                Definedness::AlwaysDefined => {
+                    let ty = place.widening.apply_if_needed(db, place.ty);
+                    Ok(TypeAndQualifiers::new(ty, place.origin, qualifiers))
+                }
+                Definedness::PossiblyUndefined => {
+                    let ty = place.widening.apply_if_needed(db, place.ty);
+                    Err(LookupError::PossiblyUndefined(TypeAndQualifiers::new(
                         ty,
-                        origin,
-                        definedness: Definedness::PossiblyUndefined,
-                        widening,
-                    }),
-                qualifiers,
-            } => {
-                let ty = widening.apply_if_needed(db, ty);
-                Err(LookupError::PossiblyUndefined(TypeAndQualifiers::new(
-                    ty, origin, qualifiers,
-                )))
-            }
+                        place.origin,
+                        qualifiers,
+                    )))
+                }
+            },
             PlaceAndQualifiers {
                 place: Place::Undefined,
                 qualifiers,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1860 on 2026-01-06 17:00_

```suggestion
                if let Place::Defined(place) = call_symbol
                    && place.is_definitely_defined()
                {
                    place.ty.try_upcast_to_callable(db)
                } else {
                    None
                }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11352 on 2026-01-06 17:03_

I would do:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index acc83a4ebe..a9239ee1c6 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -11329,24 +11329,16 @@ impl<'db> ModuleLiteralType<'db> {
         // if it exists. First, we need to look up the `__getattr__` function in the module's scope.
         if let Some(file) = self.module(db).file(db) {
             let getattr_symbol = imported_symbol(db, file, "__getattr__", None);
-            if let Place::Defined(DefinedPlace {
-                ty: getattr_type,
-                origin,
-                definedness: boundness,
-                widening,
-            }) = getattr_symbol.place
-            {
+            if let Place::Defined(place) = getattr_symbol.place {
                 // If we found a __getattr__ function, try to call it with the name argument
-                if let Ok(outcome) = getattr_type.try_call(
+                if let Ok(outcome) = place.ty.try_call(
                     db,
                     &CallArguments::positional([Type::string_literal(db, name)]),
                 ) {
                     return PlaceAndQualifiers {
                         place: Place::Defined(DefinedPlace {
                             ty: outcome.return_type(db),
-                            origin,
-                            definedness: boundness,
-                            widening,
+                            ..place
                         }),
                         qualifiers: TypeQualifiers::FROM_MODULE_GETATTR,
                     };
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/member.rs`:97 on 2026-01-06 17:07_

```suggestion
                        Place::Defined(place) => {
                            Place::Defined(DefinedPlace { ty, ..place }).with_qualifiers(qualifiers)
                        }
```

---

_@AlexWaygood approved on 2026-01-06 17:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:128 on 2026-01-06 17:09_

I would just not define it if we don't use it right now. Though one of my review suggestions adds a use 😄

---

_@AlexWaygood reviewed on 2026-01-06 17:09_

---

_Merged by @charliermarsh on 2026-01-06 17:24_

---

_Closed by @charliermarsh on 2026-01-06 17:24_

---

_Branch deleted on 2026-01-06 17:24_

---
