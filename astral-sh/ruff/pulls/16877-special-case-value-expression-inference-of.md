```yaml
number: 16877
title: Special-case value-expression inference of special form subscriptions
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/subscript-inference
created_at: 2025-03-20T17:40:51Z
updated_at: 2025-03-20T21:46:05Z
url: https://github.com/astral-sh/ruff/pull/16877
synced_at: 2026-01-10T19:40:36Z
```

# Special-case value-expression inference of special form subscriptions

---

_Pull request opened by @AlexWaygood on 2025-03-20 17:40_

## Summary

Currently for something like `X = typing.Tuple[str, str]`, we infer the value of `X` as `object`. That's because `Tuple` (like many of the symbols in the typing module) is annotated as a `_SpecialForm` instance in typeshed's stubs:

https://github.com/astral-sh/ruff/blob/23382f5f8c7b4e356368cdeb1049b8c1910baff3/crates/red_knot_vendored/vendor/typeshed/stdlib/typing.pyi#L215

and we don't understand implicit type aliases yet, and the stub for `_SpecialForm.__getitem__` says it always returns `object`:

https://github.com/astral-sh/ruff/blob/23382f5f8c7b4e356368cdeb1049b8c1910baff3/crates/red_knot_vendored/vendor/typeshed/stdlib/typing.pyi#L198-L200

We have existing false positives in our test suite due to this:

https://github.com/astral-sh/ruff/blob/23382f5f8c7b4e356368cdeb1049b8c1910baff3/crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md?plain=1#L76-L78

and it's causing _many_ new false positives in #16872, which tries to make our annotation-expression parsing stricter in some ways.

This PR therefore adds some small special casing for `KnownInstanceType` variants that fallback to `_SpecialForm`, so that these false positives can be avoided.

## Test Plan

Existing mdtest altered.

Cc. @MatthewMckee4 


---

_Label `red-knot` added by @AlexWaygood on 2025-03-20 17:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-20 17:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-20 17:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-20 17:40_

---

_Comment by @github-actions[bot] on 2025-03-20 17:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/black/src/black/trans.py:54:11: Cannot subscript object of type `object` with no `__getitem__` method
- Found 289 diagnostics
+ Found 288 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-03-20 17:50_

> ## `mypy_primer` results
> Changes were detected when running on open source projects
> 
> ```diff
> black (https://github.com/psf/black)
> - error[lint:non-subscriptable] /tmp/mypy_primer/projects/black/src/black/trans.py:54:11: Cannot subscript object of type `object` with no `__getitem__` method
> - Found 289 diagnostics
> + Found 288 diagnostics
> ```

Here's the link to the line in question: https://github.com/psf/black/blob/dd278cb316d75868716a0478c35b1fcd600a5249/src/black/trans.py#L54

`Result` is a type alias to a `Union[]` subscription, and `Union` is stubbed as an instance of `_SpecialForm` in typeshed:

https://github.com/psf/black/blob/dd278cb316d75868716a0478c35b1fcd600a5249/src/black/rusty.py#L28

So it's good that we no longer emit this error!

---

_@carljm approved on 2025-03-20 21:21_

---

_Merged by @AlexWaygood on 2025-03-20 21:46_

---

_Closed by @AlexWaygood on 2025-03-20 21:46_

---

_Branch deleted on 2025-03-20 21:46_

---
