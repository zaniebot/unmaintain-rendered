```yaml
number: 18073
title: "[ty] improve diagnostics for failure to call overloaded function"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/diag-overload
created_at: 2025-05-13T17:18:03Z
updated_at: 2025-05-14T22:16:47Z
url: https://github.com/astral-sh/ruff/pull/18073
synced_at: 2026-01-10T18:51:01Z
```

# [ty] improve diagnostics for failure to call overloaded function

---

_Pull request opened by @BurntSushi on 2025-05-13 17:18_

This PR takes a crack at improving diagnostics for invalid overloaded
function calls as a result of there being no matching overloads.

I think this is a strict improvement on the status quo, but it's
definitely not as good as we can do. In particular, this only
adds sub-diagnostics that points to each unmatched overload, but
doesn't say *why* each overload does not match. I didn't do this
because 1) this seemed like an improvement we could merge as-is and
2) I wasn't sure how to match up the overloaded `FunctionType`s
returned by `FunctionType::to_overloaded` with the `overloads` in a
`CallableBinding`. It seems like they are in correspondence with
one another, but I'm not sure if this is an API guarantee.

I guess ideally (from my uninformed perspective), it would be nice to
have the overloaded `FunctionType` on the `Binding`. But I wasn't
certain how best to go about that, or if that's the right thing to do.

Fixes astral-sh/ty#274


---

_Review requested from @carljm by @BurntSushi on 2025-05-13 17:18_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-13 17:18_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-13 17:18_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-13 17:18_

---

_Renamed from "[ty] ty_python_semantic: improve diagnostics for failure to call overloaded function" to "[ty] improve diagnostics for failure to call overloaded function" by @MichaReiser on 2025-05-13 17:18_

---

_Label `ty` added by @MichaReiser on 2025-05-13 17:19_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-13 17:19_

---

_Review request for @dcreager removed by @BurntSushi on 2025-05-13 17:19_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-05-13 17:19_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-05-13 17:19_

---

_Comment by @BurntSushi on 2025-05-13 17:22_

Here are screenshots showing before/after for this Python file:

```python
from typing import overload

@overload
def f(x: int) -> int: ...

@overload
def f(x: str) -> str: ...

def f(x: int | str) -> int | str:
    return x

f(b"foo")
```

Before:

![before](https://github.com/user-attachments/assets/fd546040-c1ec-4137-8c90-2eeb0ee2696e)

After:

![after](https://github.com/user-attachments/assets/85d2e07e-a44d-4d70-b200-198b9e5ccb12)


---

_Comment by @github-actions[bot] on 2025-05-13 17:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-13 17:57_

I'm not sure having a separate subdiagnostic for each overload definition scales well for functions with many overloads. Here's a single diagnostic I get for a bad `pow()` call with your branch:

```
error[no-matching-overload]: No overload of function `pow` matches arguments
 --> foo.py:3:1
  |
1 | class Foo: ...
2 |
3 | pow(Foo(), Foo())
  | ^^^^^^^^^^^^^^^^^
  |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1700:5
     |
1698 | # but adding a `NoReturn` overload isn't a good solution for expressing that (see #8566).
1699 | @overload
1700 | def pow(base: int, exp: int, mod: int) -> int: ...
     |     ^^^
1701 | @overload
1702 | def pow(base: int, exp: Literal[0], mod: None = None) -> Literal[1]: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1702:5
     |
1700 | def pow(base: int, exp: int, mod: int) -> int: ...
1701 | @overload
1702 | def pow(base: int, exp: Literal[0], mod: None = None) -> Literal[1]: ...
     |     ^^^
1703 | @overload
1704 | def pow(base: int, exp: _PositiveInteger, mod: None = None) -> int: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1704:5
     |
1702 | def pow(base: int, exp: Literal[0], mod: None = None) -> Literal[1]: ...
1703 | @overload
1704 | def pow(base: int, exp: _PositiveInteger, mod: None = None) -> int: ...
     |     ^^^
1705 | @overload
1706 | def pow(base: int, exp: _NegativeInteger, mod: None = None) -> float: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1706:5
     |
1704 | def pow(base: int, exp: _PositiveInteger, mod: None = None) -> int: ...
1705 | @overload
1706 | def pow(base: int, exp: _NegativeInteger, mod: None = None) -> float: ...
     |     ^^^
1707 |
1708 | # int base & positive-int exp -> int; int base & negative-int exp -> float
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1711:5
     |
1709 | # return type must be Any as `int | float` causes too many false-positive errors
1710 | @overload
1711 | def pow(base: int, exp: int, mod: None = None) -> Any: ...
     |     ^^^
1712 | @overload
1713 | def pow(base: _PositiveInteger, exp: float, mod: None = None) -> float: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1713:5
     |
1711 | def pow(base: int, exp: int, mod: None = None) -> Any: ...
1712 | @overload
1713 | def pow(base: _PositiveInteger, exp: float, mod: None = None) -> float: ...
     |     ^^^
1714 | @overload
1715 | def pow(base: _NegativeInteger, exp: float, mod: None = None) -> complex: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1715:5
     |
1713 | def pow(base: _PositiveInteger, exp: float, mod: None = None) -> float: ...
1714 | @overload
1715 | def pow(base: _NegativeInteger, exp: float, mod: None = None) -> complex: ...
     |     ^^^
1716 | @overload
1717 | def pow(base: float, exp: int, mod: None = None) -> float: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1717:5
     |
1715 | def pow(base: _NegativeInteger, exp: float, mod: None = None) -> complex: ...
1716 | @overload
1717 | def pow(base: float, exp: int, mod: None = None) -> float: ...
     |     ^^^
1718 |
1719 | # float base & float exp could return float or complex
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1723:5
     |
1721 | # as `float | complex` causes too many false-positive errors
1722 | @overload
1723 | def pow(base: float, exp: complex | _SupportsSomeKindOfPow, mod: None = None) -> Any: ...
     |     ^^^
1724 | @overload
1725 | def pow(base: complex, exp: complex | _SupportsSomeKindOfPow, mod: None = None) -> complex: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1725:5
     |
1723 | def pow(base: float, exp: complex | _SupportsSomeKindOfPow, mod: None = None) -> Any: ...
1724 | @overload
1725 | def pow(base: complex, exp: complex | _SupportsSomeKindOfPow, mod: None = None) -> complex: ...
     |     ^^^
1726 | @overload
1727 | def pow(base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co: ...  # type: ignore[overload-overlap]
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1727:5
     |
1725 | def pow(base: complex, exp: complex | _SupportsSomeKindOfPow, mod: None = None) -> complex: ...
1726 | @overload
1727 | def pow(base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co: ...  # type: ignore[overload-overlap]
     |     ^^^
1728 | @overload
1729 | def pow(base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co: ...  # type: ignore[overload-ov...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1729:5
     |
1727 | def pow(base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co: ...  # type: ignore[overload-overlap]
1728 | @overload
1729 | def pow(base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co: ...  # type: ignore[overload-ov...
     |     ^^^
1730 | @overload
1731 | def pow(base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1731:5
     |
1729 | def pow(base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co: ...  # type: ignore[overload-ov...
1730 | @overload
1731 | def pow(base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co: ...
     |     ^^^
1732 | @overload
1733 | def pow(base: _SupportsSomeKindOfPow, exp: float, mod: None = None) -> Any: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1733:5
     |
1731 | def pow(base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co: ...
1732 | @overload
1733 | def pow(base: _SupportsSomeKindOfPow, exp: float, mod: None = None) -> Any: ...
     |     ^^^
1734 | @overload
1735 | def pow(base: _SupportsSomeKindOfPow, exp: complex, mod: None = None) -> complex: ...
     |
info: Unmatched overload defined here
    --> stdlib/builtins.pyi:1735:5
     |
1733 | def pow(base: _SupportsSomeKindOfPow, exp: float, mod: None = None) -> Any: ...
1734 | @overload
1735 | def pow(base: _SupportsSomeKindOfPow, exp: complex, mod: None = None) -> complex: ...
     |     ^^^
1736 |
1737 | quit: _sitebuiltins.Quitter
     |
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
```

Does `pow()` have a lot of overloads? Yeah, it does. I've seen real-world functions out there in the wild with _hundreds_ of overloads, though. (Usually generated code! But still code that we need to be able to handle!)

---

_Comment by @AlexWaygood on 2025-05-13 17:59_

By comparison, here's mypy's diagnostic for the same bad `pow()` call. It's much more concise, but still tells me everything I need to know:

```
main.py:3: error: No overload variant of "pow" matches argument types "Foo", "Foo"  [call-overload]
main.py:3: note: Possible overload variants:
main.py:3: note:     def pow(base: int, exp: int, mod: int) -> int
main.py:3: note:     def pow(base: int, exp: Literal[0], mod: None = ...) -> Literal[1]
main.py:3: note:     def pow(base: int, exp: Literal[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25], mod: None = ...) -> int
main.py:3: note:     def pow(base: int, exp: Literal[-1, -2, -3, -4, -5, -6, -7, -8, -9, -10, -11, -12, -13, -14, -15, -16, -17, -18, -19, -20], mod: None = ...) -> float
main.py:3: note:     def pow(base: int, exp: int, mod: None = ...) -> Any
main.py:3: note:     def pow(base: Literal[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25], exp: float, mod: None = ...) -> float
main.py:3: note:     def pow(base: Literal[-1, -2, -3, -4, -5, -6, -7, -8, -9, -10, -11, -12, -13, -14, -15, -16, -17, -18, -19, -20], exp: float, mod: None = ...) -> complex
main.py:3: note:     def pow(base: float, exp: int, mod: None = ...) -> float
main.py:3: note:     def pow(base: float, exp: complex | _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = ...) -> Any
main.py:3: note:     def pow(base: complex, exp: complex | _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = ...) -> complex
main.py:3: note:     def [_E, _T_co] pow(base: _SupportsPow2[_E, _T_co], exp: _E, mod: None = ...) -> _T_co
main.py:3: note:     def [_E, _T_co] pow(base: _SupportsPow3NoneOnly[_E, _T_co], exp: _E, mod: None = ...) -> _T_co
main.py:3: note:     def [_E, _M, _T_co] pow(base: _SupportsPow3[_E, _M, _T_co], exp: _E, mod: _M) -> _T_co
main.py:3: note:     def pow(base: _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any], exp: float, mod: None = ...) -> Any
main.py:3: note:     def pow(base: _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any], exp: complex, mod: None = ...) -> complex
Found 1 error in 1 file (checked 1 source file)

```

---

_Comment by @MichaReiser on 2025-05-13 18:00_

What does rustc do if there's multiple possible methods to call (I think I saw this in the past). Does it use notes or sub diagnostics?

---

_Comment by @BurntSushi on 2025-05-13 18:45_

Notes are sub-diagnostics.

@AlexWaygood Those don't include the surrounding context or line numbers of the function definitions though? I think that might be swinging too far in the concise direction?

In any case, we don't have a way, today, of rendering concise single-line code snippets. One thing we could do, I think relatively easily, is add a way of specifying the context window size for code snippets. It's hard-coded to 2 lines (above and below), but we could shrink that down to 0. It won't be as concise as mypy, but it will be more concise than what we have today.

I think this PR is an improvement over the status quo. I definitely do not claim it is the best we can do though.

---

_Comment by @AlexWaygood on 2025-05-13 19:10_

> Those don't include the surrounding context or line numbers of the function definitions though? I think that might be swinging too far in the concise direction?

Why are those useful in this situation? I find them distracting, personally; I think all I care about in this situation is the types, if I'm a user. I think you can get a pretty-printed version of the signature of each overload by calling `.signature(db).display(db)` on each overload, and then that could be easily incorporated into a note; we don't necessarily need an annotated snippet of the original source code here.

---

_Comment by @sharkdp on 2025-05-13 19:15_

> we don't necessarily need an annotated snippet of the original source code here.

Two counter-arguments (even though I also partially agree with you @AlexWaygood):
* Not all overloads come from typeshed. The mistake might not be related to the call-site. The overload could be wrong as well. In this case, I might want to jump to the overload definition.
* An overload could theoretically include a (doc) comment with more information. In this case, showing the context might be useful.

---

_Comment by @AlexWaygood on 2025-05-13 19:18_

I agree that having _one_ link back to the original source code is useful, for sure. But a separate snippet for each overload feels quite excessive to me!

---

_Comment by @sharkdp on 2025-05-13 19:24_

> But a separate snippet for each overload feels quite excessive to me!

Yes, maybe. But that `pow` example also shouldn't be our target to calibrate this, I think? I actually have the opposite problem in an example I just tried. This includes too little context. I can't even see the whole function signature, because only the function name seems to belong to the snippet range.

```
error[no-matching-overload]: No overload of function `dataclass` matches arguments
  --> /home/shark/pydantic_test/main.py:12:2
   |
12 | @dataclasses.dataclass(init="no")
   |  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
13 | class Config:
14 |     name: str
   |
info: Unmatched overload defined here
  --> /home/shark/pydantic_test/.venv/lib/python3.13/site-packages/pydantic/dataclasses.py:32:9
   |
30 |     @dataclass_transform(field_specifiers=(dataclasses.field, Field, PrivateAttr))
31 |     @overload
32 |     def dataclass(
   |         ^^^^^^^^^
33 |         *,
34 |         init: Literal[False] = False,
   |
info: Unmatched overload defined here
  --> /home/shark/pydantic_test/.venv/lib/python3.13/site-packages/pydantic/dataclasses.py:49:9
   |
47 |     @dataclass_transform(field_specifiers=(dataclasses.field, Field, PrivateAttr))
48 |     @overload
49 |     def dataclass(
   |         ^^^^^^^^^
50 |         _cls: type[_T],  # type: ignore
51 |         *,
   |
info: Overload implementation defined here
   --> /home/shark/pydantic_test/.venv/lib/python3.13/site-packages/pydantic/dataclasses.py:98:5
    |
 97 | @dataclass_transform(field_specifiers=(dataclasses.field, Field, PrivateAttr))
 98 | def dataclass(
    |     ^^^^^^^^^
 99 |     _cls: type[_T] | None = None,
100 |     *,
    |
info: rule `no-matching-overload` is enabled by default
```

---

_Comment by @AlexWaygood on 2025-05-13 19:30_

> This includes too little context. I can't even see the whole function signature, because only the function name seems to belong to the snippet range.

But this would also be solved by my proposal of printing the _signature_ of each overload as a note rather than attempting to go back to the original _source code_ of each overload as an annotation, I think?

---

_@MichaReiser approved on 2025-05-13 19:57_

I think this is a huge improvement to what we have today. The only thing I would change (in this iteration) is to highlight the function up to all arguments so that the entire signature is visible 

I do agree with Alex that it would be nice to have a more compact representation. But I don't think this should be blocking for this improvement (and getting there is probably also easier because of what's implemented in this PR)


---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1119 on 2025-05-13 20:31_

nit: `function` / `function_literal` instead of `funty` ?

---

_@dhruvmanila approved on 2025-05-13 20:38_

Thank you!

Currently, there are only a few reasons for why an overload might not match - arity mismatch for the arguments passed in to the required parameters or a non-assignable type. This excludes the reasons due to `Todo` type because ty doesn't support certain features like unpacking arguments.

I'm thinking to either tackle that as part of https://github.com/astral-sh/ty/issues/104 or follow-up to that i.e., store the reason for why a certain overload is not matched. But, if you're planning to do this as a follow-up to this PR, I'm happy to just extend that when the full algorithm is implemented.  

---

_Comment by @AlexWaygood on 2025-05-13 21:27_

Here's a diff relative to this PR branch that would implement my suggestion:

```diff
diff --git a/crates/ty_python_semantic/src/types/call/bind.rs b/crates/ty_python_semantic/src/types/call/bind.rs
index 33fab5cc6a..7930b3c896 100644
--- a/crates/ty_python_semantic/src/types/call/bind.rs
+++ b/crates/ty_python_semantic/src/types/call/bind.rs
@@ -1113,34 +1113,37 @@ impl<'db> CallableBinding<'db> {
                         String::new()
                     }
                 ));
-                if let Some(overloaded_types) = self
-                    .signature_type
-                    .into_function_literal()
-                    .and_then(|funty| funty.to_overloaded(context.db()))
-                {
-                    for overload_type in &overloaded_types.overloads {
-                        if let Some((name_span, _)) =
-                            overload_type.parameter_span(context.db(), None)
+                if let Some(function) = self.signature_type.into_function_literal() {
+                    if let Some(overloaded_function) = function.to_overloaded(context.db()) {
+                        if let Some((first_overload_span, _)) =
+                            overloaded_function.overloads[0].parameter_span(context.db(), None)
+                        {
+                            let mut sub =
+                                SubDiagnostic::new(Severity::Info, "First overload defined here");
+                            sub.annotate(Annotation::primary(first_overload_span));
+                            diag.sub(sub);
+                        }
+
+                        diag.info(format_args!(
+                            "Possible overloads for function `{}`:",
+                            function.name(context.db())
+                        ));
+                        for overload in &function.signature(context.db()).overloads.overloads {
+                            diag.info(format_args!("  {}", overload.display(context.db())));
+                        }
+
+                        if let Some((name_span, _)) = overloaded_function
+                            .implementation
+                            .and_then(|funty| funty.parameter_span(context.db(), None))
                         {
                             let mut sub = SubDiagnostic::new(
                                 Severity::Info,
-                                "Unmatched overload defined here",
+                                "Overload implementation defined here",
                             );
                             sub.annotate(Annotation::primary(name_span));
                             diag.sub(sub);
                         }
                     }
-                    if let Some((name_span, _)) = overloaded_types
-                        .implementation
-                        .and_then(|funty| funty.parameter_span(context.db(), None))
-                    {
-                        let mut sub = SubDiagnostic::new(
-                            Severity::Info,
-                            "Overload implementation defined here",
-                        );
-                        sub.annotate(Annotation::primary(name_span));
-                        diag.sub(sub);
-                    }
                 }
                 if let Some(union_diag) = union_diag {
                     union_diag.add_union_context(context.db(), &mut diag);
diff --git a/crates/ty_python_semantic/src/types/signatures.rs b/crates/ty_python_semantic/src/types/signatures.rs
index 25169d3f57..182cce3eb4 100644
--- a/crates/ty_python_semantic/src/types/signatures.rs
+++ b/crates/ty_python_semantic/src/types/signatures.rs
@@ -123,7 +123,7 @@ pub(crate) struct CallableSignature<'db> {
     ///
     /// By using `SmallVec`, we avoid an extra heap allocation for the common case of a
     /// non-overloaded callable.
-    overloads: SmallVec<[Signature<'db>; 1]>,
+    pub(crate) overloads: SmallVec<[Signature<'db>; 1]>,
 }
```

Here's what the diagnostic would look like for my example with that change applied:

![image](https://github.com/user-attachments/assets/29ff390f-cd39-4a53-97a7-9e6026458f35)

And here's what the diagnostic would look like on @sharkdp's example (in release mode, without the `Todo`-type messages):

![image](https://github.com/user-attachments/assets/bf734e59-cf79-481f-84bb-2e69b912fdc9)


---

_Comment by @AlexWaygood on 2025-05-14 00:47_

> I've seen real-world functions out there in the wild with _hundreds_ of overloads, though. (Usually generated code! But still code that we need to be able to handle!)

Here's the project I was thinking of. Around 11,000 lines of overloads for a single function: https://github.com/henribru/google-api-python-client-stubs/blob/master/googleapiclient-stubs/discovery.pyi

---

_@BurntSushi reviewed on 2025-05-14 12:33_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/call/bind.rs`:1119 on 2025-05-14 12:33_

I guess `function` is okay, but it does return a `FunctionType`:

```
pub const fn into_function_literal(self) -> Option<FunctionType<'db>> {
```

I'll switch to `function`.

---

_Comment by @BurntSushi on 2025-05-14 12:38_

@dhruvmanila 

> store the reason for why a certain overload is not matched

Does this already happen as part of `errors` on `Binding`? (In my first example above, it has `BindingError::InvalidArgumentType`.)

---

_Comment by @BurntSushi on 2025-05-14 13:47_

@AlexWaygood Thank you! I ended up taking your patch. I'm still a little worried about printing reformatted code, but given that it seems like "tons of overloads" is not altogether uncommon, maybe it does make sense to prioritize concision here.

I did also tweak the spans (on the implementation and the first unmatched overload) to cover the entire parameter list, which should at least give good context to address @sharkdp's and @MichaReiser's concerns.

I've also added snapshot tests to cover these cases (`pow` and something resembling @sharkdp's use case, i.e., a function with a lot of parameters).

As for @dhruvmanila:

> store the reason for why a certain overload is not matched

I think there are two challenges with surfacing this information at present. The first is information display. If there are a lot of unmatched overloads, then how do we preserve conciseness while also showing the _reason_ why each overload didn't match? That seems tricky. The other challenge here, I think, is how to match up the overload bindings with the overload types returned by `FunctionType::to_overloaded`. (But you might know how to solve that challenge. It just isn't obvious to me.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/no_matching_overload…_-_No_matching_overload…_-_Call_to_function_wit…_(dd80c593d9136f35).snap`:37 on 2025-05-14 13:56_

nit: this would look more natural to me if the underline covered the function name too

```suggestion
1700 | def pow(base: int, exp: int, mod: int) -> int: ...
     |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 13:57_

we should maybe stop after 30 or something and print a note saying `<number> overloads omitted`? So that we don't print 3000 overloads if there actually are 3000 overloads?

---

_@AlexWaygood approved on 2025-05-14 13:58_

Thanks you -- this looks awesome!

---

_Comment by @BurntSushi on 2025-05-14 13:59_

I switched the snapshot test from using `pow` to something more custom, since it seems like some of the `pow` signatures get `Todo` types in them depending on whether `debug_assertions` are enabled. (Which makes snapshotting annoying.)

---

_@BurntSushi reviewed on 2025-05-14 14:12_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 14:12_

Yeah I was wondering about this. I think that seems sensible.

---

_@AlexWaygood reviewed on 2025-05-14 14:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 14:15_

I don't know what a reasonable cutoff is. 30? 50? 100? It probably shouldn't be more than 100 -- I don't know why you'd want more than 100 signatures printed to the terminal. The cutoff number is easy to iterate on, though, so it's probably fine to pick an arbitrary number now and add a comment saying that it's arbitrary and we can change it if we need to

---

_@BurntSushi reviewed on 2025-05-14 14:15_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/no_matching_overload…_-_No_matching_overload…_-_Call_to_function_wit…_(dd80c593d9136f35).snap`:37 on 2025-05-14 14:15_

I don't see a way to get a single span covering both of those. At least, it's not `StmtFunctionDef`: https://github.com/astral-sh/ruff/blob/97d7b4693684753139b15f86bc796df9ee25e76e/crates/ruff_python_ast/src/generated.rs#L6606-L6616

I can annotate _both_ the name and the parameter list, which will probably look fine in most cases, but could be a little weird if and when there is spacing between the name and the start of the parameter list.

---

_@BurntSushi reviewed on 2025-05-14 14:19_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 14:19_

I'm not sure what the right number here is, but I feel like 300 is excessive. I think I'd maybe start at... 50? I don't know if 50 is the right number precisely, but it's hard to imagine someone carefully scrutinizing a list much bigger than that.

---

_@AlexWaygood reviewed on 2025-05-14 14:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/no_matching_overload…_-_No_matching_overload…_-_Call_to_function_wit…_(dd80c593d9136f35).snap`:37 on 2025-05-14 14:19_

can't you just do something similar to what we do to get the "header span" for classes?

https://github.com/astral-sh/ruff/blob/97d7b4693684753139b15f86bc796df9ee25e76e/crates/ty_python_semantic/src/types/class.rs#L1835-L1862

---

_@MichaReiser reviewed on 2025-05-14 14:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/no_matching_overload…_-_No_matching_overload…_-_Call_to_function_wit…_(dd80c593d9136f35).snap`:37 on 2025-05-14 14:19_

You need to construct a new Text range starting at `name.start` and ending at `parameters.end`

---

_@AlexWaygood reviewed on 2025-05-14 14:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 14:21_

SGTM

---

_@AlexWaygood reviewed on 2025-05-14 14:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 14:21_

we link back to the source for the overloads in the annotation that shows you the first overload, so if you're a user and you need to look at the 51st overload it shouldn't be too hard to find it

---

_@BurntSushi reviewed on 2025-05-14 14:22_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/no_matching_overload…_-_No_matching_overload…_-_Call_to_function_wit…_(dd80c593d9136f35).snap`:37 on 2025-05-14 14:22_

I guess that works. I'm always cautious about stitching together spans, but I think it works here.

---

_@BurntSushi reviewed on 2025-05-14 14:29_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/call/bind.rs`:1144 on 2025-05-14 14:29_

(Also, sorry, if my comments look like they ignore more recent comments from you. GitHub used to add updated comments automatically, but now it seems to require a hard refresh of the page for me.)

---

_@AlexWaygood approved on 2025-05-14 15:11_

Awesome work, thank you!!

---

_Merged by @BurntSushi on 2025-05-14 15:13_

---

_Closed by @BurntSushi on 2025-05-14 15:13_

---

_Branch deleted on 2025-05-14 15:13_

---

_Comment by @dhruvmanila on 2025-05-14 19:31_

> I think there are two challenges with surfacing this information at present. The first is information display. If there are a lot of unmatched overloads, then how do we preserve conciseness while also showing the _reason_ why each overload didn't match? That seems tricky.

Yeah, this is tricky. I haven't thought about how much information should be displayed for this case. One way would be to group overloads based on the reason. For example, if there are multiple overloads which are not matched because of arity mismatch, they can be grouped under a single sub-diagnostic. We can then have sub-diagnostics for each reason and each of those sub-diagnostics would indicate the overloads that have been excluded for that specific reason. Does that make sense?

There are still open questions for the above approach like should we group multiple overloads which have invalid argument type at different positions and if so, how do we highlight the arguments in each of those overloads.

If this isn't a feasible solution, we could move back to highlighting each of the overloads like in the first version of this PR and limit the number of overloads that are displayed. The current limit is 50 but that seems a lot as well if we display the code frame as well.

> The other challenge here, I think, is how to match up the overload bindings with the overload types returned by `FunctionType::to_overloaded`. (But you might know how to solve that challenge. It just isn't obvious to me.)

I'm not exactly sure what do you mean here. Can you say more?

---

_Comment by @AlexWaygood on 2025-05-14 19:41_

> Fixes #274

@BurntSushi -- we need to include the full link to the GitHub issue now that the issues are in a different repo ("Fixes https://github.com/astral-sh/ty/issues/274" rather than "Fixes #274")

---

_Comment by @BurntSushi on 2025-05-14 21:24_

> > The other challenge here, I think, is how to match up the overload bindings with the overload types returned by `FunctionType::to_overloaded`. (But you might know how to solve that challenge. It just isn't obvious to me.)
> 
> I'm not exactly sure what do you mean here. Can you say more?

There is a sequence of [overload bindings](https://github.com/astral-sh/ruff/blob/466021d5e1793ca30e0d860cb1cd27e32f5233aa/crates/ty_python_semantic/src/types/call/bind.rs#L936-L940) and also a sequence of [overload types](https://github.com/astral-sh/ruff/blob/466021d5e1793ca30e0d860cb1cd27e32f5233aa/crates/ty_python_semantic/src/types.rs#L6558-L6559). They _might_ be in correspondence, but it's unclear to me if that's true. (And maybe there is another way to match them up. This could very well be a problem of my own ignorance here!)

---

_Comment by @dhruvmanila on 2025-05-14 22:16_

> There is a sequence of [overload bindings](https://github.com/astral-sh/ruff/blob/466021d5e1793ca30e0d860cb1cd27e32f5233aa/crates/ty_python_semantic/src/types/call/bind.rs#L936-L940) and also a sequence of [overload types](https://github.com/astral-sh/ruff/blob/466021d5e1793ca30e0d860cb1cd27e32f5233aa/crates/ty_python_semantic/src/types.rs#L6558-L6559). They _might_ be in correspondence, but it's unclear to me if that's true.

Thank you for the reference. Yes, I think those should be in correspondence. So, every overload should create a corresponding `Binding` while the `Binding::errors` field would signal whether the binding succeeded or not. (cc @dcreager who has more context on the call semantics.)

---
