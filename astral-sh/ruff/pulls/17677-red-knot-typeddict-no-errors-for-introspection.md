```yaml
number: 17677
title: "[red-knot] TypedDict: No errors for introspection dunder attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typed-dict-attributes
created_at: 2025-04-28T11:16:44Z
updated_at: 2025-04-28T11:28:44Z
url: https://github.com/astral-sh/ruff/pull/17677
synced_at: 2026-01-12T15:56:03Z
```

# [red-knot] TypedDict: No errors for introspection dunder attributes

---

_@sharkdp_

## Summary

Do not emit errors when accessing introspection dunder attributes such as `__required_keys__` on `TypedDict`s.


---

_Label `red-knot` added by @sharkdp on 2025-04-28 11:16_

---

_Comment by @github-actions[bot] on 2025-04-28 11:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kopf (https://github.com/nolar/kopf)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/kopf/kopf/_cogs/clients/creating.py:21:5: Object of type `dict` is not assignable to `RawBody | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/kopf/kopf/_cogs/clients/creating.py:21:5: Object of type `RawBody | dict` is not assignable to `RawBody | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/processing.py:443:81: Argument to this function is incorrect: Expected `BodyEssence`, found `BodyEssence | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/processing.py:443:81: Argument to this function is incorrect: Expected `BodyEssence`, found `@Todo(TypedDict) | None`

pydantic (https://github.com/pydantic/pydantic)
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pydantic/pydantic/json_schema.py:529:44: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pydantic/pydantic/json_schema.py:1554:54: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pydantic/pydantic/json_schema.py:1556:49: Unused blanket `type: ignore` directive
- Found 913 diagnostics
+ Found 916 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/media.py:250:36: Unused blanket `type: ignore` directive
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/media.py:247:13: Attribute `cleanFailure` on type `FileInfo | Unknown` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/media.py:248:13: Object of type `list` is not assignable to attribute `frames` on type `FileInfo | Unknown`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/media.py:269:31: Attribute `value` on type `FileInfo | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/media.py:271:17: Attribute `value` on type `FileInfo | Unknown` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/scrapy/scrapy/utils/log.py:245:12: Return type does not match returned value: Expected `tuple[int, str, @Todo(specialized non-generic class)]`, found `tuple[@Todo(Support for `typing.TypeVar` instances in type expressions) | None, (@Todo(Support for `typing.TypeVar` instances in type expressions) & ~AlwaysFalsy) | Literal[""], @Todo(specialized non-generic class)]`
- Found 1489 diagnostics
+ Found 1485 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_utils.py:262:5: Type `Literal[DataclassOptions]` has no attribute `__required_keys__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_utils.py:262:42: Type `Literal[DataclassOptions]` has no attribute `__optional_keys__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_utils.py:266:5: Type `Literal[StrictDataclassOptions]` has no attribute `__required_keys__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_utils.py:266:48: Type `Literal[StrictDataclassOptions]` has no attribute `__optional_keys__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_utils.py:385:13: Type `Literal[StrictDataclassOptions]` has no attribute `__required_keys__`
- Found 630 diagnostics
+ Found 625 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bokeh/src/bokeh/_specs.pyi:72:49: Cannot subscript object of type `UnionType` with no `__getitem__` method
- Found 1854 diagnostics
+ Found 1853 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-28 11:22_

---

_Review requested from @carljm by @sharkdp on 2025-04-28 11:22_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-28 11:22_

---

_Review requested from @dcreager by @sharkdp on 2025-04-28 11:22_

---

_@AlexWaygood approved on 2025-04-28 11:26_

---

_Merged by @sharkdp on 2025-04-28 11:28_

---

_Closed by @sharkdp on 2025-04-28 11:28_

---

_Branch deleted on 2025-04-28 11:28_

---
