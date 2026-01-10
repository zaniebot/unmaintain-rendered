```yaml
number: 18317
title: "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant 3"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/fix-350-3
created_at: 2025-05-26T13:13:14Z
updated_at: 2025-05-26T13:46:41Z
url: https://github.com/astral-sh/ruff/pull/18317
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant 3

---

_Pull request opened by @sharkdp on 2025-05-26 13:13_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-05-26 13:13_

---

_Comment by @github-actions[bot] on 2025-05-26 13:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ error[unresolved-attribute] beartype/_check/forward/fwdresolve.py:266:8: Type `BeartypeDecorMeta` has no attribute `func_wrappee_scope_forward`
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
+ error[unresolved-attribute] beartype/_check/forward/fwdresolve.py:575:5: Unresolved attribute `func_wrappee_scope_forward` on type `BeartypeDecorMeta`.
+ error[unresolved-attribute] beartype/_check/forward/fwdresolve.py:584:5: Type `BeartypeDecorMeta` has no attribute `func_wrappee_scope_forward`
+ error[unresolved-attribute] beartype/_check/forward/fwdresolve.py:585:5: Type `BeartypeDecorMeta` has no attribute `func_wrappee_scope_forward`
+ error[unresolved-attribute] beartype/_check/forward/fwdresolve.py:744:36: Type `BeartypeDecorMeta` has no attribute `func_wrappee_scope_forward`
+ error[unresolved-attribute] beartype/_check/forward/fwdresolve.py:851:29: Type `BeartypeDecorMeta` has no attribute `func_wrappee_scope_forward`
+ warning[possibly-unbound-attribute] beartype/claw/_importlib/_clawimpload.py:371:9: Attribute `module_name_to_beartype_conf` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgcontext.py:97:17: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgcontext.py:101:13: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgcontext.py:117:16: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgcontext.py:119:17: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkghook.py:231:38: Attribute `packages_trie_blacklist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkghook.py:313:17: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkghook.py:319:9: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkghook.py:383:38: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:393:12: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:398:25: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:408:25: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:431:9: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:434:14: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:484:9: Attribute `packages_trie_blacklist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:607:31: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/claw/_package/clawpkgtrie.py:689:9: Attribute `packages_trie_whitelist` on type `Unknown | BeartypeClawState` is possibly unbound
- Found 551 diagnostics
+ Found 572 diagnostics

kopf (https://github.com/nolar/kopf)
+ error[invalid-argument-type] kopf/_core/intents/handlers.py:32:33: Argument to function `resolve` is incorrect: Expected `Mapping[Any, Any] | None`, found `BodyEssence | None`
+ error[invalid-argument-type] kopf/_core/intents/handlers.py:33:33: Argument to function `resolve` is incorrect: Expected `Mapping[Any, Any] | None`, found `BodyEssence | None`
+ error[invalid-argument-type] kopf/_core/intents/registries.py:497:29: Argument to function `resolve` is incorrect: Expected `Mapping[Any, Any] | None`, found `BodyEssence | None`
+ error[invalid-argument-type] kopf/_core/intents/registries.py:498:29: Argument to function `resolve` is incorrect: Expected `Mapping[Any, Any] | None`, found `BodyEssence | None`
+ error[invalid-argument-type] kopf/_core/intents/registries.py:529:25: Argument to function `resolve` is incorrect: Expected `Mapping[Any, Any] | None`, found `BodyEssence | None`
+ error[invalid-argument-type] kopf/_core/intents/registries.py:530:25: Argument to function `resolve` is incorrect: Expected `Mapping[Any, Any] | None`, found `BodyEssence | None`
- error[invalid-argument-type] kopf/_core/reactor/processing.py:443:81: Argument to bound method `store` is incorrect: Expected `BodyEssence`, found `@Todo(TypedDict) | None`
+ error[invalid-argument-type] kopf/_core/reactor/processing.py:443:81: Argument to bound method `store` is incorrect: Expected `BodyEssence`, found `BodyEssence | None`
- Found 167 diagnostics
+ Found 173 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] src/graphql/utilities/value_from_ast.py:138:58: Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & ~AlwaysFalsy`
- error[missing-argument] src/graphql/utilities/value_from_ast.py:140:26: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] src/graphql/validation/rules/values_of_correct_type.py:162:28: No argument provided for required parameter `node` of function `parse_literal`
+ warning[unused-ignore-comment] tests/type/test_definition.py:138:44: Unused blanket `type: ignore` directive
- error[missing-argument] tests/type/test_definition.py:152:16: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_definition.py:154:13: No argument provided for required parameter `node` of function `parse_literal`
- error[invalid-argument-type] tests/type/test_definition.py:158:72: Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown, Unknown]`
- error[missing-argument] tests/type/test_scalars.py:60:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:222:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:348:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:471:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:616:24: No argument provided for required parameter `node` of function `parse_literal`
- Found 407 diagnostics
+ Found 397 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ error[unresolved-attribute] strawberry/federation/schema.py:163:24: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/federation/schema.py:164:37: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/federation/schema.py:186:31: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:567:36: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:652:24: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:653:17: Type `StrawberryObjectDefinition` has no attribute `origin`
- warning[possibly-unbound-attribute] strawberry/schema/schema_converter.py:663:24: Attribute `origin` on type `StrawberryObjectDefinition | None` is possibly unbound
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:663:24: Type `StrawberryObjectDefinition | None` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:953:41: Type `StrawberryObjectDefinition` has no attribute `origin`
- warning[possibly-unbound-attribute] strawberry/schema/schema_converter.py:958:24: Attribute `origin` on type `StrawberryObjectDefinition | None` is possibly unbound
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:958:24: Type `StrawberryObjectDefinition | None` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:962:40: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:1039:28: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/schema/schema_converter.py:1046:29: Type `StrawberryObjectDefinition` has no attribute `origin`
+ error[unresolved-attribute] strawberry/types/base.py:331:9: Unresolved attribute `origin` on type `StrawberryObjectDefinition`.
- Found 433 diagnostics
+ Found 444 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ error[unresolved-attribute] pydantic/_internal/_decorators.py:483:37: Type `Decorator[FieldSerializerDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:220:93: Type `Decorator[FieldDecoratorInfoType]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:1230:36: Type `FieldInfo` has no attribute `annotation`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:1230:36: Type `FieldInfo` has no attribute `annotation`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:1230:36: Type `FieldInfo` has no attribute `annotation`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:1239:66: Type `Decorator[FieldValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:1259:69: Type `Decorator[ValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2053:12: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2054:27: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2073:9: Unresolved attribute `info` on type `Decorator[ComputedFieldInfo]`.
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2073:38: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2081:92: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2081:92: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2081:92: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2089:69: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2370:54: Type `Decorator[RootValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2373:38: Type `Decorator[RootValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2388:12: Type `Decorator[ValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2414:32: Type `Decorator[ModelValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2416:32: Type `Decorator[ModelValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2418:54: Type `Decorator[ModelValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2419:12: Type `Decorator[ModelValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2424:14: Type `Decorator[ModelValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2430:20: Type `Decorator[ModelValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_model_construction.py:227:20: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/_internal/_model_construction.py:641:44: Type `Decorator[ComputedFieldInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/functional_validators.py:151:36: Type `Decorator[FieldValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/functional_validators.py:247:36: Type `Decorator[FieldValidatorDecoratorInfo]` has no attribute `info`
+ error[unresolved-attribute] pydantic/functional_validators.py:322:36: Type `Decorator[FieldValidatorDecoratorInfo]` has no attribute `info`
- Found 762 diagnostics
+ Found 791 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ error[invalid-assignment] src/urllib3/contrib/emscripten/connection.py:254:5: Object of type `EmscriptenHTTPConnection` is not assignable to `BaseHTTPConnection`
+ error[invalid-assignment] src/urllib3/contrib/emscripten/connection.py:255:5: Object of type `EmscriptenHTTPSConnection` is not assignable to `BaseHTTPSConnection`
- Found 449 diagnostics
+ Found 451 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[missing-argument] mitmproxy/proxy/layers/modes.py:29:20: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] mitmproxy/proxy/layers/modes.py:37:20: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] test/mitmproxy/proxy/test_layer.py:170:17: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] test/mitmproxy/proxy/test_layer.py:170:17: No argument provided for required parameter `event` of function `handle_event`
- Found 2114 diagnostics
+ Found 2110 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[unused-ignore-comment] static_frame/core/index_hierarchy.py:3043:81: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] static_frame/core/index_hierarchy.py:3019:9: Unresolved attribute `container` on type `TVIHAsType`.
+ error[unresolved-attribute] static_frame/core/index_hierarchy.py:3031:21: Type `TVIHAsType` has no attribute `container`
+ error[unresolved-attribute] static_frame/core/index_hierarchy.py:3038:65: Type `TVIHAsType` has no attribute `container`
+ error[unresolved-attribute] static_frame/core/index_hierarchy.py:3039:19: Type `TVIHAsType` has no attribute `container`
+ error[unresolved-attribute] static_frame/core/index_hierarchy.py:3048:62: Type `TVIHAsType` has no attribute `container`
+ error[unresolved-attribute] static_frame/core/index_hierarchy.py:3068:22: Type `TVIHAsType` has no attribute `container`
+ error[unresolved-attribute] static_frame/test/unit/test_memory_measure_getsizeof.py:605:13: Type `Quilt` has no attribute `_axis_hierarchy`
+ error[unresolved-attribute] static_frame/test/unit/test_memory_measure_getsizeof.py:634:13: Type `Quilt` has no attribute `_axis_hierarchy`
- Found 2067 diagnostics
+ Found 2074 diagnostics

rotki (https://github.com/rotki/rotki)
+ error[unresolved-attribute] rotkehlchen/api/rest.py:1924:28: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:712:104: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:727:70: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:804:23: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:829:21: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:874:29: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
- Found 2040 diagnostics
+ Found 2046 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[missing-argument] tests/profiling/test_recorder.py:43:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:23:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:44:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:55:5: No argument provided for required parameter `events` of function `push_events`
- Found 6818 diagnostics
+ Found 6814 diagnostics

zulip (https://github.com/zulip/zulip)
+ warning[possibly-unbound-attribute] zerver/views/health.py:27:16: Attribute `connection` on type `SimpleQueueClient | TornadoQueueClient` is possibly unbound
- Found 6987 diagnostics
+ Found 6988 diagnostics

sympy (https://github.com/sympy/sympy)
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
- Found 18672 diagnostics
+ Found 18676 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[missing-argument] examples/script/aarch64/hello42.py:43:5: No argument provided for required parameter `callback` of function `subscribe`
- error[missing-argument] examples/script/aarch64/hello42.py:47:5: No argument provided for required parameter `callback` of function `subscribe`
- error[missing-argument] examples/script/basic_statemerging.py:27:5: No argument provided for required parameter `callback` of function `subscribe`
- error[missing-argument] examples/script/basic_statemerging.py:28:5: No argument provided for required parameter `callback` of function `subscribe`
- Found 1232 diagnostics
+ Found 1228 diagnostics

```
</details>


---
