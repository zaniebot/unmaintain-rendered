```yaml
number: 22495
title: "[ty] Generalize union-type subtyping fast path"
type: pull_request
state: open
author: AlexWaygood
labels:
  - performance
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: alex/union-fast-path-still-vec
created_at: 2026-01-10T15:30:40Z
updated_at: 2026-01-16T20:37:18Z
url: https://github.com/astral-sh/ruff/pull/22495
synced_at: 2026-01-16T21:04:11Z
```

# [ty] Generalize union-type subtyping fast path

---

_@AlexWaygood_

## Summary

Currently we have a branch near the top of `Type::has_relation_to_impl` that returns `ConstraintSet::from(true)` if the l.h.s. is a typevar `T` and the right-hand side is a union `U` where `T` is one of the elements of `U`:

https://github.com/astral-sh/ruff/blob/337e3ebd27407f50b96dd686631c4ac4697fa973/crates/ty_python_semantic/src/types/relation.rs#L463-L475

But this branch isn't only correct for type variables! In general, it is true for _any_ type `T` that it will be assignable to, redundant with, and -- in some cases -- a subtype of a union `U` if `T` is contained within the elements of `U`. The branch is only _necessary_ for type variables, because for non-type-variables we apply a more generalized handling lower down here:

https://github.com/astral-sh/ruff/blob/337e3ebd27407f50b96dd686631c4ac4697fa973/crates/ty_python_semantic/src/types/relation.rs#L730-L739

But the generalized handling for `T <: U` can be very slow in certain pathological cases (*cough* pydantic) where a union type contains very complicated types early on in its list of elements. For example, consider the following scenario: we want to check whether `C` is a subtype of `U`, and `U` is a union with the following elements list:

```
-----------
A | B | C |
-----------
```

Unfortunately, `A` and `B` are both complex recursive structural types (*cough* pydantic's huge `TypedDict`s), and checking whether `C` is a subtype of `A` or `B` is a question that takes us a long time to answer. But with our current generalized handling for determining whether `C <: U` for a union type `U`, we _must_ answer these questions before we even ask the question "Is `C` a subtype of `C`?", for which the answer is trivial.

By extending the trivial `if union.elements(db).contains(self)` check early on in `Type::has_relation_to()` to _all_ types (not just type variables), we can achieve a significant speedup over what we have on the `main` branch. Now, in the above example, we quickly iterate over the elements of `U` once, discover that `C` is trivially contained in `U`'s elements, and return `true` for the overall subtyping check without ever having to ask whether `C` is a subtype of `A` or `B`.

Essentially this means that for _any_ type `T`, if we want to check whether `T` is a subtype of a union `U` we _may_ iterate over the elements of the union twice: once for this fast path to check whether the union trivially contains `T` in its elements, and (if it is not trivially contained) once again to do the full (slow) `has_relation_to_impl` check for each element in the union.

It's perhaps surprising that iterating over the union elements twice would be consistently faster than iterating over the union elements once. However, the Codspeed results on this PR have very consistently shown some impressive speedups and no performance regressions.

## Behaviour change

As well as achieving a big speedup, this PR also (to my surprise) improves our type-variable solving for `Callable` types. I've added a regression test for this to the PR:

```py
from typing import Callable

class Box[T]:
    def get(self) -> T:
        raise NotImplementedError

def my_iter[T](f: Callable[[], T | None]) -> Box[T]:
    return Box()

def get_int() -> int | None: ...

reveal_type(my_iter(get_int))  # `main`: Box[int | None]`; PR: Box[int]
```

I've stared at it for a while, but I don't _fully_ understand why this PR fixes the bug. It makes me a bit nervous, because I worry that the bug is still there _somewhere_ else in our code, and that this PR merely papers over the bug somehow. But @carljm encouraged me to open this up for review, so that's what I'm doing! I'm curious if the reason for the behaviour change is obvious to somebody else.

This behaviour change has a significant, positive impact on the ecosystem, because of typeshed's third overload for `builtins.iter()`:

https://github.com/astral-sh/ruff/blob/337e3ebd27407f50b96dd686631c4ac4697fa973/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L3746-L3761

## Reduction in nondeterminism??

I _may_ be imagining it -- and it's very hard to tell -- but I think this PR _may_ reduce our level of nondeterminism? There are still some flakes in the mypy_primer report, but I _think_ the level of flakes on this PR has been consistently lower than I've seen on other ty PRs recently. Since we know that the number of flakes significantly increased after https://github.com/astral-sh/ruff/pull/21551, and we know that this PR impacts our behaviour when solving type variables in `Callable` types, I wonder if this _might_ also accidentally be improving the situation there somewhat?

It's again a bit of a mystery to me why this would be the case, however. It's also hard to confirm, since there is definitely still some flakiness in the ecosystem report (and there was _some_ flakiness in the report prior to #21551, too!).

## Reflexivity of subtyping for type variables

An implementation detail of this PR is that we currently return `false` from the `Type::subtyping_is_always_reflexive()` method on `main` if a type is a `Type::TypeVar` variant, but this PR changes that so we return `true` for `Type::TypeVar` variants.

The rationale for the current return-type of `false` is stated in this comment here:

https://github.com/astral-sh/ruff/blob/337e3ebd27407f50b96dd686631c4ac4697fa973/crates/ty_python_semantic/src/types/relation.rs#L491-L501

But the `subtyping_is_always_reflexive` method only has a single callsite on `main`, and that is here:

https://github.com/astral-sh/ruff/blob/337e3ebd27407f50b96dd686631c4ac4697fa973/crates/ty_python_semantic/src/types/relation.rs#L327-L334

and we also implement on `main` that `T` is always a subtype of `T | None` if `T` is a type variable, which I think is also only safe if we are able to assume that subtyping is always reflexive for `Type::TypeVar` variants. It therefore seems like we _de-facto_ treat `Type::TypeVar` variants in exactly the same way as all `Type` variants for which we return `true` from `Type::subtyping_is_always_reflexive()`, and that therefore life becomes much simpler if we simply return `true` from that method for `Type::TypeVar()` types.

I'm curious if I'm missing something here?

## Open questions

It's _possible_ that there are pathologically large unions where iterating over the union twice would mean that this new implementation of subtyping against union types is actually slower. Should we skip the fast path for non-type-variable types if the length of the union is above a certain arbitrary threshhold? Or is that something we should just leave for now, until we have hard evidence that this is a real problem?

I initially played around with using an `FxOrderSet` for `UnionType::elements()`, in https://github.com/astral-sh/ruff/pull/22458. That would make the `.contains()` fast path `O(1)` rather than `O(n)`, which would alleviate the concern about pathologically large unions. But (to my surprise) the Codspeed reports appear to show that this PR is ~as fast as #22458, and #22458 has a memory-usage regression which this PR does not. So for now, I am proposing adding the fast path while still keeping `UnionType::elements` as a boxed slice.

## Test Plan

- Existing tests
- One new mdtest for the improvement to typevar solving for `Callable` types


---

_Label `performance` added by @AlexWaygood on 2026-01-10 15:30_

---

_Label `ty` added by @AlexWaygood on 2026-01-10 15:30_

---

_Label `performance` added by @AlexWaygood on 2026-01-10 15:30_

---

_Label `ty` added by @AlexWaygood on 2026-01-10 15:30_

---

_Comment by @astral-sh-bot[bot] on 2026-01-10 15:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-10 15:33_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/spec_parser.py:472:16: error[invalid-return-type] Return type does not match returned value: expected `list[Spec]`, found `list[Spec | None]`
- Found 4345 diagnostics
+ Found 4344 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/code/codemain.py:1199:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 496 diagnostics
+ Found 495 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/config/__init__.py:1717:26: error[invalid-argument-type] Argument to function `assert_never` is incorrect: Expected `Never`, found `Unknown & ~Literal["ini"] & ~Literal["toml"]`
- Found 414 diagnostics
+ Found 413 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/inference.py:410:32: error[invalid-argument-type] Argument is incorrect: Expected `RestrictLexicon | None`, found `object`
- sockeye/inference.py:376:32: error[no-matching-overload] No overload of bound method `get` matches arguments
- sockeye/inference.py:376:32: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 416 diagnostics
+ Found 415 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/walk.py:474:35: error[invalid-argument-type] Argument to bound method `_reorder` is incorrect: Expected `Iterator[WalkEntry]`, found `Iterator[WalkEntry | None]`
- Found 231 diagnostics
+ Found 230 diagnostics

poetry (https://github.com/python-poetry/poetry)
- tests/console/commands/test_show.py:42:12: warning[redundant-cast] Value is already of type `F@output_format_parametrize`
- Found 978 diagnostics
+ Found 977 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `serializer` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `serializer` of type `SchemaSerializer`
- pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:1949:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- tanjun/annotations.py:1996:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- Found 133 diagnostics
+ Found 131 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/contrib/search/__init__.py:44:34: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `Never`, found `Unknown & ~Literal["en"]`
- mkdocs/contrib/search/__init__.py:48:34: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `Never`, found `Unknown & ~Literal["en"]`
- mkdocs/contrib/search/__init__.py:49:34: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `Unknown & ~AlwaysFalsy`
- Found 223 diagnostics
+ Found 220 diagnostics

altair (https://github.com/vega/altair)
- altair/datasets/_reader.py:335:16: error[invalid-argument-type] Argument to bound method `lazy` is incorrect: Argument type `IntoFrameT@Reader` does not satisfy upper bound `LazyFrame[LazyFrameT@LazyFrame]` of type variable `Self`
- Found 1059 diagnostics
+ Found 1058 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/lib/markdown/__init__.py:1083:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- zerver/lib/markdown/__init__.py:1087:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- zerver/lib/markdown/__init__.py:1107:27: error[not-iterable] Object of type `None` is not iterable
- zerver/lib/markdown/__init__.py:1149:50: error[invalid-argument-type] Argument to bound method `handle_video_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None]`
- zerver/lib/markdown/__init__.py:1159:50: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None] | ResultWithFamily[tuple[str, str]]`
+ zerver/lib/markdown/__init__.py:1159:50: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None]] | ResultWithFamily[tuple[str, str]]`
- zerver/lib/markdown/__init__.py:1171:56: error[invalid-argument-type] Argument to bound method `handle_youtube_url_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None]`
- zerver/lib/markdown/nested_code_blocks.py:30:58: error[invalid-argument-type] Argument to bound method `get_nested_code_blocks` is incorrect: Expected `list[ResultWithFamily[tuple[str, str | None]]]`, found `list[ResultWithFamily[tuple[str, str | None] | None]]`
- Found 3675 diagnostics
+ Found 3669 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/advantage_air/__init__.py:69:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
- homeassistant/components/airvisual/__init__.py:220:5: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `runtime_data` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/airvisual_pro/__init__.py:91:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, Any] | None]`
- homeassistant/components/asuswrt/router.py:104:16: error[invalid-return-type] Return type does not match returned value: expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, int] | None]`
- homeassistant/components/hvv_departures/binary_sensor.py:123:27: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/iammeter/sensor.py:146:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/iammeter/sensor.py:147:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/iammeter/sensor.py:151:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
- homeassistant/components/iammeter/sensor.py:156:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/led_ble/__init__.py:91:59: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/meteo_france/__init__.py:96:18: error[unresolved-attribute] Object of type `None` has no attribute `position`
+ homeassistant/components/meteo_france/__init__.py:96:18: error[unresolved-attribute] Object of type `dict[str, Any]` has no attribute `position`
- homeassistant/components/nut/__init__.py:119:40: error[invalid-argument-type] Argument to function `_unique_id_from_status` is incorrect: Expected `dict[str, str]`, found `dict[str, str] | None`
- homeassistant/components/nut/__init__.py:129:28: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `dict[str, str] | None`
- homeassistant/components/nut/__init__.py:156:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, str] | None]`
+ homeassistant/components/nws/__init__.py:133:9: error[invalid-argument-type] Argument is incorrect: Expected `TimestampDataUpdateCoordinator[None]`, found `TimestampDataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/nws/__init__.py:134:9: error[invalid-argument-type] Argument is incorrect: Expected `TimestampDataUpdateCoordinator[None]`, found `TimestampDataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/pi_hole/__init__.py:154:42: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/powerwall/__init__.py:236:43: error[invalid-assignment] Invalid assignment to key "coordinator" with declared type `DataUpdateCoordinator[PowerwallData] | None` on TypedDict `PowerwallRuntimeData`: value of type `DataUpdateCoordinator[PowerwallData | None]`
- homeassistant/components/renault/services.py:124:42: error[unresolved-attribute] Object of type `None` has no attribute `raw_data`
- homeassistant/components/renault/services.py:133:9: error[unresolved-attribute] Object of type `None` has no attribute `update`
- homeassistant/components/renault/services.py:136:16: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:138:47: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:153:9: error[unresolved-attribute] Object of type `None` has no attribute `update`
- homeassistant/components/renault/services.py:156:16: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:158:45: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
+ homeassistant/components/reolink/__init__.py:261:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/reolink/__init__.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/reolink/__init__.py:269:36: error[invalid-argument-type] Argument to function `register_callbacks` is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/schluter/climate.py:74:42: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/senz/__init__.py:79:46: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Unknown] | None]` is not assignable to `SENZDataUpdateCoordinator`
- homeassistant/components/smarttub/controller.py:73:9: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `coordinator` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/spotify/__init__.py:82:63: error[invalid-assignment] Object of type `DataUpdateCoordinator[list[Unknown] | None]` is not assignable to `DataUpdateCoordinator[list[Unknown]]`
- homeassistant/components/supla/__init__.py:123:36: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/tesla_wall_connector/__init__.py:71:42: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to `DataUpdateCoordinator[dict[str, Any]]`
- Found 14509 diagnostics
+ Found 14490 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2026-01-10 15:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **improve performance by 27.64%**




`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-fast-path-still-vec?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 10.4 s | 8.1 s | +27.64% |
---

<sub>Comparing <code>alex/union-fast-path-still-vec</code> (31d7e8c) with <code>main</code> (ed355b6)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-fast-path-still-vec?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-fast-path-still-vec?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @AlexWaygood on 2026-01-16 19:57_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-16 19:57_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-16 19:57_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-16 19:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-16 20:04_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 20:09_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 8 | 79 | 2 |
| `possibly-missing-attribute` | 0 | 10 | 3 |
| `unresolved-attribute` | 0 | 10 | 3 |
| `invalid-assignment` | 0 | 6 | 4 |
| `invalid-await` | 0 | 9 | 0 |
| `invalid-return-type` | 0 | 3 | 4 |
| `not-subscriptable` | 0 | 4 | 0 |
| `no-matching-overload` | 0 | 2 | 0 |
| `unused-ignore-comment` | 1 | 1 | 0 |
| `not-iterable` | 0 | 1 | 0 |
| `redundant-cast` | 0 | 1 | 0 |
| **Total** | **9** | **126** | **16** |


**[Full report with detailed diff](https://5622a012.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://5622a012.ty-ecosystem-ext.pages.dev/timing))



---

_@carljm approved on 2026-01-16 20:37_

This looks good to me, nice work! I think the behavior change is an improvement, and I don't think we necessarily need to fully explain it. It's intuitive to me that it arises from doing fewer unnecessary `has_relation_to` comparisons in union cases, though I haven't fully traced out the details.

---
