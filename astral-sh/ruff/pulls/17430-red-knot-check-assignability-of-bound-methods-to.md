```yaml
number: 17430
title: "[red-knot] Check assignability of bound methods to callables"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/bound-method-assignability
created_at: 2025-04-16T17:52:40Z
updated_at: 2025-04-16T18:52:01Z
url: https://github.com/astral-sh/ruff/pull/17430
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Check assignability of bound methods to callables

---

_@dhruvmanila_

## Summary

This is similar to https://github.com/astral-sh/ruff/pull/17095, it adds assignability check for bound methods to callables.

## Test Plan

Add test cases to for assignability; specifically it uses gradual types because otherwise it would just delegate to `is_subtype_of`.


---

_Label `red-knot` added by @dhruvmanila on 2025-04-16 17:52_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-16 17:52_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-16 17:52_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-16 17:52_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-16 17:52_

---

_Comment by @github-actions[bot] on 2025-04-16 17:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.subscribe(target: Unknown | @Todo(Inference of subscript on special form), *, desired_interval: int | float, use_timing_thread: bool | None = None, use_async_context: bool) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:110:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.subscribe(target: Unknown | @Todo(Inference of subscript on special form), *, desired_interval: int | float, use_timing_thread: bool | None = None, use_async_context: bool) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:125:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.unsubscribe(target: Unknown | @Todo(Inference of subscript on special form)) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:126:19: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method StackSampler.unsubscribe(target: Unknown | @Todo(Inference of subscript on special form)) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:136:17: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method ClassWithMethods.long_method() -> Unknown`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:28:5: Object of type `@Todo(map_with_boundness: intersections with negative contributions) | (bound method FakeClock.get_time() -> Unknown)` is not assignable to attribute `timer_func` of type `(() -> int | float) | None`
- Found 275 diagnostics
+ Found 269 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/cells.py:30:1: Object of type `(bound method frozenset.issuperset(s: @Todo(generics), /) -> bool) | @Todo(instance attribute on class with dynamic base)` is not assignable to `(str, /) -> bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/text.py:737:29: Argument to this function is incorrect: Expected `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method Console.get_style(name: str | Style, *, default: Style | str | None = None) -> Style`
- Found 560 diagnostics
+ Found 558 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:376:21: Object of type `bound method list.append(object: Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None` is not assignable to `(bytes, /) -> Any`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/test.py:98:9: Object of type `(bound method BytesIO.write(buffer: @Todo(Support for `typing.TypeAlias`), /) -> int) | @Todo(instance attribute on class with dynamic base)` is not assignable to `(bytes, /) -> int`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/local.py:370:50: Unused blanket `type: ignore` directive
- Found 687 diagnostics
+ Found 686 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:570:49: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:573:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:584:50: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_contracts.py:587:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `bound method DemoSpider.returns_request(response) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:228:30: Argument to this function is incorrect: Expected `(...) -> Any`, found `bound method A.method(a, b, c) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_utils_python.py:235:30: Argument to this function is incorrect: Expected `(...) -> Any`, found `bound method Literal[" "].join(**kwargs: @Todo(todo signature **kwargs)) -> @Todo(return type of overloaded function)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:207:57: Argument to this function is incorrect: Expected `(...) -> Any`, found `Unknown & ~AlwaysFalsy | @Todo(Inference of subscript on special form) & ~AlwaysFalsy | Unknown & @Todo(Inference of subscript on special form) & ~AlwaysFalsy | (bound method Spider._parse(response: Response, **kwargs: Any) -> Any)`
- Found 1583 diagnostics
+ Found 1576 diagnostics

```
</details>


---

_Comment by @dhruvmanila on 2025-04-16 17:55_

(Checking the ecosystem diff...)

---

_Comment by @dhruvmanila on 2025-04-16 18:03_

They look mostly correct as most of them expected a gradual callable type and received bound methods. Earlier, this would be delegated to `is_subtype_of` which would see that it's not a fully static type and thus return an early `false`.

There's this additional warning about unused suppression comment which I want to look closely at: https://github.com/pallets/werkzeug/blob/7868bef5d978093a8baa0784464ebe5d775ae92a/src/werkzeug/local.py#L370.

---

_Comment by @dhruvmanila on 2025-04-16 18:17_

> There's this additional warning about unused suppression comment which I want to look closely at: [pallets/werkzeug@`7868bef`/src/werkzeug/local.py#L370](https://github.com/pallets/werkzeug/blob/7868bef5d978093a8baa0784464ebe5d775ae92a/src/werkzeug/local.py#L370).

This also seems correct because otherwise (without this PR) we get the following diagnostic:
```
red-knot: Return type does not match returned value: Expected `(...) -> Any`, found `bound method Any.i_op(other: Any) -> @Todo(generics)` [lint:invalid-return-type]
```
which seems like a clear false positive that's now is fixed and thus does not require a `type: ignore` comment but I'd prefer if @sharkdp would take a second look.

It seems like another win for a thorough descriptor protocol implementation from @sharkdp!

---

_@carljm approved on 2025-04-16 18:46_

Nice, thank you!

---

_@sharkdp approved on 2025-04-16 18:47_

As discussed on Discord, the ecosystem change looks good to me — thank you!

---

_Comment by @dhruvmanila on 2025-04-16 18:51_

> As discussed on Discord, the ecosystem change looks good to me — thank you!

For context:

> So what `i_op.__get__(obj, type(obj))` does is that it takes this `i_op` function, and uses the fact that functions are descriptors, calls the `__get__` method on the function object with an instance argument of `obj`. And what the function descriptor does in that case is create a bound-method object 
>
> [...] it binds to `Any`

---

_Merged by @dhruvmanila on 2025-04-16 18:52_

---

_Closed by @dhruvmanila on 2025-04-16 18:52_

---

_Branch deleted on 2025-04-16 18:52_

---
