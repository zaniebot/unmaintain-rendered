```yaml
number: 19866
title: "[ty] Fix the inferred interface of specialized generic protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/generic-protocols
created_at: 2025-08-11T16:25:59Z
updated_at: 2025-08-27T17:22:00Z
url: https://github.com/astral-sh/ruff/pull/19866
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Fix the inferred interface of specialized generic protocols

---

_Pull request opened by @AlexWaygood on 2025-08-11 16:25_

## Summary

This PR fixes a number of issues with our inference for generic protocols:

- `SupportsAbs[int]` should internally be considered a protocol class, even though `is_protocol(SupportsAbs[int])` returns `False` at runtime
- The stored interface for `SupportsAbs` should be ``{"__abs__": MethodMember(`(self) -> Unknown`)}``, not ``{"__abs__": MethodMember(`(self) -> _T_co@SupportsAbs`)}``
- The stored interface for `SupportsAbs[int]` should be ``{"__abs__": MethodMember(`(self) -> int`)}``

This PR results in quite a few ecosystem diagnostics going away, which I didn't expect! I analysed them in https://github.com/astral-sh/ruff/pull/19866#issuecomment-3223656138. Most of them are due to interactions between protocol assignability/equivalence and the overload call evaluation algorithm.

There's a fairly steep regression on the `DateType` benchmark here, but most of the execution time for `DateType` is spent doing subtype checks between some fairly huge protocols; it's an extreme case. There aren't significant regressions on other benchmarks. I tried out an optimization where we only _lazily_ specialized each protocol member, rather than eagerly specializing the whole interface in `cached_protocol_interface` (https://github.com/astral-sh/ruff/pull/19866/commits/decb3d49be4b62ce279cc9e913e2abca77fa77f0). This did indeed speedup the `DateType` benchmark quite a bit, but caused stack overflows on `optuna` which weren't fixable without adding new Salsa queries -- and adding the new Salsa queries undid the speedup on `DateType`. See https://github.com/astral-sh/ruff/pull/19866#issuecomment-3176804864, https://github.com/astral-sh/ruff/pull/19866#issuecomment-3207956231 and https://github.com/astral-sh/ruff/pull/19866#issuecomment-3208294155. I've therefore backed out that attempted optimisation; I think at this point we just have to eat the regression on this (somewhat strange and specific) benchmark, unless anybody else has any ideas! A regression test for the `optuna` stack overflow was added in https://github.com/astral-sh/ruff/pull/20095.

## Test Plan

Mdtests added and updated


---

_Label `ty` added by @AlexWaygood on 2025-08-11 16:26_

---

_Comment by @github-actions[bot] on 2025-08-11 16:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-11 16:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
- src/blackd/middlewares.py:15:39: error[unsupported-operator] Operator `in` is not supported for types `str` and `under_cached_property[Unknown]`, in comparing `Literal["Access-Control-Request-Method"]` with `Unknown | under_cached_property[Unknown]`
- src/blackd/middlewares.py:21:18: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 65 diagnostics
+ Found 63 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_core/engines/posting.py:99:20: warning[redundant-cast] Value is already of type `Iterator[Body]`
- kopf/_core/engines/posting.py:112:20: warning[redundant-cast] Value is already of type `Iterator[Body]`
- kopf/_core/engines/posting.py:125:20: warning[redundant-cast] Value is already of type `Iterator[Body]`
- kopf/_core/engines/posting.py:143:20: warning[redundant-cast] Value is already of type `Iterator[Body]`
- kopf/_kits/hierarchies.py:40:16: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- kopf/_kits/hierarchies.py:77:16: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- kopf/_kits/hierarchies.py:119:16: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- kopf/_kits/hierarchies.py:171:16: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- kopf/_kits/hierarchies.py:224:16: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- Found 59 diagnostics
+ Found 50 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/data_io.py:1364:18: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- sockeye/data_io.py:1365:18: warning[redundant-cast] Value is already of type `Iterator[Unknown]`
- Found 315 diagnostics
+ Found 313 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/asynchronous/cursor.py:993:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pymongo/synchronous/cursor.py:991:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 505 diagnostics
+ Found 507 diagnostics

flake8 (https://github.com/pycqa/flake8)
- src/flake8/processor.py:284:16: error[invalid-return-type] Return type does not match returned value: expected `dict[int, str]`, found `dict[int, LiteralString]`
- Found 41 diagnostics
+ Found 40 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/auth.py:163:37: warning[redundant-cast] Value is already of type `Iterable[Unknown | tuple[str, str]]`
- Found 45 diagnostics
+ Found 44 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/config/config_options.py:524:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 180 diagnostics
+ Found 179 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/output/sanitization.py:54:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/schemathesis/schemas.py:281:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 272 diagnostics
+ Found 270 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/vcs/git/backend.py:548:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- tests/integration/test_utils_vcs_git.py:432:43: error[unresolved-attribute] Type `Literal[b""]` has no attribute `encode`
- Found 931 diagnostics
+ Found 929 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/http.py:113:12: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- discord/http.py:399:26: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- discord/http.py:402:38: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- discord/http.py:404:34: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- discord/http.py:407:23: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- discord/http.py:411:59: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- discord/webhook/async_.py:194:37: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- discord/webhook/async_.py:208:36: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 541 diagnostics
+ Found 533 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/builders/linkcheck.py:758:20: error[invalid-return-type] Return type does not match returned value: expected `str | None`, found `Literal[b""]`
- sphinx/directives/code.py:146:34: error[invalid-argument-type] Argument to function `dedent_lines` is incorrect: Expected `list[str]`, found `list[LiteralString]`
- Found 520 diagnostics
+ Found 518 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/coverstore/utils.py:92:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, dict[str, str]]`, found `tuple[Literal[b""], dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)]]`
- openlibrary/plugins/upstream/utils.py:1516:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 711 diagnostics
+ Found 709 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/client.py:771:33: warning[possibly-unbound-attribute] Attribute `get` on type `@Todo(Inference of subscript on special form) | under_cached_property[Unknown]` is possibly unbound
- aiohttp/client.py:771:68: warning[possibly-unbound-attribute] Attribute `get` on type `@Todo(Inference of subscript on special form) | under_cached_property[Unknown]` is possibly unbound
- aiohttp/client_middleware_digest_auth.py:399:23: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/client_middleware_digest_auth.py:428:18: warning[possibly-unbound-attribute] Attribute `origin` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/connector.py:1545:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RequestInfo`, found `Unknown | under_cached_property[Unknown]`
- aiohttp/connector.py:1546:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[ClientResponse, ...]`, found `Unknown | under_cached_property[Unknown]`
- aiohttp/connector.py:1549:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MultiMapping[str] | None`, found `Unknown | under_cached_property[Unknown]`
- aiohttp/web_app.py:394:12: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_fileresponse.py:206:31: warning[possibly-unbound-attribute] Attribute `timestamp` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_fileresponse.py:219:32: warning[possibly-unbound-attribute] Attribute `timestamp` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_fileresponse.py:255:27: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_fileresponse.py:309:67: warning[possibly-unbound-attribute] Attribute `timestamp` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_fileresponse.py:319:25: warning[possibly-unbound-attribute] Attribute `start` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_fileresponse.py:320:38: warning[possibly-unbound-attribute] Attribute `stop` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_log.py:125:16: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_log.py:137:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `(Unknown & ~None) | under_cached_property[Unknown] | Literal["-"]`
- aiohttp/web_log.py:155:13: warning[possibly-unbound-attribute] Attribute `major` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_log.py:156:13: warning[possibly-unbound-attribute] Attribute `minor` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:84:23: warning[possibly-unbound-attribute] Attribute `route` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:86:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `under_cached_property[Unknown]`, in comparing `Literal["?"]` with `Unknown | under_cached_property[Unknown]`
- aiohttp/web_middlewares.py:87:31: warning[possibly-unbound-attribute] Attribute `split` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:94:39: error[no-matching-overload] No overload of function `sub` matches arguments
- aiohttp/web_middlewares.py:95:37: warning[possibly-unbound-attribute] Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:96:39: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | under_cached_property[Unknown]` and `Literal["/"]`
- aiohttp/web_middlewares.py:97:33: warning[possibly-unbound-attribute] Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:98:39: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- aiohttp/web_middlewares.py:100:58: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | under_cached_property[Unknown]` and `Literal["/"]`
- aiohttp/web_middlewares.py:101:51: warning[possibly-unbound-attribute] Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:102:34: error[no-matching-overload] No overload of function `sub` matches arguments
- aiohttp/web_middlewares.py:109:42: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | under_cached_property[Unknown]` and `Unknown | Literal[""]`
- aiohttp/web_middlewares.py:119:16: warning[possibly-unbound-attribute] Attribute `current_app` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_middlewares.py:120:9: error[invalid-assignment] Object of type `Application` is not assignable to attribute `current_app` on type `Unknown | under_cached_property[Unknown]`
- aiohttp/web_protocol.py:791:31: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_response.py:348:27: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_response.py:689:45: error[invalid-argument-type] Argument to function `should_remove_content_length` is incorrect: Expected `str`, found `Unknown | under_cached_property[Unknown]`
- aiohttp/web_urldispatcher.py:321:14: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_urldispatcher.py:366:39: warning[possibly-unbound-attribute] Attribute `path_safe` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_urldispatcher.py:625:16: warning[possibly-unbound-attribute] Attribute `path_safe` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_urldispatcher.py:644:19: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- aiohttp/web_urldispatcher.py:821:16: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_urldispatcher.py:1014:20: warning[possibly-unbound-attribute] Attribute `path_safe` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_urldispatcher.py:1042:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | under_cached_property[Unknown]`
- aiohttp/web_ws.py:229:27: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_ws.py:234:26: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_ws.py:237:29: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_ws.py:240:21: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_ws.py:246:12: error[unsupported-operator] Operator `in` is not supported for types `istr` and `under_cached_property[Unknown]`, in comparing `istr` with `Unknown | under_cached_property[Unknown]`
- aiohttp/web_ws.py:249:30: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- aiohttp/web_ws.py:266:19: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_ws.py:271:15: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp/web_ws.py:292:26: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 209 diagnostics
+ Found 158 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/wrap/wrap.py:788:21: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- Found 808 diagnostics
+ Found 807 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/www.py:115:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 1789 diagnostics
+ Found 1788 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_compat/common/_helpers.py:917:21: warning[redundant-cast] Value is already of type `Collection[SupportsIndex]`
- Found 2057 diagnostics
+ Found 2056 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/_trace/utils_valkey.py:77:71: error[invalid-argument-type] Argument to function `_set_span_tags` is incorrect: Expected `list[Unknown] | None`, found `LiteralString`
- ddtrace/_trace/utils_valkey.py:93:71: error[invalid-argument-type] Argument to function `_set_span_tags` is incorrect: Expected `list[Unknown] | None`, found `LiteralString`
- Found 6597 diagnostics
+ Found 6595 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-bitbucket/prefect_bitbucket/repository.py:136:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""] | Unknown`
- src/integrations/prefect-github/prefect_github/repository.py:77:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""] | Unknown`
- src/integrations/prefect-gitlab/prefect_gitlab/repositories.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""] | Unknown`
- src/prefect/runner/storage.py:197:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 3017 diagnostics
+ Found 3013 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/tests/unit/accounting/test_staking.py:88:42: error[invalid-argument-type] Argument is incorrect: Expected `FVal`, found `int`
- Found 1589 diagnostics
+ Found 1588 diagnostics

zulip (https://github.com/zulip/zulip)
- corporate/views/remote_billing_page.py:513:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- zerver/lib/bot_lib.py:115:81: error[invalid-argument-type] Argument to function `internal_send_group_direct_message` is incorrect: Expected `list[str] | None`, found `list[LiteralString]`
- Found 2631 diagnostics
+ Found 2629 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/_lib/array_api_compat/array_api_compat/common/_helpers.py:927:21: warning[redundant-cast] Value is already of type `Collection[SupportsIndex]`
- scipy/interpolate/_bsplines.py:2168:26: error[no-matching-overload] No overload of function `sum` matches arguments
- Found 6421 diagnostics
+ Found 6419 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~26MB
+     struct metadata = ~27MB

```
</details>


---

_Comment by @AlexWaygood on 2025-08-11 16:31_

That is... many more ecosystem hits than I was expecting 😆

---

_Comment by @codspeed-hq[bot] on 2025-08-11 16:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fgeneric-protocols?runnerMode=Instrumentation)

### Merging #19866 will **degrade performances by 13.81%**

<sub>Comparing <code>alex/generic-protocols</code> (eec874b) with <code>main</code> (7d0c8e0)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` DateType `` | 234.4 ms | 271.9 ms | -13.81% |


---

_Comment by @AlexWaygood on 2025-08-11 18:17_

If I apply this diff, it makes the `DateType` benchmark a lot faster and continues to pass all tests, but causes stack overflows on `optuna` in mypy_primer runs (possibly others too):

<details>

```diff
diff --git a/crates/ty_python_semantic/src/types/protocol_class.rs b/crates/ty_python_semantic/src/types/protocol_class.rs
index 7b82192ece..673aeecbf5 100644
--- a/crates/ty_python_semantic/src/types/protocol_class.rs
+++ b/crates/ty_python_semantic/src/types/protocol_class.rs
@@ -7,6 +7,7 @@ use ruff_python_ast::name::Name;
 
 use super::TypeVarVariance;
 use crate::semantic_index::place_table;
+use crate::types::generics::Specialization;
 use crate::{
     Db, FxOrderSet,
     place::{Boundness, Place, PlaceAndQualifiers, place_from_bindings, place_from_declarations},
@@ -124,6 +125,7 @@ impl<'db> ProtocolInterface<'db> {
                     ProtocolMemberData {
                         qualifiers: TypeQualifiers::default(),
                         kind: ProtocolMemberKind::Property(property),
+                        specialization: None,
                     },
                 )
             })
@@ -142,18 +144,34 @@ impl<'db> ProtocolInterface<'db> {
     where
         'db: 'a,
     {
-        self.inner(db).iter().map(|(name, data)| ProtocolMember {
-            name,
-            kind: data.kind,
-            qualifiers: data.qualifiers,
+        self.inner(db).iter().map(|(name, data)| {
+            let ProtocolMemberData {
+                kind,
+                specialization,
+                qualifiers,
+            } = data;
+
+            ProtocolMember {
+                name,
+                kind: kind.apply_optional_specialization(db, *specialization),
+                qualifiers: *qualifiers,
+            }
         })
     }
 
     fn member_by_name<'a>(self, db: &'db dyn Db, name: &'a str) -> Option<ProtocolMember<'a, 'db>> {
-        self.inner(db).get(name).map(|data| ProtocolMember {
-            name,
-            kind: data.kind,
-            qualifiers: data.qualifiers,
+        self.inner(db).get(name).map(|data| {
+            let ProtocolMemberData {
+                kind,
+                specialization,
+                qualifiers,
+            } = data;
+
+            ProtocolMember {
+                name,
+                kind: kind.apply_optional_specialization(db, *specialization),
+                qualifiers: *qualifiers,
+            }
         })
     }
 
@@ -237,8 +255,13 @@ impl<'db> ProtocolInterface<'db> {
         impl std::fmt::Display for ProtocolInterfaceDisplay<'_> {
             fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
                 f.write_char('{')?;
-                for (i, (name, data)) in self.interface.inner(self.db).iter().enumerate() {
-                    write!(f, "\"{name}\": {data}", data = data.display(self.db))?;
+                for (i, member) in self.interface.members(self.db).enumerate() {
+                    let ProtocolMember {
+                        name,
+                        kind,
+                        qualifiers: _,
+                    } = member;
+                    write!(f, "\"{name}\": {kind}", kind = kind.display(self.db))?;
                     if i < self.interface.inner(self.db).len() - 1 {
                         f.write_str(", ")?;
                     }
@@ -257,6 +280,7 @@ impl<'db> ProtocolInterface<'db> {
 #[derive(Debug, PartialEq, Eq, Clone, Hash, salsa::Update)]
 pub(super) struct ProtocolMemberData<'db> {
     kind: ProtocolMemberKind<'db>,
+    specialization: Option<Specialization<'db>>,
     qualifiers: TypeQualifiers,
 }
 
@@ -268,6 +292,7 @@ impl<'db> ProtocolMemberData<'db> {
     fn normalized_impl(&self, db: &'db dyn Db, visitor: &mut TypeTransformer<'db>) -> Self {
         Self {
             kind: self.kind.normalized_impl(db, visitor),
+            specialization: self.specialization.map(|s| s.normalized_impl(db, visitor)),
             qualifiers: self.qualifiers,
         }
     }
@@ -276,6 +301,9 @@ impl<'db> ProtocolMemberData<'db> {
         Self {
             kind: self.kind.apply_type_mapping(db, type_mapping),
             qualifiers: self.qualifiers,
+            specialization: self
+                .specialization
+                .map(|s| s.apply_type_mapping(db, type_mapping)),
         }
     }
 
@@ -291,41 +319,7 @@ impl<'db> ProtocolMemberData<'db> {
         Self {
             kind: self.kind.materialize(db, variance),
             qualifiers: self.qualifiers,
-        }
-    }
-
-    fn display(&self, db: &'db dyn Db) -> impl std::fmt::Display {
-        struct ProtocolMemberDataDisplay<'db> {
-            db: &'db dyn Db,
-            data: ProtocolMemberKind<'db>,
-        }
-
-        impl std::fmt::Display for ProtocolMemberDataDisplay<'_> {
-            fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
-                match self.data {
-                    ProtocolMemberKind::Method(callable) => {
-                        write!(f, "MethodMember(`{}`)", callable.display(self.db))
-                    }
-                    ProtocolMemberKind::Property(property) => {
-                        let mut d = f.debug_struct("PropertyMember");
-                        if let Some(getter) = property.getter(self.db) {
-                            d.field("getter", &format_args!("`{}`", &getter.display(self.db)));
-                        }
-                        if let Some(setter) = property.setter(self.db) {
-                            d.field("setter", &format_args!("`{}`", &setter.display(self.db)));
-                        }
-                        d.finish()
-                    }
-                    ProtocolMemberKind::Other(ty) => {
-                        write!(f, "AttributeMember(`{}`)", ty.display(self.db))
-                    }
-                }
-            }
-        }
-
-        ProtocolMemberDataDisplay {
-            db,
-            data: self.kind,
+            specialization: self.specialization.map(|s| s.materialize(db, variance)),
         }
     }
 }
@@ -366,6 +360,16 @@ impl<'db> ProtocolMemberKind<'db> {
         }
     }
 
+    fn apply_optional_specialization(
+        self,
+        db: &'db dyn Db,
+        specialization: Option<Specialization<'db>>,
+    ) -> Self {
+        specialization
+            .map(|s| self.apply_type_mapping(db, &TypeMapping::Specialization(s)))
+            .unwrap_or(self)
+    }
+
     fn find_legacy_typevars(
         &self,
         db: &'db dyn Db,
@@ -391,6 +395,38 @@ impl<'db> ProtocolMemberKind<'db> {
             }
         }
     }
+
+    fn display(self, db: &'db dyn Db) -> impl std::fmt::Display {
+        struct ProtocolMemberKindDisplay<'db> {
+            db: &'db dyn Db,
+            kind: ProtocolMemberKind<'db>,
+        }
+
+        impl std::fmt::Display for ProtocolMemberKindDisplay<'_> {
+            fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
+                match self.kind {
+                    ProtocolMemberKind::Method(callable) => {
+                        write!(f, "MethodMember(`{}`)", callable.display(self.db))
+                    }
+                    ProtocolMemberKind::Property(property) => {
+                        let mut d = f.debug_struct("PropertyMember");
+                        if let Some(getter) = property.getter(self.db) {
+                            d.field("getter", &format_args!("`{}`", &getter.display(self.db)));
+                        }
+                        if let Some(setter) = property.setter(self.db) {
+                            d.field("setter", &format_args!("`{}`", &setter.display(self.db)));
+                        }
+                        d.finish()
+                    }
+                    ProtocolMemberKind::Other(ty) => {
+                        write!(f, "AttributeMember(`{}`)", ty.display(self.db))
+                    }
+                }
+            }
+        }
+
+        ProtocolMemberKindDisplay { db, kind: self }
+    }
 }
 
 /// A single member of a protocol interface.
@@ -581,8 +617,6 @@ fn cached_protocol_interface<'db>(
                 })
                 .filter(|(name, _, _, _)| !excluded_from_proto_members(name))
                 .map(|(name, ty, qualifiers, bound_on_class)| {
-                    let ty = ty.apply_optional_specialization(db, specialization);
-
                     let kind = match (ty, bound_on_class) {
                         // TODO: if the getter or setter is a function literal, we should
                         // upcast it to a `CallableType` so that two protocols with identical property
@@ -601,7 +635,12 @@ fn cached_protocol_interface<'db>(
                         _ => ProtocolMemberKind::Other(ty),
                     };
 
-                    let member = ProtocolMemberData { kind, qualifiers };
+                    let member = ProtocolMemberData {
+                        kind,
+                        specialization,
+                        qualifiers,
+                    };
+
                     (name.clone(), member)
                 }),
         );
```

</details>

I guess I should try to minimize that stack overflow and add an mdtest...

---

_Comment by @AlexWaygood on 2025-08-11 20:31_

> I guess I should try to minimize that stack overflow and add an mdtest...

minimized:

```py
from typing import Protocol

class Foo[T](Protocol):
    def x(self) -> "T | Foo[T]": ...


x: str | Foo[str]
```

---

_Comment by @carljm on 2025-08-11 21:43_

To debug a stack overflow, the first thing you need is a backtrace. To get a backtrace, run the test binary directly (instead of via `cargo test`); the test output tells you the test binary name. Generally something like `/Users/carlmeyer/projects/ruff/target/debug/deps/mdtest-7855bafbe7e714e0 mdtest__pep695_type_aliases` (where I'm also passing an argument to narrow which tests are run). Run that to confirm it still stack-overflows, then run it under lldb (e.g. `lldb -- /Users/carlmeyer/projects/ruff/target/debug/deps/mdtest-7855bafbe7e714e0 mdtest__pep695_type_aliases`). When lldb starts up, use `run` to run the program, then once it stack-overflows use `bt` to get a backtrace. Scroll up to the point where the backtrace starts repeating itself, and you can figure out the cycle of functions that are calling themselves in an infinite loop. That should give a pretty good idea where we need to add cycle detection.

---

_Comment by @AlexWaygood on 2025-08-20 20:13_

This is the section of the backtrace that repeats unendingly if I apply the patch in https://github.com/astral-sh/ruff/pull/19866#issuecomment-3176261676:

```
    frame #3805: 0x0000000100e74a7c ty`ty_python_semantic::types::signatures::CallableSignature::from_overloads::h160c085c76571d62(overloads=<unavailable>) at signatures.rs:51:46
    frame #3806: 0x00000001011f814c ty`ty_python_semantic::types::signatures::CallableSignature::apply_type_mapping::h2f18f781c1b51baa(self=0x00000001187100c0, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcacb8, type_mapping=0x000000016ffcae80) at signatures.rs:84:9
    frame #3807: 0x0000000100ac4d70 ty`ty_python_semantic::types::CallableType::apply_type_mapping::h424be50bb82634f9(self=CallableType @ 0x000000016ffcad80, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcad88, type_mapping=0x000000016ffcae80) at types.rs:8592:33
    frame #3808: 0x0000000100b9434c ty`ty_python_semantic::types::protocol_class::ProtocolMemberKind::apply_type_mapping::hf269b9bee42a1fc1(self=0x000000016ffcb030, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcae40, type_mapping=0x000000016ffcae80) at protocol_class.rs:354:53
    frame #3809: 0x0000000100fb64e8 ty`ty_python_semantic::types::protocol_class::ProtocolMemberKind::apply_optional_specialization::_$u7b$$u7b$closure$u7d$$u7d$::h91d24b2ae9362918(s=Specialization @ 0x000000016ffcaea8) at protocol_class.rs:371:27
    frame #3810: 0x0000000101211ef8 ty`core::option::Option$LT$T$GT$::map::h94974adb26c19027(self=Option<ty_python_semantic::types::generics::Specialization> @ 0x000000016ffcaee8, f={closure_env#0} @ 0x000000016ffcaf78) at option.rs:1146:29
    frame #3811: 0x0000000100b94454 ty`ty_python_semantic::types::protocol_class::ProtocolMemberKind::apply_optional_specialization::h48d63c0b31501ffc(self=ProtocolMemberKind @ 0x000000016ffcb030, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcafb8, specialization=Option<ty_python_semantic::types::generics::Specialization> @ 0x000000016ffcafc8) at protocol_class.rs:371:14
    frame #3812: 0x0000000100fb5f84 ty`ty_python_semantic::types::protocol_class::ProtocolInterface::members::_$u7b$$u7b$closure$u7d$$u7d$::hf0a2a86390dcb245((null)=(&ruff_python_ast::name::Name, &ty_python_semantic::types::protocol_class::ProtocolMemberData) @ 0x000000016ffcb058) at protocol_class.rs:155:28
    frame #3813: 0x0000000100f9c154 ty`core::iter::adapters::map::map_try_fold::_$u7b$$u7b$closure$u7d$$u7d$::hb1749a3ecbb42801(acc=<unavailable>, elt=(&ruff_python_ast::name::Name, &ty_python_semantic::types::protocol_class::ProtocolMemberData) @ 0x000000016ffcb140) at map.rs:95:28
    frame #3814: 0x000000010124e45c ty`core::iter::traits::iterator::Iterator::try_fold::hb39dd7ecf0581ac4(self=0x000000016ffcb2e8, init=<unavailable>, f={closure_env#0}<(&ruff_python_ast::name::Name, &ty_python_semantic::types::protocol_class::ProtocolMemberData), ty_python_semantic::types::protocol_class::ProtocolMember, (), core::ops::control_flow::ControlFlow<(), ()>, ty_python_semantic::types::protocol_class::{impl#5}::members::{closure_env#0}, core::iter::traits::iterator::Iterator::all::check::{closure_env#0}<ty_python_semantic::types::protocol_class::ProtocolMember, ty_python_semantic::types::instance::{impl#0}::satisfies_protocol::{closure_env#0}>> @ 0x000000016ffcb210) at iterator.rs:2426:21
    frame #3815: 0x0000000100f8ae34 ty`_$LT$core..iter..adapters..map..Map$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::try_fold::h7275394e5ccc9e51(self=0x000000016ffcb2e8, init=<unavailable>, g=<unavailable>) at map.rs:121:19
    frame #3816: 0x0000000100f8f244 ty`core::iter::traits::iterator::Iterator::all::h4cb32e4a1b2caea4(self=0x000000016ffcb2e8, f=<unavailable>) at iterator.rs:2772:14
    frame #3817: 0x0000000100abc160 ty`ty_python_semantic::types::instance::_$LT$impl$u20$ty_python_semantic..types..Type$GT$::satisfies_protocol::h67156676ed5b63c6(self=Type @ 0x000000016ffcbb50, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcb370, protocol=<unavailable>, relation=Subtyping) at instance.rs:111:14
    frame #3818: 0x0000000100acd8e4 ty`ty_python_semantic::types::Type::has_relation_to_impl::h8b0097a4b76d0e5f(self=Type @ 0x000000016ffcc880, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcc468, target=Type @ 0x000000016ffcc8a0, relation=Subtyping, visitor=0x000000016ffcc7f8) at types.rs:1535:22
    frame #3819: 0x0000000100acc874 ty`ty_python_semantic::types::Type::has_relation_to::h7e2349bc8ff47bb1(self=Type @ 0x000000016ffcc910, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcc868, target=Type @ 0x000000016ffcc930, relation=Subtyping) at types.rs:1308:14
    frame #3820: 0x0000000100acc768 ty`ty_python_semantic::types::Type::is_subtype_of::h9c836aab433ba280(self=<unavailable>, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcc900, target=<unavailable>) at types.rs:1297:14
    frame #3821: 0x0000000100c807f0 ty`ty_python_semantic::types::builder::UnionBuilder::add_in_place::h3bb7ba0f5eb45363(self=0x000000016ffcdb58, ty=Type @ 0x000000016ffcdb00) at builder.rs:458:44
    frame #3822: 0x0000000100c7f38c ty`ty_python_semantic::types::builder::UnionBuilder::add::h287bf0f22c873d09(self=<unavailable>, ty=<unavailable>) at builder.rs:233:14
    frame #3823: 0x0000000100ec96b0 ty`ty_python_semantic::types::UnionType::from_elements::_$u7b$$u7b$closure$u7d$$u7d$::hc6f09c8049f9318b((null)=0x000000016ffcde90, builder=<unavailable>, element=Type @ 0x000000016ffcdc20) at types.rs:9078:25
    frame #3824: 0x0000000100fa2af4 ty`core::iter::adapters::map::map_fold::_$u7b$$u7b$closure$u7d$$u7d$::hb2d01db2559425c3(acc=<unavailable>, elt=0x0000600001ff5e20) at map.rs:88:21
    frame #3825: 0x0000000100d63b4c ty`_$LT$core..slice..iter..Iter$LT$T$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::fold::h80522d278141e979(self=Iter<ty_python_semantic::types::Type> @ 0x000000016ffcdd20, init=UnionBuilder @ 0x000000016ffcdf40, f={closure_env#0}<&ty_python_semantic::types::Type, ty_python_semantic::types::Type, ty_python_semantic::types::builder::UnionBuilder, ty_python_semantic::types::{impl#124}::apply_type_mapping_impl::{closure_env#0}, ty_python_semantic::types::{impl#50}::from_elements::{closure_env#0}<core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::Type>, ty_python_semantic::types::{impl#124}::apply_type_mapping_impl::{closure_env#0}>, ty_python_semantic::types::Type>> @ 0x000000016ffcde70) at macros.rs:255:27
    frame #3826: 0x0000000100f7f760 ty`_$LT$core..iter..adapters..map..Map$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::fold::heedc7d410fe64563(self=<unavailable>, init=<unavailable>, g={closure_env#0}<core::iter::adapters::map::Map<core::slice::iter::Iter<ty_python_semantic::types::Type>, ty_python_semantic::types::{impl#124}::apply_type_mapping_impl::{closure_env#0}>, ty_python_semantic::types::Type> @ 0x000000016ffcdebf) at map.rs:128:19
    frame #3827: 0x0000000100ec6b9c ty`ty_python_semantic::types::UnionType::from_elements::h05cbc68a1023a295(db=&dyn ty_python_semantic::db::Db @ 0x000000016ffcdf70, elements=<unavailable>) at types.rs:9077:14
    frame #3828: 0x0000000100ecb430 ty`ty_python_semantic::types::UnionType::map::h4ef8cc223a4f6c76(self=UnionType @ 0x000000016ffce038, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffce040, transform_fn={closure_env#0} @ 0x000000016ffce350) at types.rs:9107:9
    frame #3829: 0x0000000100ae481c ty`ty_python_semantic::types::Type::apply_type_mapping_impl::hdfd001f8957775df(self=Type @ 0x000000016ffce8c0, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffce5c0, type_mapping=0x000000016ffcf380, visitor=0x000000016ffce820) at types.rs:5917:41
    frame #3830: 0x0000000100ae439c ty`ty_python_semantic::types::Type::apply_type_mapping::hcb943c00803553cf(self=Type @ 0x000000016ffce910, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffce8a8, type_mapping=0x000000016ffcf380) at types.rs:5797:14
    frame #3831: 0x0000000100e7548c ty`ty_python_semantic::types::signatures::Signature::apply_type_mapping::_$u7b$$u7b$closure$u7d$$u7d$::h006b29af6283e830(ty=<unavailable>) at signatures.rs:438:30
    frame #3832: 0x0000000101214fa8 ty`core::option::Option$LT$T$GT$::map::hf372718cdfe3321c(self=Option<ty_python_semantic::types::Type> @ 0x000000016ffcea70, f={closure_env#0} @ 0x000000016ffcea98) at option.rs:1146:29
    frame #3833: 0x00000001011f90cc ty`ty_python_semantic::types::signatures::Signature::apply_type_mapping::h6754f65400223c3b(self=0x00000001187100c8, db=&dyn ty_python_semantic::db::Db @ 0x000000016ffceab8, type_mapping=0x000000016ffcf380) at signatures.rs:438:18
    frame #3834: 0x0000000100e74e08 ty`ty_python_semantic::types::signatures::CallableSignature::apply_type_mapping::_$u7b$$u7b$closure$u7d$$u7d$::h838a44a1727422ca(signature=0x00000001187100c8) at signatures.rs:87:44
    frame #3835: 0x0000000100f8408c ty`_$LT$core..iter..adapters..map..Map$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::next::h5c7894b2788c7476 [inlined] core::ops::function::impls::_$LT$impl$u20$core..ops..function..FnOnce$LT$A$GT$$u20$for$u20$$RF$mut$u20$F$GT$::call_once::h903f4f5b86a41506(self=0x000000016ffcec58, args=(&ty_python_semantic::types::signatures::Signature) @ 0x000000016ffcebb8) at function.rs:305:21
    frame #3836: 0x0000000100f84088 ty`_$LT$core..iter..adapters..map..Map$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::next::h5c7894b2788c7476 [inlined] core::option::Option$LT$T$GT$::map::h3ba7d47343250a43(self=Option<&ty_python_semantic::types::signatures::Signature> @ 0x000000016ffceb40, f=0x000000016ffcec58) at option.rs:1146:29
    frame #3837: 0x0000000100f84088 ty`_$LT$core..iter..adapters..map..Map$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::next::h5c7894b2788c7476(self=0x000000016ffcec48) at map.rs:107:26
    frame #3838: 0x0000000100f1f048 ty`_$LT$smallvec..SmallVec$LT$A$GT$$u20$as$u20$core..iter..traits..collect..Extend$LT$$LT$A$u20$as$u20$smallvec..Array$GT$..Item$GT$$GT$::extend::h44737aaa289b2b26(self=0x000000016ffcef80, iterable=<unavailable>) at lib.rs:2101:41
    frame #3839: 0x0000000100f22c64 ty`_$LT$smallvec..SmallVec$LT$A$GT$$u20$as$u20$core..iter..traits..collect..FromIterator$LT$$LT$A$u20$as$u20$smallvec..Array$GT$..Item$GT$$GT$::from_iter::hc241bc4940d3e286(iterable=Map<core::slice::iter::Iter<ty_python_semantic::types::signatures::Signature>, ty_python_semantic::types::signatures::{impl#0}::apply_type_mapping::{closure_env#0}> @ 0x000000016ffcf040) at lib.rs:2085:11
    frame #3840: 0x0000000100f9721c ty`core::iter::traits::iterator::Iterator::collect::hc362bf1bbfc44466(self=<unavailable>) at iterator.rs:2027:9
    frame #3841: 0x0000000100e74a7c ty`ty_python_semantic::types::signatures::CallableSignature::from_overloads::h160c085c76571d62(overloads=<unavailable>) at signatures.rs:51:46
```

It's not totally obvious to me how to fix this with our existing `CycleDetector`s and `TypeTransformer`s.

---

_Comment by @carljm on 2025-08-20 22:46_

What this looks like is that in the process of specializing a protocol type, we have to map over a union in `apply_type_mapping_impl`, and in constructing/simplifying the new union we do a subtype check involving the original protocol, and that subtype check requires specializing that protocol type (this is the part I _think_ is new in the patch?) -- and there we have a cycle.

I agree this isn't easily amenable to our existing tools. Like the tuple-subtype-check-builds-a-union-of-its-elements issue I initially ran into in the recursive-type-aliases PR, the problem is the cycle passes through UnionBuilder and `has_relation_to_impl`, where we would lose our visitor from `apply_type_mapping_impl`.

The part about "subtype check of a protocol requires applying a type mapping to it" feels a bit off to me (just in that applying a type mapping is kind of a heavyweight transformation that builds a new type, and it doesn't seem like we should have to do that just in order to make a subtype check), but I'm not sure if it's wrong or not without digging a lot deeper into this diff.

Assuming that part is necessary, another way to break the cycle could be to re-introduce `UnionStrategy` and not simplify subtypes out of unions when we map over a union in `apply_type_mapping_impl`?

Or to find another place we can insert a Salsa query to catch and iterate on the cycle (like we did for `TupleType::into_class_type`, which fixed the problem on the recursive type aliases PR.) It's not clear to me where that would be in this case, but maybe it would be on `Protocol::members` or similar?

In general, we have decent tools now for preventing cycles within a `Type` method, but not when we introduce cycles that bounce back and forth between multiple different `Type` methods and `UnionBuilder`.

Honestly, a very comprehensive fix for this would be to make e.g. `is_subtype_of` a Salsa query, but I think that might have significant memory costs.

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-26 09:41_

---

_Comment by @github-actions[bot] on 2025-08-26 09:51_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unbound-attribute` | 0 | 43 | 2 |
| `invalid-return-type` | 0 | 17 | 0 |
| `unresolved-attribute` | 0 | 17 | 0 |
| `invalid-argument-type` | 0 | 12 | 2 |
| `redundant-cast` | 0 | 14 | 0 |
| `unsupported-operator` | 0 | 6 | 0 |
| `non-subscriptable` | 0 | 5 | 0 |
| `no-matching-overload` | 0 | 4 | 0 |
| `index-out-of-bounds` | 0 | 3 | 0 |
| `invalid-assignment` | 0 | 3 | 0 |
| `unused-ignore-comment` | 3 | 0 | 0 |
| **Total** | **3** | **124** | **4** |

**[Full report with detailed diff](https://alex-generic-protocols.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-08-26 10:52_

# Ecosystem analysis

I haven't gone through every single hit but I've gone through enough to be fairly confident that everything here is basically working as expected. Most of the hits are due to interesting interactions between protocol assignability/equivalence and the overload evaluation algorithm.

## `aiohttp`, `discord.py`, `black`

Nearly all of these hits are due to `aiohttp`'s use of `@reify` as a decorator in places like [this](https://github.com/aio-libs/aiohttp/blob/6d76b651b2c1e99be0de733aee733874be4a698a/aiohttp/client_reqrep.py#L347-L380). `reify` is actually an import of `propcache.api.under_cached_property`, and ty is finding type information for that class [here](https://github.com/aio-libs/propcache/blob/b97e7e37cbe8329e2a4d8383166c094f471a0d6a/src/propcache/_helpers_py.py#L26-L62). A minimized example of the code that we now infer differently is this:

```py
from typing import Mapping, Any, Protocol, Callable, overload, Self

class _CacheImpl[CacheT: Mapping[str, Any]](Protocol):
    cache: CacheT

class under_cached_property[T]:
    def __init__(self, wrapped: Callable[[Any], _T]) -> None: ...

    @overload
    def __get__(self: Self, inst: None, owner: type | None = None) -> Self: ...
    @overload
    def __get__(self, inst: _CacheImpl[Any], owner: type | None = None) -> _T: ...  # ty: ignore[invalid-overload]

class Response:
    cache: Mapping[str, Any]

    @under_cached_property
    def headers(self) -> Mapping[str, Any]: ...  # ty: ignore[invalid-return-type]

def _(x: Response):
    reveal_type(x.headers)
    reveal_type(x.headers.get)
```

On `main`, we reveal `under_cached_property[Unknown]` for the first `reveal_type` call, and emit an error on the second `reveal_type` call saying that "`under_cached_property` has no member "get"`. This indicates that on `main` we incorrectly pick the first overload for `under_cached_property.__get__` when resolving the `x.headers` attribute access.

On this PR branch, however, we reveal `Unknown` for both `reveal_type` calls and do not emit an `unresolved-attribute` diagnostic. That indicates that we're now correctly picking the second overload for `under_cached_property.__get__` when resolving the `x.headers` attribute access -- but due to limitations in our constraints solver, we're still not able to solve the TypeVar.

The conclusion here then is that the reason why our behaviour is changing on this snippet is that previously did not consider `Response` to be assignable to the type `_CacheImpl[Any]`, but now we do. This is confirmed by the fact that this assertion fails on `main`, but passes on my PR branch:

```py
static_assert(is_assignable_to(Response, _CacheImpl[Any]))
```

This all makes sense, since `_CacheImpl` is a generic protocol, and this PR fixes the interfaces of generic protocols

## `ddtrace-py`, `flake8`, `pwndbg`

These hits are because we now infer a different type here:

```py
def f(x):
    reveal_type("\n".join(x))  # `LiteralString` on `main`, `Unknown` on this PR
```

On `main`, we're mistakenly confident that [the first overload for `str.join` is the correct one](https://github.com/python/typeshed/blob/4fa56fbc236c99159c6132254d8f182598469df8/stdlib/builtins.pyi#L521-L524) to select here. The first overload has a parameter annotation of `Iterable[LiteralString]`, while the second overload has a parameter annotation of `Iterable[str]`.

A repro of this issue that doesn't depend on typeshed's stubs for `str.join` is:

```py
from typing import Iterable, overload, LiteralString, Protocol
from ty_extensions import Unknown, is_assignable_to

class Foo:
    @overload
    def join(self, iterable: Iterable[LiteralString], /) -> LiteralString: ...
    @overload
    def join(self, iterable: Iterable[str], /) -> str: ...  # ty: ignore[invalid-overload]

def f(x, y: Foo):
    reveal_type(x)                                         # `Unknown` on `main` and this PR
    reveal_type(is_assignable_to(Unknown, Iterable[str]))  # `Literal[True]` on `main` and this PR
    reveal_type(y.join(x))                                 # `LiteralString` on `main`, `Unknown` on this PR
```

The reason for the bug on `main` has to do with Step (5) of the [overload call evaluation algorithm](https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation), which states:

> For all arguments, determine whether all possible [materializations](https://typing.python.org/en/latest/spec/glossary.html#term-materialize) of the argument’s type are assignable to the corresponding parameter type for each of the remaining overloads. If so, eliminate all of the subsequent remaining overloads.

On `main`, we mistakenly view `Iterable[str]` and `Iterable[LiteralString]` as equivalent types! This means that we eliminate the second overload (which uses `Iterable[str]`) during step (5), leaving only the first overload (using `Iterable[LiteralString]`) remaining. With this PR branch, however, we understand that `Iterable[str]` and `Iterable[LiteralString]` are _not_ equivalent types, and that the parameter annotations of both overloads are satisfied, meaning that we should (as per step (5) of the overload evaluation algorithm) infer `Unknown` as the result of the call. (It is ambiguous which overload we should choose, and the return types of the two remaining overloads are not equivalent, meaning we cannot proceed to step (6).)

## `ignite`, `rotki`

I haven't studied these ones in depth, but they look like they're almost certainly the same underlying issue as the `ddtrace-py` and `flake8` hits. It looks like the issue is that on `main` we currently infer the wrong result for a call to `sum()`, and `sum()` [is an overloaded function in typeshed](https://github.com/python/typeshed/blob/5509c518320f2d5fbb5ca3ea4e79b89b0a5f441a/stdlib/builtins.pyi#L1923-L1932) where the different overloads all take `Iterable`s, but with `Iterable` specialized differently in each overload (exactly the same as `str.join()`).

## `meson`, `mkdocs`, `openlibrary`, `poetry`, `prefect`, `schemathesis`, `static-frame`

Also looks like the same thing as with `ddtrace-py` and `flake8`, but this time with [`urllib.parse.urlunparse()`](https://github.com/python/typeshed/blob/5509c518320f2d5fbb5ca3ea4e79b89b0a5f441a/stdlib/urllib/parse.pyi#L191-L194) and [`urllib.parse.urlunsplit()`](https://github.com/python/typeshed/blob/5509c518320f2d5fbb5ca3ea4e79b89b0a5f441a/stdlib/urllib/parse.pyi#L197-L200)

## `mitmproxy`

This is the same again, but a slightly more elaborate example of how `Unknown`s can cascade through a program and become viral. The code has this variable assigned:

```py
    exclude = re.compile(
        "|".join(
            f"({fnmatch.translate(x)})"
            for x in data["tool"]["pytest"]["individual_coverage"]["exclude"]
        )
    )
```

We infer the result of the generator expression as `Unknown`, and on this PR we now correctly infer the result of passing an instance of `Unknown` into `str.join()` as being `Unknown`. But that then means that the `exclude` variable is now also inferred as `Unknown`; this is because `re.compile()` is also an overloaded function, and it seems like passing an object of type `Unknown` is also causing us to return early at step (5) of the algorithm for that function.

(I'm not sure it _should_ be -- `Pattern[Unknown]` would be a fine type for us to infer here IMO, since step (5) of the overload evaluation algorithm only specifies that you need to stop at that step if the return types of all remaining overloads are not equivalent. For `re.compile()`, they arguably _are_ all equivalent -- they're all `Pattern[str]` -- so I think we could probably proceed to step (6) here. But that's a separate issue.)

Anyway, the fact that we now infer `exclude` as being `Unknown` rather than `Pattern[str]` means that a true positive on `main` goes away on this branch lower down in the function.

## `kopf`

We issue these `redundant-cast` diagnostics on `main` because we incorrectly view `Iterator[Unknown]` and `Iterator[bodies.Body]` as being equivalent types. Ideally of course we'd improve our TypeVar solving for protocols so that we'd infer a better type than `Iterator[Unknown]` for the result of the `dicts.walk()` call [here](https://github.com/nolar/kopf/blob/ca3e0d0e2d3147c6728f3f01c4c7d2524f654b36/kopf/_core/engines/posting.py#L99), but that's beyond the scope of this PR



---

_Renamed from "[ty] Various fixes for generic protocols" to "[ty] Fix the inferred interface of specialized generic protocols" by @AlexWaygood on 2025-08-27 12:22_

---

_Marked ready for review by @AlexWaygood on 2025-08-27 12:33_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-27 12:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-27 12:33_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-27 12:33_

---

_Comment by @AlexWaygood on 2025-08-27 12:33_

Okay, I think this is now ready for review! I've updated the PR description.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:1076 on 2025-08-27 16:30_

Could add a TODO comment here that `Any & (str | LiteralString)` (simplifying in this case to `Any & str`) would also be a valid choice that's a bit more precise than the spec but still respects the gradual guarantee.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1117 on 2025-08-27 16:33_

What about generic protocols with specializations that are different but equivalent? (Can be a TODO for this PR, but I think worth testing; we have some issues with this in other areas.)

---

_@carljm approved on 2025-08-27 16:35_

Nice!

---

_@AlexWaygood reviewed on 2025-08-27 17:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1117 on 2025-08-27 17:11_

added some extreme stress tests in https://github.com/astral-sh/ruff/pull/19866/commits/eec874b1dbe6afee11adc0df4b9ffe3bb41a131b! Pretty cool that they already all pass as expected 😃

---

_Merged by @AlexWaygood on 2025-08-27 17:16_

---

_Closed by @AlexWaygood on 2025-08-27 17:16_

---

_Branch deleted on 2025-08-27 17:16_

---
