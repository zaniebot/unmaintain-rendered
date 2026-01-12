```yaml
number: 18847
title: "[ty] Improve protocol member type checking and relation handling"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: protocol-member-checks
created_at: 2025-06-21T03:57:30Z
updated_at: 2025-06-29T15:50:47Z
url: https://github.com/astral-sh/ruff/pull/18847
synced_at: 2026-01-12T15:56:26Z
```

# [ty] Improve protocol member type checking and relation handling

---

_@mtshiba_

## Summary

This PR adds handling for checking the types of protocol members to `Type::satisfies_protocol` and `Type::is_disjoint_from`.
It only adds checks for simple members, not methods or properties for now.

The reason for adding this change is that `Protocol` member checks are required to implement advanced attribute narrowing (see https://github.com/astral-sh/ty/issues/643). I understand that there are some issues with the current `Protocol` support that need to be resolved, and if adding this PR would get in the way of solving those issues, I would like to wait until those issues are resolved.

## Test Plan

`Protocol` tests are added.


---

_Comment by @github-actions[bot] on 2025-06-21 04:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

werkzeug (https://github.com/pallets/werkzeug)
- error[non-subscriptable] src/werkzeug/utils.py:91:13: Cannot subscript object of type `object` with no `__getitem__` method
+ error[non-subscriptable] src/werkzeug/utils.py:91:13: Cannot subscript object of type `property` with no `__getitem__` method
- error[non-subscriptable] src/werkzeug/utils.py:118:17: Cannot subscript object of type `object` with no `__getitem__` method
+ error[non-subscriptable] src/werkzeug/utils.py:118:17: Cannot subscript object of type `property` with no `__getitem__` method

black (https://github.com/psf/black)
+ warning[possibly-unbound-attribute] src/blackd/__init__.py:104:12: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] src/blackd/__init__.py:110:12: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-argument-type] src/blackd/__init__.py:113:31: Argument to function `parse_mode` is incorrect: Expected `MultiMapping[str]`, found `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] src/blackd/__init__.py:116:27: Attribute `read` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] src/blackd/__init__.py:145:26: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[unsupported-operator] src/blackd/middlewares.py:15:39: Operator `in` is not supported for types `str` and `under_cached_property[Unknown]`, in comparing `Literal["Access-Control-Request-Method"]` with `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] src/blackd/middlewares.py:21:18: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 66 diagnostics
+ Found 73 diagnostics

trio (https://github.com/python-trio/trio)
+ error[invalid-assignment] src/trio/_util.py:221:17: Invalid assignment to data descriptor attribute `__name__` on type `<Protocol with members '__name__'>` with custom `__set__` method
+ error[invalid-assignment] src/trio/_util.py:223:21: Object of type `str` is not assignable to attribute `__qualname__` on type `<Protocol with members '__name__'> & <Protocol with members '__qualname__'>`
- Found 888 diagnostics
+ Found 890 diagnostics

rich (https://github.com/Textualize/rich)
+ error[invalid-argument-type] rich/layout.py:113:46: Argument to function `ratio_resolve` is incorrect: Expected `Sequence[Edge]`, found `Sequence[Layout]`
+ error[invalid-argument-type] rich/layout.py:133:48: Argument to function `ratio_resolve` is incorrect: Expected `Sequence[Edge]`, found `Sequence[Layout]`
- Found 327 diagnostics
+ Found 329 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:932:24: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & type & ~type[HydraConf]) | (DataClass_ & type & ~type[HydraConf])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:932:24: Return type does not match returned value: expected `DataClass_ | ListConfig | DictConfig`, found `(((...) -> Any) & type & ~type[HydraConf]) | (DataClass_ & type & ~type[HydraConf]) | (ListConfig & type & ~type[HydraConf]) | (DictConfig & type & ~type[HydraConf])`
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:941:16: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | list[Any] | dict[Any, Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:941:16: Return type does not match returned value: expected `DataClass_ | ListConfig | DictConfig`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | list[Any] | dict[Any, Any] | (ListConfig & ~type) | (DictConfig & ~type)`
- error[invalid-parameter-default] src/hydra_zen/wrapper/_implementations.py:1479:9: Default value of type `def default_to_config(target: ((...) -> Any) | DataClass_ | list[Any] | dict[Any, Any], CustomBuildsFn: @Todo(unsupported type[X] special form) = <class 'DefaultBuilds'>, **kw: Any) -> DataClass_` is not assignable to annotated parameter type `(F, /) -> @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-parameter-default] src/hydra_zen/wrapper/_implementations.py:1479:9: Default value of type `def default_to_config(target: ((...) -> Any) | DataClass_ | list[Any] | dict[Any, Any] | ListConfig | DictConfig, CustomBuildsFn: @Todo(unsupported type[X] special form) = <class 'DefaultBuilds'>, **kw: Any) -> DataClass_ | ListConfig | DictConfig` is not assignable to annotated parameter type `(F, /) -> @Todo(Support for `typing.TypeAlias`)`
- warning[unused-ignore-comment] tests/annotations/declarations.py:462:23: Unused blanket `type: ignore` directive
+ error[invalid-assignment] tests/annotations/declarations.py:461:5: Object of type `partial[Unknown]` is not assignable to `Partial[int]`
- warning[unused-ignore-comment] tests/annotations/declarations.py:473:42: Unused blanket `type: ignore` directive
+ error[invalid-assignment] tests/annotations/declarations.py:471:5: Object of type `partial[Unknown]` is not assignable to `Partial[int]`
+ error[invalid-assignment] tests/annotations/declarations.py:472:5: Object of type `partial[Unknown]` is not assignable to `Partial[bool]`
- Found 597 diagnostics
+ Found 598 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ error[invalid-argument-type] strawberry/cli/commands/export_schema.py:32:32: Argument to function `print_schema` is incorrect: Expected `BaseSchema`, found `Schema`
+ error[invalid-argument-type] strawberry/cli/debug_server.py:25:33: Argument to bound method `__init__` is incorrect: Expected `BaseSchema`, found `Schema`
+ error[invalid-argument-type] strawberry/schema_codegen/__init__.py:199:34: Argument to function `_get_directives` is incorrect: Expected `HasDirectives`, found `FieldDefinitionNode | InputValueDefinitionNode`
+ error[invalid-argument-type] strawberry/schema_codegen/__init__.py:387:34: Argument to function `_get_directives` is incorrect: Expected `HasDirectives`, found `ObjectTypeDefinitionNode | ObjectTypeExtensionNode | InterfaceTypeDefinitionNode | InputObjectTypeDefinitionNode`
- Found 362 diagnostics
+ Found 366 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ error[invalid-assignment] pymongo/ssl_support.py:100:13: Object of type `bool` is not assignable to attribute `check_ocsp_endpoint` on type `SSLContext | (SSLContext & <Protocol with members 'check_ocsp_endpoint'>)`
- Found 455 diagnostics
+ Found 456 diagnostics

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB
-     memo fields = ~106MB
+     memo fields = ~97MB

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ warning[possibly-unbound-attribute] aiohttp_devtools/runserver/log_handlers.py:35:75: Attribute `startswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp_devtools/runserver/log_handlers.py:35:99: Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[no-matching-overload] aiohttp_devtools/runserver/log_handlers.py:60:32: No overload of bound method `__init__` matches arguments
+ warning[possibly-unbound-attribute] aiohttp_devtools/runserver/serve.py:76:20: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp_devtools/runserver/serve.py:84:24: Attribute `startswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[non-subscriptable] aiohttp_devtools/runserver/serve.py:365:35: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ error[non-subscriptable] aiohttp_devtools/runserver/serve.py:375:17: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ error[non-subscriptable] aiohttp_devtools/runserver/serve.py:381:25: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ error[non-subscriptable] aiohttp_devtools/runserver/serve.py:442:17: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- Found 50 diagnostics
+ Found 59 diagnostics

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

colour (https://github.com/colour-science/colour)
+ error[invalid-assignment] colour/utilities/array.py:892:13: Object of type `@Todo(unsupported type[X] special form)` is not assignable to attribute `DTYPE_INT_DEFAULT` on type `ModuleType & <Protocol with members 'DTYPE_INT_DEFAULT'>`
+ error[invalid-assignment] colour/utilities/array.py:942:13: Object of type `@Todo(unsupported type[X] special form)` is not assignable to attribute `DTYPE_FLOAT_DEFAULT` on type `ModuleType & <Protocol with members 'DTYPE_FLOAT_DEFAULT'>`
- Found 498 diagnostics
+ Found 500 diagnostics

altair (https://github.com/vega/altair)
- error[invalid-argument-type] altair/utils/data.py:362:35: Argument to function `sanitize_geo_interface` is incorrect: Expected `MutableMapping[Any, Any]`, found `Series[Unknown] | @Todo(map_with_boundness: intersections with negative contributions)`
- Found 1277 diagnostics
+ Found 1276 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

discord.py (https://github.com/Rapptz/discord.py)
+ error[invalid-argument-type] discord/asset.py:174:39: Argument to bound method `__init__` is incorrect: Expected `str | None`, found `str | None | Any | under_cached_property[Unknown]`
+ error[no-matching-overload] discord/asset.py:419:19: No overload of function `splitext` matches arguments
+ error[no-matching-overload] discord/asset.py:504:19: No overload of function `splitext` matches arguments
- warning[unused-ignore-comment] discord/embeds.py:376:58: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] discord/embeds.py:432:62: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] discord/embeds.py:475:66: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] discord/embeds.py:518:62: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] discord/embeds.py:529:60: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] discord/embeds.py:540:58: Unused blanket `type: ignore` directive
+ error[non-subscriptable] discord/http.py:112:12: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ warning[possibly-unbound-attribute] discord/http.py:392:26: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] discord/http.py:395:38: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] discord/http.py:397:34: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] discord/http.py:400:23: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[non-subscriptable] discord/http.py:404:59: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ error[non-subscriptable] discord/utils.py:893:20: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ warning[possibly-unbound-attribute] discord/utils.py:894:24: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 549 diagnostics
+ Found 554 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ warning[possibly-unbound-attribute] aiohttp/client_middleware_digest_auth.py:397:23: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/client_middleware_digest_auth.py:426:18: Attribute `origin` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-argument-type] aiohttp/client_reqrep.py:961:43: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
+ error[invalid-argument-type] aiohttp/client_reqrep.py:961:59: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
+ warning[possibly-unbound-attribute] aiohttp/connector.py:1393:12: Attribute `endswith` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/connector.py:1394:20: Attribute `rstrip` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/connector.py:1415:17: Attribute `rstrip` on type `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Unknown & ~None) | under_cached_property[Unknown] | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound
+ error[invalid-argument-type] aiohttp/cookiejar.py:237:47: Argument to function `is_ip_address` is incorrect: Expected `str | None`, found `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] aiohttp/cookiejar.py:276:24: Attribute `startswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[non-subscriptable] aiohttp/cookiejar.py:280:34: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ warning[possibly-unbound-attribute] aiohttp/cookiejar.py:280:43: Attribute `rfind` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-argument-type] aiohttp/cookiejar.py:350:26: Argument to function `is_ip_address` is incorrect: Expected `str | None`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
+ warning[possibly-unbound-attribute] aiohttp/cookiejar.py:361:38: Attribute `split` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-argument-type] aiohttp/cookiejar.py:365:24: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | under_cached_property[Unknown]`
+ error[invalid-argument-type] aiohttp/helpers.py:199:43: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
+ error[invalid-argument-type] aiohttp/helpers.py:199:59: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
+ error[invalid-argument-type] aiohttp/helpers.py:310:46: Argument to function `proxy_bypass` is incorrect: Expected `str`, found `(Unknown & ~None) | under_cached_property[Unknown]`
+ error[call-non-callable] aiohttp/helpers.py:315:22: Method `__getitem__` of type `bound method dict[str, ProxyInfo].__getitem__(key: str, /) -> ProxyInfo` is not callable on object of type `dict[str, ProxyInfo]`
+ warning[possibly-unbound-attribute] aiohttp/web_app.py:394:12: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_fileresponse.py:206:31: Attribute `timestamp` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_fileresponse.py:219:32: Attribute `timestamp` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_fileresponse.py:255:27: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_fileresponse.py:309:67: Attribute `timestamp` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_fileresponse.py:319:25: Attribute `start` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_fileresponse.py:320:38: Attribute `stop` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_log.py:125:16: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-return-type] aiohttp/web_log.py:137:16: Return type does not match returned value: expected `str`, found `(Unknown & ~None) | under_cached_property[Unknown] | Literal["-"]`
+ warning[possibly-unbound-attribute] aiohttp/web_log.py:155:13: Attribute `major` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_log.py:156:13: Attribute `minor` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_middlewares.py:84:23: Attribute `route` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[unsupported-operator] aiohttp/web_middlewares.py:86:16: Operator `in` is not supported for types `str` and `under_cached_property[Unknown]`, in comparing `Literal["?"]` with `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] aiohttp/web_middlewares.py:87:31: Attribute `split` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[no-matching-overload] aiohttp/web_middlewares.py:94:39: No overload of function `sub` matches arguments
+ warning[possibly-unbound-attribute] aiohttp/web_middlewares.py:95:37: Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[unsupported-operator] aiohttp/web_middlewares.py:96:39: Operator `+` is unsupported between objects of type `Unknown | under_cached_property[Unknown]` and `Literal["/"]`
+ warning[possibly-unbound-attribute] aiohttp/web_middlewares.py:97:33: Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[non-subscriptable] aiohttp/web_middlewares.py:98:39: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ error[unsupported-operator] aiohttp/web_middlewares.py:100:58: Operator `+` is unsupported between objects of type `Unknown | under_cached_property[Unknown]` and `Literal["/"]`
+ warning[possibly-unbound-attribute] aiohttp/web_middlewares.py:101:51: Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[no-matching-overload] aiohttp/web_middlewares.py:102:34: No overload of function `sub` matches arguments
+ warning[possibly-unbound-attribute] aiohttp/web_middlewares.py:119:16: Attribute `current_app` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-assignment] aiohttp/web_middlewares.py:120:9: Object of type `Application` is not assignable to attribute `current_app` on type `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] aiohttp/web_protocol.py:791:31: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_response.py:348:27: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-argument-type] aiohttp/web_response.py:689:45: Argument to function `should_remove_content_length` is incorrect: Expected `str`, found `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:321:14: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:366:39: Attribute `path_safe` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:625:16: Attribute `path_safe` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[non-subscriptable] aiohttp/web_urldispatcher.py:644:19: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:814:55: Attribute `split` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-return-type] aiohttp/web_urldispatcher.py:817:20: Return type does not match returned value: expected `str`, found `(Unknown & ~None) | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:821:16: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:1014:20: Attribute `path_safe` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[invalid-argument-type] aiohttp/web_urldispatcher.py:1042:56: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | under_cached_property[Unknown]`
+ error[invalid-return-type] aiohttp/web_urldispatcher.py:1260:12: Return type does not match returned value: expected `str`, found `Unknown | under_cached_property[Unknown]`
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:229:27: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:234:26: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:237:29: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:240:21: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ error[unsupported-operator] aiohttp/web_ws.py:246:12: Operator `in` is not supported for types `istr` and `under_cached_property[Unknown]`, in comparing `istr` with `Unknown | under_cached_property[Unknown]`
+ error[non-subscriptable] aiohttp/web_ws.py:249:30: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:266:19: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:271:15: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] aiohttp/web_ws.py:292:26: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 133 diagnostics
+ Found 197 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[invalid-assignment] ddtrace/appsec/_iast/_ast/visitor.py:669:17: Object of type `Load` is not assignable to attribute `ctx` on type `expr & <Protocol with members 'ctx'>`
- Found 6839 diagnostics
+ Found 6840 diagnostics

sympy (https://github.com/sympy/sympy)
-     memo fields = ~1399MB
+     memo fields = ~1538MB

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-06-21 08:47_

---

_Comment by @codspeed-hq[bot] on 2025-06-23 13:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aprotocol-member-checks?runnerMode=Instrumentation)

### Merging #18847 will **not alter performance**

<sub>Comparing <code>mtshiba:protocol-member-checks</code> (0beafec) with <code>main</code> (9218bf7)</sub>



### Summary

`✅ 39` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-06-23 13:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aprotocol-member-checks?runnerMode=WallTime)

### Merging #18847 will **not alter performance**

<sub>Comparing <code>mtshiba:protocol-member-checks</code> (0beafec) with <code>main</code> (9218bf7)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Marked ready for review by @mtshiba on 2025-06-23 14:16_

---

_Review requested from @carljm by @mtshiba on 2025-06-23 14:16_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-06-23 14:16_

---

_Review requested from @sharkdp by @mtshiba on 2025-06-23 14:16_

---

_Review requested from @dcreager by @mtshiba on 2025-06-23 14:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1968 on 2025-06-26 14:37_

shouldn't these be `TypeRelation::Assignability` rather than `TypeRelation::Subtyping`?

If I have a gradual protocol like this:

```py
class Foo(Protocol):
    def __bool__(self) -> Any: ...
```

Then the type `Literal[True]` won't be a subtype of that protocol, but it will be assignable to that protocol. `Literal[True]` isn't disjoint from this protocol

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:71 on 2025-06-26 14:46_

this isn't really correct for attribute members, because attribute members on protocols are invariant rather than covariant. In order for a protocol member to be covariant, it either needs to be a read-only `property` member or a method member. We also need to consider the type of the attribute on the meta-type as well, unfortunately, in order to make meta-protocols sound.

For a protocol like this:

```py
class Foo(Protocol):
    x: int
```

What this says is, "To satisfy this interface, a type `T` must have a mutable attribute `x` that can have `int` instances assigned to it and, when read, yields an instance of `int`. The `x` attribute only needs to be available on inhabitants of `T`".

For a protocol like this:

```py
class Bar(Protocol):
    x: str = "foo"
```

What this says is, "To satisfy this interface, a type `U` must have a mutable attribute `x` that can have `str` instances assigned to it and, when read, yields an instance of `str`. the `x` attribute needs to be readable on inhabitants of `U` _and_ inhabitants of `U`'s meta-type".

For a protocol like this:

```py
class Baz(Protocol):
    x: ClassVar[bytes]
```

What this says is, "To satisfy this interface, a type `V` must have a mutable attribute `x` that, when read, yields an instance of `str`. It does not need to be writable on inhabitants of `V` but it must be readable and writable on `T`'s meta-type."

---

For now, I think it's okay to leave a lot of the logic about protocol meta-types as a TODO, but it would be great to at least reflect the fact that mutable attributes need to be treated invariantly. I.e., in the following snippet, `Bar` is not a subtype of `Foo`, because `Foo` has a mutable attribute member `x`, and the type of `Bar.x` is a subtype of the type of `Foo.x` rather than being exactly equivalent to the type of `Foo.x`:

```py
class Foo(Protocol):
    x: int

class Bar:
    def __init__(self):
        self.x = True
```

---

_@AlexWaygood reviewed on 2025-06-26 14:48_

Thanks! This looks okay overall, but I think it would be nice to implement invariance of mutable attribute members; it isn't really correct to treat them covariantly.

You could also consider adding a `ProtocolMemberKind` enum like I did in #18659? I think we'll need some way of distinguishing between method members, property members and everything else anyway, and your PR already starts to add that distinction

---

_Comment by @AlexWaygood on 2025-06-26 14:49_

Sorry for the delayed review!!

---

_Comment by @AlexWaygood on 2025-06-26 14:55_

> The reason for adding this change is that `Protocol` member checks are required to implement advanced attribute narrowing (see [astral-sh/ty#643](https://github.com/astral-sh/ty/issues/643)).

I'm curious how you plan to use protocols to implement that feature. For something like this:

```py
class Foo:
    x: str

def f(foo: Foo):
    if foo.x == "bar":
        ...
```

It would be incorrect to narrow the type of `foo` to something like `Foo & SynthesizedProtocol[{"x": Literal["bar"]}]` there. The reason is that an instance of `Foo` could have an `x` attribute that's a `str` subclass which compares equal to `"bar"`, but for an object to inhabit `Literal["bar"]` its class must be _exactly_ `str` (not a subclass). So you can only narrow the type there if you know that the type prior to narrowing was a subtype of `LiteralString`, since all inhabitants of `LiteralString` have to be instances of exactly `str` (not a subclass of `str`).

---

_Comment by @mtshiba on 2025-06-26 16:30_

> I'm curious how you plan to use protocols to implement that feature. For something like this:
> 
> ```python
> class Foo:
>     x: str
> 
> def f(foo: Foo):
>     if foo.x == "bar":
>         ...
> ```
> 
> It would be incorrect to narrow the type of `foo` to something like `Foo & SynthesizedProtocol[{"x": Literal["bar"]}]` there. The reason is that an instance of `Foo` could have an `x` attribute that's a `str` subclass which compares equal to `"bar"`, but for an object to inhabit `Literal["bar"]` its class must be _exactly_ `str` (not a subclass). So you can only narrow the type there if you know that the type prior to narrowing was a subtype of `LiteralString`, since all inhabitants of `LiteralString` have to be instances of exactly `str` (not a subclass of `str`).

Yeah, you mean, for example, if such a "malicious" subclass is used, narrowing is unsound?

```python
class BadStr(str):
    def __eq__(self, other):
        return True

class Foo:
    tag: str = BadStr("Foo")
    value: int

class Bar:
    tag: str = BadStr("Bar")
    value: str

def _(x: Foo | Bar):
    if x.tag == "Foo":
        reveal_type(x.tag)  # str, not Literal["Foo"]
        reveal_type(x.value)  # str | int, not int
```

The determination that `x.tag == "Foo"` does not immediately imply `x.tag: Literal["Foo"]` (it does only if the LHS type is single-valued) is already implemented. So we can use it to assume `x: Protocol[{"tag": Literal["Foo"]}]` only if the constraint `x.tag: Literal["Foo"]` can be assumed.

---

_Comment by @AlexWaygood on 2025-06-26 16:48_

> Yeah, you mean, for example, if such a "malicious" subclass is used, narrowing is unsound?

Exactly, yes! Okay, it sounds like you're already aware of the issue, which is great 👍

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-27 08:16_

---

_@mtshiba reviewed on 2025-06-27 15:00_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/protocol_class.rs`:286 on 2025-06-27 15:00_

I cherry-picked your commit 63d34efa8f7e2df2d786608ca6065aba467a5b57 and made a slight change to avoid the stack overflow.
This is a temporary workaround, hopefully we'll see a complete solution in #18659.

---

_@mtshiba reviewed on 2025-06-27 16:12_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/instance.rs`:70 on 2025-06-27 16:12_

If `relation` is `Assignability`, I simply assumed that `ty` satisfies `protocol` only when it is `Dynamic`, but this may not be complete. A class with partially-typed methods in the following code doesn't satisfy the protocol. I think this goes against the gradual guarantee.

https://github.com/mtshiba/ruff/blob/019a1b05008fbb962c354fc31a97ca9b0f89da71/crates/ty_python_semantic/resources/mdtest/protocols.md?plain=1#L563-L571

To fix this, I think we need a check that `ty` can be equivalent to the protocol member type at a materialization
(~~I think this can be implemented by adding a option such as `MaterializationOption::{Exist, ForAll}` to `is_equivalent_to`~~ No, this function will be an asymmetric function, unlike `is_equivalent_to`)?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:70 on 2025-06-27 19:52_

I think the basic check here should just be to delegate to `member.ty().has_relation_to(db, ty, relation)`. The exception to this is a simple mutable attribute (e.g. `x: int`), which needs to be treated as invariant (effectively we need to model in this case that the protocol includes both a getter for `x` returning `int` and a setter accepting `int`). Equivalence is not the right relation to check for this when `relation` is `Assignability`, as you observe (it is too strict). I think the correct approach here is simply to check assignability in both directions (`member.ty().has_relation_to(db, ty, relation) && ty.has_relation_to(db, member.ty(), relation)`).

I think it might be clearer if this method `satisfies_protocol` were simply renamed `has_relation_to`, as that's what it implements. But that doesn't need to be in this PR.

---

_@carljm reviewed on 2025-06-27 19:53_

I made one inline comment here, but I think it's fine if this is merged with TODOs. I'd like @AlexWaygood to make the call on merging this, but let me know if there are any other parts of it you'd like me to review.

---

_@AlexWaygood reviewed on 2025-06-27 21:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:70 on 2025-06-27 21:28_

I pushed some changes to the PR to fix this up. Because of the invariance we need to check `attribute_type.has_relation_to(db, protocol_member_type)` _and_ `protocol_member_type.has_relation_to(db, attribute_type)`

---

_@AlexWaygood approved on 2025-06-27 21:29_

Thanks, this looks great to me! I pushed a few small changes.

Would you be able to take a quick look through the mypy_primer diff and see if the new diagnostics are all expected? Once that's done I'm happy to land this -- thank you!

---

_Comment by @mtshiba on 2025-06-28 08:15_

I found that the current protocol member check has a problem in the following case.

```python
def f(x: int | LiteralString):
    if hasattr(x, "capitalize"):
        reveal_type(x)  # should be: (int & <Protocol with members 'capitalize'>) | LiteralString
    else:
        reveal_type(x)  # should be: int & ~<Protocol with members 'capitalize'>
```

It is inferred like this.

```
info[revealed-type]: Revealed type
 --> protocol_check.py:5:21
  |
3 | def f(x: int | LiteralString):
4 |     if hasattr(x, "capitalize"):
5 |         reveal_type(x)
  |                     ^ `int & <Protocol with members 'capitalize'>`
6 |     else:
7 |         reveal_type(x)
  |

info[revealed-type]: Revealed type
 --> protocol_check.py:7:21
  |
5 |         reveal_type(x)
6 |     else:
7 |         reveal_type(x)
  |                     ^ `(int & ~<Protocol with members 'capitalize'>) | LiteralString`
  |
```

The protocol synthesized from the predicate `hasattr(x, "capitalize")` is `Protocol[{"capitalize": object}]`.
Currently, we check the compatibility of protocol members and attributes with [`member_type.has_relation_to(db, attribute_type, relation) && attribute_type.has_relation_to(db, *member_type, relation)`](https://github.com/mtshiba/ruff/blob/dfcf0e097188c953a881beb4e5244b1d2d911285/crates/ty_python_semantic/src/types/protocol_class.rs#L380-L381), which leads to the paradoxical situation where `LiteralString` does not satisfy the protocol.

---

_Comment by @AlexWaygood on 2025-06-28 08:39_

Ah, nice catch. The synthesised protocols created from `hasattr` narrowing should use covariant read-only property members rather than invariant mutable attribute members. I think just making that change should fix things there for now, though we'll obviously need to do a lot more work on property members in the future since they'll require some special-cased subtyping logic of their own

---

_Comment by @mtshiba on 2025-06-28 19:36_

Now all the new errors/warnings in mypy_primer look ok.
They are either due to checks working as intended (writing to `hasattr` attributes, etc.) or due to TODOs (`Protocol` members with default values, `@property` members, generic members).

---

_@AlexWaygood approved on 2025-06-29 10:43_

Thanks again -- this is great work!

---

_Comment by @AlexWaygood on 2025-06-29 10:44_

I think we'll need to invest into improving the diagnostic if somebody tries to write to an attribute on a synthesized protocol that has that attribute available, but only as a read-only property member. But we can defer that for now.

---

_Merged by @AlexWaygood on 2025-06-29 10:46_

---

_Closed by @AlexWaygood on 2025-06-29 10:46_

---

_Branch deleted on 2025-06-29 15:50_

---
