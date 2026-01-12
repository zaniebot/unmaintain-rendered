```yaml
number: 22154
title: "[ty] Offer string literal completion suggestions in function calls, annotated assignments, and TypedDict contexts"
type: pull_request
state: open
author: Minibrams
labels:
  - server
  - ty
assignees: []
base: main
head: string-literal-completions
created_at: 2025-12-23T02:15:11Z
updated_at: 2026-01-12T17:25:36Z
url: https://github.com/astral-sh/ruff/pull/22154
synced_at: 2026-01-12T18:23:35Z
```

# [ty] Offer string literal completion suggestions in function calls, annotated assignments, and TypedDict contexts

---

_@Minibrams_

## Summary

This PR adds completion logic to the `ty` language server to offer string literal completions when the user is typing a string literal in a function call (arg or kwarg), in the rhs of an annotated assignment, or as a key in a TypedDict-compatible object.

```python
from typing import Literal, TypedDict

class TD(TypedDict):
    left: int
    right: str

type A = Literal['a', 'b', 'c']

def func(a: A):
     pass

func("")   # Completions offered here
v: A = ""  # Completions offered here

td: TD = {"left": 1, "right": "x"}
td[""]  # Completions offered here
```

This functionality addresses two of the items in the [`ty` Code completion plan](https://github.com/astral-sh/ty/issues/86).

Fixes https://github.com/astral-sh/ty/issues/2189

## Test Plan

Four e2e tests have been added to `./crates/ty_server/tests/e2e/completions.rs`:

```rust
fn string_literal_completions_for_calls() ...
fn string_literal_completions_for_typed_assignment() ...
fn string_literal_completions_filter_non_strings() ...
fn string_literal_completions_for_typed_dict_subscript_keys() ...
```


---

_Review requested from @carljm by @Minibrams on 2025-12-23 02:15_

---

_Review requested from @AlexWaygood by @Minibrams on 2025-12-23 02:15_

---

_Review requested from @sharkdp by @Minibrams on 2025-12-23 02:15_

---

_Review requested from @dcreager by @Minibrams on 2025-12-23 02:15_

---

_Review requested from @MichaReiser by @Minibrams on 2025-12-23 02:15_

---

_Review request for @carljm removed by @carljm on 2025-12-23 03:15_

---

_Label `server` added by @AlexWaygood on 2025-12-23 09:25_

---

_Label `ty` added by @AlexWaygood on 2025-12-23 09:25_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-12-23 09:25_

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 09:27_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-23 09:28_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1837 diagnostics
+ Found 1840 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5167 diagnostics
+ Found 5170 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~49MB
+     memo fields = ~52MB

trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~20MB
+     struct fields = ~21MB
-     memo metadata = ~73MB
+     memo metadata = ~76MB
-     memo fields = ~176MB
+     memo fields = ~185MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~725MB
-     memo metadata = ~167MB
+     memo metadata = ~176MB
-     memo fields = ~424MB
+     memo fields = ~445MB


```

</details>




---

_Closed by @AlexWaygood on 2025-12-23 09:40_

---

_Reopened by @AlexWaygood on 2025-12-23 09:40_

---

_Comment by @AlexWaygood on 2025-12-23 11:14_

> Three e2e tests have been added to `./crates/ty_server/tests/e2e/completions.rs`:

most of our completions tests are in `crates/ty_ide/completions.rs` -- could you say a bit more about why you chose to use end-to-end server tests for these?

---

_Comment by @AlexWaygood on 2025-12-23 11:15_

(Thanks for the PR! 😃)

---

_Comment by @Minibrams on 2025-12-23 11:15_

Found time to add completions for TypedDict keys as well, so went ahead and added it. I'll leave it alone for review now 😄 

---

_Comment by @Minibrams on 2025-12-23 11:22_

> > Three e2e tests have been added to `./crates/ty_server/tests/e2e/completions.rs`:
> 
> most of our completions tests are in `crates/ty_ide/completions.rs` -- could you say a bit more about why you chose to use end-to-end server tests for these?

No particularly good reason, it just seemed like the natural place to put them 😄 Wasn't sure what the scope of the tests in crates/ty_ide/completions.rs was, but was sure I wanted to write e2e tests, so there they went. I tend to prefer e2e tests as I find they capture the important bits of functionality more naturally than typical unit tests do.

But I see some of the tests in crates/ty_ide/completions.rs practically do the same thing but with less setup so I can put them in there instead no problem if we prefer.

---

_Comment by @AlexWaygood on 2025-12-23 12:38_

If there's no specific reason these tests need the full e2e framework, I'd be inclined to put them with most of our other completions tests in `completions.rs`.

---

_Renamed from "[ty] Offer string literal completion suggestions in function calls, a…" to "[ty] Offer string literal completion suggestions in function calls, annotated assignments, and TypedDict contexts" by @AlexWaygood on 2025-12-23 18:07_

---

_Comment by @Minibrams on 2025-12-23 22:33_

> If there's no specific reason these tests need the full e2e framework, I'd be inclined to put them with most of our other completions tests in `completions.rs`.

Done

---

_@AlexWaygood reviewed on 2025-12-24 11:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14509 on 2025-12-24 11:26_

I would just make the `value` getter public rather than adding an `as_str` method that does exactly the same thing:

```diff
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index c211871e1c..a74a811750 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -1334,7 +1334,7 @@ fn collect_string_literals_from_type<'db>(
 
     match ty {
         Type::StringLiteral(literal) => {
-            out.entry(literal.as_str(db)).or_insert(literal);
+            out.entry(literal.value(db)).or_insert(literal);
         }
         Type::Union(union) => {
             for ty in union.elements(db) {
@@ -1368,7 +1368,7 @@ fn collect_typed_dict_literals_from_type<'db>(
             for (name, _) in typed_dict.items(db) {
                 let name = name.as_str();
                 let literal = StringLiteralType::new(db, name);
-                out.entry(literal.as_str(db)).or_insert(literal);
+                out.entry(literal.value(db)).or_insert(literal);
             }
         }
         Type::Union(union) => {
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 8d96bf255c..77337cd520 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -14491,7 +14491,7 @@ impl<'db> IntersectionType<'db> {
 #[derive(PartialOrd, Ord)]
 pub struct StringLiteralType<'db> {
     #[returns(deref)]
-    value: CompactString,
+    pub value: CompactString,
 }
 
 // The Salsa heap is tracked separately.
@@ -14502,11 +14502,6 @@ impl<'db> StringLiteralType<'db> {
     pub(crate) fn python_len(self, db: &'db dyn Db) -> usize {
         self.value(db).chars().count()
     }
-
-    /// Returns the contents of this string literal.
-    pub fn as_str(self, db: &'db dyn Db) -> &'db str {
-        self.value(db)
-    }
 }
```

---

_@Minibrams reviewed on 2025-12-24 12:08_

---

_Review comment by @Minibrams on `crates/ty_python_semantic/src/types.rs`:14509 on 2025-12-24 12:08_

Fixed in 19a4b2b222d9bcfdc024340dc63be4658d6bb6e1

---

_Comment by @AlexWaygood on 2026-01-02 13:49_

(Sorry for the slow review here, most of us have been on leave over the Christmas period! Normal Astral review response times should resume from the beginning of next week 😄)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:659 on 2026-01-06 13:28_

I would do `if let Some(literal) = cursor.string_literal() {` here. That would avoid calling `cursor.is_in_string()` twice.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:745 on 2026-01-06 13:33_

```suggestion
        let covering = self.covering_node(range);
```

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:743 on 2026-01-06 13:54_

I'm curious about the approach here. It seems fraught to need to enumerate all the possible places where the type of a string literal is known. For example:

```python
from typing import Literal
type A = Literal["foo", "bar", "baz"]
xs: list[A] = ["<CURSOR>"]
```

I don't mean to suggest that you should add the above case here, but rather, is there a more general approach we can use here? This approach gets further complicated below where there is a lot logic dedicated to each particular instance. This will also only grow as more sources of string literals are supported.

It seems like we want to offer these sorts of completions _anywhere_ a string literal is found. Can we find the type of a string literal more generally and then offer completions from there? It seems like that should be possible.

(Maybe I'm wrong about this. @AlexWaygood might be a good person to chime in here.)

---

_@BurntSushi reviewed on 2026-01-06 13:54_

---

_@Minibrams reviewed on 2026-01-10 06:38_

---

_Review comment by @Minibrams on `crates/ty_ide/src/completion.rs`:743 on 2026-01-10 06:38_

That would definitely be a more robust approach, do we have any `expected_type(expr)` or similar API in the codebase currently? If not I can take a shot at one later today.

---

_Review requested from @Gankra by @Minibrams on 2026-01-10 06:42_

---

_@Minibrams reviewed on 2026-01-10 07:41_

---

_Review comment by @Minibrams on `crates/ty_ide/src/completion.rs`:659 on 2026-01-10 07:41_

This breaks the incomplete t-string tests like `no_completions_in_tstring_incomplete_double_quote` since there the cursor is inside a string token, but there's no ExprStringLiteral node yet (which `cursor.string_literal()` requires)

---

_@Minibrams reviewed on 2026-01-10 11:34_

---

_Review comment by @Minibrams on `crates/ty_ide/src/completion.rs`:743 on 2026-01-10 11:34_

I have added a `HasExpectedType` API in `SemanticModel` with a `expected_type()` function that returns the contextual type annotation (if any), which `add_string_literal_completions()` now uses to offer completions anywhere the expected type of an expression is a string literal.

I also added your list-of-literals example as a test-case.

---

_Comment by @codspeed-hq[bot] on 2026-01-10 11:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 19.45%**

<sub>Comparing <code>Minibrams:string-literal-completions</code> (15fd69d) with <code>main</code> (11cc324)</sub>



### Summary

`❌ 5` regressed benchmarks  
`✅ 18` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 86.6 s | 106 s | -18.28% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 62.3 s | 75 s | -16.88% |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 50.5 s | 53 s | -4.66% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21 s | 26.1 s | -19.45% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.8 s | 8.8 s | -11.76% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/Minibrams%3Astring-literal-completions?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@Minibrams reviewed on 2026-01-10 14:04_

---

_Review comment by @Minibrams on `crates/ty_ide/src/completion.rs`:743 on 2026-01-10 14:04_

Oof, 20% performance hit. Happy to hear ideas on how to optimize. Maybe we could lazyload expected types?

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:659 on 2026-01-12 14:19_

I'm not following. If there's no `ExprStringLiteral`, then doesn't `cursor.string_literal()` return `None` and thus this function bails and no completions are provided?

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:375 on 2026-01-12 14:21_

I think this should go away. I'm guessing it slipped in via a bad merge or something.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:659 on 2026-01-12 14:24_

Basically, `string_literal()` starts by bailing when `cursor.is_in_string()` is `false`. But the _only_ call to `string_literal()` is guarded by the condition that `cursor.is_in_string()` is `true`. I think there should be a simplification here?

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1397 on 2026-01-12 14:41_

I think this should be removed? It's available at `cursor.parsed` anyway.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1467 on 2026-01-12 14:45_

It seems like you could accept `QuoteStyle` here and just do `QuoteStyle::as_char()`. And then delete the case analysis above.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1425 on 2026-01-12 14:47_

If we're just going to create a `Box<str>` anyway, then why have `escape_for_quote` return a `Cow`?

Even better, probably just have `escape_for_quote` return a `Name`, since that's ultimately what we want here.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1467 on 2026-01-12 14:51_

Also, please add a contract to this function.

It took me a minute to figure out why this was needed, so saying more about that would help too.

Also, from a quick glance, I don't see tests that exercise a code path that uses this.

Finally, are we sure a routine like this doesn't already exist? It looks like maybe there is something in `ruff_python_literal`. And if not, perhaps this routine should live there. cc @MichaReiser 

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1421 on 2026-01-12 14:54_

Hmmm. Somewhat recently, I changed the completion display to prioritize the actual insertion text:

https://github.com/astral-sh/ruff/blob/1094009790a799e8274e4088158c99c5273bd891/crates/ty_server/src/server/api/requests/completion.rs#L84

But I think this implies that this will show escape quotes to the human when that really should be hidden.

I guess this means that we probably _shouldn't_ show the insertion text in completions and instead have more sophistication around just improving `name` instead.

I'd be fine ignoring this for now and creating an issue when this PR gets merged tracking a fix for this.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1462 on 2026-01-12 14:57_

My understanding is that if we're doing recursion based on the structure of `Type`, then we need cycle detection. e.g.,

https://github.com/astral-sh/ruff/blob/1094009790a799e8274e4088158c99c5273bd891/crates/ty_ide/src/completion.rs#L2331-L2387

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1458 on 2026-01-12 14:58_

Should this only add values present in all branches of the intersection?

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1464 on 2026-01-12 14:59_

Can you add tests the cover each of these cases?

---

_@BurntSushi reviewed on 2026-01-12 15:03_

Thank you for working on this! I feel like the new approach here is probably the right way to go about this instead of trying to write bespoke code for each type of string literal. I left a number of comments specifically on the completion side of things here.

I'm less well equipped to review the changes for adding `HasExpectedType` though. It might make sense to land that in a separate PR and get review from probably @AlexWaygood. But I'll defer to Alex to whether a separate PR makes sense.

---

_Review request for @sharkdp removed by @BurntSushi on 2026-01-12 15:09_

---

_Review request for @dcreager removed by @BurntSushi on 2026-01-12 15:09_

---

_Comment by @MichaReiser on 2026-01-12 17:25_

Can either of you say more about what `ExpectedType` is used for and what we store, ideally with an example?

---
