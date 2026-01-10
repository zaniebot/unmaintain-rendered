```yaml
number: 18438
title: "[ty] Infer `list[T]` when unpacking non-tuple type"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/list-unpacking
created_at: 2025-06-03T09:42:00Z
updated_at: 2025-06-03T13:48:01Z
url: https://github.com/astral-sh/ruff/pull/18438
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Infer `list[T]` when unpacking non-tuple type

---

_Pull request opened by @dhruvmanila on 2025-06-03 09:42_

## Summary

Follow-up from #18401, I was looking at whether that would fix the issue at https://github.com/astral-sh/ty/issues/247#issuecomment-2917656676 and it didn't, which made me realize that the PR only inferred `list[T]` when the value type was tuple but it could be other types as well.

This PR fixes the actual issue by inferring `list[T]` for the non-tuple type case.

## Test Plan

Add test cases for starred expression involved with non-tuple type. I also added a few test cases for list type and list literal.

I also verified that the example in the linked issue comment works:
```py
def _(line: str):
    a, b, *c = line.split(maxsplit=2)
    c.pop()
```


---

_Review requested from @carljm by @dhruvmanila on 2025-06-03 09:42_

---

_Label `ty` added by @dhruvmanila on 2025-06-03 09:42_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-06-03 09:42_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-06-03 09:42_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-03 09:42_

---

_Renamed from "[ty] Infer `list[T]` when unpacking non-literal list" to "[ty] Infer `list[T]` when unpacking non-tuple type" by @dhruvmanila on 2025-06-03 09:43_

---

_Comment by @github-actions[bot] on 2025-06-03 09:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
graphql-core (https://github.com/graphql-python/graphql-core)
- error[not-iterable] tests/utils/assert_matching_values.py:11:18: Object of type `T` is not iterable
- Found 441 diagnostics
+ Found 440 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[invalid-argument-type] porcupine/menubar.py:166:19: Argument to function `_join` is incorrect: Expected `list[str]`, found `str`
- Found 81 diagnostics
+ Found 80 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-argument-type] mypyc/analysis/ircheck.py:363:16: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Value] | Value`
- error[no-matching-overload] mypyc/analysis/ircheck.py:367:39: No overload of function `__new__` matches arguments
- error[no-matching-overload] mypyc/analysis/ircheck.py:367:39: No overload of function `__new__` matches arguments
- error[no-matching-overload] mypyc/analysis/ircheck.py:367:39: No overload of function `__new__` matches arguments
- error[not-iterable] mypyc/ir/pprint.py:189:60: Object of type `Unknown | list[Value] | Value` may not be iterable
- error[not-iterable] mypyc/transform/ir_transform.py:298:48: Object of type `Unknown | list[Value] | Value` may not be iterable
- Found 3268 diagnostics
+ Found 3262 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[not-iterable] src/prefect/utilities/collections.py:137:27: Object of type `KT` is not iterable
- Found 4495 diagnostics
+ Found 4494 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[not-iterable] mesonbuild/backend/backends.py:565:18: Object of type `ExternalProgram | BuildTarget | CustomTarget | File | str` may not be iterable
- Found 1373 diagnostics
+ Found 1372 diagnostics

zulip (https://github.com/zulip/zulip)
- error[not-iterable] corporate/lib/decorator.py:142:41: Object of type `Parameter` is not iterable
- error[not-iterable] corporate/lib/decorator.py:229:41: Object of type `Parameter` is not iterable
- Found 6946 diagnostics
+ Found 6944 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-06-03 09:47_

Random thing I noticed while playing around with this branch locally just now: do you know why `Never` is given as the inlay type on the playground here? (This is a local deploy of the ty playground on your branch)

![image](https://github.com/user-attachments/assets/c9efc181-f47b-426f-ab2c-6c495803419a)


---

_Comment by @dhruvmanila on 2025-06-03 09:49_

> Random thing I noticed while playing around with this branch locally just now: do you know why `Never` is given as the inlay type on the playground here? (This is a local deploy of the ty playground on your branch)

Yeah, I noticed that as well. I'm not exactly sure why that is happening. I plan to look at it in a bit.

---

_Comment by @dhruvmanila on 2025-06-03 09:55_

All of the diagnostics removed in the ecosystem analysis are false positives because ty was inferring `T` instead of `list[T]` in cases like `a, b, *c = some_value`.

---

_@AlexWaygood approved on 2025-06-03 10:11_

I'm not too familiar with the unpacking code, but this looks reasonable and the primer report looks great!

Some other edge cases that look like they're maybe not-yet covered in `unpacking.md`, and could be good to add as tests. It looks like we have the correct behaviour on all of them:

```py
() = []
[] = ()

(*a,) = ()
reveal_type(a)  # revealed: list[Unknown]

(*a,) = (1,)
reveal_type(b)  # revealed: list[Literal[1]]
```

---

_Comment by @dhruvmanila on 2025-06-03 13:43_

> Some other edge cases that look like they're maybe not-yet covered in `unpacking.md`, and could be good to add as tests. It looks like we have the correct behaviour on all of them:

Thanks!

I'm not exactly sure what's the value of the empty list / tuple test case. What is it that you had in mind to test that for? I remember there being similar test cases in the corpus source which seems more useful for these as it would test whether the inference is being done as expected or not.

The single unpacking elements examples that you've provided is being tested by way of being surrounded by other elements, not in isolation. For example, [this](https://github.com/astral-sh/ruff/blob/bdf83fd246f54a51ae3038a176be7e382e219632/crates/ty_python_semantic/resources/mdtest/unpacking.md?plain=1#L117-L124) is the `list[Unknown]` case and [this](https://github.com/astral-sh/ruff/blob/bdf83fd246f54a51ae3038a176be7e382e219632/crates/ty_python_semantic/resources/mdtest/unpacking.md?plain=1#L126-L133) is the `list[Literal[1]]` case in your example.

I'll go ahead and merge this PR but happy to follow-up with the empty list / tuple test cases if you think it's useful.

---

_Merged by @dhruvmanila on 2025-06-03 13:47_

---

_Closed by @dhruvmanila on 2025-06-03 13:47_

---

_Branch deleted on 2025-06-03 13:47_

---

_Comment by @AlexWaygood on 2025-06-03 13:47_

> What is it that you had in mind to test that for?

It's just a weird edge case that only works by virtue of unpacking at runtime: a zero-length list on the right-hand side is assigned to a zero-length target list on the right-hand side. It's useful to show that we model the runtime accurately: that we understand such code as valid and do not emit any diagnostics on it.

---
