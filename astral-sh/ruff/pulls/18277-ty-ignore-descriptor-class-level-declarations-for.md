```yaml
number: 18277
title: "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant 2"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/fix-350-2
created_at: 2025-05-23T13:25:55Z
updated_at: 2025-05-26T12:57:17Z
url: https://github.com/astral-sh/ruff/pull/18277
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant 2

---

_@sharkdp_

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

_Label `ty` added by @sharkdp on 2025-05-23 13:26_

---

_Comment by @github-actions[bot] on 2025-05-23 13:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `Unknown | None` is possibly unbound

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
+ Found 788 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[missing-argument] mitmproxy/proxy/layers/modes.py:29:20: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] mitmproxy/proxy/layers/modes.py:37:20: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] test/mitmproxy/proxy/test_layer.py:170:17: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] test/mitmproxy/proxy/test_layer.py:170:17: No argument provided for required parameter `event` of function `handle_event`
- Found 2111 diagnostics
+ Found 2107 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ error[unresolved-attribute] lib/streamlit/elements/lib/built_in_chart_utils.py:340:37: Type `AddRowsMetadata` has no attribute `columns`
+ error[unresolved-attribute] lib/streamlit/elements/lib/built_in_chart_utils.py:340:37: Type `AddRowsMetadata` has no attribute `columns`
- Found 3260 diagnostics
+ Found 3262 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[missing-argument] tests/profiling/test_recorder.py:43:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:23:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:44:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:55:5: No argument provided for required parameter `events` of function `push_events`
- Found 6867 diagnostics
+ Found 6863 diagnostics

rotki (https://github.com/rotki/rotki)
+ error[unresolved-attribute] rotkehlchen/api/rest.py:1923:28: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
- error[invalid-argument-type] rotkehlchen/exchanges/bitstamp.py:358:25: Argument to bound method `edit_event_extra_data` is incorrect: Expected `Mapping[str, Any]`, found `AssetMovementExtraData | None | dict[Unknown, Unknown]`
+ error[invalid-argument-type] rotkehlchen/exchanges/bitstamp.py:358:25: Argument to bound method `edit_event_extra_data` is incorrect: Expected `Mapping[str, Any]`, found `Unknown | AssetMovementExtraData | None | dict[Unknown, Unknown]`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:712:104: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:727:70: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:804:23: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:829:21: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
+ error[unresolved-attribute] rotkehlchen/rotkehlchen.py:874:29: Type `SingleBlockchainAccountData[Unknown]` has no attribute `address`
- Found 2040 diagnostics
+ Found 2046 diagnostics

sympy (https://github.com/sympy/sympy)
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
+ error[unresolved-attribute] sympy/polys/puiseux.py:557:30: Type `PuiseuxPoly` has no attribute `poly`
- Found 18752 diagnostics
+ Found 18756 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[missing-argument] examples/script/aarch64/hello42.py:43:5: No argument provided for required parameter `callback` of function `subscribe`
+ error[call-non-callable] examples/script/aarch64/hello42.py:43:5: Object of type `None` is not callable
- error[missing-argument] examples/script/aarch64/hello42.py:47:5: No argument provided for required parameter `callback` of function `subscribe`
+ error[call-non-callable] examples/script/aarch64/hello42.py:47:5: Object of type `None` is not callable
- error[missing-argument] examples/script/basic_statemerging.py:27:5: No argument provided for required parameter `callback` of function `subscribe`
- error[missing-argument] examples/script/basic_statemerging.py:28:5: No argument provided for required parameter `callback` of function `subscribe`
+ error[call-non-callable] examples/script/basic_statemerging.py:27:5: Object of type `None` is not callable
+ error[call-non-callable] examples/script/basic_statemerging.py:28:5: Object of type `None` is not callable

```
</details>


---

_Renamed from "David/fix 350 2" to "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant" by @sharkdp on 2025-05-26 12:57_

---

_Renamed from "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant" to "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes, variant 2" by @sharkdp on 2025-05-26 12:57_

---
