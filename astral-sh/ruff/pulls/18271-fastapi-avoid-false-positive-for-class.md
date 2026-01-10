```yaml
number: 18271
title: "[`fastapi`] Avoid false positive for class dependencies (`FAST003`)"
type: pull_request
state: merged
author: twentyone212121
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-fast003-class-dependencies
created_at: 2025-05-23T07:36:56Z
updated_at: 2025-06-02T18:34:51Z
url: https://github.com/astral-sh/ruff/pull/18271
synced_at: 2026-01-10T18:45:04Z
```

# [`fastapi`] Avoid false positive for class dependencies (`FAST003`)

---

_Pull request opened by @twentyone212121 on 2025-05-23 07:36_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #17226.

This PR updates the `FAST003` rule to correctly handle [FastAPI class dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/classes-as-dependencies/). Specifically, if a path parameter is declared in either:

- a `pydantic.BaseModel` used as a dependency, or  
- the `__init__` method of a class used as a dependency,  

then `FAST003` will no longer incorrectly report it as unused.

FastAPI allows a shortcut when using annotated class dependencies - `Depends` can be called without arguments, e.g.:

```python
class MyParams(BaseModel):
    my_id: int

@router.get("/{my_id}")
def get_id(params: Annotated[MyParams, Depends()]): ...
```
This PR ensures that such usage is properly supported by the linter.

Note: Support for dataclasses is not included in this PR. Let me know if you’d like it to be added.

## Test Plan

Added relevant test cases to the `FAST003.py` fixture.


---

_Comment by @github-actions[bot] on 2025-05-23 07:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-05-28 21:02_

---

_Review requested from @ntBre by @ntBre on 2025-05-28 21:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:291 on 2025-05-28 22:13_

I'm pretty sure it's impossible to get here if `tuple.elts` is empty, but we could use `tuple.elts.first()?` just in case.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:414 on 2025-05-28 22:17_

```suggestion
   semantic.resolve_qualified_name(expr)
   .is_some_and(|qualified_name| matches!(qualified_name.segments(), ["pydantic", "BaseModel"]))
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:374 on 2025-05-28 22:26_

Can we use `non_posonly_non_variadic_parameters` like in the function branch and just tack `skip(1)` on the end? I think that would be the same as this, just with the `chain` and `skip` calls swapped, which seems like it should be okay.

---

_@ntBre approved on 2025-05-28 22:47_

Thanks! I had a couple of very minor suggestions, but this looks good to me.

How much work do you think it would be to add support for dataclasses? I don't feel too strongly either way. I think we could handle [`typing.NamedTuple`](https://docs.python.org/3/library/typing.html#typing.NamedTuple)s with just a `| ["typing", "NamedTuple"]` in the `is_pydantic_base_model` match,  but that's probably even less common.

The issue was about `BaseModel` anyway, so I think it's totally fine to leave it here.

---

_Comment by @twentyone212121 on 2025-05-29 09:17_

Thanks for the review! I’ve made the changes you suggested in the latest commit.

Regarding dataclasses, the simple option would be to check if `"dataclass"` is in `class_def.decorator_list`. But since `pydantic` has its own version (`pydantic.dataclasses.dataclass`) and there’s also support from the `attrs` library, it gets a bit trickier.

I saw that Ruff already has some logic for handling this in `ruff/rules/helpers.rs`, which seems pretty thorough. But it seems like I can't access it from `fastapi` module.
https://github.com/astral-sh/ruff/blob/04dc48e17c2518f97436d76284a509499087d81c/crates/ruff_linter/src/rules/ruff/rules/helpers.rs#L95-L109

That said, I’m not sure we need that level of complexity for this rule. Open to your thoughts if you think it’s worth adding support for them here.

---

_Comment by @twentyone212121 on 2025-05-29 09:19_

CI / ecosystem check is failing due to a Connect Timeout Error. Not sure what to do about this

---

_Comment by @ntBre on 2025-05-29 13:26_

Ah okay, I didn't really consider the full variety of "dataclasses." I think it's fine to leave the PR as is! Nice find on the helpers, though, that will come in handy if we update this later. I think we'd just need to change `pub(super)` to `pub(crate)` to access them here, but it's possible that we'd have to change some module visibilities or even move the code too.

> CI / ecosystem check is failing due to a Connect Timeout Error. Not sure what to do about this

Looks like this resolved eventually, probably just a temporary network issue.

I'll give this another quick review soon, hopefully today!

---

_@ntBre approved on 2025-06-02 18:34_

Looks great, thanks again!

---

_Merged by @ntBre on 2025-06-02 18:34_

---

_Closed by @ntBre on 2025-06-02 18:34_

---
