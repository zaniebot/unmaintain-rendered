```yaml
number: 17930
title: "[ty] Recognise functions containing `yield from` expressions as being generator functions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/yield-from
created_at: 2025-05-07T22:24:52Z
updated_at: 2025-05-07T22:29:46Z
url: https://github.com/astral-sh/ruff/pull/17930
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Recognise functions containing `yield from` expressions as being generator functions

---

_Pull request opened by @AlexWaygood on 2025-05-07 22:24_

## Summary

#17871 taught ty that functions containing `yield` expressions were generator functions, but I forgot that `yield from` expressions are an entirely separate AST node. This fixes some more false positives that are appearing in the primer diff for https://github.com/astral-sh/ruff/pull/17832.

## Test Plan

mdtest added


---

_Review requested from @carljm by @AlexWaygood on 2025-05-07 22:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-07 22:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-07 22:24_

---

_Label `ty` added by @AlexWaygood on 2025-05-07 22:24_

---

_@carljm approved on 2025-05-07 22:25_

---

_Comment by @github-actions[bot] on 2025-05-07 22:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paroxython (https://github.com/laowantong/paroxython)
- error[lint:invalid-return-type] paroxython/filter_programs.py:452:10: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- Found 16 diagnostics
+ Found 15 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:invalid-return-type] src/pip/_vendor/rich/console.py:491:10: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- Found 967 diagnostics
+ Found 966 diagnostics

paasta (https://github.com/yelp/paasta)
- error[lint:invalid-return-type] paasta_tools/setup_istio_mesh.py:287:6: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- Found 964 diagnostics
+ Found 963 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[lint:invalid-return-type] mitmproxy/eventsequence.py:78:30: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- Found 2152 diagnostics
+ Found 2151 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:invalid-return-type] rich/console.py:491:10: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- Found 385 diagnostics
+ Found 384 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[lint:invalid-return-type] misc/python/materialize/mzexplore/sql.py:86:10: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] misc/python/materialize/mzexplore/sql.py:100:10: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] misc/python/materialize/mzexplore/sql.py:111:10: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] misc/python/materialize/mzexplore/sql.py:121:45: Function can implicitly return `None`, which is not assignable to return type `Generator`
- Found 3326 diagnostics
+ Found 3322 diagnostics

```
</details>


---

_Merged by @AlexWaygood on 2025-05-07 22:29_

---

_Closed by @AlexWaygood on 2025-05-07 22:29_

---

_Branch deleted on 2025-05-07 22:29_

---
