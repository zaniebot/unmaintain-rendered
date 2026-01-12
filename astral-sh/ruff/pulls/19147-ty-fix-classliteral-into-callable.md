```yaml
number: 19147
title: "[ty] Fix `ClassLiteral.into_callable`"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
draft: true
base: main
head: fix-class-literal-into-callable
created_at: 2025-07-04T15:56:41Z
updated_at: 2025-07-16T21:12:33Z
url: https://github.com/astral-sh/ruff/pull/19147
synced_at: 2026-01-12T15:56:33Z
```

# [ty] Fix `ClassLiteral.into_callable`

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Slightly refactor `ClassLiteral.into_callable` to use `Type.into_callable`. Essentially to firstly fix the issue with dataclasses, as the synthensized `__init__` created by dataclass isn't covered as it isn't `FunctionLiteral`




---

_Renamed from "Fix class literal into callable" to "[ty] Fix `ClassLiteral.into_callable`" by @MatthewMckee4 on 2025-07-04 15:56_

---

_Label `ty` added by @AlexWaygood on 2025-07-04 15:58_

---

_Comment by @github-actions[bot] on 2025-07-04 16:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~60MB
+     memo fields = ~54MB

aiortc (https://github.com/aiortc/aiortc)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

rich (https://github.com/Textualize/rich)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-assignment] strawberry/codegen/query_codegen.py:648:13: Object of type `<class 'GraphQLOptional'> | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
- error[invalid-assignment] strawberry/codegen/query_codegen.py:656:13: Object of type `<class 'GraphQLList'> | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
- error[invalid-parameter-default] strawberry/codegen/query_codegen.py:760:9: Default value of type `<class 'GraphQLObjectType'>` is not assignable to annotated parameter type `(str, /) -> GraphQLObjectType`
- Found 360 diagnostics
+ Found 357 diagnostics

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~72MB
+     memo fields = ~66MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB

vision (https://github.com/pytorch/vision)
- TOTAL MEMORY USAGE: ~368MB
+ TOTAL MEMORY USAGE: ~334MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~189MB

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-argument-type] src/bokeh/embed/bundle.py:186:29: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `<class 'URL'>`
- error[invalid-argument-type] src/bokeh/embed/bundle.py:189:30: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `<class 'URL'>`
- Found 863 diagnostics
+ Found 861 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~276MB

sympy (https://github.com/sympy/sympy)
- error[invalid-argument-type] sympy/polys/polytools.py:4936:22: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Integer'>`
- error[invalid-argument-type] sympy/polys/rootoftools.py:1048:34: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Rational'>`
- error[invalid-argument-type] sympy/stats/frv_types.py:229:24: Argument to function `__new__` is incorrect: Expected `(int, /) -> Unknown`, found `<class 'Integer'>`
- error[invalid-argument-type] sympy/stats/frv_types.py:517:24: Argument to function `__new__` is incorrect: Expected `(int, /) -> Unknown`, found `<class 'Integer'>`
- error[invalid-argument-type] sympy/stats/frv_types.py:696:24: Argument to function `__new__` is incorrect: Expected `(int, /) -> Unknown`, found `<class 'Integer'>`
- error[invalid-argument-type] sympy/stats/frv_types.py:798:24: Argument to function `__new__` is incorrect: Expected `(int, /) -> Unknown`, found `<class 'Integer'>`
- error[invalid-argument-type] sympy/stats/stochastic_process_types.py:1097:50: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Integer'>`
- Found 13296 diagnostics
+ Found 13289 diagnostics

```
</details>


---

_Closed by @MatthewMckee4 on 2025-07-07 23:46_

---

_Branch deleted on 2025-07-16 21:12_

---
