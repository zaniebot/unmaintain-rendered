```yaml
number: 16695
title: "[red-knot] Infer `lambda` return type as `Unknown`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/lambda-return-type
created_at: 2025-03-13T03:14:23Z
updated_at: 2025-03-18T17:18:12Z
url: https://github.com/astral-sh/ruff/pull/16695
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Infer `lambda` return type as `Unknown`

---

_Pull request opened by @dhruvmanila on 2025-03-13 03:14_

## Summary

Part of astral-sh/ruff#15382

This PR infers the return type `lambda` expression as `Unknown`. In the future, it would be more useful to infer the expression type considering the surrounding context (#16696).

~This is done by adding the expression that represents the default value of the parameters as a standalone expression. Without this, it would create cycles as described in https://github.com/astral-sh/ruff/pull/16547. Both function and lambda expression's parameter defaults are added as standalone expression as we cannot differentiate between the two when doing the inference in `infer_parameter_definition`.~

## Test Plan

Update existing test cases from `@todo` to the (verified) return type.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-13 03:14_

---

_Comment by @github-actions[bot] on 2025-03-13 03:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/metrics/overhead.py:32:16: Object of type `() -> Unknown` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/metrics/overhead.py:45:24: Object of type `() -> Unknown` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/middleware.py:50:13: Object of type `(request) -> @Todo` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/middleware.py:50:13: Object of type `(request) -> Unknown` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/metrics/overhead.py:32:16: Object of type `() -> @Todo` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/metrics/overhead.py:45:24: Object of type `() -> @Todo` is not callable

pybind11 (https://github.com/pybind/pybind11)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:280:13: Object of type `() -> @Todo` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:280:13: Object of type `() -> Unknown` is not callable

isort (https://github.com/pycqa/isort)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:55:17: Object of type `(key) -> @Todo` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:55:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:66:17: Object of type `(key) -> @Todo` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:66:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:268:17: Object of type `(key) -> @Todo` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:268:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`

```
</details>


---

_Marked ready for review by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-17 14:19_

---

_Closed by @AlexWaygood on 2025-03-17 16:43_

---

_Reopened by @AlexWaygood on 2025-03-17 16:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-17 16:43_

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-03-17 16:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-17 16:50_

---

_Comment by @dhruvmanila on 2025-03-17 17:26_

I'm a bit unsure about the mypy_primer failure, the logs doesn't suggest anything obvious. I plan to look at it tomorrow morning.

---

_Comment by @AlexWaygood on 2025-03-17 17:39_

Is it possible that this PR is causing us to hang on one of the mypy_primer projects? I haven't checked to see if it's stopping on the same project every time

---

_Comment by @carljm on 2025-03-17 22:37_

mypy-primer is hitting a problem on `isort`. I'm looking into it. It looks similar to some issues we already see on some other mypy-primer projects. I think this PR is not urgent so we can hold off until we understand the issue?

---

_Comment by @carljm on 2025-03-17 22:47_

On looking at this a bit more, I don't think we should go forward with landing this approach. I think instead, as I mentioned in Discord, we should simply record `Type::Unknown` as the return type of a lambda's callable type for now.

Useful inference of a lambda's return type will require a different approach, which does the inference of the body expression based on arguments at each call site, rather than eagerly computing a return type without knowing the argument types. I don't think anything from this approach would carry over to that, so I don't think it's worth making all function defaults standalone expressions just so that we can temporarily have slightly more information about lambda return types.

---

_Comment by @dhruvmanila on 2025-03-18 03:12_

> Useful inference of a lambda's return type will require a different approach, which does the inference of the body expression based on arguments at each call site, rather than eagerly computing a return type without knowing the argument types. I don't think anything from this approach would carry over to that, so I don't think it's worth making all function defaults standalone expressions just so that we can temporarily have slightly more information about lambda return types.

Yeah, that makes sense. Thanks for looking into it, I'll update the PR to use `Unknown`, add a TODO.

---

_Renamed from "[red-knot] Infer return type of `lambda` expression" to "[red-knot] Infer `lambda` return type as `Unknown`" by @dhruvmanila on 2025-03-18 03:12_

---

_@carljm approved on 2025-03-18 14:26_

---

_Merged by @dhruvmanila on 2025-03-18 17:18_

---

_Closed by @dhruvmanila on 2025-03-18 17:18_

---

_Branch deleted on 2025-03-18 17:18_

---
