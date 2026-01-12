```yaml
number: 17243
title: "[red-knot] Allow ellipsis default params in stub functions"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: ellipsis-in-stub-method
created_at: 2025-04-06T21:19:52Z
updated_at: 2025-04-07T18:04:17Z
url: https://github.com/astral-sh/ruff/pull/17243
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Allow ellipsis default params in stub functions

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #17234

## Test Plan

Add tests to functions/paremeters.md


---

_Comment by @github-actions[bot] on 2025-04-06 21:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:17:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `Literal[False]`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:26:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `None`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:34:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `Literal[False]`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:35:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `None`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:42:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:43:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `Path | None`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/typeshed-stats/scripts/runtests.py:44:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `bool`
- Found 33 diagnostics
+ Found 26 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:387:9: Default value of type `ellipsis` is not assignable to annotated parameter type `Literal[True]`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:568:15: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:568:34: Default value of type `ellipsis` is not assignable to annotated parameter type `Literal[False]`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:568:66: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:573:15: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:573:34: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/request.py:573:54: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/accept.py:163:55: Default value of type `ellipsis` is not assignable to annotated parameter type `str`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/accept.py:289:55: Default value of type `ellipsis` is not assignable to annotated parameter type `str`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/response.py:596:24: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/response.py:596:43: Default value of type `ellipsis` is not assignable to annotated parameter type `Literal[False]`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/response.py:599:24: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/wrappers/response.py:599:43: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/headers.py:286:19: Default value of type `ellipsis` is not assignable to annotated parameter type `int | None`
- Found 697 diagnostics
+ Found 683 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/scrapy/scrapy/utils/spider.py:70:5: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/scrapy/scrapy/utils/spider.py:71:5: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/scrapy/scrapy/utils/spider.py:80:5: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/scrapy/scrapy/utils/spider.py:81:5: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/scrapy/scrapy/utils/spider.py:90:5: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/scrapy/scrapy/utils/spider.py:91:5: Default value of type `ellipsis` is not assignable to annotated parameter type `bool`
- Found 1502 diagnostics
+ Found 1496 diagnostics

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-04-06 21:24_

---

_Label `bug` added by @AlexWaygood on 2025-04-06 21:24_

---

_Marked ready for review by @MatthewMckee4 on 2025-04-06 21:26_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-06 21:26_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-06 21:26_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-06 21:26_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-06 21:26_

---

_Renamed from "Allow ellipsis default params in stub functions" to "[red-knot] Allow ellipsis default params in stub functions" by @MatthewMckee4 on 2025-04-06 21:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-06 21:31_

nit: maybe call this `in_overloaded_or_abstract_function`?

```suggestion
    fn in_overloaded_or_abstract_function(&self) -> bool {
```

---

_@AlexWaygood reviewed on 2025-04-06 21:41_

Nice, that primer diff looks fantastic! And I confirmed that this gets rid of all `invalid-parameter-default` diagnostics when running red-knot on typeshed-stats

---

_@AlexWaygood reviewed on 2025-04-06 21:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:1 on 2025-04-06 21:44_

could you also add a test (and check that it passes) for a `Protocol` class with PEP-695 type parameters. These can be tricky, because they introduce an extra scope:

```py
class GenericFoo[T](Protocol):
    def x(self, y: bool = ...) -> T: ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-06 21:54_

I'm not sure about this naming change, but I'm also not sure about the semantics of the function in the first place. I think being "in an overloaded function" suggests that it would include the main implementation (which is not decorated with @overload), but that _shouldn't_ be included here. Have to go at the moment but would like to look at this a bit more.

---

_@carljm reviewed on 2025-04-06 21:54_

---

_@MatthewMckee4 reviewed on 2025-04-06 21:58_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-06 21:58_

I'll test to see if this surprises an error of an ellipsis in the main overload

---

_@MatthewMckee4 reviewed on 2025-04-06 22:07_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-06 22:07_

it doesn't, but i also see thats not really what you were referring to

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-07 17:12_

Ok sorry for the cryptic note here, I was on a plane yesterday and the flight attendant was literally asking me to put away my computer as I was submitting that comment 😆 

There were three things I wanted to look at here:

1) Do we handle generic _methods_ correctly (not just methods of generic classes, which you already added a test for)? The answer is yes, we do, but I'll push a couple tests for that as well.

2) I wanted to clarify my understanding of how this same code works for both default values and for return statements inside the function, since default values are inferred in the outer scope, not the function body scope. But what I realized looking at it more closely is that function parameter _definitions_ are handled when inferring the function body scope, so we are actually in the function body scope for all call sites of these functions. I think I might add doc comments to these methods clarifying their usage, but otherwise I think this looks good.

3) The question of how to name this function. I think the name (and doc comment) should be clear that it returns true for _overloads_, not for _overloaded functions_, since the latter could be read to mean the main implementation function. Thus I wouldn't favor the name change suggested above, and I'm not coming up with anything I like significantly better than the current name.

---

_@carljm approved on 2025-04-07 17:12_

Thank you!

---

_@AlexWaygood reviewed on 2025-04-07 17:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-07 17:18_

> 3\. The question of how to name this function. I think the name (and doc comment) should be clear that it returns true for _overloads_, not for _overloaded functions_, since the latter could be read to mean the main implementation function. Thus I wouldn't favor the name change suggested above, and I'm not coming up with anything I like significantly better than the current name.

I do understand what you're saying here... `in_function_decorated_with_abstractmethod_or_overload`? But it's obviously awfully verbose.

`in_stublike_function`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-07 17:28_

I'm ok with the very verbose name if you like it better, we won't have tons of call sites. I think we could also go with just `in_function_overload_or_abstractmethod` if you like that better? Tbh I don't know what alternatives to suggest since I'm not clear on precisely what it is you dislike about the current name :)

---

_@carljm reviewed on 2025-04-07 17:28_

---

_@AlexWaygood reviewed on 2025-04-07 17:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-07 17:33_

I don't like the current name because it doesn't make grammatical sense -- you can have a "function overload", sure, but you can't have a "function abstract" (it would be "abstract function")

`in_overload_or_abstractmethod` or similar would work fine for me!

---

_Merged by @carljm on 2025-04-07 17:34_

---

_Closed by @carljm on 2025-04-07 17:34_

---

_@carljm reviewed on 2025-04-07 17:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-07 17:42_

But I think the current name is perfectly grammatical, it just needs to be read with different grouping :) It's "in (function overload) or (abstractmethod)", not "in function (overload or abstractmethod)"

---

_@AlexWaygood reviewed on 2025-04-07 17:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1220 on 2025-04-07 17:47_

Ah sorry, I think we were talking at cross purposes here? I didn't realise the name had already changed. The original name of this function in the first version of this PR was `in_function_overload_or_abstract()`, and that's the version of this PR that my original comment here was attached to. The name of the function in the merged version of the PR is `in_function_overload_or_abstractmethod()`, which, yes, I agree is perfectly grammatical and much better!

---

_Branch deleted on 2025-04-07 18:04_

---
