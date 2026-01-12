```yaml
number: 16748
title: "[red-knot] Add custom `__setattr__` support"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/setattr-support
created_at: 2025-03-14T14:04:35Z
updated_at: 2025-04-09T06:04:13Z
url: https://github.com/astral-sh/ruff/pull/16748
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] Add custom `__setattr__` support

---

_@sharkdp_

## Summary

Add support for classes with a custom `__setattr__` method.

## Test Plan

New Markdown tests, ecosystem checks.

---

_Label `red-knot` added by @sharkdp on 2025-03-14 14:04_

---

_Comment by @github-actions[bot] on 2025-03-14 14:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/compat/overlay.py:34:1: Unresolved attribute `Path` on type `HashableNamespace`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/compat/overlay.py:35:1: Unresolved attribute `_path` on type `HashableNamespace`.
- Found 4 diagnostics
+ Found 2 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/stack_sampler.py:282:9: Object of type `StackSampler` is not assignable to attribute `stack_sampler` on type `Unknown | _local`
- Found 291 diagnostics
+ Found 290 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/coolmagic/utils.py:67:9: Object of type `Unknown` is not assignable to attribute `request` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/couchy/application.py:18:9: Object of type `Unknown` is not assignable to attribute `application` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/couchy/application.py:30:9: Object of type `Unknown` is not assignable to attribute `application` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/couchy/application.py:32:9: Object of type `Unknown | MapAdapter` is not assignable to attribute `url_adapter` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/simplewiki/application.py:52:9: Object of type `Unknown` is not assignable to attribute `application` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/simplewiki/utils.py:79:9: Object of type `Unknown` is not assignable to attribute `request` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/plnt/webapp.py:32:9: Object of type `Unknown` is not assignable to attribute `application` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/plnt/webapp.py:36:9: Object of type `Request` is not assignable to attribute `request` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/plnt/webapp.py:37:9: Object of type `Unknown | MapAdapter` is not assignable to attribute `url_adapter` on type `Unknown | Local`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_wrappers.py:685:5: Unresolved attribute `realm` on type `WWWAuthenticate`.
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/shorty/application.py:19:9: Object of type `Unknown` is not assignable to attribute `application` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/shorty/application.py:28:9: Object of type `Unknown` is not assignable to attribute `application` on type `Unknown | Local`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/examples/shorty/application.py:30:9: Object of type `Unknown | MapAdapter` is not assignable to attribute `url_adapter` on type `Unknown | Local`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:31:5: Unresolved attribute `foo` on type `Local`.
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:36:9: Object of type `Unknown` is not assignable to attribute `foo` on type `Unknown | Local`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:59:5: Unresolved attribute `foo` on type `Local`.
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:64:9: Object of type `Unknown` is not assignable to attribute `foo` on type `Unknown | Local`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:87:5: Unresolved attribute `foo` on type `Local`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:138:5: Unresolved attribute `foo` on type `Local`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:176:5: Unresolved attribute `foo` on type `Local`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:177:5: Unresolved attribute `bar` on type `Local`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:221:5: Unresolved attribute `value` on type `Local`.
- Found 801 diagnostics
+ Found 779 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_property.py:150:5: Unresolved attribute `num` on type `unique_ptr_field_proxy_poc`.
- Found 288 diagnostics
+ Found 287 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/typeshed-stats/scripts/regenerate.py:40:13: Unresolved attribute `typeshed_dir` on type `Namespace`.
- Found 177 diagnostics
+ Found 176 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/commands/parse.py:379:17: Unresolved attribute `meta` on type `Namespace`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/commands/parse.py:390:17: Unresolved attribute `cbkwargs` on type `Namespace`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/commands/__init__.py:169:13: Unresolved attribute `spargs` on type `Namespace`.
- Found 1591 diagnostics
+ Found 1588 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-03-14 14:14_

> where dunder lookups should skip [symbols on `object`](https://github.com/python/typeshed/blob/0b8dcfc00b06b045f82cb10a22ae52b613a939db/stdlib/builtins.pyi#L116-L118).

I wonder if it's actually helpful to any type checkers for that definition to exist in typeshed? Sure, it exists at runtime, so we'd have to add an allowlist entry in typeshed's tests if we omitted it in the stubs (or typeshed's tests would complain that there was a method at runtime that was missed in the stubs). But it's okay to add allowlist entries like that if the costs of including the method definition in the stubs outweigh the benefits — that may be the case here. 

It might be worth sending an experimental PR to typeshed seeing what happens on mypy_primer if it's removed?

---

_Comment by @AlexWaygood on 2025-03-14 16:54_

Following up: we discussed this on Discord and realised that removing `object.__setattr__` from typeshed would be a pretty bad idea, since it would mean that a type checker would emit an error if you tried to call it manually (e.g. `object.__setattr__(x, "foo", 42)`. There are use cases for doing this.

---

_Comment by @sharkdp on 2025-03-14 19:57_

I'm going to leave this open for now, because I think it makes sense to wait for #16512. But if someone wants to pick this up, please go ahead.

---

_Comment by @codspeed-hq[bot] on 2025-04-08 21:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fsetattr-support)

### Merging #16748 will **not alter performance**

<sub>Comparing <code>david/setattr-support</code> (ac68c23) with <code>main</code> (fab7d82)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Marked ready for review by @sharkdp on 2025-04-08 21:47_

---

_Review requested from @carljm by @sharkdp on 2025-04-08 21:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-08 21:47_

---

_Review requested from @dcreager by @sharkdp on 2025-04-08 21:47_

---

_@carljm approved on 2025-04-09 04:03_

Nice!

---

_Merged by @sharkdp on 2025-04-09 06:04_

---

_Closed by @sharkdp on 2025-04-09 06:04_

---

_Branch deleted on 2025-04-09 06:04_

---
