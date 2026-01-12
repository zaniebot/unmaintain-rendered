```yaml
number: 17318
title: "[red-knot] add new diagnostic reporter API and use it in some places"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/red-knot-new-diagnostic-data-model
created_at: 2025-04-09T15:45:08Z
updated_at: 2025-04-10T17:21:02Z
url: https://github.com/astral-sh/ruff/pull/17318
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] add new diagnostic reporter API and use it in some places

---

_@BurntSushi_

I got to a decent checkpoint in my diagnostic refactor of Red Knot
and wanted to get it up in a PR to make sure reviews are reasonably
digestible. And also to get feedback on what I have so far before
changing it becomes gratuitously annoying.

Basically, this PR adds a new reporter API to Red Knot's
`InferContext`. It supplants the old `report_lint` and
`report_diagnostic` APIs. The actual implementation doesn't change too
much; we're mostly just shuffling around when we check rule selection
and suppressions for diagnostic skipping. The nice benefit of this new
API is that it brings `Diagnostic` to Red Knot writ large and uses the
type system to skip constructing `Diagnostic` values that are known to
never be shown.

At present, we don't quite skip as much as we possibly could. In
particular, suppressions are only checked after the `Diagnostic` is
constructed. This is because checking suppressions requires a range. So
in order to front-load suppression checking, we'd need to provide the
range up-front instead of just attaching it to the diagnostic. (There
are other API designs, but none of them stood out as obviously the
right path to follow.) I went this route because it's still an overall
improvement to the status quo (since we now check rule selection and
other things before building a diagnostic at all) and because we can
improve this in the future if it ends up being worth doing.

In the course of this work, I also tweaked some diagnostic messages,
including the "revealed type" message. I spent some effort trying
to make sure the concise message is still understandable by humans.
Although, as discussed on Discord, we do expect concise messages to get
vaguer and overall worse probably as we improve the "verbose" messages
that we expect most humans to see most of the time. We felt that we
could circle back around to improve concise messages if and when there
are compelling reasons to do so.

Ref astral-sh/ruff#16809


---

_Review requested from @carljm by @BurntSushi on 2025-04-09 15:45_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-09 15:45_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-09 15:45_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-09 15:45_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-09 15:45_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-09 15:46_

---

_Label `diagnostics` added by @AlexWaygood on 2025-04-09 15:46_

---

_Comment by @github-actions[bot] on 2025-04-09 15:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:175:31: Object of type `Literal[b""]` cannot be assigned to parameter 3 (`data`) of bound method `writestr`; expected type `SizedBuffer | str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:175:31: Argument to this function is incorrect: Expected `SizedBuffer | str`, found `Literal[b""]`

bidict (https://github.com/jab/bidict)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/__init__.py:96:10: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/__init__.py:96:10: Argument to this function is incorrect: Expected `str`, found `Literal[suppress]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:100:16: Object of type `Literal[_OrderedBidictKeysView]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:100:16: Argument to this function is incorrect: Expected `str`, found `Literal[_OrderedBidictKeysView]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:25:26: Object of type `Literal[deque]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:25:26: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[deque]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_base.py:253:53: Object of type `Literal[BidictKeysView]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_base.py:253:53: Argument to this function is incorrect: Expected `str`, found `Literal[BidictKeysView]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:88:25: Object of type `Literal[OnDup]` cannot be assigned to parameter 2 (`function`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:129:13: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:130:13: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:135:17: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:136:17: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:178:13: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:179:13: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:189:13: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:190:13: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:196:13: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:197:13: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:203:13: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:204:13: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:212:13: Object of type `partial` cannot be assigned to parameter 1 (`call1`) of function `assert_calls_match`; expected type `(...) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:213:13: Object of type `partial` cannot be assigned to parameter 2 (`call2`) of function `assert_calls_match`; expected type `(...) -> Any`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:88:25: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[OnDup]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:129:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:130:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:135:17: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:136:17: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:178:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:179:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:189:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:190:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:196:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:197:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:203:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:204:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:212:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:213:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/metrics/frame_info.py:15:20: Argument to this function is incorrect: Expected `FrameType`, found `FrameType | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:61:67: Argument to this function is incorrect: Expected `str`, found `str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/metrics/frame_info.py:15:20: Object of type `FrameType | None` cannot be assigned to parameter 1 (`frame`) of function `get_frame_info`; expected type `FrameType`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_pstats_renderer.py:125:43: Argument to this function is incorrect: Expected `Session`, found `Session | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:97:22: Argument to this function is incorrect: Expected `str`, found `Literal[suppress]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:102:18: Argument to this function is incorrect: Expected `str`, found `Literal[suppress]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:61:67: Object of type `str | None` cannot be assigned to parameter 1; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:97:22: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:102:18: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:136:17: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `<bound method `long_method` of `ClassWithMethods`>`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:225:80: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[SpeedscopeFrame]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:300:80: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[SpeedscopeEvent]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `subscribe` of `StackSampler`>`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:110:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `subscribe` of `StackSampler`>`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:125:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `unsubscribe` of `StackSampler`>`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:126:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `unsubscribe` of `StackSampler`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:54:47: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:54:47: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:77:51: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:77:51: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:166:44: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:166:44: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:201:44: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:201:44: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:233:44: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:238:32: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:233:44: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:238:32: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_threaded.py:37:24: Argument to this function is incorrect: Expected `((FrameType, str, Any, /) -> Any) | None`, found `CallCounter`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Object of type `<bound method `subscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:110:9: Object of type `<bound method `subscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:125:19: Object of type `<bound method `unsubscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:126:19: Object of type `<bound method `unsubscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/stack_sampler.py:249:53: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:336:9: Object of type `Session` cannot be assigned to parameter `context` of bound method `__init__`; expected type `FrameContext | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:336:9: Argument to this function is incorrect: Expected `FrameContext | None`, found `Session`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_pstats_renderer.py:125:43: Object of type `Session | None` cannot be assigned to parameter 2 (`session`) of bound method `render`; expected type `Session`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_threaded.py:37:24: Object of type `CallCounter` cannot be assigned to parameter 1 (`target`) of function `setstatprofile`; expected type `((FrameType, str, Any, /) -> Any) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/stack_sampler.py:249:53: Object of type `int | float | None` cannot be assigned to parameter 1 (`value`) of function `format_float_with_sig_figs`; expected type `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:56:32: Object of type `str | None` cannot be assigned to parameter 2 (`name`) of function `setattr`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:56:32: Argument to this function is incorrect: Expected `str`, found `str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:332:49: Object of type `str | None` cannot be assigned to parameter 1 (`identifier`) of function `load_report_from_temp_storage`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:332:49: Argument to this function is incorrect: Expected `str`, found `str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:445:40: Object of type `str | None` cannot be assigned to parameter 1 (`pat`) of function `translate`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:458:40: Object of type `str | None` cannot be assigned to parameter 1 (`pat`) of function `translate`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:445:40: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:458:40: Argument to this function is incorrect: Expected `str`, found `str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:533:48: Object of type `str | None` cannot be assigned to parameter 1 (`outfile`) of function `guess_renderer_from_outfile`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:533:48: Argument to this function is incorrect: Expected `str`, found `str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:136:17: Object of type `<bound method `long_method` of `ClassWithMethods`>` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:225:80: Object of type `Literal[SpeedscopeFrame]` cannot be assigned to parameter 1 (`class_or_instance`) of function `fields`; expected type `DataclassInstance | type[DataclassInstance]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:300:80: Object of type `Literal[SpeedscopeEvent]` cannot be assigned to parameter 1 (`class_or_instance`) of function `fields`; expected type `DataclassInstance | type[DataclassInstance]`

async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:62:12: Object of type `_HK` is not assignable to return type `Hashable`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:62:12: Return type does not match returned value: Expected `Hashable`, found `_HK`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:108:36: Object of type `partial` cannot be assigned to parameter 2 (`fn`) of bound method `add_done_callback`; expected type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:108:36: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:187:36: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:187:36: Object of type `partial` cannot be assigned to parameter 2 (`fn`) of bound method `add_done_callback`; expected type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/_misc/_queue_testing.py:75:52: Object of type `StreamHandler` cannot be assigned to parameter `*handlers` of bound method `__init__`; expected type `Handler`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:167:36: Object of type `partial` cannot be assigned to parameter 2 (`fn`) of bound method `add_done_callback`; expected type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:167:36: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:253:36: Object of type `partial` cannot be assigned to parameter 2 (`fn`) of bound method `add_done_callback`; expected type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:253:36: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/_misc/_queue_testing.py:75:52: Argument to this function is incorrect: Expected `Handler`, found `StreamHandler`

itsdangerous (https://github.com/pallets/itsdangerous)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_url_safe.py:14:24: Object of type `Literal[URLSafeSerializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_url_safe.py:24:24: Object of type `Literal[URLSafeTimedSerializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_signer.py:21:24: Object of type `Literal[Signer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_url_safe.py:14:24: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[URLSafeSerializer]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_url_safe.py:24:24: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[URLSafeTimedSerializer]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_signer.py:21:24: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[Signer]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_timed.py:32:24: Object of type `Literal[TimestampSigner]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_timed.py:99:24: Object of type `Literal[TimedSerializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:37:49: Object of type `Literal[Serializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_timed.py:32:24: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[TimestampSigner]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_timed.py:99:24: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[TimedSerializer]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:37:49: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[Serializer]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:188:23: Object of type `Literal[Serializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:188:23: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[Serializer]`

packaging (https://github.com/pypa/packaging)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/requirements.py:43:40: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:40:9: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:88:13: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:205:13: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:223:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:21: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:51: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:19: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:36: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:247:25: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:248:19: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:35: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:253:24: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:254:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:23: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:40: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:271:29: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:272:23: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:275:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:276:20: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:277:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:278:20: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:478:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:508:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:537:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:550:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:575:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:598:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:617:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:624:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:638:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:41: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:21: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:52: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:652:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:661:18: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:662:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:666:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:675:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:684:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:694:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:699:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:705:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:709:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:738:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:740:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:765:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:54: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:779:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:781:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:782:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:57: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:785:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:58: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:789:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:793:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:797:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:57: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:803:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:58: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:809:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:22: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:61: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:22: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:62: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:824:13: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:19: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:19: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:39: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:19: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:36: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:843:25: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:844:19: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:43: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:43: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:23: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:40: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:863:29: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:864:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:38: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:869:24: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:870:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:873:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:874:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:888:37: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:50: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:804:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:804:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:808:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:808:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:844:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:844:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:40:9: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:88:13: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:205:13: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:223:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:21: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:51: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:19: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:36: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:247:25: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:248:19: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:35: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:253:24: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:254:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:23: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:40: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:271:29: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:272:23: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:275:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:276:20: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:277:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:278:20: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:478:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:508:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:537:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:550:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:575:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:598:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:617:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:624:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:638:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:41: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:21: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:52: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:652:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:661:18: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:662:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:666:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:675:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:684:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:694:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:699:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:705:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:709:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:738:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:740:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:765:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:54: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:779:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:781:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:782:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:57: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:785:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:58: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:789:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:793:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:797:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:57: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:803:26: Object o...*[Comment body truncated]*

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:92 on 2025-04-09 15:56_

Unrelated to your PR but I think we should change the range to only include the inner expression.

---

_@sharkdp reviewed on 2025-04-09 16:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Many_parameters.snap`:36 on 2025-04-09 16:42_

This seems to be a slightly unfortunate regression? Previously, we would mark the precise parameter in the "info" span, but now we only highlight the function name. Is this intentional/temporary?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1263 on 2025-04-09 16:43_

The API looks great!

---

_@sharkdp reviewed on 2025-04-09 16:43_

---

_@sharkdp approved on 2025-04-09 16:46_

Thank you!

I only reviewed the Markdown-diagnostic changes and the snapshot changes. Especially the latter look like clear improvements to me! (with one exception, see inline comment)

---

_@BurntSushi reviewed on 2025-04-09 16:50_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Many_parameters.snap`:36 on 2025-04-09 16:50_

It's still there! I just removed the message part. But there is `^^^` for the function name and `------` for the parameter. Maybe I went a little too spartan and I should add the message back saying, "parameter declared here"?

---

_@sharkdp reviewed on 2025-04-09 16:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Many_parameters.snap`:36 on 2025-04-09 16:59_

Oh, haha :see_no_evil:. I think I completely ignored it because that new `^^^` span was diff-highlighted in dark green, whereas the `------` span just stayed as it was.

In this case, feel free to ignore my comment, but I *think* I would prefer a short "parameter declared here", yes.

---

_@BurntSushi reviewed on 2025-04-09 17:01_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Many_parameters.snap`:36 on 2025-04-09 17:01_

I think I agree. I've added a message back!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1243 on 2025-04-09 17:04_

Nice!

---

_Closed by @BurntSushi on 2025-04-09 17:40_

---

_Reopened by @BurntSushi on 2025-04-09 17:40_

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:92 on 2025-04-09 20:43_

Obviously orthogonal to this PR, but I feel like it would be a bit better if we reduced this range to just the argument expression, rather than the whole call.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/annotation.md`:12 on 2025-04-09 20:45_

nit: should we standardize on whether diagnostic messages start with a capital letter? I feel like in the past they all have, and "Revealed type" still does, but now the argument and return type ones don't...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Basic.snap`:36 on 2025-04-09 20:50_

Not sure there's anything to be done about this, but this layout (which I guess will be common for errors in calls to unary functions) was visually confusing to me -- I thought the dashes were just a line pointing to the carets, so I thought it was saying "parameter declared here" and pointing to the function name, which seemed off. It was only when I saw later multi-line examples that I realized that the carets are related to the info message above, and the dashes are actually marking the parameter.

I think actually this is improved in a real terminal by added color cues.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1214 on 2025-04-09 21:00_

TODO for future: we can also handle `BoundMethod` and maybe some other types here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/context.rs`:320 on 2025-04-09 21:14_

The rationale for this split depends on the idea that the top-line message may contain additional details that vary between instances of the lint. Interestingly this doesn't seem to be the case for the lints you've adapted so far? In both cases the top-line message doesn't vary, the details are all in annotations and sub-diagnostics. Just might be something to keep an eye on over time, how much this split actually buys us.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/context.rs`:97 on 2025-04-09 21:14_

The prose here doesn't seem to account for the builder/reporter split.

---

_Review comment by @carljm on `crates/ruff_db/src/diagnostic/mod.rs`:145 on 2025-04-09 21:16_

```suggestion
    /// Introspects this diagnostic and returns what kind of "primary" message
```

---

_Review comment by @carljm on `crates/ruff_db/src/diagnostic/mod.rs`:173 on 2025-04-09 21:18_

any reason not to use the `get_message()` method you define below, both here and above?

---

_@carljm approved on 2025-04-09 21:19_

Looks great, thank you!!

---

_@BurntSushi reviewed on 2025-04-09 22:38_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/call/annotation.md`:12 on 2025-04-09 22:38_

That's my bad. In my experience, we start messages with a capital letter pretty much everywhere (in uv too). I mean to conform with that, but sometimes I goof it up (because in my projects I use the opposite convention). I'll fix these. Nice catch. :-)

---

_@BurntSushi reviewed on 2025-04-09 22:38_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Basic.snap`:36 on 2025-04-09 22:38_

Yeah I think when you're looking at in a terminal fully rendered it looks much clearer. The colors particularly help.

---

_@BurntSushi reviewed on 2025-04-09 22:42_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/context.rs`:320 on 2025-04-09 22:42_

Hmmm. That's an interesting point. In the sense that the top line message could be derived directly from the lint metadata? Yeah, it's definitely not obvious to me that it will always be that way. We could choose to _make_ it that way. I'm not sure we have enough experience yet to make that call, so I agree with keeping an eye on this. But there are for sure common patterns that we don't know about yet that we can tweak the API to make those cases more streamlined.

Mostly what I was thinking here is that the message itself isn't needed in order to determine whether the diagnostic will be reported or not. And so having an API that forces the caller to provide it up-front can be annoying. But you're right that if the message is invariant for every lint, then this isn't actually strictly necessary because it can be derived (like the severity and diagnostic ID are today) from `LintMetadata`. (Presumably it would need to be added to the lint metadata though.)

(Sorry, kinda realized that the above two paragraphs are mostly saying the same thing.)

---

_@BurntSushi reviewed on 2025-04-09 22:43_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:173 on 2025-04-09 22:43_

Probably not. I'll check this tomorrow. I think I just added `get_message()` after-the-fact.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:320 on 2025-04-10 07:50_

We have many cases in Ruff where the top-level message differs for the same lint. 

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:92 on 2025-04-10 07:55_

Agree, that was the first thing I noticed too. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1241 on 2025-04-10 07:58_

I've a small preference to keep this named `report_lint` or can't you use `report_lint` because the old method still exists? Maybe `lint_diagnostic_builder` or similar. 

The reason I dislike `lint` is because it suggests that it looks up that lint, which it doesn't. Unless we change the return type from a `builder` to be something more along information about the lint's severity and if it's suppressed at this location. But I feel like this would muddle together too many different concerns.

I'm fine going with `lint` if we plan on renaming it to `report_lint` later (or we could rename the old `report_lint` to `report_lint_old` to make the name available. That would fit your refactoring pattern ;))

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1253 on 2025-04-10 08:04_

I'm writing this before I read builder's or reporter's documentation. Consider me in the position where I read this code for the first time without context about this PR or the infrastructure. 

* I'm having a hard time to understand the difference between `builder` and `reporter`. What's the difference? Why do we need both? 
* It seems that we don't really use the `builder` to build anything other than a `reporter` which then immediately gets transformed into a `diagnostic`. Why is it then that `builder` is a "builder" if we don't build things with it (it's not the builder pattern). 
* Why is it that we can build the `SubDiagnostic` without using `builder` or `reporter`? 

I don't have great suggestions, maybe after reading the implementation. But these were some of the questions that popped up when I saw the code the first time.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:286 on 2025-04-10 08:13_

The downside of this approach is that we still end up constructing the entire diagnostic even if it is suppressed. This, e.g. prevents users from using suppressions to circumvent bugs in the diagnostic construction (there's a bug somewhere for ruff where Ruff logs a warning because it fails to construct the fix but there's nothing the user can do about it).

What's the motivation for not passing the primary range to `lint`? Is it because you worry that determining the primary range could be expensive? Or is it because it makes the API more awkward because you want users to use the `Diagnostic::annotate` method?

I'm somewhat leaning towards changing `lint` to always take a primary annotation. I'm not sure of any use case where the `primary_annotation` would be missing. Would this also help to remove the need for the `Reporter` / `Builder` split?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:297 on 2025-04-10 08:18_

This **must** be the same file or the diagnostic won't show up if we only check the primay's span file but not the `infer_context.file`. I think it's worth adding an `assertion` that this invariant is uphold. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:304 on 2025-04-10 08:19_

Yeah, I think we should just make this mandatory. A missing range means that the diagnostic isn't actionable. You don't even know from which file it comes from. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:418 on 2025-04-10 08:23_

I think this fits better into `LintReporter` because it's only there where we have rules. 

---

_@MichaReiser reviewed on 2025-04-10 08:27_

This is great. 

I left a few comments around `builder`/`reporter`. I'm not sure if the current split is more complicated than what we need. Mainly, because I think we should make the primary annotation mandatory (and assert that it belongs to the same file as `infer_context.file()`). See my inline comments for more a more in-depth explanation.

---

_@sharkdp reviewed on 2025-04-10 10:22_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:92 on 2025-04-10 10:22_

I created a ticket for this a while back: https://github.com/astral-sh/ruff/issues/15907

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-10 13:58_

---

_@BurntSushi reviewed on 2025-04-10 17:05_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:92 on 2025-04-10 17:05_

Yeah. This was linked above: https://github.com/astral-sh/ruff/issues/15907

---

_@BurntSushi reviewed on 2025-04-10 17:10_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1241 on 2025-04-10 17:10_

I personally like `lint()` for this, since it's being used as a verb and that is, at a high level, what is being done: you call it when you want to lint something.

With that said, I don't think I feel strongly and I'm happy to trust your instincts on this one.

I did: `report_lint() -> report_lint_old()`, `lint() -> report_lint()` and `report() -> report_diagnostic()`.

---

_Comment by @BurntSushi on 2025-04-10 17:17_

I've addressed all of the minor concerns here. Once CI is green, I'll plan to merge this because I'm touching a lot of places in the code and I'd like to avoid accruing more conflicts.

@MichaReiser and I discussed his other concerns here earlier, principally revolving around these two things:

* The reporter builder API is perhaps a little annoying to use, and in particular, we might be able to simplify it somewhat by _requiring_ that all lint diagnostics come with a primary annotation.
* There is really nothing here that _enforces_ that the primary span of a `Diagnostic` has a `File` that corresponds to the `File` being type checked that created the `Diagnostic`. We should probably _at least_ have a runtime panic if this is violated, but perhaps we can enforce this at the API level as well.

I have some ideas on how to resolve this but I want to play around with the API a bit to see what feels right. I want to try and get this right _before_ refactoring the rest of Red Knot to hopefully avoid going through that multiple times.

---

_Merged by @BurntSushi on 2025-04-10 17:21_

---

_Closed by @BurntSushi on 2025-04-10 17:21_

---

_Branch deleted on 2025-04-10 17:21_

---
