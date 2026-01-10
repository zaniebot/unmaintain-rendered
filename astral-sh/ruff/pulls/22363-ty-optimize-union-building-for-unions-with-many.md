```yaml
number: 22363
title: "[ty] Optimize union building for unions with many enum-literal members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/enum-literal-unions
created_at: 2026-01-03T21:22:46Z
updated_at: 2026-01-08T10:57:56Z
url: https://github.com/astral-sh/ruff/pull/22363
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Optimize union building for unions with many enum-literal members

---

_Pull request opened by @AlexWaygood on 2026-01-03 21:22_

## Summary

When building unions, we have fast paths for int-literals, bytes-literals and string-literals so that we avoid repeated expensive subtype-of checks. However, we currently have no equivalent fast path for enum literals. This PR adds that fast path.

This PR leads to a dramatic performance improvement for the snippet in https://github.com/astral-sh/ty/issues/1588#issue-3641475502. (But I would still say our performance is too slow even with this PR, so I wouldn't say it closes that issue.)

## Test Plan

- Existing mdtests all pass
- I added a new mdtest for an enum with many members, showing that our reachability analysis still works as expected
- `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` still passes

---

_Label `performance` added by @AlexWaygood on 2026-01-03 21:22_

---

_Label `ty` added by @AlexWaygood on 2026-01-03 21:22_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 21:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-03 21:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5361 diagnostics
+ Found 5366 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2084 diagnostics
+ Found 2083 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB


```

</details>




---

_Comment by @AlexWaygood on 2026-01-03 21:48_

This PR gets our execution time on this snippet (adapted from the gist in https://github.com/astral-sh/ty/issues/1588#issue-3641475502) down to ~8.6s for me locally using a `--profile=profiling` build (it takes around 49s on `main`):

<details>

```py
from enum import StrEnum, unique

from typing_extensions import assert_never

@unique
class ExampleEnum(StrEnum):
    OPTION0 = "0"
    OPTION1 = "1"
    OPTION2 = "2"
    OPTION3 = "3"
    OPTION4 = "4"
    OPTION5 = "5"
    OPTION6 = "6"
    OPTION7 = "7"
    OPTION8 = "8"
    OPTION9 = "9"
    OPTION10 = "10"
    OPTION11 = "11"
    OPTION12 = "12"
    OPTION13 = "13"
    OPTION14 = "14"
    OPTION15 = "15"
    OPTION16 = "16"
    OPTION17 = "17"
    OPTION18 = "18"
    OPTION19 = "19"
    OPTION20 = "20"
    OPTION21 = "21"
    OPTION22 = "22"
    OPTION23 = "23"
    OPTION24 = "24"
    OPTION25 = "25"
    OPTION26 = "26"
    OPTION27 = "27"
    OPTION28 = "28"
    OPTION29 = "29"
    OPTION30 = "30"
    OPTION31 = "31"
    OPTION32 = "32"
    OPTION33 = "33"
    OPTION34 = "34"
    OPTION35 = "35"
    OPTION36 = "36"
    OPTION37 = "37"
    OPTION38 = "38"
    OPTION39 = "39"
    OPTION40 = "40"
    OPTION41 = "41"
    OPTION42 = "42"
    OPTION43 = "43"
    OPTION44 = "44"
    OPTION45 = "45"
    OPTION46 = "46"
    OPTION47 = "47"
    OPTION48 = "48"
    OPTION49 = "49"
    OPTION50 = "50"
    OPTION51 = "51"
    OPTION52 = "52"
    OPTION53 = "53"
    OPTION54 = "54"
    OPTION55 = "55"
    OPTION56 = "56"
    OPTION57 = "57"
    OPTION58 = "58"
    OPTION59 = "59"
    OPTION60 = "60"
    OPTION61 = "61"
    OPTION62 = "62"
    OPTION63 = "63"
    OPTION64 = "64"
    OPTION65 = "65"
    OPTION66 = "66"
    OPTION67 = "67"
    OPTION68 = "68"
    OPTION69 = "69"
    OPTION70 = "70"
    OPTION71 = "71"
    OPTION72 = "72"
    OPTION73 = "73"
    OPTION74 = "74"
    OPTION75 = "75"
    OPTION76 = "76"
    OPTION77 = "77"
    OPTION78 = "78"
    OPTION79 = "79"
    OPTION80 = "80"
    OPTION81 = "81"
    OPTION82 = "82"
    OPTION83 = "83"
    OPTION84 = "84"
    OPTION85 = "85"
    OPTION86 = "86"
    OPTION87 = "87"
    OPTION88 = "88"
    OPTION89 = "89"
    OPTION90 = "90"
    OPTION91 = "91"
    OPTION92 = "92"
    OPTION93 = "93"
    OPTION94 = "94"
    OPTION95 = "95"
    OPTION96 = "96"
    OPTION97 = "97"
    OPTION98 = "98"
    OPTION99 = "99"
    OPTION100 = "100"
    OPTION101 = "101"
    OPTION102 = "102"
    OPTION103 = "103"
    OPTION104 = "104"
    OPTION105 = "105"
    OPTION106 = "106"
    OPTION107 = "107"
    OPTION108 = "108"
    OPTION109 = "109"
    OPTION110 = "110"
    OPTION111 = "111"
    OPTION112 = "112"
    OPTION113 = "113"
    OPTION114 = "114"
    OPTION115 = "115"
    OPTION116 = "116"
    OPTION117 = "117"
    OPTION118 = "118"
    OPTION119 = "119"
    OPTION120 = "120"
    OPTION121 = "121"
    OPTION122 = "122"
    OPTION123 = "123"
    OPTION124 = "124"
    OPTION125 = "125"
    OPTION126 = "126"
    OPTION127 = "127"
    OPTION128 = "128"
    OPTION129 = "129"
    OPTION130 = "130"
    OPTION131 = "131"
    OPTION132 = "132"
    OPTION133 = "133"
    OPTION134 = "134"
    OPTION135 = "135"
    OPTION136 = "136"
    OPTION137 = "137"
    OPTION138 = "138"
    OPTION139 = "139"
    OPTION140 = "140"
    OPTION141 = "141"
    OPTION142 = "142"
    OPTION143 = "143"
    OPTION144 = "144"
    OPTION145 = "145"
    OPTION146 = "146"
    OPTION147 = "147"
    OPTION148 = "148"
    OPTION149 = "149"
    OPTION150 = "150"
    OPTION151 = "151"
    OPTION152 = "152"
    OPTION153 = "153"
    OPTION154 = "154"
    OPTION155 = "155"
    OPTION156 = "156"
    OPTION157 = "157"
    OPTION158 = "158"
    OPTION159 = "159"
    OPTION160 = "160"
    OPTION161 = "161"
    OPTION162 = "162"
    OPTION163 = "163"
    OPTION164 = "164"
    OPTION165 = "165"
    OPTION166 = "166"
    OPTION167 = "167"
    OPTION168 = "168"
    OPTION169 = "169"
    OPTION170 = "170"
    OPTION171 = "171"
    OPTION172 = "172"
    OPTION173 = "173"
    OPTION174 = "174"
    OPTION175 = "175"
    OPTION176 = "176"
    OPTION177 = "177"
    OPTION178 = "178"
    OPTION179 = "179"
    OPTION180 = "180"
    OPTION181 = "181"
    OPTION182 = "182"
    OPTION183 = "183"
    OPTION184 = "184"
    OPTION185 = "185"
    OPTION186 = "186"
    OPTION187 = "187"
    OPTION188 = "188"
    OPTION189 = "189"
    OPTION190 = "190"
    OPTION191 = "191"
    OPTION192 = "192"
    OPTION193 = "193"
    OPTION194 = "194"
    OPTION195 = "195"
    OPTION196 = "196"
    OPTION197 = "197"
    OPTION198 = "198"
    OPTION199 = "199"
    OPTION200 = "200"
    OPTION201 = "201"
    OPTION202 = "202"
    OPTION203 = "203"
    OPTION204 = "204"
    OPTION205 = "205"
    OPTION206 = "206"
    OPTION207 = "207"
    OPTION208 = "208"
    OPTION209 = "209"
    OPTION210 = "210"
    OPTION211 = "211"
    OPTION212 = "212"
    OPTION213 = "213"
    OPTION214 = "214"
    OPTION215 = "215"
    OPTION216 = "216"
    OPTION217 = "217"
    OPTION218 = "218"
    OPTION219 = "219"
    OPTION220 = "220"
    OPTION221 = "221"
    OPTION222 = "222"
    OPTION223 = "223"
    OPTION224 = "224"
    OPTION225 = "225"
    OPTION226 = "226"
    OPTION227 = "227"
    OPTION228 = "228"
    OPTION229 = "229"
    OPTION230 = "230"
    OPTION231 = "231"
    OPTION232 = "232"
    OPTION233 = "233"
    OPTION234 = "234"
    OPTION235 = "235"
    OPTION236 = "236"
    OPTION237 = "237"
    OPTION238 = "238"
    OPTION239 = "239"
    OPTION240 = "240"
    OPTION241 = "241"
    OPTION242 = "242"
    OPTION243 = "243"
    OPTION244 = "244"
    OPTION245 = "245"
    OPTION246 = "246"
    OPTION247 = "247"
    OPTION248 = "248"
    OPTION249 = "249"
    OPTION250 = "250"
    OPTION251 = "251"
    OPTION252 = "252"
    OPTION253 = "253"
    OPTION254 = "254"
    OPTION255 = "255"
    OPTION256 = "256"
    OPTION257 = "257"
    OPTION258 = "258"
    OPTION259 = "259"
    OPTION260 = "260"
    OPTION261 = "261"
    OPTION262 = "262"
    OPTION263 = "263"
    OPTION264 = "264"
    OPTION265 = "265"
    OPTION266 = "266"
    OPTION267 = "267"
    OPTION268 = "268"
    OPTION269 = "269"
    OPTION270 = "270"
    OPTION271 = "271"
    OPTION272 = "272"
    OPTION273 = "273"
    OPTION274 = "274"
    OPTION275 = "275"
    OPTION276 = "276"
    OPTION277 = "277"
    OPTION278 = "278"
    OPTION279 = "279"
    OPTION280 = "280"
    OPTION281 = "281"
    OPTION282 = "282"
    OPTION283 = "283"
    OPTION284 = "284"
    OPTION285 = "285"
    OPTION286 = "286"
    OPTION287 = "287"
    OPTION288 = "288"
    OPTION289 = "289"
    OPTION290 = "290"
    OPTION291 = "291"
    OPTION292 = "292"
    OPTION293 = "293"
    OPTION294 = "294"
    OPTION295 = "295"
    OPTION296 = "296"
    OPTION297 = "297"
    OPTION298 = "298"
    OPTION299 = "299"
    OPTION300 = "300"
    OPTION301 = "301"
    OPTION302 = "302"
    OPTION303 = "303"
    OPTION304 = "304"
    OPTION305 = "305"
    OPTION306 = "306"
    OPTION307 = "307"
    OPTION308 = "308"
    OPTION309 = "309"
    OPTION310 = "310"
    OPTION311 = "311"
    OPTION312 = "312"
    OPTION313 = "313"
    OPTION314 = "314"
    OPTION315 = "315"
    OPTION316 = "316"
    OPTION317 = "317"
    OPTION318 = "318"
    OPTION319 = "319"
    OPTION320 = "320"
    OPTION321 = "321"
    OPTION322 = "322"
    OPTION323 = "323"
    OPTION324 = "324"
    OPTION325 = "325"
    OPTION326 = "326"
    OPTION327 = "327"
    OPTION328 = "328"
    OPTION329 = "329"
    OPTION330 = "330"
    OPTION331 = "331"
    OPTION332 = "332"
    OPTION333 = "333"
    OPTION334 = "334"
    OPTION335 = "335"
    OPTION336 = "336"
    OPTION337 = "337"
    OPTION338 = "338"
    OPTION339 = "339"
    OPTION340 = "340"
    OPTION341 = "341"
    OPTION342 = "342"
    OPTION343 = "343"
    OPTION344 = "344"
    OPTION345 = "345"
    OPTION346 = "346"
    OPTION347 = "347"
    OPTION348 = "348"
    OPTION349 = "349"
    OPTION350 = "350"
    OPTION351 = "351"
    OPTION352 = "352"
    OPTION353 = "353"
    OPTION354 = "354"
    OPTION355 = "355"
    OPTION356 = "356"
    OPTION357 = "357"
    OPTION358 = "358"
    OPTION359 = "359"
    OPTION360 = "360"
    OPTION361 = "361"
    OPTION362 = "362"
    OPTION363 = "363"
    OPTION364 = "364"
    OPTION365 = "365"
    OPTION366 = "366"
    OPTION367 = "367"
    OPTION368 = "368"
    OPTION369 = "369"
    OPTION370 = "370"
    OPTION371 = "371"
    OPTION372 = "372"
    OPTION373 = "373"
    OPTION374 = "374"
    OPTION375 = "375"
    OPTION376 = "376"
    OPTION377 = "377"
    OPTION378 = "378"
    OPTION379 = "379"
    OPTION380 = "380"
    OPTION381 = "381"
    OPTION382 = "382"
    OPTION383 = "383"
    OPTION384 = "384"
    OPTION385 = "385"
    OPTION386 = "386"
    OPTION387 = "387"
    OPTION388 = "388"
    OPTION389 = "389"
    OPTION390 = "390"
    OPTION391 = "391"
    OPTION392 = "392"
    OPTION393 = "393"
    OPTION394 = "394"
    OPTION395 = "395"
    OPTION396 = "396"
    OPTION397 = "397"
    OPTION398 = "398"
    OPTION399 = "399"
    OPTION400 = "400"
    OPTION401 = "401"
    OPTION402 = "402"
    OPTION403 = "403"
    OPTION404 = "404"
    OPTION405 = "405"
    OPTION406 = "406"
    OPTION407 = "407"
    OPTION408 = "408"
    OPTION409 = "409"
    OPTION410 = "410"
    OPTION411 = "411"
    OPTION412 = "412"
    OPTION413 = "413"
    OPTION414 = "414"
    OPTION415 = "415"
    OPTION416 = "416"
    OPTION417 = "417"
    OPTION418 = "418"
    OPTION419 = "419"
    OPTION420 = "420"
    OPTION421 = "421"
    OPTION422 = "422"
    OPTION423 = "423"
    OPTION424 = "424"
    OPTION425 = "425"
    OPTION426 = "426"
    OPTION427 = "427"
    OPTION428 = "428"
    OPTION429 = "429"
    OPTION430 = "430"
    OPTION431 = "431"
    OPTION432 = "432"
    OPTION433 = "433"
    OPTION434 = "434"
    OPTION435 = "435"
    OPTION436 = "436"
    OPTION437 = "437"
    OPTION438 = "438"
    OPTION439 = "439"
    OPTION440 = "440"
    OPTION441 = "441"
    OPTION442 = "442"
    OPTION443 = "443"
    OPTION444 = "444"
    OPTION445 = "445"
    OPTION446 = "446"
    OPTION447 = "447"
    OPTION448 = "448"
    OPTION449 = "449"
    OPTION450 = "450"
    OPTION451 = "451"
    OPTION452 = "452"
    OPTION453 = "453"
    OPTION454 = "454"
    OPTION455 = "455"
    OPTION456 = "456"
    OPTION457 = "457"
    OPTION458 = "458"
    OPTION459 = "459"
    OPTION460 = "460"
    OPTION461 = "461"
    OPTION462 = "462"
    OPTION463 = "463"
    OPTION464 = "464"
    OPTION465 = "465"
    OPTION466 = "466"
    OPTION467 = "467"
    OPTION468 = "468"
    OPTION469 = "469"
    OPTION470 = "470"
    OPTION471 = "471"
    OPTION472 = "472"
    OPTION473 = "473"
    OPTION474 = "474"
    OPTION475 = "475"
    OPTION476 = "476"
    OPTION477 = "477"
    OPTION478 = "478"
    OPTION479 = "479"
    OPTION480 = "480"
    OPTION481 = "481"
    OPTION482 = "482"
    OPTION483 = "483"
    OPTION484 = "484"
    OPTION485 = "485"
    OPTION486 = "486"
    OPTION487 = "487"
    OPTION488 = "488"
    OPTION489 = "489"
    OPTION490 = "490"
    OPTION491 = "491"
    OPTION492 = "492"
    OPTION493 = "493"
    OPTION494 = "494"
    OPTION495 = "495"
    OPTION496 = "496"
    OPTION497 = "497"
    OPTION498 = "498"
    OPTION499 = "499"

    def derived_value(self) -> int:
        match self:
            case ExampleEnum.OPTION0:
                value = 0
            case ExampleEnum.OPTION1:
                value = 1
            case ExampleEnum.OPTION2:
                value = 2
            case ExampleEnum.OPTION3:
                value = 3
            case ExampleEnum.OPTION4:
                value = 4
            case ExampleEnum.OPTION5:
                value = 5
            case ExampleEnum.OPTION6:
                value = 6
            case ExampleEnum.OPTION7:
                value = 7
            case ExampleEnum.OPTION8:
                value = 8
            case ExampleEnum.OPTION9:
                value = 9
            case ExampleEnum.OPTION10:
                value = 10
            case ExampleEnum.OPTION11:
                value = 11
            case ExampleEnum.OPTION12:
                value = 12
            case ExampleEnum.OPTION13:
                value = 13
            case ExampleEnum.OPTION14:
                value = 14
            case ExampleEnum.OPTION15:
                value = 15
            case ExampleEnum.OPTION16:
                value = 16
            case ExampleEnum.OPTION17:
                value = 17
            case ExampleEnum.OPTION18:
                value = 18
            case ExampleEnum.OPTION19:
                value = 19
            case ExampleEnum.OPTION20:
                value = 20
            case ExampleEnum.OPTION21:
                value = 21
            case ExampleEnum.OPTION22:
                value = 22
            case ExampleEnum.OPTION23:
                value = 23
            case ExampleEnum.OPTION24:
                value = 24
            case ExampleEnum.OPTION25:
                value = 25
            case ExampleEnum.OPTION26:
                value = 26
            case ExampleEnum.OPTION27:
                value = 27
            case ExampleEnum.OPTION28:
                value = 28
            case ExampleEnum.OPTION29:
                value = 29
            case ExampleEnum.OPTION30:
                value = 30
            case ExampleEnum.OPTION31:
                value = 31
            case ExampleEnum.OPTION32:
                value = 32
            case ExampleEnum.OPTION33:
                value = 33
            case ExampleEnum.OPTION34:
                value = 34
            case ExampleEnum.OPTION35:
                value = 35
            case ExampleEnum.OPTION36:
                value = 36
            case ExampleEnum.OPTION37:
                value = 37
            case ExampleEnum.OPTION38:
                value = 38
            case ExampleEnum.OPTION39:
                value = 39
            case ExampleEnum.OPTION40:
                value = 40
            case ExampleEnum.OPTION41:
                value = 41
            case ExampleEnum.OPTION42:
                value = 42
            case ExampleEnum.OPTION43:
                value = 43
            case ExampleEnum.OPTION44:
                value = 44
            case ExampleEnum.OPTION45:
                value = 45
            case ExampleEnum.OPTION46:
                value = 46
            case ExampleEnum.OPTION47:
                value = 47
            case ExampleEnum.OPTION48:
                value = 48
            case ExampleEnum.OPTION49:
                value = 49
            case ExampleEnum.OPTION50:
                value = 50
            case ExampleEnum.OPTION51:
                value = 51
            case ExampleEnum.OPTION52:
                value = 52
            case ExampleEnum.OPTION53:
                value = 53
            case ExampleEnum.OPTION54:
                value = 54
            case ExampleEnum.OPTION55:
                value = 55
            case ExampleEnum.OPTION56:
                value = 56
            case ExampleEnum.OPTION57:
                value = 57
            case ExampleEnum.OPTION58:
                value = 58
            case ExampleEnum.OPTION59:
                value = 59
            case ExampleEnum.OPTION60:
                value = 60
            case ExampleEnum.OPTION61:
                value = 61
            case ExampleEnum.OPTION62:
                value = 62
            case ExampleEnum.OPTION63:
                value = 63
            case ExampleEnum.OPTION64:
                value = 64
            case ExampleEnum.OPTION65:
                value = 65
            case ExampleEnum.OPTION66:
                value = 66
            case ExampleEnum.OPTION67:
                value = 67
            case ExampleEnum.OPTION68:
                value = 68
            case ExampleEnum.OPTION69:
                value = 69
            case ExampleEnum.OPTION70:
                value = 70
            case ExampleEnum.OPTION71:
                value = 71
            case ExampleEnum.OPTION72:
                value = 72
            case ExampleEnum.OPTION73:
                value = 73
            case ExampleEnum.OPTION74:
                value = 74
            case ExampleEnum.OPTION75:
                value = 75
            case ExampleEnum.OPTION76:
                value = 76
            case ExampleEnum.OPTION77:
                value = 77
            case ExampleEnum.OPTION78:
                value = 78
            case ExampleEnum.OPTION79:
                value = 79
            case ExampleEnum.OPTION80:
                value = 80
            case ExampleEnum.OPTION81:
                value = 81
            case ExampleEnum.OPTION82:
                value = 82
            case ExampleEnum.OPTION83:
                value = 83
            case ExampleEnum.OPTION84:
                value = 84
            case ExampleEnum.OPTION85:
                value = 85
            case ExampleEnum.OPTION86:
                value = 86
            case ExampleEnum.OPTION87:
                value = 87
            case ExampleEnum.OPTION88:
                value = 88
            case ExampleEnum.OPTION89:
                value = 89
            case ExampleEnum.OPTION90:
                value = 90
            case ExampleEnum.OPTION91:
                value = 91
            case ExampleEnum.OPTION92:
                value = 92
            case ExampleEnum.OPTION93:
                value = 93
            case ExampleEnum.OPTION94:
                value = 94
            case ExampleEnum.OPTION95:
                value = 95
            case ExampleEnum.OPTION96:
                value = 96
            case ExampleEnum.OPTION97:
                value = 97
            case ExampleEnum.OPTION98:
                value = 98
            case ExampleEnum.OPTION99:
                value = 99
            case ExampleEnum.OPTION100:
                value = 100
            case ExampleEnum.OPTION101:
                value = 101
            case ExampleEnum.OPTION102:
                value = 102
            case ExampleEnum.OPTION103:
                value = 103
            case ExampleEnum.OPTION104:
                value = 104
            case ExampleEnum.OPTION105:
                value = 105
            case ExampleEnum.OPTION106:
                value = 106
            case ExampleEnum.OPTION107:
                value = 107
            case ExampleEnum.OPTION108:
                value = 108
            case ExampleEnum.OPTION109:
                value = 109
            case ExampleEnum.OPTION110:
                value = 110
            case ExampleEnum.OPTION111:
                value = 111
            case ExampleEnum.OPTION112:
                value = 112
            case ExampleEnum.OPTION113:
                value = 113
            case ExampleEnum.OPTION114:
                value = 114
            case ExampleEnum.OPTION115:
                value = 115
            case ExampleEnum.OPTION116:
                value = 116
            case ExampleEnum.OPTION117:
                value = 117
            case ExampleEnum.OPTION118:
                value = 118
            case ExampleEnum.OPTION119:
                value = 119
            case ExampleEnum.OPTION120:
                value = 120
            case ExampleEnum.OPTION121:
                value = 121
            case ExampleEnum.OPTION122:
                value = 122
            case ExampleEnum.OPTION123:
                value = 123
            case ExampleEnum.OPTION124:
                value = 124
            case ExampleEnum.OPTION125:
                value = 125
            case ExampleEnum.OPTION126:
                value = 126
            case ExampleEnum.OPTION127:
                value = 127
            case ExampleEnum.OPTION128:
                value = 128
            case ExampleEnum.OPTION129:
                value = 129
            case ExampleEnum.OPTION130:
                value = 130
            case ExampleEnum.OPTION131:
                value = 131
            case ExampleEnum.OPTION132:
                value = 132
            case ExampleEnum.OPTION133:
                value = 133
            case ExampleEnum.OPTION134:
                value = 134
            case ExampleEnum.OPTION135:
                value = 135
            case ExampleEnum.OPTION136:
                value = 136
            case ExampleEnum.OPTION137:
                value = 137
            case ExampleEnum.OPTION138:
                value = 138
            case ExampleEnum.OPTION139:
                value = 139
            case ExampleEnum.OPTION140:
                value = 140
            case ExampleEnum.OPTION141:
                value = 141
            case ExampleEnum.OPTION142:
                value = 142
            case ExampleEnum.OPTION143:
                value = 143
            case ExampleEnum.OPTION144:
                value = 144
            case ExampleEnum.OPTION145:
                value = 145
            case ExampleEnum.OPTION146:
                value = 146
            case ExampleEnum.OPTION147:
                value = 147
            case ExampleEnum.OPTION148:
                value = 148
            case ExampleEnum.OPTION149:
                value = 149
            case ExampleEnum.OPTION150:
                value = 150
            case ExampleEnum.OPTION151:
                value = 151
            case ExampleEnum.OPTION152:
                value = 152
            case ExampleEnum.OPTION153:
                value = 153
            case ExampleEnum.OPTION154:
                value = 154
            case ExampleEnum.OPTION155:
                value = 155
            case ExampleEnum.OPTION156:
                value = 156
            case ExampleEnum.OPTION157:
                value = 157
            case ExampleEnum.OPTION158:
                value = 158
            case ExampleEnum.OPTION159:
                value = 159
            case ExampleEnum.OPTION160:
                value = 160
            case ExampleEnum.OPTION161:
                value = 161
            case ExampleEnum.OPTION162:
                value = 162
            case ExampleEnum.OPTION163:
                value = 163
            case ExampleEnum.OPTION164:
                value = 164
            case ExampleEnum.OPTION165:
                value = 165
            case ExampleEnum.OPTION166:
                value = 166
            case ExampleEnum.OPTION167:
                value = 167
            case ExampleEnum.OPTION168:
                value = 168
            case ExampleEnum.OPTION169:
                value = 169
            case ExampleEnum.OPTION170:
                value = 170
            case ExampleEnum.OPTION171:
                value = 171
            case ExampleEnum.OPTION172:
                value = 172
            case ExampleEnum.OPTION173:
                value = 173
            case ExampleEnum.OPTION174:
                value = 174
            case ExampleEnum.OPTION175:
                value = 175
            case ExampleEnum.OPTION176:
                value = 176
            case ExampleEnum.OPTION177:
                value = 177
            case ExampleEnum.OPTION178:
                value = 178
            case ExampleEnum.OPTION179:
                value = 179
            case ExampleEnum.OPTION180:
                value = 180
            case ExampleEnum.OPTION181:
                value = 181
            case ExampleEnum.OPTION182:
                value = 182
            case ExampleEnum.OPTION183:
                value = 183
            case ExampleEnum.OPTION184:
                value = 184
            case ExampleEnum.OPTION185:
                value = 185
            case ExampleEnum.OPTION186:
                value = 186
            case ExampleEnum.OPTION187:
                value = 187
            case ExampleEnum.OPTION188:
                value = 188
            case ExampleEnum.OPTION189:
                value = 189
            case ExampleEnum.OPTION190:
                value = 190
            case ExampleEnum.OPTION191:
                value = 191
            case ExampleEnum.OPTION192:
                value = 192
            case ExampleEnum.OPTION193:
                value = 193
            case ExampleEnum.OPTION194:
                value = 194
            case ExampleEnum.OPTION195:
                value = 195
            case ExampleEnum.OPTION196:
                value = 196
            case ExampleEnum.OPTION197:
                value = 197
            case ExampleEnum.OPTION198:
                value = 198
            case ExampleEnum.OPTION199:
                value = 199
            case ExampleEnum.OPTION200:
                value = 200
            case ExampleEnum.OPTION201:
                value = 201
            case ExampleEnum.OPTION202:
                value = 202
            case ExampleEnum.OPTION203:
                value = 203
            case ExampleEnum.OPTION204:
                value = 204
            case ExampleEnum.OPTION205:
                value = 205
            case ExampleEnum.OPTION206:
                value = 206
            case ExampleEnum.OPTION207:
                value = 207
            case ExampleEnum.OPTION208:
                value = 208
            case ExampleEnum.OPTION209:
                value = 209
            case ExampleEnum.OPTION210:
                value = 210
            case ExampleEnum.OPTION211:
                value = 211
            case ExampleEnum.OPTION212:
                value = 212
            case ExampleEnum.OPTION213:
                value = 213
            case ExampleEnum.OPTION214:
                value = 214
            case ExampleEnum.OPTION215:
                value = 215
            case ExampleEnum.OPTION216:
                value = 216
            case ExampleEnum.OPTION217:
                value = 217
            case ExampleEnum.OPTION218:
                value = 218
            case ExampleEnum.OPTION219:
                value = 219
            case ExampleEnum.OPTION220:
                value = 220
            case ExampleEnum.OPTION221:
                value = 221
            case ExampleEnum.OPTION222:
                value = 222
            case ExampleEnum.OPTION223:
                value = 223
            case ExampleEnum.OPTION224:
                value = 224
            case ExampleEnum.OPTION225:
                value = 225
            case ExampleEnum.OPTION226:
                value = 226
            case ExampleEnum.OPTION227:
                value = 227
            case ExampleEnum.OPTION228:
                value = 228
            case ExampleEnum.OPTION229:
                value = 229
            case ExampleEnum.OPTION230:
                value = 230
            case ExampleEnum.OPTION231:
                value = 231
            case ExampleEnum.OPTION232:
                value = 232
            case ExampleEnum.OPTION233:
                value = 233
            case ExampleEnum.OPTION234:
                value = 234
            case ExampleEnum.OPTION235:
                value = 235
            case ExampleEnum.OPTION236:
                value = 236
            case ExampleEnum.OPTION237:
                value = 237
            case ExampleEnum.OPTION238:
                value = 238
            case ExampleEnum.OPTION239:
                value = 239
            case ExampleEnum.OPTION240:
                value = 240
            case ExampleEnum.OPTION241:
                value = 241
            case ExampleEnum.OPTION242:
                value = 242
            case ExampleEnum.OPTION243:
                value = 243
            case ExampleEnum.OPTION244:
                value = 244
            case ExampleEnum.OPTION245:
                value = 245
            case ExampleEnum.OPTION246:
                value = 246
            case ExampleEnum.OPTION247:
                value = 247
            case ExampleEnum.OPTION248:
                value = 248
            case ExampleEnum.OPTION249:
                value = 249
            case ExampleEnum.OPTION250:
                value = 250
            case ExampleEnum.OPTION251:
                value = 251
            case ExampleEnum.OPTION252:
                value = 252
            case ExampleEnum.OPTION253:
                value = 253
            case ExampleEnum.OPTION254:
                value = 254
            case ExampleEnum.OPTION255:
                value = 255
            case ExampleEnum.OPTION256:
                value = 256
            case ExampleEnum.OPTION257:
                value = 257
            case ExampleEnum.OPTION258:
                value = 258
            case ExampleEnum.OPTION259:
                value = 259
            case ExampleEnum.OPTION260:
                value = 260
            case ExampleEnum.OPTION261:
                value = 261
            case ExampleEnum.OPTION262:
                value = 262
            case ExampleEnum.OPTION263:
                value = 263
            case ExampleEnum.OPTION264:
                value = 264
            case ExampleEnum.OPTION265:
                value = 265
            case ExampleEnum.OPTION266:
                value = 266
            case ExampleEnum.OPTION267:
                value = 267
            case ExampleEnum.OPTION268:
                value = 268
            case ExampleEnum.OPTION269:
                value = 269
            case ExampleEnum.OPTION270:
                value = 270
            case ExampleEnum.OPTION271:
                value = 271
            case ExampleEnum.OPTION272:
                value = 272
            case ExampleEnum.OPTION273:
                value = 273
            case ExampleEnum.OPTION274:
                value = 274
            case ExampleEnum.OPTION275:
                value = 275
            case ExampleEnum.OPTION276:
                value = 276
            case ExampleEnum.OPTION277:
                value = 277
            case ExampleEnum.OPTION278:
                value = 278
            case ExampleEnum.OPTION279:
                value = 279
            case ExampleEnum.OPTION280:
                value = 280
            case ExampleEnum.OPTION281:
                value = 281
            case ExampleEnum.OPTION282:
                value = 282
            case ExampleEnum.OPTION283:
                value = 283
            case ExampleEnum.OPTION284:
                value = 284
            case ExampleEnum.OPTION285:
                value = 285
            case ExampleEnum.OPTION286:
                value = 286
            case ExampleEnum.OPTION287:
                value = 287
            case ExampleEnum.OPTION288:
                value = 288
            case ExampleEnum.OPTION289:
                value = 289
            case ExampleEnum.OPTION290:
                value = 290
            case ExampleEnum.OPTION291:
                value = 291
            case ExampleEnum.OPTION292:
                value = 292
            case ExampleEnum.OPTION293:
                value = 293
            case ExampleEnum.OPTION294:
                value = 294
            case ExampleEnum.OPTION295:
                value = 295
            case ExampleEnum.OPTION296:
                value = 296
            case ExampleEnum.OPTION297:
                value = 297
            case ExampleEnum.OPTION298:
                value = 298
            case ExampleEnum.OPTION299:
                value = 299
            case ExampleEnum.OPTION300:
                value = 300
            case ExampleEnum.OPTION301:
                value = 301
            case ExampleEnum.OPTION302:
                value = 302
            case ExampleEnum.OPTION303:
                value = 303
            case ExampleEnum.OPTION304:
                value = 304
            case ExampleEnum.OPTION305:
                value = 305
            case ExampleEnum.OPTION306:
                value = 306
            case ExampleEnum.OPTION307:
                value = 307
            case ExampleEnum.OPTION308:
                value = 308
            case ExampleEnum.OPTION309:
                value = 309
            case ExampleEnum.OPTION310:
                value = 310
            case ExampleEnum.OPTION311:
                value = 311
            case ExampleEnum.OPTION312:
                value = 312
            case ExampleEnum.OPTION313:
                value = 313
            case ExampleEnum.OPTION314:
                value = 314
            case ExampleEnum.OPTION315:
                value = 315
            case ExampleEnum.OPTION316:
                value = 316
            case ExampleEnum.OPTION317:
                value = 317
            case ExampleEnum.OPTION318:
                value = 318
            case ExampleEnum.OPTION319:
                value = 319
            case ExampleEnum.OPTION320:
                value = 320
            case ExampleEnum.OPTION321:
                value = 321
            case ExampleEnum.OPTION322:
                value = 322
            case ExampleEnum.OPTION323:
                value = 323
            case ExampleEnum.OPTION324:
                value = 324
            case ExampleEnum.OPTION325:
                value = 325
            case ExampleEnum.OPTION326:
                value = 326
            case ExampleEnum.OPTION327:
                value = 327
            case ExampleEnum.OPTION328:
                value = 328
            case ExampleEnum.OPTION329:
                value = 329
            case ExampleEnum.OPTION330:
                value = 330
            case ExampleEnum.OPTION331:
                value = 331
            case ExampleEnum.OPTION332:
                value = 332
            case ExampleEnum.OPTION333:
                value = 333
            case ExampleEnum.OPTION334:
                value = 334
            case ExampleEnum.OPTION335:
                value = 335
            case ExampleEnum.OPTION336:
                value = 336
            case ExampleEnum.OPTION337:
                value = 337
            case ExampleEnum.OPTION338:
                value = 338
            case ExampleEnum.OPTION339:
                value = 339
            case ExampleEnum.OPTION340:
                value = 340
            case ExampleEnum.OPTION341:
                value = 341
            case ExampleEnum.OPTION342:
                value = 342
            case ExampleEnum.OPTION343:
                value = 343
            case ExampleEnum.OPTION344:
                value = 344
            case ExampleEnum.OPTION345:
                value = 345
            case ExampleEnum.OPTION346:
                value = 346
            case ExampleEnum.OPTION347:
                value = 347
            case ExampleEnum.OPTION348:
                value = 348
            case ExampleEnum.OPTION349:
                value = 349
            case ExampleEnum.OPTION350:
                value = 350
            case ExampleEnum.OPTION351:
                value = 351
            case ExampleEnum.OPTION352:
                value = 352
            case ExampleEnum.OPTION353:
                value = 353
            case ExampleEnum.OPTION354:
                value = 354
            case ExampleEnum.OPTION355:
                value = 355
            case ExampleEnum.OPTION356:
                value = 356
            case ExampleEnum.OPTION357:
                value = 357
            case ExampleEnum.OPTION358:
                value = 358
            case ExampleEnum.OPTION359:
                value = 359
            case ExampleEnum.OPTION360:
                value = 360
            case ExampleEnum.OPTION361:
                value = 361
            case ExampleEnum.OPTION362:
                value = 362
            case ExampleEnum.OPTION363:
                value = 363
            case ExampleEnum.OPTION364:
                value = 364
            case ExampleEnum.OPTION365:
                value = 365
            case ExampleEnum.OPTION366:
                value = 366
            case ExampleEnum.OPTION367:
                value = 367
            case ExampleEnum.OPTION368:
                value = 368
            case ExampleEnum.OPTION369:
                value = 369
            case ExampleEnum.OPTION370:
                value = 370
            case ExampleEnum.OPTION371:
                value = 371
            case ExampleEnum.OPTION372:
                value = 372
            case ExampleEnum.OPTION373:
                value = 373
            case ExampleEnum.OPTION374:
                value = 374
            case ExampleEnum.OPTION375:
                value = 375
            case ExampleEnum.OPTION376:
                value = 376
            case ExampleEnum.OPTION377:
                value = 377
            case ExampleEnum.OPTION378:
                value = 378
            case ExampleEnum.OPTION379:
                value = 379
            case ExampleEnum.OPTION380:
                value = 380
            case ExampleEnum.OPTION381:
                value = 381
            case ExampleEnum.OPTION382:
                value = 382
            case ExampleEnum.OPTION383:
                value = 383
            case ExampleEnum.OPTION384:
                value = 384
            case ExampleEnum.OPTION385:
                value = 385
            case ExampleEnum.OPTION386:
                value = 386
            case ExampleEnum.OPTION387:
                value = 387
            case ExampleEnum.OPTION388:
                value = 388
            case ExampleEnum.OPTION389:
                value = 389
            case ExampleEnum.OPTION390:
                value = 390
            case ExampleEnum.OPTION391:
                value = 391
            case ExampleEnum.OPTION392:
                value = 392
            case ExampleEnum.OPTION393:
                value = 393
            case ExampleEnum.OPTION394:
                value = 394
            case ExampleEnum.OPTION395:
                value = 395
            case ExampleEnum.OPTION396:
                value = 396
            case ExampleEnum.OPTION397:
                value = 397
            case ExampleEnum.OPTION398:
                value = 398
            case ExampleEnum.OPTION399:
                value = 399
            case ExampleEnum.OPTION400:
                value = 400
            case ExampleEnum.OPTION401:
                value = 401
            case ExampleEnum.OPTION402:
                value = 402
            case ExampleEnum.OPTION403:
                value = 403
            case ExampleEnum.OPTION404:
                value = 404
            case ExampleEnum.OPTION405:
                value = 405
            case ExampleEnum.OPTION406:
                value = 406
            case ExampleEnum.OPTION407:
                value = 407
            case ExampleEnum.OPTION408:
                value = 408
            case ExampleEnum.OPTION409:
                value = 409
            case ExampleEnum.OPTION410:
                value = 410
            case ExampleEnum.OPTION411:
                value = 411
            case ExampleEnum.OPTION412:
                value = 412
            case ExampleEnum.OPTION413:
                value = 413
            case ExampleEnum.OPTION414:
                value = 414
            case ExampleEnum.OPTION415:
                value = 415
            case ExampleEnum.OPTION416:
                value = 416
            case ExampleEnum.OPTION417:
                value = 417
            case ExampleEnum.OPTION418:
                value = 418
            case ExampleEnum.OPTION419:
                value = 419
            case ExampleEnum.OPTION420:
                value = 420
            case ExampleEnum.OPTION421:
                value = 421
            case ExampleEnum.OPTION422:
                value = 422
            case ExampleEnum.OPTION423:
                value = 423
            case ExampleEnum.OPTION424:
                value = 424
            case ExampleEnum.OPTION425:
                value = 425
            case ExampleEnum.OPTION426:
                value = 426
            case ExampleEnum.OPTION427:
                value = 427
            case ExampleEnum.OPTION428:
                value = 428
            case ExampleEnum.OPTION429:
                value = 429
            case ExampleEnum.OPTION430:
                value = 430
            case ExampleEnum.OPTION431:
                value = 431
            case ExampleEnum.OPTION432:
                value = 432
            case ExampleEnum.OPTION433:
                value = 433
            case ExampleEnum.OPTION434:
                value = 434
            case ExampleEnum.OPTION435:
                value = 435
            case ExampleEnum.OPTION436:
                value = 436
            case ExampleEnum.OPTION437:
                value = 437
            case ExampleEnum.OPTION438:
                value = 438
            case ExampleEnum.OPTION439:
                value = 439
            case ExampleEnum.OPTION440:
                value = 440
            case ExampleEnum.OPTION441:
                value = 441
            case ExampleEnum.OPTION442:
                value = 442
            case ExampleEnum.OPTION443:
                value = 443
            case ExampleEnum.OPTION444:
                value = 444
            case ExampleEnum.OPTION445:
                value = 445
            case ExampleEnum.OPTION446:
                value = 446
            case ExampleEnum.OPTION447:
                value = 447
            case ExampleEnum.OPTION448:
                value = 448
            case ExampleEnum.OPTION449:
                value = 449
            case ExampleEnum.OPTION450:
                value = 450
            case ExampleEnum.OPTION451:
                value = 451
            case ExampleEnum.OPTION452:
                value = 452
            case ExampleEnum.OPTION453:
                value = 453
            case ExampleEnum.OPTION454:
                value = 454
            case ExampleEnum.OPTION455:
                value = 455
            case ExampleEnum.OPTION456:
                value = 456
            case ExampleEnum.OPTION457:
                value = 457
            case ExampleEnum.OPTION458:
                value = 458
            case ExampleEnum.OPTION459:
                value = 459
            case ExampleEnum.OPTION460:
                value = 460
            case ExampleEnum.OPTION461:
                value = 461
            case ExampleEnum.OPTION462:
                value = 462
            case ExampleEnum.OPTION463:
                value = 463
            case ExampleEnum.OPTION464:
                value = 464
            case ExampleEnum.OPTION465:
                value = 465
            case ExampleEnum.OPTION466:
                value = 466
            case ExampleEnum.OPTION467:
                value = 467
            case ExampleEnum.OPTION468:
                value = 468
            case ExampleEnum.OPTION469:
                value = 469
            case ExampleEnum.OPTION470:
                value = 470
            case ExampleEnum.OPTION471:
                value = 471
            case ExampleEnum.OPTION472:
                value = 472
            case ExampleEnum.OPTION473:
                value = 473
            case ExampleEnum.OPTION474:
                value = 474
            case ExampleEnum.OPTION475:
                value = 475
            case ExampleEnum.OPTION476:
                value = 476
            case ExampleEnum.OPTION477:
                value = 477
            case ExampleEnum.OPTION478:
                value = 478
            case ExampleEnum.OPTION479:
                value = 479
            case ExampleEnum.OPTION480:
                value = 480
            case ExampleEnum.OPTION481:
                value = 481
            case ExampleEnum.OPTION482:
                value = 482
            case ExampleEnum.OPTION483:
                value = 483
            case ExampleEnum.OPTION484:
                value = 484
            case ExampleEnum.OPTION485:
                value = 485
            case ExampleEnum.OPTION486:
                value = 486
            case ExampleEnum.OPTION487:
                value = 487
            case ExampleEnum.OPTION488:
                value = 488
            case ExampleEnum.OPTION489:
                value = 489
            case ExampleEnum.OPTION490:
                value = 490
            case ExampleEnum.OPTION491:
                value = 491
            case ExampleEnum.OPTION492:
                value = 492
            case ExampleEnum.OPTION493:
                value = 493
            case ExampleEnum.OPTION494:
                value = 494
            case ExampleEnum.OPTION495:
                value = 495
            case ExampleEnum.OPTION496:
                value = 496
            case ExampleEnum.OPTION497:
                value = 497
            case ExampleEnum.OPTION498:
                value = 498
            case ExampleEnum.OPTION499:
                value = 499
            case _:
                assert_never(self)
        return value
```

</details>

But... there's a bug here somewhere. We don't emit any diagnostics on that snippet on `main`, but we emit a diagnostic on the `assert_never()` call on this PR.

---

_Comment by @AlexWaygood on 2026-01-03 22:11_

Ah... it's not technically a bug. We're running into the limit here: https://github.com/astral-sh/ruff/blob/8464aca795bc3580ca15fcb52b21616939cea9a9/crates/ty_python_semantic/src/types/builder.rs#L226-L228

The bug only appears if there are 257 enum members or more.

The reason why that limit is relevant to the execution time on the above snippet is this line here, where we construct a (potentially huge) union of enum literals in order to simplify an intersection that has enum literals in its negative set: https://github.com/astral-sh/ruff/blob/8464aca795bc3580ca15fcb52b21616939cea9a9/crates/ty_python_semantic/src/types/builder.rs#L768-L785

---

_Closed by @AlexWaygood on 2026-01-04 17:04_

---

_Reopened by @AlexWaygood on 2026-01-04 17:04_

---

_Comment by @codspeed-hq[bot] on 2026-01-04 17:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
### Merging this PR will **improve performance by ×2.2**




### Summary

`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` ty_micro[many_enum_members_2] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fenum-literal-unions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members_2%3A%3Aty_micro%5Bmany_enum_members_2%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 340.7 ms | 152.5 ms | ×2.2 |
---

<sub>Comparing <code>alex/enum-literal-unions</code> (9186059) with <code>main</code> (7319c37)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/alex%2Fenum-literal-unions?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fenum-literal-unions?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @AlexWaygood on 2026-01-04 18:23_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-04 18:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-04 18:23_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-04 18:23_

---

_Comment by @MichaReiser on 2026-01-05 09:39_

Nice, this is a huge improvement for `discord.py` (and a small improvement for homeassistant)

```
❯ uv run benchmark --tool ty --ty-path ~/astral/ruff/target/profiling/ty-main --ty-path ~/astral/ruff/target/profiling/ty
      Built ty-benchmark @ file:///Users/micha/astral/ruff/scripts/ty_benchmark
Uninstalled 1 package in 1ms
Installed 1 package in 1ms
discord.py
----------

Benchmark 1: /Users/micha/astral/ruff/target/profiling/ty-main
  Time (mean ± σ):     242.9 ms ±   5.0 ms    [User: 1423.4 ms, System: 114.8 ms]
  Range (min … max):   238.1 ms … 256.1 ms    12 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: /Users/micha/astral/ruff/target/profiling/ty
  Time (mean ± σ):     175.7 ms ±   3.1 ms    [User: 1324.9 ms, System: 111.3 ms]
  Range (min … max):   171.6 ms … 181.2 ms    16 runs

  Warning: Ignoring non-zero exit code.

Summary
  /Users/micha/astral/ruff/target/profiling/ty ran
    1.38 ± 0.04 times faster than /Users/micha/astral/ruff/target/profiling/ty-main

-------------------------------------------------------------------------------

homeassistant
-------------

Benchmark 1: /Users/micha/astral/ruff/target/profiling/ty-main
  Time (mean ± σ):      2.218 s ±  0.067 s    [User: 22.101 s, System: 3.110 s]
  Range (min … max):    2.128 s …  2.347 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: /Users/micha/astral/ruff/target/profiling/ty
  Time (mean ± σ):      2.116 s ±  0.088 s    [User: 21.415 s, System: 3.123 s]
  Range (min … max):    1.998 s …  2.245 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  /Users/micha/astral/ruff/target/profiling/ty ran
    1.05 ± 0.05 times faster than /Users/micha/astral/ruff/target/profiling/ty-main

-------------------------------------------------------------------------------
```

However, it's also a substantial regression for pytorch. Do you think this is because of the now higher limit?

```
 uv run benchmark --tool ty --ty-path ~/astral/ruff/target/profiling/ty-main --ty-path ~/astral/ruff/target/profiling/ty --project pytorch
pytorch
-------

Benchmark 1: /Users/micha/astral/ruff/target/profiling/ty-main
  Time (mean ± σ):      1.201 s ±  0.085 s    [User: 11.724 s, System: 1.407 s]
  Range (min … max):    1.113 s …  1.375 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: /Users/micha/astral/ruff/target/profiling/ty
  Time (mean ± σ):      1.294 s ±  0.089 s    [User: 12.175 s, System: 1.437 s]
  Range (min … max):    1.144 s …  1.423 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  /Users/micha/astral/ruff/target/profiling/ty-main ran
    1.08 ± 0.11 times faster than /Users/micha/astral/ruff/target/profiling/ty
```

---

_Comment by @AlexWaygood on 2026-01-05 09:45_

> However, it's also a substantial regression for pytorch. Do you think this is because of the now higher limit?

Hmm, we currently have no limit at all for enum literals on `main`, so I don't think that can be the cause. The more probable cause is that by rewriting all our enum-literal logic in the union builder to add the new fast path for pathological cases where there are many enum literals in the union, I also made some cases slower. (Our existing logic for enum Literals in the union builder is non-trivial; it's very possible I accidentally got rid of an optimisation we already had on `main`.)

---

_Comment by @AlexWaygood on 2026-01-05 15:01_

@MichaReiser, I can reproduce the speedup on discord.py locally but I can't reproduce the slowdown on pytorch. When I run ty_benchmark, it reports a 3% speedup for this PR on pytorch:

```
pytorch
-------

Benchmark 1: /Users/alexw/dev/ruff/target/profiling/ty-main
  Time (mean ± σ):      1.230 s ±  0.073 s    [User: 12.440 s, System: 1.419 s]
  Range (min … max):    1.158 s …  1.371 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: /Users/alexw/dev/ruff/target/profiling/ty
  Time (mean ± σ):      1.196 s ±  0.055 s    [User: 12.499 s, System: 1.374 s]
  Range (min … max):    1.149 s …  1.294 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  /Users/alexw/dev/ruff/target/profiling/ty ran
    1.03 ± 0.08 times faster than /Users/alexw/dev/ruff/target/profiling/ty-main
```

---

_Comment by @MichaReiser on 2026-01-05 16:58_

Hmm. I did ran the benchmark twice before, but I'm now unable to reproduce. So maybe just a flake on my machine.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:316 on 2026-01-07 19:06_

I'm not aware of anything that prevents enum classes from being generic, so I expect this should not directly construct a `ClassType::NonGeneric`, but should use a constructor that inspects the class instead. (IIRC we aim to maintain an invariant that we never represent an unspecialized generic class with `ClassType::NonGeneric`)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:533 on 2026-01-07 19:07_

Similar here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:554 on 2026-01-07 19:10_

Do we have any tests involving a recursively-defined union with more than 10 enum elements in it? (Or one with less than 10, for that matter). Could probably look at the PR that introduced this constant to find similar tests for other kinds of literals, to work from.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:549 on 2026-01-07 19:16_

Could this be a match guard condition? Shouldn't really matter either way, would just be slightly more concise.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:559 on 2026-01-07 19:16_

Same comment here about `ClassType::NonGeneric`.

It seems like this (make an instance type out of an enum class and add it in place) is something we do a lot of different places in this diff. I'm not sure it can be abstracted smaller than the two lines to do those two things, but it would be nice if this particular repeated line didn't repeat non-germane assumptions about genericness in any way (whether they are correct assumptions or not.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:568 on 2026-01-07 19:21_

I assume this PR just pre-dated your other one, but probably this should be `is_redundant_with` instead? (Or that could be changed in your other PR, if you land this one first.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:573 on 2026-01-07 19:21_

Same here

---

_@carljm approved on 2026-01-07 19:22_

This is great, thank you!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:316 on 2026-01-07 21:59_

The concept of a generic enum class is inherently incoherent.

Apart from that, attempting to create a generic enum class usually fails at runtime:

```pycon
>>> from enum import Enum
>>> class Foo[T](Enum):
...     X: T
...     
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    class Foo[T](Enum):
        X: T
  File "<python-input-1>", line 1, in <generic parameters of Foo>
    class Foo[T](Enum):
        X: T
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 491, in __prepare__
    member_type, first_enum = metacls._get_mixins_(cls, bases)
                              ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 956, in _get_mixins_
    raise TypeError("new enumerations should be created as "
            "`EnumName([mixin_type, ...] [data_type,] enum_type)`")
TypeError: new enumerations should be created as `EnumName([mixin_type, ...] [data_type,] enum_type)`
>>> from typing import Generic, TypeVar
>>> T = TypeVar("T")
>>> class Foo(Enum, Generic[T]):
...     X: T
...     
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    class Foo(Enum, Generic[T]):
        X: T
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 491, in __prepare__
    member_type, first_enum = metacls._get_mixins_(cls, bases)
                              ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 956, in _get_mixins_
    raise TypeError("new enumerations should be created as "
            "`EnumName([mixin_type, ...] [data_type,] enum_type)`")
TypeError: new enumerations should be created as `EnumName([mixin_type, ...] [data_type,] enum_type)`
```

even when the class creation does not immediately fail at runtime, it does not work in the way you'd expect, because of the fact that `Generic.__class_getitem__` is clobbered by `EnumMeta.__getitem__`:

```pycon
>>> from enum import Enum
>>> from typing import Generic, TypeVar
>>> T = TypeVar("T")
>>> class Foo(Generic[T], Enum):
...     X: T
...     
>>> Foo[int]
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    Foo[int]
    ~~~^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 789, in __getitem__
    return cls._member_map_[name]
           ~~~~~~~~~~~~~~~~^^^^^^
KeyError: <class 'int'>
```

Generic enums are explicitly forbidden by mypy and pyrefly:
- https://mypy-play.net/?mypy=latest&python=3.12&gist=35ef91038c460dd48cd557bb8704076d
- https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0BxGOhiUIAYwA0dACrMYANVSUAOujDV6wgK70uPPnQCi6XWrUy6AXlnyllABQqQM5wEpz6cVFRw4dADFcXAchETFxAG0ZAF1pE103RDU6VLp8RFk1EEkQbQZoOBJyRBAAYjoAVQKoCCY6MG0vAtx0OE9MGDAG3hpUBgB9UxpsUQcMznQGNzoAWgA%2BOjgGSmT0NLpKGAZtSnWwZwA5XVHVumB8AF9nbNyyLbAoUkIGWigKCoAFUgenpYwcAQ6OJWpA2Lt%2BhBWoQ1BUAMowGB0AAWDAYxDgiAA9Fj7l0noReGwscIsZhcOI4FiQeoIODKJDWliepQ6KgAG6oaCobCwYGgukQlrrXDEYVFNRkBjI1qzdmiOBQ9Y2ZwAZkIAEYAEw3dAgS65VDiArygLQGAUNBYPBEMj6oA

and while we do not yet explicitly forbid them, we also make it impossible for generic enums to exist in our model due to the way we represent enum-literal types -- `EnumLiteralType` wraps a `ClassLiteral` rather than a `ClassType`:

https://github.com/astral-sh/ruff/blob/c02d164357d51e57e80703ceb3a72c5ae368077a/crates/ty_python_semantic/src/types.rs#L12696-L12704

So I do not believe that generic enums are a concept that we can or should support, and I would instead view this as a missing lint that we should implement in a separate PR (where we explicitly forbid enum classes to multiple-inherit from `Generic[]` or have PEP-695 type parameters)

---

_@AlexWaygood reviewed on 2026-01-07 21:59_

---

_@AlexWaygood reviewed on 2026-01-07 22:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:568 on 2026-01-07 22:00_

yeah, I'm well aware that the two PRs cause merge conflicts with each other 😆 but thanks!

---

_@AlexWaygood reviewed on 2026-01-07 22:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:549 on 2026-01-07 22:26_

It could be, yeah. I personally prefer find it more readable how it is, though I don't feel strongly :-)

---

_@AlexWaygood reviewed on 2026-01-07 22:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:559 on 2026-01-07 22:27_

yeah, I can extract this out into a method on `EnumLiteralType`

---

_@carljm reviewed on 2026-01-07 22:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:316 on 2026-01-07 22:32_

Fair enough, that works for me! Looks like I just picked the wrong type checker to test in (pyright supports generic enums to at least some extent.) The one part of an enum that could be coherently generic is methods, maybe? But I'm happy to just say we don't support generic enums and never will.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:559 on 2026-01-08 10:28_

Oh, we actually already have the helper method you're asking for, I just didn't know about it/forgot about it:

https://github.com/astral-sh/ruff/blob/7319c37f4eb063e9590e1f09c8e92d7dabc63403/crates/ty_python_semantic/src/types.rs#L12709-L12713

---

_@AlexWaygood reviewed on 2026-01-08 10:28_

---

_@AlexWaygood reviewed on 2026-01-08 10:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:554 on 2026-01-08 10:45_

It doesn't look to me like we have any tests for recursively defined unions with lots of `Literal` elements in them. I looked at https://github.com/astral-sh/ruff/commit/c7b5067ef89de735beb7a88dd813e259ad4a0972, which originally introduced the size limit on the number of `Literal` elements we allow to coexist in a union before widening the `Literal`s, and https://github.com/astral-sh/ruff/commit/f3e5713d90f0b1bfcba9bfe3a3ab0c3df33ece83, which increased the limit for non-recursive unions -- neither commit appeared to include tests of the kind you're asking about here.

Happy to look into this more as a followup if you think it's important, but for now I'm going to move on!

---

_Merged by @AlexWaygood on 2026-01-08 10:50_

---

_Closed by @AlexWaygood on 2026-01-08 10:50_

---

_Branch deleted on 2026-01-08 10:50_

---
