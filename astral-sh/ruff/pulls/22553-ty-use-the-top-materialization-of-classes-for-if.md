```yaml
number: 22553
title: "[ty] Use the top materialization of classes for `if type(x) is y` narrowing"
type: pull_request
state: open
author: AlexWaygood
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: alex/type-narrow-top-materialize
created_at: 2026-01-13T16:24:48Z
updated_at: 2026-01-13T16:48:57Z
url: https://github.com/astral-sh/ruff/pull/22553
synced_at: 2026-01-13T17:25:40Z
```

# [ty] Use the top materialization of classes for `if type(x) is y` narrowing

---

_@AlexWaygood_

## Summary

This is consistent with our `isinstance()`/`issubclass()` narrowing, fixes some open TODOs in our mdtests, and leads to clearly better results.

## Test Plan

Added mdtests that fail on `main`, and updated existing mdtests that had TODOs.


---

_Review requested from @carljm by @AlexWaygood on 2026-01-13 16:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-13 16:24_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-13 16:24_

---

_Label `bug` added by @AlexWaygood on 2026-01-13 16:24_

---

_Label `ty` added by @AlexWaygood on 2026-01-13 16:24_

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 16:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-13 16:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

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

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:95:17: error[invalid-assignment] Object of type `partial[Config]` is not assignable to attribute `config_class` of type `type[SomeConfig@SubConfig]`
- Found 224 diagnostics
+ Found 225 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/native/cpu/abstractcpu.py:350:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 11069 diagnostics
+ Found 11068 diagnostics

archinstall (https://github.com/archlinux/archinstall)
+ archinstall/tui/ui/result.py:26:10: warning[redundant-cast] Value is already of type `list[ValueT@Result]`
- Found 57 diagnostics
+ Found 58 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`

core (https://github.com/home-assistant/core)
+ homeassistant/components/influxdb/__init__.py:559:27: error[unsupported-operator] Operator `-` is not supported between objects of type `int | float` and `object`
+ homeassistant/components/websocket_api/connection.py:212:12: warning[possibly-unresolved-reference] Name `cur_id` used when possibly not defined
+ homeassistant/components/websocket_api/connection.py:215:21: warning[possibly-unresolved-reference] Name `cur_id` used when possibly not defined
+ homeassistant/components/websocket_api/connection.py:220:53: warning[possibly-unresolved-reference] Name `type_` used when possibly not defined
+ homeassistant/components/websocket_api/connection.py:221:62: warning[possibly-unresolved-reference] Name `type_` used when possibly not defined
+ homeassistant/components/websocket_api/connection.py:224:21: warning[possibly-unresolved-reference] Name `cur_id` used when possibly not defined
+ homeassistant/components/websocket_api/connection.py:241:24: warning[possibly-unresolved-reference] Name `cur_id` used when possibly not defined
+ homeassistant/core.py:1795:13: error[invalid-assignment] Object of type `Mapping[str, Any] & Top[ReadOnlyDict[Unknown, Unknown]]` is not assignable to attribute `attributes` of type `ReadOnlyDict[str, Any]`
+ homeassistant/helpers/entity_platform.py:547:34: error[invalid-assignment] Object of type `Iterable[Entity] & Top[list[Unknown]]` is not assignable to `list[Entity]`
+ homeassistant/helpers/entity_platform.py:572:34: error[invalid-assignment] Object of type `Iterable[Entity] & Top[list[Unknown]]` is not assignable to `list[Entity]`
+ homeassistant/helpers/entity_platform.py:720:34: error[invalid-assignment] Object of type `Iterable[Entity] & Top[list[Unknown]]` is not assignable to `list[Entity]`
- Found 14499 diagnostics
+ Found 14510 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-13 16:31_

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 16:36_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 5 | 0 | 5 |
| `invalid-return-type` | 1 | 0 | 7 |
| `invalid-argument-type` | 2 | 2 | 3 |
| `possibly-unresolved-reference` | 6 | 0 | 0 |
| `unused-ignore-comment` | 2 | 3 | 0 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `redundant-cast` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **18** | **10** | **18** |


**[Full report with detailed diff](https://de53a325.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://de53a325.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @AlexWaygood on 2026-01-13 16:48_

Okay, so the first homeassistant hit looks like this:

```py
    def get_events_json(self):
        """Return a batch of events formatted for writing."""
        queue_seconds = QUEUE_BACKLOG_SECONDS + self.max_tries * RETRY_DELAY

        count = 0
        json = []

        dropped = 0

        with suppress(queue.Empty):
            while len(json) < BATCH_BUFFER_SIZE and not self.shutdown:
                timeout = None if count == 0 else self.batch_timeout()
                item = self.queue.get(timeout=timeout)
                count += 1

                if item is None:
                    self.shutdown = True
                elif type(item) is tuple:
                    timestamp, event = item
                    age = time.monotonic() - timestamp
```

and we're now emitting a diagnostic on the last line where we didn't before. Adding some reveal_type calls in there shows this with the `main` branch:

<details>
<summary>Screenshot</summary>

<img width="2680" height="1218" alt="image" src="https://github.com/user-attachments/assets/27489c68-db51-42e3-a48f-6e414644e840" />

</details>

and this on my PR branch:

<details>
<summary>Screenshot</summary>

<img width="2216" height="1796" alt="image" src="https://github.com/user-attachments/assets/3177b46f-fcea-4848-b6f1-2adc67379bc8" />

</details>

So this is basically a textbook case of https://github.com/astral-sh/ty/issues/1578. This PR makes our `if type(x) is y` narrowing more consistent with our narrowing elsewhere, and gets rid of undesirable `Unknown`s and strange types like `list[Unknown] & list[str]` like we have on `main`. But unpacking `(Event & tuple[object, ...]) | tuple[int | float, Mapping]` is going to result in types inferred as `object` unexpectedly.

I'm guessing the other new ecosystem diagnostics are like this too. I still think the answer here is probably better explanatory diagnostics that tell the user why we've unexpectedly inferred `object` (https://github.com/astral-sh/ty/issues/2221).

---
