```yaml
number: 19784
title: "[ty] `typing.Self` is bound by the method, not the class"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/self-binding-context
created_at: 2025-08-06T13:26:04Z
updated_at: 2025-08-06T21:26:20Z
url: https://github.com/astral-sh/ruff/pull/19784
synced_at: 2026-01-12T15:56:47Z
```

# [ty] `typing.Self` is bound by the method, not the class

---

_@dcreager_

This fixes our logic for binding a legacy typevar with its binding context. (To recap, a legacy typevar starts out "unbound" when it is first created, and each time it's used in a generic class or function, we "bind" it with the corresponding `Definition`.)

We treat `typing.Self` the same as a legacy typevar, and so we apply this binding logic to it too. Before, we were using the enclosing class as its binding context. But that's not correct — it's the method where `typing.Self` is used that binds the typevar. (Each invocation of the method will find a new specialization of `Self` based on the specific instance type containing the invoked method.)

This required plumbing through some additional state to the `in_type_expression` method.

This also revealed that we weren't handling `Self`-typed instance attributes correctly (but were coincidentally not getting the expected false positive diagnostics).

---

_Review requested from @carljm by @dcreager on 2025-08-06 13:26_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-06 13:26_

---

_Review requested from @sharkdp by @dcreager on 2025-08-06 13:26_

---

_Label `ty` added by @dcreager on 2025-08-06 13:26_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:110 on 2025-08-06 13:27_

We already have good support for properties, so I would suggest opening up a `help wanted` issue to tackle this as a follow-on

---

_@dcreager reviewed on 2025-08-06 13:27_

---

_Comment by @github-actions[bot] on 2025-08-06 13:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-06 19:18:54.969213042 +0000
+++ new-output.txt	2025-08-06 19:18:55.034213160 +0000
@@ -1,7 +1,7 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-_directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Spam`
+_directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
@@ -188,7 +188,7 @@
 constructors_call_new.py:113:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class8[list[T@Class8]]`
 constructors_call_new.py:116:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[int]]`
 constructors_call_new.py:117:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[str]]`
-constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Class9`
+constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 constructors_call_new.py:140:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class11[int]`
 constructors_call_type.py:40:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of function `__new__`
 constructors_call_type.py:50:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of bound method `__init__`
@@ -197,7 +197,7 @@
 constructors_callable.py:37:1: error[type-assertion-failure] Argument does not have asserted type `Class1`
 constructors_callable.py:49:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:50:1: error[type-assertion-failure] Argument does not have asserted type `Class2`
-constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Class3`
+constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:64:1: error[type-assertion-failure] Argument does not have asserted type `Class3`
 constructors_callable.py:73:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -432,28 +432,28 @@
 generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:42:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:43:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
-generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@ParentA`
+generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@prop1`
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
-generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@ParentB`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@ChildB]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@ChildB]`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `Self@LinkedList | None`, found `LinkedList[int]`
-generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `Self@LinkedList | None`
-generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `Self@LinkedList | None`
-generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@Shape`
-generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Shape`, found `Shape`
-generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Shape`, found `Shape`
+generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
+generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self]`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self`
+generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self`
+generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self]`
+generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self`
+generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self`
+generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `Self | None`, found `LinkedList[int]`
+generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `Self | None`
+generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `Self | None`
+generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale`
+generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
+generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
 generics_self_basic.py:51:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
 generics_self_basic.py:52:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
 generics_self_basic.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
 generics_self_basic.py:55:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
-generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Container`
+generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@set_value`
 generics_self_basic.py:67:26: error[invalid-type-form] Special form `typing.Self` expected no type parameter
 generics_self_basic.py:74:5: error[type-assertion-failure] Argument does not have asserted type `Container[int]`
 generics_self_basic.py:75:5: error[type-assertion-failure] Argument does not have asserted type `Container[str]`
@@ -462,16 +462,16 @@
 generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:76:6: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:82:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Foo2`
-generics_self_usage.py:86:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Foo3`, found `Foo3`
+generics_self_usage.py:82:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@has_existing_self_annotation`
+generics_self_usage.py:86:16: error[invalid-return-type] Return type does not match returned value: expected `Self@return_concrete_type`, found `Foo3`
 generics_self_usage.py:98:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@Bar`
 generics_self_usage.py:101:15: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:103:12: error[invalid-base] Invalid class base with type `typing.Self`
 generics_self_usage.py:106:30: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:111:19: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Base`
-generics_self_usage.py:116:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Base`
-generics_self_usage.py:121:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@MyMetaclass`
-generics_self_usage.py:125:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[Self@MyMetaclass]`
+generics_self_usage.py:111:19: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@make`
+generics_self_usage.py:116:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@return_parameter`
+generics_self_usage.py:121:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
+generics_self_usage.py:125:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[Self@__mul__]`
 generics_syntax_compatibility.py:23:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `V@ClassC | K@method1`
 generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M@method2 | K`
 generics_syntax_declarations.py:17:1: error[invalid-generic-class] Cannot both inherit from `typing.Generic` and use PEP 695 type variables
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-06 13:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
- expression/collections/seq.py:444:24: error[invalid-argument-type] Argument is incorrect: Expected `_TSource@choose`, found `_TSource@mapper`
- Found 229 diagnostics
+ Found 228 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/filters.py:1352:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `V@do_sum` and `Any | V@do_sum`
- Found 190 diagnostics
+ Found 191 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@UpdateDictMixin`, found `UpdateDictMixin[Any, Any]`
+ src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `UpdateDictMixin[Any, Any]`
+ src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `Self@setdefault`
- src/werkzeug/datastructures/mixins.py:278:18: error[invalid-super-argument] `Self@UpdateDictMixin` is not an instance or subclass of `<class 'UpdateDictMixin'>` in `super(<class 'UpdateDictMixin'>, Self@UpdateDictMixin)` call
+ src/werkzeug/datastructures/mixins.py:278:18: error[invalid-super-argument] `Self@pop` is not an instance or subclass of `<class 'UpdateDictMixin'>` in `super(<class 'UpdateDictMixin'>, Self@pop)` call
+ src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `Self@pop`
+ src/werkzeug/datastructures/structures.py:1104:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@remove`
+ src/werkzeug/datastructures/structures.py:1119:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@update`
+ src/werkzeug/datastructures/structures.py:1159:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@clear`
+ src/werkzeug/datastructures/structures.py:1185:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@__delitem__`
+ src/werkzeug/datastructures/structures.py:1193:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@__setitem__`
- Found 367 diagnostics
+ Found 374 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/client.py:342:16: error[invalid-return-type] Return type does not match returned value: expected `ConnectionState[Self@Client]`, found `ConnectionState[Client]`
+ discord/client.py:342:16: error[invalid-return-type] Return type does not match returned value: expected `ConnectionState[Self@_get_state]`, found `ConnectionState[Client]`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/wrapper/_implementations.py:1578:20: error[invalid-return-type] Return type does not match returned value: expected `F@__call__ | Self@ZenStore`, found `ZenStore`
+ src/hydra_zen/wrapper/_implementations.py:1578:20: error[invalid-return-type] Return type does not match returned value: expected `F@__call__ | Self@__call__`, found `ZenStore`
- src/hydra_zen/wrapper/_implementations.py:1638:20: error[invalid-return-type] Return type does not match returned value: expected `F@__call__ | Self@ZenStore`, found `F | Self@ZenStore`
+ src/hydra_zen/wrapper/_implementations.py:1638:20: error[invalid-return-type] Return type does not match returned value: expected `F@__call__ | Self@__call__`, found `F | Self@__call__`

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/elements/dialog_decorator.py:292:16: error[invalid-return-type] Return type does not match returned value: expected `F@dialog_decorator | ((F@dialog_decorator, /) -> F@dialog_decorator)`, found `def wrapper[F: (...) -> Any](f: F@wrapper) -> F@wrapper`
+ lib/streamlit/elements/dialog_decorator.py:292:16: error[invalid-return-type] Return type does not match returned value: expected `F@dialog_decorator | ((F@dialog_decorator, /) -> F@dialog_decorator)`, found `def wrapper(f: F@dialog_decorator) -> F@dialog_decorator`
- lib/streamlit/elements/dialog_decorator.py:294:5: error[invalid-assignment] Object of type `F@dialog_decorator & ~str` is not assignable to `F`
+ lib/streamlit/elements/dialog_decorator.py:294:5: error[invalid-assignment] Object of type `F@dialog_decorator & ~str` is not assignable to `F@dialog_decorator`
- lib/streamlit/elements/dialog_decorator.py:333:16: error[invalid-return-type] Return type does not match returned value: expected `F@experimental_dialog_decorator | ((F@experimental_dialog_decorator, /) -> F@experimental_dialog_decorator)`, found `def wrapper[F: (...) -> Any](f: F@wrapper) -> F@wrapper`
+ lib/streamlit/elements/dialog_decorator.py:333:16: error[invalid-return-type] Return type does not match returned value: expected `F@experimental_dialog_decorator | ((F@experimental_dialog_decorator, /) -> F@experimental_dialog_decorator)`, found `def wrapper(f: F@experimental_dialog_decorator) -> F@experimental_dialog_decorator`
- lib/streamlit/elements/dialog_decorator.py:335:5: error[invalid-assignment] Object of type `F@experimental_dialog_decorator & ~str` is not assignable to `F`
+ lib/streamlit/elements/dialog_decorator.py:335:5: error[invalid-assignment] Object of type `F@experimental_dialog_decorator & ~str` is not assignable to `F@experimental_dialog_decorator`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/_internal/websockets.py:134:9: error[invalid-super-argument] `Self@WebsocketProxyConnect` is not an instance or subclass of `<class 'WebsocketProxyConnect'>` in `super(<class 'WebsocketProxyConnect'>, Self@WebsocketProxyConnect)` call
+ src/prefect/_internal/websockets.py:134:9: error[invalid-super-argument] `Self@_proxy_connect` is not an instance or subclass of `<class 'WebsocketProxyConnect'>` in `super(<class 'WebsocketProxyConnect'>, Self@_proxy_connect)` call
- src/prefect/context.py:199:15: error[invalid-super-argument] `Self@ContextModel` is not an instance or subclass of `<class 'ContextModel'>` in `super(<class 'ContextModel'>, Self@ContextModel)` call
+ src/prefect/context.py:199:15: error[invalid-super-argument] `Self@model_copy` is not an instance or subclass of `<class 'ContextModel'>` in `super(<class 'ContextModel'>, Self@model_copy)` call
- src/prefect/context.py:308:20: error[invalid-super-argument] `Self@AsyncClientContext` is not an instance or subclass of `<class 'AsyncClientContext'>` in `super(<class 'AsyncClientContext'>, Self@AsyncClientContext)` call
+ src/prefect/context.py:308:20: error[invalid-super-argument] `Self@__aenter__` is not an instance or subclass of `<class 'AsyncClientContext'>` in `super(<class 'AsyncClientContext'>, Self@__aenter__)` call
- src/prefect/context.py:316:20: error[invalid-super-argument] `Self@AsyncClientContext` is not an instance or subclass of `<class 'AsyncClientContext'>` in `super(<class 'AsyncClientContext'>, Self@AsyncClientContext)` call
+ src/prefect/context.py:316:20: error[invalid-super-argument] `Self@__aexit__` is not an instance or subclass of `<class 'AsyncClientContext'>` in `super(<class 'AsyncClientContext'>, Self@__aexit__)` call
- src/prefect/utilities/templating.py:425:24: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `T`
- Found 3841 diagnostics
+ Found 3840 diagnostics

zulip (https://github.com/zulip/zulip)
- analytics/lib/counts.py:83:12: error[unresolved-attribute] Type `Self@Model` has no attribute `state`
+ analytics/lib/counts.py:83:12: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:84:20: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/lib/counts.py:84:20: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:85:16: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/lib/counts.py:85:16: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:164:10: error[unresolved-attribute] Type `Self@Model` has no attribute `state`
+ analytics/lib/counts.py:164:10: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:165:56: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/lib/counts.py:165:56: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:166:40: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/lib/counts.py:166:40: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:167:28: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/lib/counts.py:167:28: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:168:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self@Model`
+ analytics/lib/counts.py:168:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self`
- analytics/lib/counts.py:170:10: error[unresolved-attribute] Type `Self@Model` has no attribute `state`
+ analytics/lib/counts.py:170:10: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:171:28: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/lib/counts.py:171:28: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:173:68: error[unresolved-attribute] Type `Self@Model` has no attribute `state`
+ analytics/lib/counts.py:173:68: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:189:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self@Model`
+ analytics/lib/counts.py:189:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self`
- analytics/lib/counts.py:191:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self@Model`
+ analytics/lib/counts.py:191:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self`
- analytics/management/commands/populate_analytics_db.py:124:54: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:124:54: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:125:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self@Model`.
+ analytics/management/commands/populate_analytics_db.py:125:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self`.
- analytics/management/commands/populate_analytics_db.py:131:13: error[invalid-argument-type] Argument to function `create_stream_subscription` is incorrect: Expected `Recipient`, found `Self@Model`
+ analytics/management/commands/populate_analytics_db.py:131:13: error[invalid-argument-type] Argument to function `create_stream_subscription` is incorrect: Expected `Recipient`, found `Self`
- analytics/management/commands/populate_analytics_db.py:132:13: error[invalid-argument-type] Argument to function `create_stream_subscription` is incorrect: Expected `Stream`, found `Self@Model`
+ analytics/management/commands/populate_analytics_db.py:132:13: error[invalid-argument-type] Argument to function `create_stream_subscription` is incorrect: Expected `Stream`, found `Self`
- analytics/management/commands/populate_analytics_db.py:292:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:292:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:293:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:293:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:297:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:297:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:298:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:298:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:299:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:299:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:300:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:300:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:301:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:301:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:302:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:302:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:303:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:303:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:304:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:304:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:305:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:305:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:306:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:306:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:310:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:310:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:311:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:311:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:312:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:312:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:313:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:313:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:314:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:314:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:315:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:315:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:316:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:316:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:317:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:317:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:318:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:318:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:319:13: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/management/commands/populate_analytics_db.py:319:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:163:54: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_counts.py:163:54: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:164:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self@Model`.
+ analytics/tests/test_counts.py:164:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self`.
- analytics/tests/test_counts.py:166:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Stream, Recipient]`, found `tuple[Self@Model, Self@Model]`
+ analytics/tests/test_counts.py:166:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Stream, Recipient]`, found `tuple[Self, Self]`
- analytics/tests/test_counts.py:177:21: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_counts.py:177:21: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:179:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self@Model`.
+ analytics/tests/test_counts.py:179:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self`.
- analytics/tests/test_counts.py:181:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DirectMessageGroup, Recipient]`, found `tuple[Self@Model, Self@Model]`
+ analytics/tests/test_counts.py:181:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DirectMessageGroup, Recipient]`, found `tuple[Self, Self]`
- analytics/tests/test_counts.py:199:16: error[invalid-return-type] Return type does not match returned value: expected `Message`, found `Self@Model`
+ analytics/tests/test_counts.py:199:16: error[invalid-return-type] Return type does not match returned value: expected `Message`, found `Self`
- analytics/tests/test_counts.py:209:16: error[invalid-return-type] Return type does not match returned value: expected `Attachment`, found `Self@Model`
+ analytics/tests/test_counts.py:209:16: error[invalid-return-type] Return type does not match returned value: expected `Attachment`, found `Self`
- analytics/tests/test_counts.py:287:26: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ analytics/tests/test_counts.py:287:26: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/tests/test_counts.py:288:26: error[unresolved-attribute] Type `Self@Model` has no attribute `state`
+ analytics/tests/test_counts.py:288:26: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/tests/test_counts.py:870:26: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_counts.py:870:26: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:918:26: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_counts.py:918:26: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:1257:9: error[unresolved-attribute] Unresolved attribute `state` on type `Self@Model`.
+ analytics/tests/test_counts.py:1257:9: error[unresolved-attribute] Unresolved attribute `state` on type `Self`.
- analytics/tests/test_counts.py:1263:9: error[unresolved-attribute] Unresolved attribute `property` on type `Self@Model`.
+ analytics/tests/test_counts.py:1263:9: error[unresolved-attribute] Unresolved attribute `property` on type `Self`.
- analytics/tests/test_counts.py:1735:13: error[invalid-argument-type] Argument to function `do_revoke_user_invite` is incorrect: Expected `PreregistrationUser`, found `Self@Model | None`
+ analytics/tests/test_counts.py:1735:13: error[invalid-argument-type] Argument to function `do_revoke_user_invite` is incorrect: Expected `PreregistrationUser`, found `Self | None`
- analytics/tests/test_counts.py:1741:39: error[invalid-argument-type] Argument to function `do_send_user_invite_email` is incorrect: Expected `PreregistrationUser`, found `Self@Model | None`
+ analytics/tests/test_counts.py:1741:39: error[invalid-argument-type] Argument to function `do_send_user_invite_email` is incorrect: Expected `PreregistrationUser`, found `Self | None`
- analytics/tests/test_counts.py:2150:34: error[unresolved-attribute] Type `Self@Model` has no attribute `remote_id`
+ analytics/tests/test_counts.py:2150:34: error[unresolved-attribute] Type `Self` has no attribute `remote_id`
- analytics/tests/test_stats_views.py:223:18: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_stats_views.py:223:18: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:223:35: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_stats_views.py:223:35: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:223:52: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_stats_views.py:223:52: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:224:18: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_stats_views.py:224:18: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:224:35: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/tests/test_stats_views.py:224:35: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:526:9: error[unresolved-attribute] Unresolved attribute `end_time` on type `Self@Model`.
+ analytics/tests/test_stats_views.py:526:9: error[unresolved-attribute] Unresolved attribute `end_time` on type `Self`.
- analytics/views/stats.py:138:20: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/views/stats.py:138:20: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/views/stats.py:140:52: error[unresolved-attribute] Type `Self@Model` has no attribute `hostname`
+ analytics/views/stats.py:140:52: error[unresolved-attribute] Type `Self` has no attribute `hostname`
- analytics/views/stats.py:243:20: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ analytics/views/stats.py:243:20: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/views/stats.py:245:38: error[unresolved-attribute] Type `Self@Model` has no attribute `hostname`
+ analytics/views/stats.py:245:38: error[unresolved-attribute] Type `Self` has no attribute `hostname`
- confirmation/models.py:114:8: error[unresolved-attribute] Type `Self@Model` has no attribute `expiry_date`
+ confirmation/models.py:114:8: error[unresolved-attribute] Type `Self` has no attribute `expiry_date`
- confirmation/models.py:114:66: error[unresolved-attribute] Type `Self@Model` has no attribute `expiry_date`
+ confirmation/models.py:114:66: error[unresolved-attribute] Type `Self` has no attribute `expiry_date`
- confirmation/models.py:117:11: error[unresolved-attribute] Type `Self@Model` has no attribute `content_object`
+ confirmation/models.py:117:11: error[unresolved-attribute] Type `Self` has no attribute `content_object`
- confirmation/models.py:132:16: error[unresolved-attribute] Type `Self@Model` has no attribute `type`
+ confirmation/models.py:132:16: error[unresolved-attribute] Type `Self` has no attribute `type`
- confirmation/models.py:172:12: error[invalid-return-type] Return type does not match returned value: expected `Confirmation`, found `Self@Model`
+ confirmation/models.py:172:12: error[invalid-return-type] Return type does not match returned value: expected `Confirmation`, found `Self`
- corporate/lib/decorator.py:118:40: error[unresolved-attribute] Type `Self@Model` has no attribute `host`
+ corporate/lib/decorator.py:118:40: error[unresolved-attribute] Type `Self` has no attribute `host`
- corporate/lib/remote_billing_util.py:129:9: error[unresolved-attribute] Type `Self@Model` has no attribute `registration_deactivated`
+ corporate/lib/remote_billing_util.py:129:9: error[unresolved-attribute] Type `Self` has no attribute `registration_deactivated`
- corporate/lib/remote_billing_util.py:130:12: error[unresolved-attribute] Type `Self@Model` has no attribute `realm_deactivated`
+ corporate/lib/remote_billing_util.py:130:12: error[unresolved-attribute] Type `Self` has no attribute `realm_deactivated`
- corporate/lib/remote_billing_util.py:131:12: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/lib/remote_billing_util.py:131:12: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/lib/remote_billing_util.py:148:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteRealm, RemoteRealmBillingUser]`, found `tuple[Self@Model, Self@Model]`
+ corporate/lib/remote_billing_util.py:148:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteRealm, RemoteRealmBillingUser]`, found `tuple[Self, Self]`
- corporate/lib/remote_billing_util.py:168:8: error[unresolved-attribute] Type `Self@Model` has no attribute `deactivated`
+ corporate/lib/remote_billing_util.py:168:8: error[unresolved-attribute] Type `Self` has no attribute `deactivated`
- corporate/lib/remote_billing_util.py:173:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteZulipServer, RemoteServerBillingUser | None]`, found `tuple[Self@Model, None]`
+ corporate/lib/remote_billing_util.py:173:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteZulipServer, RemoteServerBillingUser | None]`, found `tuple[Self, None]`
- corporate/lib/remote_billing_util.py:182:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteZulipServer, RemoteServerBillingUser | None]`, found `tuple[Self@Model, Self@Model | None]`
+ corporate/lib/remote_billing_util.py:182:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteZulipServer, RemoteServerBillingUser | None]`, found `tuple[Self, Self | None]`
- corporate/lib/stripe.py:391:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlanOffer | None`, found `Self@Model | None`
+ corporate/lib/stripe.py:391:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlanOffer | None`, found `Self | None`
- corporate/lib/stripe.py:1049:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self@Model | None`
+ corporate/lib/stripe.py:1049:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self | None`
- corporate/lib/stripe.py:1074:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self@Model`
+ corporate/lib/stripe.py:1074:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self`
- corporate/lib/stripe.py:1202:17: error[unresolved-attribute] Unresolved attribute `status` on type `Self@Model`.
+ corporate/lib/stripe.py:1202:17: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/lib/stripe.py:1961:16: error[unresolved-attribute] Type `Self@Model` has no attribute `status`
+ corporate/lib/stripe.py:1961:16: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/lib/stripe.py:1974:17: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self@Model`.
+ corporate/lib/stripe.py:1974:17: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:2031:29: error[unresolved-attribute] Type `Self@Model` has no attribute `fixed_price`
+ corporate/lib/stripe.py:2031:29: error[unresolved-attribute] Type `Self` has no attribute `fixed_price`
- corporate/lib/stripe.py:2033:27: error[unresolved-attribute] Type `Self@Model` has no attribute `tier`
+ corporate/lib/stripe.py:2033:27: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:2044:20: error[unresolved-attribute] Type `Self@Model` has no attribute `next_invoice_date`
+ corporate/lib/stripe.py:2044:20: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/lib/stripe.py:2054:33: error[unresolved-attribute] Type `Self@Model` has no attribute `next_invoice_date`
+ corporate/lib/stripe.py:2054:33: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/lib/stripe.py:2055:33: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/lib/stripe.py:2055:33: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2192:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self@Model`.
+ corporate/lib/stripe.py:2192:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:2201:39: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/lib/stripe.py:2201:39: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2210:40: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/lib/stripe.py:2210:40: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2288:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self@Model]`
+ corporate/lib/stripe.py:2288:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self]`
- corporate/lib/stripe.py:2333:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self@Model]`
+ corporate/lib/stripe.py:2333:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self]`
- corporate/lib/stripe.py:2352:17: error[unresolved-attribute] Unresolved attribute `status` on type `Self@Model`.
+ corporate/lib/stripe.py:2352:17: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/lib/stripe.py:2354:47: error[unresolved-attribute] Type `Self@Model` has no attribute `tier`
+ corporate/lib/stripe.py:2354:47: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:2355:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self@Model]`
+ corporate/lib/stripe.py:2355:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self]`
- corporate/lib/stripe.py:2403:43: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/lib/stripe.py:2403:43: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2407:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[Self@Model, Self@Model]`
+ corporate/lib/stripe.py:2407:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[Self, Self]`
- corporate/lib/stripe.py:2449:44: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/lib/stripe.py:2449:44: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2453:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[Self@Model, Self@Model]`
+ corporate/lib/stripe.py:2453:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[Self, Self]`
- corporate/lib/stripe.py:2469:20: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self@Model | None`
+ corporate/lib/stripe.py:2469:20: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self | None`
- corporate/lib/stripe.py:3188:16: error[unresolved-attribute] Type `Self@Model` has no attribute `automanage_licenses`
+ corporate/lib/stripe.py:3188:16: error[unresolved-attribute] Type `Self` has no attribute `automanage_licenses`
- corporate/lib/stripe.py:3442:17: error[unresolved-attribute] Type `Self@Model` has no attribute `type`
+ corporate/lib/stripe.py:3442:17: error[unresolved-attribute] Type `Self` has no attribute `type`
- corporate/lib/stripe.py:3446:32: error[unresolved-attribute] Type `Self@Model` has no attribute `to_dict`
+ corporate/lib/stripe.py:3446:32: error[unresolved-attribute] Type `Self` has no attribute `to_dict`
- corporate/lib/stripe.py:3457:39: error[unresolved-attribute] Type `Self@Model` has no attribute `to_dict`
+ corporate/lib/stripe.py:3457:39: error[unresolved-attribute] Type `Self` has no attribute `to_dict`
- corporate/lib/stripe.py:3859:27: error[unresolved-attribute] Type `Self@Model` has no attribute `tier`
+ corporate/lib/stripe.py:3859:27: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:3872:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self@Model`.
+ corporate/lib/stripe.py:3872:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:3914:81: error[unresolved-attribute] Type `Self@Model` has no attribute `tier`
+ corporate/lib/stripe.py:3914:81: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:3927:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self@Model`.
+ corporate/lib/stripe.py:3927:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:4130:20: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self@Model`
+ corporate/lib/stripe.py:4130:20: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/stripe.py:4135:20: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self@Model`
+ corporate/lib/stripe.py:4135:20: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/stripe.py:4534:21: error[unresolved-attribute] Type `Self@Model` has no attribute `annual_discounted_price`
+ corporate/lib/stripe.py:4534:21: error[unresolved-attribute] Type `Self` has no attribute `annual_discounted_price`
- corporate/lib/stripe.py:4535:21: error[unresolved-attribute] Type `Self@Model` has no attribute `monthly_discounted_price`
+ corporate/lib/stripe.py:4535:21: error[unresolved-attribute] Type `Self` has no attribute `monthly_discounted_price`
- corporate/lib/stripe.py:4537:13: error[unresolved-attribute] Unresolved attribute `flat_discounted_months` on type `Self@Model`.
+ corporate/lib/stripe.py:4537:13: error[unresolved-attribute] Unresolved attribute `flat_discounted_months` on type `Self`.
- corporate/lib/stripe.py:4540:16: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self@Model`
+ corporate/lib/stripe.py:4540:16: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/stripe.py:4969:21: error[unresolved-attribute] Type `Self@Model` has no attribute `annual_discounted_price`
+ corporate/lib/stripe.py:4969:21: error[unresolved-attribute] Type `Self` has no attribute `annual_discounted_price`
- corporate/lib/stripe.py:4970:21: error[unresolved-attribute] Type `Self@Model` has no attribute `monthly_discounted_price`
+ corporate/lib/stripe.py:4970:21: error[unresolved-attribute] Type `Self` has no attribute `monthly_discounted_price`
- corporate/lib/stripe.py:4972:13: error[unresolved-attribute] Unresolved attribute `flat_discounted_months` on type `Self@Model`.
+ corporate/lib/stripe.py:4972:13: error[unresolved-attribute] Unresolved attribute `flat_discounted_months` on type `Self`.
- corporate/lib/stripe.py:4975:16: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self@Model`
+ corporate/lib/stripe.py:4975:16: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/support.py:392:17: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ corporate/lib/support.py:392:17: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- corporate/lib/support.py:435:17: error[unresolved-attribute] Type `Self@Model` has no attribute `end_time`
+ corporate/lib/support.py:435:17: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- corporate/lib/support.py:460:24: error[unresolved-attribute] Type `Self@Model` has no attribute `event_time`
+ corporate/lib/support.py:460:24: error[unresolved-attribute] Type `Self` has no attribute `event_time`
- corporate/models/customers.py:81:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self@Model | None`
+ corporate/models/customers.py:81:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self | None`
- corporate/models/customers.py:85:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self@Model | None`
+ corporate/models/customers.py:85:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self | None`
- corporate/models/customers.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self@Model | None`
+ corporate/models/customers.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self | None`
- corporate/models/plans.py:256:12: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self@Model | None`
+ corporate/models/plans.py:256:12: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self | None`
- corporate/models/stripe_state.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `Event | None`, found `Self@Model | None`
+ corporate/models/stripe_state.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `Event | None`, found `Self | None`
- corporate/tests/test_activity_views.py:177:24: error[unresolved-attribute] Type `Self@Model` has no attribute `last_updated`
+ corporate/tests/test_activity_views.py:177:24: error[unresolved-attribute] Type `Self` has no attribute `last_updated`
- corporate/tests/test_activity_views.py:215:63: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_activity_views.py:215:63: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_activity_views.py:219:70: error[unresolved-attribute] Type `Self@Model` has no attribute `uuid`
+ corporate/tests/test_activity_views.py:219:70: error[unresolved-attribute] Type `Self` has no attribute `uuid`
- corporate/tests/test_activity_views.py:314:28: error[unresolved-attribute] Type `Self@Model` has no attribute `last_updated`
+ corporate/tests/test_activity_views.py:314:28: error[unresolved-attribute] Type `Self` has no attribute `last_updated`
- corporate/tests/test_activity_views.py:333:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:333:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:334:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:334:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:339:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:339:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:340:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:340:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:341:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:341:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:342:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:342:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:347:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:347:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:348:28: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_activity_views.py:348:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:348:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self@Model`
+ corporate/tests/test_activity_views.py:348:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_activity_views.py:353:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:353:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:354:28: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_activity_views.py:354:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:354:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self@Model`
+ corporate/tests/test_activity_views.py:354:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_activity_views.py:359:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:359:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:360:28: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_activity_views.py:360:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:360:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self@Model`
+ corporate/tests/test_activity_views.py:360:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_activity_views.py:362:25: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_activity_views.py:362:25: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:376:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self@Model`
+ corporate/tests/test_activity_views.py:376:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:377:28: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_activity_views.py:377:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:377:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self@Model`
+ corporate/tests/test_activity_views.py:377:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_remote_billing.py:126:30: error[unresolved-attribute] Type `Self@Model` has no attribute `user_uuid`
+ corporate/tests/test_remote_billing.py:126:30: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:127:30: error[unresolved-attribute] Type `Self@Model` has no attribute `email`
+ corporate/tests/test_remote_billing.py:127:30: error[unresolved-attribute] Type `Self` has no attribute `email`
- corporate/tests/test_remote_billing.py:130:30: error[unresolved-attribute] Type `Self@Model` has no attribute `created_user`
+ corporate/tests/test_remote_billing.py:130:30: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:131:30: error[unresolved-attribute] Type `Self@Model` has no attribute `date_joined`
+ corporate/tests/test_remote_billing.py:131:30: error[unresolved-attribute] Type `Self` has no attribute `date_joined`
- corporate/tests/test_remote_billing.py:169:36: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:169:36: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:179:26: error[unresolved-attribute] Type `Self@Model` has no attribute `last_login`
+ corporate/tests/test_remote_billing.py:179:26: error[unresolved-attribute] Type `Self` has no attribute `last_login`
- corporate/tests/test_remote_billing.py:443:26: error[unresolved-attribute] Type `Self@Model` has no attribute `user_uuid`
+ corporate/tests/test_remote_billing.py:443:26: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:444:26: error[unresolved-attribute] Type `Self@Model` has no attribute `tos_version`
+ corporate/tests/test_remote_billing.py:444:26: error[unresolved-attribute] Type `Self` has no attribute `tos_version`
- corporate/tests/test_remote_billing.py:463:26: error[unresolved-attribute] Type `Self@Model` has no attribute `user_uuid`
+ corporate/tests/test_remote_billing.py:463:26: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:464:26: error[unresolved-attribute] Type `Self@Model` has no attribute `tos_version`
+ corporate/tests/test_remote_billing.py:464:26: error[unresolved-attribute] Type `Self` has no attribute `tos_version`
- corporate/tests/test_remote_billing.py:688:29: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:688:29: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:688:51: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:688:51: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:699:26: error[unresolved-attribute] Type `Self@Model` has no attribute `user_uuid`
+ corporate/tests/test_remote_billing.py:699:26: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:700:26: error[unresolved-attribute] Type `Self@Model` has no attribute `email`
+ corporate/tests/test_remote_billing.py:700:26: error[unresolved-attribute] Type `Self` has no attribute `email`
- corporate/tests/test_remote_billing.py:703:26: error[unresolved-attribute] Type `Self@Model` has no attribute `created_user`
+ corporate/tests/test_remote_billing.py:703:26: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:719:26: error[unresolved-attribute] Type `Self@Model` has no attribute `created_user`
+ corporate/tests/test_remote_billing.py:719:26: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:725:58: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:725:58: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:792:60: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:792:60: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:833:56: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:833:56: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:835:26: error[unresolved-attribute] Type `Self@Model` has no attribute `host`
+ corporate/tests/test_remote_billing.py:835:26: error[unresolved-attribute] Type `Self` has no attribute `host`
- corporate/tests/test_remote_billing.py:836:49: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:836:49: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:843:13: error[unresolved-attribute] Type `Self@Model` has no attribute `plan_type`
+ corporate/tests/test_remote_billing.py:843:13: error[unresolved-attribute] Type `Self` has no attribute `plan_type`
- corporate/tests/test_remote_billing.py:850:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:850:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:887:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:887:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:898:9: error[unresolved-attribute] Unresolved attribute `plan_type` on type `Self@Model`.
+ corporate/tests/test_remote_billing.py:898:9: error[unresolved-attribute] Unresolved attribute `plan_type` on type `Self`.
- corporate/tests/test_remote_billing.py:908:87: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_remote_billing.py:908:87: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_remote_billing.py:908:127: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:908:127: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:918:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self@Model`.
+ corporate/tests/test_remote_billing.py:918:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/tests/test_remote_billing.py:923:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self@Model`.
+ corporate/tests/test_remote_billing.py:923:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/tests/test_remote_billing.py:933:87: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_remote_billing.py:933:87: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_remote_billing.py:933:127: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:933:127: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:945:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self@Model`.
+ corporate/tests/test_remote_billing.py:945:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/tests/test_remote_billing.py:958:87: error[unresolved-attribute] Type `Self@Model` has no attribute `server`
+ corporate/tests/test_remote_billing.py:958:87: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_remote_billing.py:958:127: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:958:127: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:982:55: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:982:55: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:990:26: error[unresolved-attribute] Type `Self@Model` has no attribute `plan_type`
+ corporate/tests/test_remote_billing.py:990:26: error[unresolved-attribute] Type `Self` has no attribute `plan_type`
- corporate/tests/test_remote_billing.py:1059:60: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:1059:60: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1064:26: error[unresolved-attribute] Type `Self@Model` has no attribute `customer`
+ corporate/tests/test_remote_billing.py:1064:26: error[unresolved-attribute] Type `Self` has no attribute `customer`
- corporate/tests/test_remote_billing.py:1094:56: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:1094:56: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1096:26: error[unresolved-attribute] Type `Self@Model` has no attribute `host`
+ corporate/tests/test_remote_billing.py:1096:26: error[unresolved-attribute] Type `Self` has no attribute `host`
- corporate/tests/test_remote_billing.py:1097:49: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:1097:49: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1103:26: error[unresolved-attribute] Type `Self@Model` has no attribute `plan_type`
+ corporate/tests/test_remote_billing.py:1103:26: error[unresolved-attribute] Type `Self` has no attribute `plan_type`
- corporate/tests/test_remote_billing.py:1108:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/tests/test_remote_billing.py:1108:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1317:36: error[unresolved-attribute] Type `Self@Model` has no attribute `id`
+ corporate/tests/test_remote_billing.py:1317:36: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:1324:26: error[unresolved-attribute] Type `Self@Model` has no attribute `last_login`
+ corporate/tests/test_remote_billing.py:1324:26: error[unresolved-attribute] Type `Self` has no attribute `last_login`
- corporate/tests/test_remote_billing.py:1446:26: error[unresolved-attribute] Type `Self@Model` has no attribute `email`
+ corporate/tests/test_remote_billing.py:1446:26: error[unresolved-attribute] Type `Self` has no attribute `email`
- corporate/tests/test_remote_billing.py:1449:26: error[unresolved-attribute] Type `Self@Model` has no attribute `created_user`
+ corporate/tests/test_remote_billing.py:1449:26: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:1450:26: error[unresolved-attribute] Type `Self@Model` has no attribute `date_joined`
+ corporate/tests/test_remote_billing.py:1450:26: error[unresolved-attribute] Type `Self` has no attribute `date_joined`
- corporate/tests/test_stripe.py:694:13: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_invoice_id`
+ corporate/tests/test_stripe.py:694:13: error[unresolved-attribute] Type `Self` has no attribute `stripe_invoice_id`
- corporate/tests/test_stripe.py:700:13: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_invoice_id`
+ corporate/tests/test_stripe.py:700:13: error[unresolved-attribute] Type `Self` has no attribute `stripe_invoice_id`
- corporate/tests/test_stripe.py:706:32: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_invoice_id`
+ corporate/tests/test_stripe.py:706:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_invoice_id`
- corporate/tests/test_stripe.py:724:36: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_customer_id`
+ corporate/tests/test_stripe.py:724:36: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1032:32: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_customer_id`
+ corporate/tests/test_stripe.py:1032:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1319:32: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_customer_id`
+ corporate/tests/test_stripe.py:1319:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1571:30: error[unresolved-attribute] Type `Self@Model` has no attribute `status`
+ corporate/tests/test_stripe.py:1571:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1572:30: error[unresolved-attribute] Type `Self@Model` has no attribute `next_invoice_date`
+ corporate/tests/test_stripe.py:1572:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1577:30: error[unresolved-attribute] Type `Self@Model` has no attribute `status`
+ corporate/tests/test_stripe.py:1577:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1578:30: error[unresolved-attribute] Type `Self@Model` has no attribute `next_invoice_date`
+ corporate/tests/test_stripe.py:1578:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1624:13: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self@Model`.
+ corporate/tests/test_stripe.py:1624:13: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self`.
- corporate/tests/test_stripe.py:1647:13: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self@Model`.
+ corporate/tests/test_stripe.py:1647:13: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self`.
- corporate/tests/test_stripe.py:1653:9: error[unresolved-attribute] Unresolved attribute `fixed_price` on type `Self@Model`.
+ corporate/tests/test_stripe.py:1653:9: error[unresolved-attribute] Unresolved attribute `fixed_price` on type `Self`.
- corporate/tests/test_stripe.py:1654:9: error[unresolved-attribute] Unresolved attribute `price_per_license` on type `Self@Model`.
+ corporate/tests/test_stripe.py:1654:9: error[unresolved-attribute] Unresolved attribute `price_per_license` on type `Self`.
- corporate/tests/test_stripe.py:1681:36: error[unresolved-attribute] Type `Self@Model` has no attribute `stripe_customer_id`
+ corporate/tests/test_stripe.py:1681:36: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1795:30: error[unresolved-attribute] Type `Self@Model` has no attribute `status`
+ corporate/tests/test_stripe.py:1795:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1796:30: error[unresolved-attribute] Type `Self@Model` has no attribute `next_invoice_date`
+ corporate/tests/test_stripe.py:1796:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1823:30: error[unresolved-attribute] Type `Self@Model` has no attribute `status`
+ corporate/tests/test_stripe.py:1823:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1824:30: error[unresolved-attribute] Type `Self@Model` has no attribute `next_invoice_date`
+ corporate/tests/test_stripe.py:1824:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1827:30: error[unresolved-attribute] Type `S...*[Comment body truncated]*

---

_Review requested from @MichaReiser by @dcreager on 2025-08-06 13:46_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-06 14:05_

---

_Comment by @dcreager on 2025-08-06 14:07_

Most of the ecosystem results are just renaming the binding context that we show when rendering a typevar.

This result is interesting:

```
werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/structures.py:1104:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@remove`
```

The class's `__init__` method takes in a `Callable` that includes `typing.Self`, which is saved into an instance attribute and then called later.  We get the same diagnostic with legacy typevars [[playground](https://play.ty.dev/7d0aa1aa-5223-4b18-8d30-c7c86c795081)]:

```py
T = TypeVar("T")

class C:
    def __init__(self, callback: Callable[[T], None]) -> None:
        self.callback = callback

    def method(self: Self, u: T) -> None:
        self.callback(u)
```

That makes me feel okay about these new ecosystem results, since it means that we're handling `typing.Self` more consistently with how we handle legacy typevars.

---

_Comment by @github-actions[bot] on 2025-08-06 14:15_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 0 | 2,830 |
| `invalid-argument-type` | 7 | 1 | 745 |
| `invalid-return-type` | 0 | 1 | 136 |
| `possibly-unbound-attribute` | 0 | 0 | 27 |
| `invalid-assignment` | 0 | 0 | 13 |
| `invalid-super-argument` | 0 | 0 | 5 |
| `unsupported-operator` | 1 | 0 | 1 |
| **Total** | **8** | **2** | **3,757** |

**[Full report with detailed diff](https://dcreager-self-binding-contex.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-08-06 15:50_

The new diagnostics on altair look like false positives? It looks like casts to `Self` like [this one here](https://github.com/vega/altair/blob/d30d030a89e2e2e72c6f5a4d497968b7dfb743bf/altair/vegalite/v6/api.py#L3796-L3800) no longer work as expected.

It seems like that comes up fairly rarely in practice, though, so I'm fine if we mark that as a TODO and tackle it later!

---

_Comment by @dcreager on 2025-08-06 17:07_

> The new diagnostics on altair look like false positives? It looks like casts to `Self` like [this one here](https://github.com/vega/altair/blob/d30d030a89e2e2e72c6f5a4d497968b7dfb743bf/altair/vegalite/v6/api.py#L3796-L3800) no longer work as expected.

That one turns out to be an issue with decorators, similar to https://github.com/astral-sh/ruff/pull/19604#issuecomment-3144769600. In this case, the decorator doesn't have a return type annotation, so we assume it returns `Unknown`. That makes us think the signature of hte containing function is `Callable[..., Any]` when trying to determine the binding context for the `Self` in the cast.

---

_Comment by @carljm on 2025-08-06 17:32_

> the decorator doesn't have a return type annotation, so we assume it returns `Unknown`. That makes us think the signature of hte containing function is `Callable[..., Any]` when trying to determine the binding context for the `Self` in the cast.

It seems like for a method decorated with an unknown decorator, we should probably still assume the first argument is implicitly typed as `Self`?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:110 on 2025-08-06 17:38_

That sounds fine (feel free to create the issue), but will that be enough to solve this case? Wouldn't we consider these to be two separate "bindings" of `Self` (the one on the return type of the `next_node` property and the one on the return type of `next` method), and not allow the one to be assignable to the other, even if they share the same upper bound?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:6466 on 2025-08-06 17:53_

I think this is kind of orthogonal to this PR, and relates instead to a previous PR that landed last week while I was out -- but it doesn't feel right to me that we are handling `Type::TypeVar(...)` and `Type::KnownInstance(KnownInstanceType::TypeVar(...))` in such a parallel way here. It seems like we are missing some invariants that we ought to be enforcing? I think it should be an invariant that `Type::TypeVar` can never possibly wrap an unbound `TypeVarInstance` in the first place, so a `Type::TypeVar` should never need re-binding. `Type::TypeVar` represents an actual value of the variable type -- this is meaningless for an unbound typevar.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:302 on 2025-08-06 17:54_

I must be missing something, but it's not clear to me how this works for implicit `Self` (when it's not annotated explicitly).

---

_@carljm reviewed on 2025-08-06 17:54_

May need to come back to this later to fully understand everything that's happening here, but left some questions inline.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:302 on 2025-08-06 18:18_

This doesn't cover implicit `Self` yet, so the `is_implicit` method name might be a red herring. (I've actually changed it to `is_self` in #19786)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:6466 on 2025-08-06 18:20_

This came up in last week's PR as well, and I agree. https://github.com/astral-sh/ty/issues/926 tracks handling bound and unbound typevars more separately. `KnownInstanceType::TypeVar` would only be able to wrap an unbound typevar, and `Type::TypeVar` would only be able to wrap a bound one.


---

_@dcreager reviewed on 2025-08-06 18:21_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:110 on 2025-08-06 18:23_

Accessing the attribute would count as a call to the property getter, so my thinking was that we would infer a specialization of `{Self@next_node = Self@next}` which would make them assignable.

---

_@dcreager reviewed on 2025-08-06 18:23_

---

_Comment by @dcreager on 2025-08-06 19:18_

> It seems like for a method decorated with an unknown decorator, we should probably still assume the first argument is implicitly typed as `Self`?

Or put differently, when we're inferring things inside the function body, we shouldn't care about decorators at all! I was able to resolve this by tracking the undecorated type in the `DefinitionInference` for function definitions, and then using that when looking for enclosing generic function scopes.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-08-06 19:32_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-06 19:32_

---

_Comment by @carljm on 2025-08-06 20:52_

> I was able to resolve this by tracking the undecorated type in the `DefinitionInference` for function definitions, and then using that when looking for enclosing generic function scopes.

Nice, that fix looks good!

---

_@carljm reviewed on 2025-08-06 20:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:6466 on 2025-08-06 20:56_

Makes sense. That issue proposes one possible fix (split `TypeVarInstance` into separate Rust types for bound and unbound type variables) which would clearly allow us to enforce the invariant, but even short of that it seems like we could have some asserts/unreachables instead of confusingly handling cases that should be unnecessary/impossible (like here, I don't think we should ever call `bind_legacy_typevar` on something inside `Type::TypeVar`, that should always be already bound) to better clarify the semantics. Doesn't have to be in this PR, though.

---

_@carljm reviewed on 2025-08-06 21:07_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:110 on 2025-08-06 21:07_

That makes sense!

---

_@carljm approved on 2025-08-06 21:15_

Looks good! I think there's some cleanup/clarification to be done around typevar binding still, but I think it's orthogonal to this PR.

---

_Merged by @dcreager on 2025-08-06 21:26_

---

_Closed by @dcreager on 2025-08-06 21:26_

---

_Branch deleted on 2025-08-06 21:26_

---
