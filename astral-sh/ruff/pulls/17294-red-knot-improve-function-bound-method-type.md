```yaml
number: 17294
title: "[red-knot] improve function/bound method type display"
type: pull_request
state: merged
author: mishamsk
labels:
  - ty
assignees: []
merged: true
base: main
head: 17238-red-knot-improve-function-and-class-literal-type-display
created_at: 2025-04-08T12:56:29Z
updated_at: 2025-04-27T23:20:58Z
url: https://github.com/astral-sh/ruff/pull/17294
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] improve function/bound method type display

---

_@mishamsk_

## Summary

* Partial #17238
* Flyby from discord discussion - `todo_type!` now statically checks for no parens in the message to avoid issues between debug & release build tests

Mdtests are not updated yet per discussion. Here is the current knot vs. pyright vs. mypy:

```py
from typing import reveal_type

def foo(): ...
class C:
    def meth(self, x: int) -> str: ...

# knot: Revealed type is 'function foo: () -> Unknown'
# pyright: Type of "foo" is "() -> None"
# mypy: Revealed type is "def () -> Any"
reveal_type(foo)

# knot: Revealed type is 'function meth: (self, x: int) -> str'
# pyright: Type of "C.meth" is "(self: C, x: int) -> str"
# mypy: Revealed type is "def (self: __main__.C, x: builtins.int) -> builtins.str"
reveal_type(C.meth)

# knot: Revealed type is 'bound method "meth" of "C": (x: int) -> str'
# pyright: Type of "C().meth" is "(x: int) -> str"
# mypy: Revealed type is "def (x: builtins.int) -> builtins.str"
reveal_type(C().meth)
```

## Test Plan

many mdtests are changing


---

_Review requested from @carljm by @mishamsk on 2025-04-08 12:56_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-04-08 12:56_

---

_Review requested from @sharkdp by @mishamsk on 2025-04-08 12:56_

---

_Review requested from @dcreager by @mishamsk on 2025-04-08 12:56_

---

_Comment by @github-actions[bot] on 2025-04-08 12:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:65:1: Object of type `Literal[_make_key]` is not assignable to `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Hashable`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:65:1: Object of type `def _make_key(args: @Todo(full tuple[...] support), kwds: @Todo(generics), *, /, _typ: (object, /) -> type = Literal[type], _fast_types: @Todo(generics) = set) -> Hashable` is not assignable to `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Hashable`

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:28:5: Object of type `@Todo(map_with_boundness: intersections with negative contributions) | (bound method FakeClock.get_time() -> Unknown)` is not assignable to attribute `timer_func` of type `(() -> int | float) | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:136:17: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method ClassWithMethods.long_method() -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.subscribe(target: Unknown | @Todo(Inference of subscript on special form), *, desired_interval: int | float, use_timing_thread: bool | None = None, use_async_context: bool) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:110:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.subscribe(target: Unknown | @Todo(Inference of subscript on special form), *, desired_interval: int | float, use_timing_thread: bool | None = None, use_async_context: bool) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:125:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.unsubscribe(target: Unknown | @Todo(Inference of subscript on special form)) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:126:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.unsubscribe(target: Unknown | @Todo(Inference of subscript on special form)) -> Unknown`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:28:5: Object of type `@Todo(map_with_boundness: intersections with negative contributions) | <bound method `get_time` of `FakeClock`>` is not assignable to attribute `timer_func` of type `(() -> int | float) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `subscribe` of `StackSampler`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:110:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `subscribe` of `StackSampler`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:125:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `unsubscribe` of `StackSampler`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:126:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `unsubscribe` of `StackSampler`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:136:17: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `<bound method `long_method` of `ClassWithMethods`>`

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:473:30: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeAlias`)`, found `Literal[view_func]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:473:30: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeAlias`)`, found `def view_func(endpoint, values) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/middleware/test_profiler.py:37:13: Argument to this function is incorrect: Expected `str`, found `Literal[filename_format]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/test_formparser.py:436:45: Argument to this function is incorrect: Expected `TStreamFactory | None`, found `Literal[stream_factory]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/headers.py:65:75: Cannot subscript object of type `Literal[set]` with no `__getitem__` method
+ error[lint:non-subscriptable] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/headers.py:65:75: Cannot subscript object of type `def set(self, key: str, value: Any, /, **kwargs: Any) -> None` with no `__getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/headers.py:230:75: Cannot subscript object of type `Literal[set]` with no `__getitem__` method
+ error[lint:non-subscriptable] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/headers.py:230:75: Cannot subscript object of type `def set(self, key: str, value: Any, /, **kwargs: Any) -> None` with no `__getitem__` method
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/middleware/test_profiler.py:37:13: Argument to this function is incorrect: Expected `str`, found `def filename_format(env) -> Unknown`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:181:13: Object of type `def default_stream_factory(total_content_length: int | None, content_type: str | None, filename: str | None, content_length: int | None = None) -> @Todo(generics)` is not assignable to `TStreamFactory | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:305:13: Object of type `def default_stream_factory(total_content_length: int | None, content_type: str | None, filename: str | None, content_length: int | None = None) -> @Todo(generics)` is not assignable to `TStreamFactory | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:181:13: Object of type `Literal[default_stream_factory]` is not assignable to `TStreamFactory | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:305:13: Object of type `Literal[default_stream_factory]` is not assignable to `TStreamFactory | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:376:21: Object of type `<bound method `append` of `list`>` is not assignable to `(bytes, /) -> Any`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:376:21: Object of type `bound method list.append(object: Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None` is not assignable to `(bytes, /) -> Any`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/test_wrappers.py:371:47: Argument to this function is incorrect: Expected `Response`, found `(def wsgi_application(environ, start_response) -> Unknown) | Response`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/test_formparser.py:436:45: Argument to this function is incorrect: Expected `TStreamFactory | None`, found `def stream_factory() -> Unknown`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/test.py:98:9: Object of type `(bound method BytesIO.write(buffer: @Todo(Support for `typing.TypeAlias`), /) -> int) | @Todo(instance attribute on class with dynamic base)` is not assignable to `(bytes, /) -> int`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/repr.py:117:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def proxy(self: DebugReprGenerator, obj: @Todo(generics), recursive: bool) -> str`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/test.py:98:9: Object of type `<bound method `write` of `BytesIO`> | @Todo(instance attribute on class with dynamic base)` is not assignable to `(bytes, /) -> int`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/repr.py:117:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `Literal[proxy]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/tests/test_wrappers.py:371:47: Argument to this function is incorrect: Expected `Response`, found `Literal[wsgi_application] | Response`

black (https://github.com/psf/black)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:511:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[main]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:511:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def main(ctx: Context, code: str | None, line_length: int, target_version: @Todo(generics), check: bool, diff: bool, line_ranges: @Todo(generics), color: bool, fast: bool, pyi: bool, ipynb: bool, python_cell_magics: @Todo(generics), skip_source_first_line: bool, skip_string_normalization: bool, skip_magic_trailing_comma: bool, preview: bool, unstable: bool, enable_unstable_feature: @Todo(generics), quiet: bool, verbose: bool, required_version: str | None, include: @Todo(generics), exclude: @Todo(generics) | None, extend_exclude: @Todo(generics) | None, force_exclude: @Todo(generics) | None, stdin_filename: str | None, workers: int | None, src: @Todo(full tuple[...] support), config: str | None) -> None`

rich (https://github.com/Textualize/rich)
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/cells.py:30:1: Object of type `(bound method frozenset.issuperset(s: @Todo(generics), /) -> bool) | @Todo(instance attribute on class with dynamic base)` is not assignable to `(str, /) -> bool`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/table.py:359:5: Argument to this function is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: Unknown | @Todo(Inference of subscript on special form)) -> Table`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/text.py:737:29: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method Console.get_style(name: str | Style, *, default: Style | str | None = None) -> Style`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_repr.py:57:5: Unresolved attribute `angular` on type `def __rich_repr__(self) -> Unknown`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_repr.py:57:5: Unresolved attribute `angular` on type `Literal[__rich_repr__]`.
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/cells.py:30:1: Object of type `<bound method `issuperset` of `frozenset`> | @Todo(instance attribute on class with dynamic base)` is not assignable to `(str, /) -> bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/text.py:737:29: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `<bound method `get_style` of `Console`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/table.py:359:5: Argument to this function is incorrect: Expected `(Any, Any, /) -> None`, found `Literal[padding]`

scrapy (https://github.com/scrapy/scrapy)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:95:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method list.append(object: Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:107:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method list.append(object: Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:121:64: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method list.append(object: Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:134:63: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method list.append(object: Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_robotstxt.py:159:16: Type `<bound method `_logerror` of `RobotsTxtMiddleware`>` has no attribute `called`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_robotstxt.py:159:16: Type `bound method RobotsTxtMiddleware._logerror(failure: Unknown, request: Request, spider: Spider) -> Unknown` has no attribute `called`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:118:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def cached(self) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:228:30: Argument to this function is incorrect: Expected `(...) -> Any`, found `bound method A.method(a, b, c) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:235:30: Argument to this function is incorrect: Expected `(...) -> Any`, found `bound method Literal[" "].join(**kwargs: @Todo(todo signature **kwargs)) -> @Todo(return type of overloaded function)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:105:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _headers_encoding(self) -> str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:134:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _body_declared_encoding(self) -> str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:138:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _bom_encoding(self) -> str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:570:49: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:573:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:584:50: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:587:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_command_check.py:198:9: Type `bound method CrawlerProcess.crawl(crawler_or_spidercls: type[Spider] | str | Crawler, *args: Any, **kwargs: Any) -> @Todo(generics)` has no attribute `assert_not_called`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:207:57: Argument to this function is incorrect: Expected `(...) -> Any`, found `Unknown & ~AlwaysFalsy | @Todo(Inference of subscript on special form) & ~AlwaysFalsy | Unknown & @Todo(Inference of subscript on special form) & ~AlwaysFalsy | (bound method Spider._parse(response: Response, **kwargs: Any) -> Any)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:118:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[cached]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:228:30: Argument to this function is incorrect: Expected `(...) -> Any`, found `<bound method `method` of `A`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:235:30: Argument to this function is incorrect: Expected `(...) -> Any`, found `<bound method `join` of `Literal[" "]`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:105:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[_headers_encoding]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:134:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[_body_declared_encoding]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:138:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[_bom_encoding]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:570:49: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `returns_request` of `DemoSpider`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:573:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `returns_request` of `DemoSpider`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:584:50: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `returns_request` of `DemoSpider`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:587:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<bound method `returns_request` of `DemoSpider`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:95:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `<bound method `append` of `list`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:107:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `<bound method `append` of `list`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:121:64: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `<bound method `append` of `list`>`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_defer.py:134:63: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `<bound method `append` of `list`>`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_command_check.py:198:9: Type `<bound method `crawl` of `CrawlerProcess`>` has no attribute `assert_not_called`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:207:57: Argument to this function is incorrect: Expected `(...) -> Any`, found `Unknown & ~AlwaysFalsy | @Todo(Inference of subscript on special form) & ~AlwaysFalsy | Unknown & @Todo(Inference of subscript on special form) & ~AlwaysFalsy | <bound method `_parse` of `Spider`>`

```
</details>


---

_Comment by @InSyncWithFoo on 2025-04-08 13:33_

What does Red Knot say in this case?

```python
def foo(): ...

a = foo
reveal_type(a)  # ?
```

If the message is also `function foo: () -> Unknown`, then it will be rather confusing.

---

_Comment by @mishamsk on 2025-04-08 13:52_

> What does Red Knot say in this case?
> 
> ```python
> def foo(): ...
> 
> a = foo
> reveal_type(a)  # ?
> ```
> 
> If the message is also `function foo: () -> Unknown`, then it will be rather confusing.

it does... I am conflicted. I agree that it may be confusing, and that's probably the reason why pyright/mypy do not carry on the name from the definition, they just have a different default text for reveal_type which always includes the binding.

But on the other hand, this alternative approach correctly leads to the original definition, which may be an interesting way to triage binding source ;-)

I'll wait for more opinions and will do a second take based on all of the feedback

---

_Comment by @MichaReiser on 2025-04-08 13:58_

I'd prefer if we could use `def` over `function` because it is valid python syntax, allowing us to use python-code blocks in on-hover (editors can then do syntax highlighting on the types). Or that we omit the `function` entirely (similar to pyright), representing it as callable (it's then the LSP that adds the `def` and function name but only on-hover (when showing the signature)

---

_Comment by @carljm on 2025-04-08 14:00_

I don't think it's confusing; it's an accurate reflection of runtime behavior:

```py
>>> def foo(): ...
...
>>> a = foo
>>> a.__name__
'foo'
>>> a
<function foo at 0x105212840>
```

Functions carry their original name-at-definition with them, even if reassigned to a different name.

---

_Comment by @carljm on 2025-04-08 14:01_

> I'd prefer if we could use `def` over `function` because it is valid python syntax

This could work for functions, and I don't mind it, but it won't give us this property universally. How would we display the type of a bound method? We want to show the signature without `self`, but also reflect the fact that it is a bound method, and show the bound instance type.

---

_Comment by @carljm on 2025-04-08 14:09_

Here's a proposal:

regular function: `def foo(x: int) -> str`
bound method: `def C.foo(x: int) -> str`

It's less explicit than if we include the text "bound method", but I think it's pretty intuitive, and it is valid Python syntax? (edit: well I guess not quite, `def C.foo` isn't valid syntax -- but maybe close enough to still work in the renderer? I don't think we will be able to maintain that all type displays are fully valid Python syntax)

Also, a nit: PR title claims it changes class literal display, too, but the PR does not.

The `todo_type!` change looks great, thank you! In future would probably prefer unrelated things in separate PRs, but since it's already here, no problem keeping it here.

---

_Comment by @InSyncWithFoo on 2025-04-08 14:17_

> regular function: `def foo(x: int) -> str`  \
> bound method: `def C.foo(x: int) -> str`

That does seem to be a very good choice. It reminds me of Kotlin's [extension functions](https://kotlinlang.org/docs/extensions.html#extension-functions):

```kotlin
interface A<T> { val v: T }

fun A<Int>.foo() = println("Int: $v")
fun A<String>.foo() = println("String: $v")
```

> Functions carry their original name-at-definition with them, even if reassigned to a different name.

That's true. Perhaps a better suggestion would be to change the entire message as a whole:

```python
reveal_type(a)  # Revealed type for variable `a` is `def foo() -> Unknown`
                #                   ^^^^^^^^^^^^ Customized for each kind of expression
```

---

_Comment by @dhruvmanila on 2025-04-08 14:20_

> bound method: `def C.foo(x: int) -> str`

Not a strong preference but another possibility is to use `self` parameter like `def foo(self: C, x: int) -> str` which by the looks of it what Pyright does.

> Perhaps a better suggestion would be to change the entire message as a whole:

I'd be fine to include the _expression_ (similar to Pyright) but that doesn't seem related to this PR. We should avoid "variable" because `reveal_type` could include any expression.

---

_Comment by @carljm on 2025-04-08 14:29_

> I'd be fine to include the _expression_ (similar to Pyright) but that doesn't seem related to this PR.

Agreed on both counts.

> We should avoid "variable" because `reveal_type` could include any expression.

I think this was already acknowledged by the "customized for each kind of expression" note. But we should discuss the design of this feature in its own issue, not here.

> another possibility is to use `self` parameter like `def foo(self: C, x: int) -> str`

I don't like this because it wrongly suggests that `self` is part of the callable signature of the bound method (and can/should be provided by a caller).

---

_Comment by @mishamsk on 2025-04-08 15:21_

I agree with Carl rgd self.

How about this:

regular function: `def foo(x: int) -> str`
bound method: `bound method C.foo(x: int) -> str`

?

Rgd @MichaReiser point about being able to render as markdown. If we really want to make it work, we would need to do: `def foo(x: int) -> str: ...` and only for function. I think it is fine actually and will look like something coming from a `pyi` file

---

_Renamed from "[red-knot] improve function and class literal type display" to "[red-knot] improve function/bound method type display" by @mishamsk on 2025-04-08 15:41_

---

_Comment by @carljm on 2025-04-08 18:05_

> regular function: `def foo(x: int) -> str`
> bound method: `bound method C.foo(x: int) -> str`

I'm personally fine with this option, too. I don't think I have a clear understanding of how far we can stretch away from "valid Python syntax" and still get nice rendering. Perhaps @MichaReiser or @dhruvmanila can offer more details here.

I don't think we want to require fully valid Python syntax; adding a trailing `: ...` is just noise.

---

_Comment by @MichaReiser on 2025-04-08 19:51_

The lsp specification is pretty vague but I expect that most lsp do syntax highlighting similar to what you get in markdown files. That means incomplete syntax is fine. 

The part I'm more worried about are unions over functions because a def keyword in those positions is simply invalid.

The easiest way to test this is to make the change and change the markup renditions in on hover to use a python code block (there are other types that use <> syntax that prevent us from switching to python just yet)

---

_Comment by @carljm on 2025-04-08 21:30_

I actually think we could probably even eliminate the `def` keyword and just do `f(x: int) -> str` and `C.f(x: int) -> str`. This would then look very much like our general callable type syntax, which is the same but without the function name: `(x: int) -> str`.

But I'm not sure how any of these would look if syntax-highlighted as Python inside a union (separated by `|`)

---

_Comment by @mishamsk on 2025-04-09 02:19_

@MichaReiser @carljm screenshots!

so the suggested `def foo() -> Unknown` works if I set markdown to use `python` as code fence language:
![CleanShot 2025-04-08 at 22 13 12@2x](https://github.com/user-attachments/assets/8dc1c959-9f6c-4a00-88af-54847c198908)

and as Carl said, dots don't influence rendering at all:
![CleanShot 2025-04-08 at 22 14 45@2x](https://github.com/user-attachments/assets/eda9e5e3-2909-41dc-8c27-b0021900c293)

bound methods are not all black and white either, so not too bad IMHO:
![CleanShot 2025-04-08 at 22 14 54@2x](https://github.com/user-attachments/assets/82495c8d-7925-43b5-90c9-4f4265cab97a)

and unions too!
![CleanShot 2025-04-08 at 22 15 42@2x](https://github.com/user-attachments/assets/e8f469a8-033e-4da5-9ae9-596ab80133d6)

So all in all I suggest moving forward with this:

> regular function: def foo(x: int) -> str
> bound method: bound method C.foo(x: int) -> str

let me know if any objections left.

Also - I assume I am NOT changing the code-fence language, Micha will do it at some point. Correct?

---

_Comment by @carljm on 2025-04-09 03:29_

Thanks for doing this experimentation to drive this forward!

I think my remaining concern here is just about ambiguity of unions and intersection display; isn't it possible that two different types could both be rendered as `def foo() -> str | def bar() -> str` -- either a union of two callables, `foo` and `bar`, both nullary functions returning `str`, or a single function `foo` that returns either `str` or a function `bar`?

Perhaps this is not a problem we need to deal with in the function-literal display, but rather in the union/intersection display by wrapping some types with possibly-ambiguous representations in extra parentheses?

Other than this, I'm fine with the proposed display.

---

_Comment by @InSyncWithFoo on 2025-04-09 11:01_

> I think my remaining concern here is just about ambiguity of unions and intersection display [...]

#16907 and #16914 did this for pure callables. Shouldn't be hard to extend to literal functions as well.

---

_Comment by @mishamsk on 2025-04-09 11:24_

> > I think my remaining concern here is just about ambiguity of unions and intersection display [...]
> 
> #16907 and #16914 did this for pure callables. Shouldn't be hard to extend to literal functions as well.

ah, well, I faked the display when doing screenshots (just hardcoded two unioned signatures for a single callable). So seems like I can indeed proceed as unions/intersections are already handled.

I’ll wait for formal ok from @MichaReiser and will go ahead and update all mdtests on top of most recent main

---

_Comment by @MichaReiser on 2025-04-09 13:31_

Sounds good to me.

---

_Comment by @AlexWaygood on 2025-04-09 14:48_

What's our plans for overloads? Neither mypy nor pyright has what I would consider a user-friendly display for overloads:

```py
from typing import overload

@overload
def f(x: str) -> int: ...
@overload
def f(x: int) -> str: ...
def f(x: int | str) -> int | str:
    if isinstance(x, int):
        return "foo"
    return 42

reveal_type(f)
```

Mypy says:

```
main.py:12: note: Revealed type is "Overload(def (x: builtins.str) -> builtins.int, def (x: builtins.int) -> builtins.str)"
```

Pyright says:

```
Type of "f" is "Overload[(x: str) -> int, (x: int) -> str]"
```

What if instead of trying to cram the whole signature(s) into the top-level message, we displayed the signature as a subdiagnostic? That might work well for single-signature functions and multi-signature (overloaded) functions. It would also more clearly distinguish function-literal types from abstract `Callable` types or callable protocols.

For a single-signature function we could say:

```
Revealed type is <function 'f'>

--> Note: signature of 'f' is:

    def f(x: str) -> int: ...
```

And for an overloaded function:

```
Revealed type is <function 'f'>

--> Note: signature of 'f' is:

    @overload
    def f(x: str) -> int: ...
    @overload
    def f(x: int) -> str: ...
```

---

_Comment by @carljm on 2025-04-09 14:52_

> What if instead of trying to cram the whole signature(s) into the top-level message, we displayed the signature as a subdiagnostic?

I think we could do this for `reveal_type` specifically, but I'm not sure how it could scale to all the other places where we need a display for a type (e.g. displaying it inside a union, or as part of some other diagnostic message about some assignability error, etc). So I think we still need to have a single-string representation of all types. We could decide that that representation can just be opaque for overloads... but I think this will result in some error messages that aren't very understandable.

I think that we should do something similar to mypy and pyright, where we just show the signatures of all the overloads. It's a lot of information, but that's just because there's a lot of information there! I don't have strong feelings about the exact notation.

---

_Comment by @AlexWaygood on 2025-04-09 14:56_

Fair enough! I guess I'd still be interested in possibly special-casing `reveal_type` to make its output more readable, but that's a good point that the display of a type matters in lots of other places too. And this PR doesn't preclude doing further work to improve the `reveal_type` output in the future!

---

_Comment by @AlexWaygood on 2025-04-09 15:06_

> ```diff
> pyinstrument (https://github.com/joerick/pyinstrument)
> - error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Object of type `<bound method `subscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
> + error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Object of type `bound method StackSampler.subscribe(target: Unknown | @Todo(Inference of subscript on special form), *, desired_interval: int | float, use_timing_thread: bool | None = None, use_async_context: bool) -> Unknown` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
> ```

I'm still not sure we should be reporting the _whole_ signature in the default display... it makes error messages like this _very_ long. To me, the message here was great as it was for a top-line summary. The signature is important in order to understand _why_ the object is not assignable to parameter 2, but that information should be presented as a subdiagnostic IMO.

Should we consider falling back to a more concise display if the signature is very long, like this? And then always reporting the full signature in a subdiagnostic where it's relevant to the reason the diagnostic was emitted? Or do we just need to be more careful about using the full display in top-line diagnostic messages like this in the future?

---

_Comment by @InSyncWithFoo on 2025-04-09 15:19_

@AlexWaygood For Pyright, this is perhaps not a problem, since if a user needed to see all signatures of a function, they would likely hover over its name rather than using `reveal_type()`.

---

_Comment by @dhruvmanila on 2025-04-09 15:30_

> What if instead of trying to cram the whole signature(s) into the top-level message, we displayed the signature as a subdiagnostic?

I think this might be an interesting path to explore for hover implementation as we have the flexibility there i.e., using multi-line signature. Unions might be an issue here though.

> I'm still not sure we should be reporting the _whole_ signature in the default display... it makes error messages like this _very_ long.

Similarly, for hover implementation, we could split the parameter list across multiple lines similar to what `ruff format` would do and use that.

---

_Comment by @carljm on 2025-04-09 20:39_

> Should we consider falling back to a more concise display if the signature is very long

I think we could consider this, but I don't think it should block this PR. For short signatures (and many are) it's clearly better to just get all the information at once, so I think that should be our starting point, and we can consider layering on something more complex for long signatures as a follow-up.

---

_Label `red-knot` added by @carljm on 2025-04-10 20:44_

---

_Comment by @mishamsk on 2025-04-11 02:19_

well, this was much more painful than I expected. Also, noticed how weird the `Literal[C]` and `type[C]` now look, but these should be easy to replace with regexes, the heavy lifting was with signatures as there were a lot of non-uniform changes. I did write a small helper script which I won't be sharing ;-)

there are a couple of other rough edges, like those pointed out by @AlexWaygood but I'd rather merge this one and then do an incremental change than re-applying everything from scratch.

---

_Review requested from @MichaReiser by @mishamsk on 2025-04-11 02:23_

---

_Review comment by @carljm on `crates/red_knot_ide/src/hover.rs`:316 on 2025-04-11 02:39_

This doesn't look right, this should be something like `(def foo(a, b) -> Unknown) | (def bar(a, b) -> Unknown)`. It looks like the code for formatting callables in unions still wrongly considers them literals.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1288 on 2025-04-11 02:40_

It looks like here we need parenthesis-wrapping to avoid ambiguity?
```suggestion
reveal_type(Foo.__repr__)  # revealed: (def __repr__(self) -> str) & Any
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1587 on 2025-04-11 02:44_

This looks like it needs some attention. It seems like we are emitting the representation of the function literal `f` as the bound self type for the `__call__` method, but that ends up looking quite odd. I think it could work but we need to parenthesize at the very least.
```suggestion
reveal_type(f.__call__)  # revealed: bound method (def f() -> Unknown).__call__(*args: Any, **kwargs: Any) -> Any
```
It also looks like we have the wrong signature for `__call__` here, but that's a separate issue, not part of this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:148 on 2025-04-11 02:47_

Here again we are wrongly wrapping a callable type in a union inside `Literal[...]`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:147 on 2025-04-11 02:48_

```suggestion
    # TODO: Ideally, this would just be `def index(...)`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:184 on 2025-04-11 02:53_

I think the bound method type also needs parenthesization in a union

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:273 on 2025-04-11 02:55_

Hmm, this is an interesting case. Not actually sure how this should render! It could be `Meta.f(arg: int) -> str`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:596 on 2025-04-11 02:58_

More cases here of functions inside unions rendering wrongly as `Literal[...]`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conditional.md`:106 on 2025-04-11 02:59_

```suggestion
# TODO: We should disambiguate in such cases between `b.f` and `c.f`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly_invalid_`__getitem__`_methods.snap`:51 on 2025-04-11 03:02_

I think in practice I'm finding the "bound method " initial text for bound method a bit verbose and overly spaced out from the rest of the type. Not sure at the moment what to do instead, but I still think this is an improvement over the status quo, so we can iterate on this as a follow up.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:104 on 2025-04-11 03:07_

Not sure of the accuracy of this TODO. I think in most cases when we have an actual generic function, it won't be specialized. There's no syntax to specialize a function, and no "constructor" where it would be specialized, typically it is only "specialized" when actually called, based on inference from the arguments at that call site. So I don't know if we will ever need to display specialized generic functions as a type.

What I think we do want for generic functions is to show them as generic like they would be defined, e.g. `def foo[F](x: F) -> F`. So this doesn't involve showing a _specialization_ in brackets, it involves showing the type parameters in brackets.

When it's a method of a specialized generic class, we should specialize it by simply replacing all occurrences of class typevars in the signature with their specialized types, but not showing the specialization in brackets.

---

_@carljm approved on 2025-04-11 03:10_

I think in 9/10 cases I'm seeing, this is a clear improvement. There are some issues I think we need to iron out, commented inline. I'm ok with doing them as follow-ups if necessary to avoid accumulating merge conflicts here, but some of them would be pretty hi-pri follow-ups (e.g. the display of function literals inside `Literal[...]` when in unions, the very weird-looking display when the self-type of a bound method is a function literal).

---

_@mishamsk reviewed on 2025-04-11 11:35_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:273 on 2025-04-11 11:35_

yeah, I saw this one - I suggest to keep this for a follow-up, because I anticipate debate here and it maybe more prudent to first fix how we display class literals and look at this case.

---

_@mishamsk reviewed on 2025-04-11 11:35_

---

_Review comment by @mishamsk on `crates/red_knot_ide/src/hover.rs`:316 on 2025-04-11 11:35_

totally agree. I'll look into unions in this PR, hopefully not controversial as we already have an agreed upon approach for them

---

_@mishamsk reviewed on 2025-04-11 11:37_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1587 on 2025-04-11 11:37_

thanks a lot for spotting, I think I missed this one yesterday. As-in it was replaced by my script and I haven't noticed it.

I'll look into this as part of this PR as it seems odd.

---

_@mishamsk reviewed on 2025-04-11 11:41_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly_invalid_`__getitem__`_methods.snap`:51 on 2025-04-11 11:41_

the only other option is to do something like `Clazz().meth(...)` or `Clazz.math(self: Clazz, ..)` but given that python itself uses the `<bound method...` at runtime, I think it is helpful even if verbose. I think habit/learned patterns trumps "beauty" in many cases. People are used to something and it "clicks".

But being once own devil's advocate, I can argue that it is rare for regular devs to see `<bound method` message, but they often look at bound method signatures from lsp's...

Anyways - this is controversial, so I suggest to tackle this in a follow-up

---

_@mishamsk reviewed on 2025-04-11 11:43_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/display.rs`:104 on 2025-04-11 11:43_

yeah, sorry, my bad. I was using the word `specialisation` for both actual type and generic TypeVar. I meant what you said, but used poor language. Will fix. Appreciate your attention to details!

---

_@mishamsk reviewed on 2025-04-13 20:58_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1587 on 2025-04-13 20:58_

had to add new variant of method wrapper, but it turned out to be very simple and cause no other changes. The code with these method wrappers screams to be abstracted as they all seem very similar, but a tolerable duplication for now IMHO

---

_Review requested from @carljm by @mishamsk on 2025-04-13 21:02_

---

_Review request for @sharkdp removed by @sharkdp on 2025-04-14 07:33_

---

_@carljm approved on 2025-04-14 22:55_

Looks good, thank you!!

---

_Merged by @carljm on 2025-04-14 22:56_

---

_Closed by @carljm on 2025-04-14 22:56_

---

_Branch deleted on 2025-04-27 23:20_

---
