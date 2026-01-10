```yaml
number: 17575
title: "[red-knot] GenericAlias instances as a base class"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/generic-alias-base
created_at: 2025-04-23T07:46:45Z
updated_at: 2025-04-23T08:39:11Z
url: https://github.com/astral-sh/ruff/pull/17575
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] GenericAlias instances as a base class

---

_Pull request opened by @sharkdp on 2025-04-23 07:46_

## Summary

We currently emit a diagnostic for code like the following:
```py
from typing import Any

# error: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
class C(tuple[Any, ...]): ...
```

The changeset here silences this diagnostic by recognizing instances of `GenericAlias` in `ClassBase::try_from_type`, and inferring a `@Todo` type for them. This is a change in preparation for #17557, because `C` previously had `Unknown` in its MRO …
```py
reveal_type(C.__mro__)  # tuple[Literal[C], Unknown, Literal[object]]
```
… which would cause us to think that `C` is assignable to everything.

The changeset also removes some false positive `invalid-base` diagnostics across the ecosystem.

## Test Plan

Updated Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-04-23 07:46_

---

_Comment by @github-actions[bot] on 2025-04-23 07:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- error[lint:invalid-base] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_streams.py:17:5: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 155 diagnostics
+ Found 154 diagnostics

starlette (https://github.com/encode/starlette)
- error[lint:invalid-base] /tmp/mypy_primer/projects/starlette/tests/test_formparsers.py:20:26: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 223 diagnostics
+ Found 222 diagnostics

kopf (https://github.com/nolar/kopf)
- error[lint:invalid-base] /tmp/mypy_primer/projects/kopf/kopf/_cogs/structs/patches.py:65:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/kopf/kopf/_core/engines/indexing.py:173:24: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/kopf/kopf/_cogs/structs/ephemera.py:10:12: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 169 diagnostics
+ Found 166 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[lint:invalid-base] /tmp/mypy_primer/projects/poetry/src/poetry/packages/package_collection.py:15:25: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/poetry/src/poetry/utils/threading.py:21:28: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 1128 diagnostics
+ Found 1126 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[lint:invalid-base] /tmp/mypy_primer/projects/schema_salad/schema_salad/ref_resolver.py:96:16: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 585 diagnostics
+ Found 584 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[lint:invalid-base] /tmp/mypy_primer/projects/urllib3/src/urllib3/_collections.py:156:30: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 463 diagnostics
+ Found 462 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[lint:invalid-base] /tmp/mypy_primer/projects/pydantic/pydantic/_internal/_generics.py:43:19: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 927 diagnostics
+ Found 926 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/cwltool/cwltool/workflow_job.py:362:35: Value is already of type `Unknown`
- Found 428 diagnostics
+ Found 427 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:invalid-base] /tmp/mypy_primer/projects/scrapy/scrapy/item.py:24:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 1511 diagnostics
+ Found 1510 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/tests/pool/test_pool_async.py:47:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/psycopg_pool/psycopg_pool/_acompat.py:46:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/psycopg_pool/psycopg_pool/_acompat.py:88:14: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_acompat.py:26:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_acompat.py:38:14: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/tests/pool/test_pool_null.py:44:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/tests/pool/test_pool_null_async.py:44:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/psycopg/tests/pool/test_pool.py:47:13: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 1327 diagnostics
+ Found 1319 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:invalid-base] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/mixins.py:245:23: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/structures.py:57:26: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/structures.py:45:52: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/structures.py:961:61: Unused blanket `type: ignore` directive
- error[lint:invalid-base] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/structures.py:1038:43: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/datastructures/structures.py:1038:1: Cannot create a consistent method resolution order (MRO) for class `CallbackDict` with bases list `[@Todo(GenericAlias instance), @Todo(GenericAlias instance)]`

setuptools (https://github.com/pypa/setuptools)
- error[lint:invalid-base] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/command/bdist.py:37:18: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/setuptools/pkg_resources/__init__.py:1119:18: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/setuptools/pkg_resources/__init__.py:1951:20: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 1110 diagnostics
+ Found 1107 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- error[lint:invalid-base] /tmp/mypy_primer/projects/pyodide/src/py/pyodide/webloop.py:21:21: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/pyodide/src/py/pyodide/webloop.py:166:19: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/pyodide/src/py/pyodide/console.py:238:21: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pyodide/src/py/pyodide/webloop.py:166:1: Cannot create a consistent method resolution order (MRO) for class `PyodideTask` with bases list `[@Todo(GenericAlias instance), @Todo(GenericAlias instance)]`
- Found 1271 diagnostics
+ Found 1269 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/utils/datastructures.pyi:60:22: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/utils/datastructures.pyi:92:21: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/utils/datastructures.pyi:103:19: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/contrib/sessions/backends/base.pyi:13:19: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/utils/html.pyi:46:18: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/db/migrations/migration.pyi:27:22: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/contrib/gis/gdal/raster/band.pyi:48:16: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/forms/utils.pyi:49:17: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/contrib/gis/gdal/raster/source.pyi:12:22: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/template/base.pyi:151:16: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/django-stubs/django-stubs/test/utils.pyi:33:19: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 1438 diagnostics
+ Found 1427 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/ptmalloc2.py:96:16: Operator `<=` is not supported for types `None` and `int`, in comparing `None | Unknown` with `Literal[7]`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/ptmalloc2.py:97:25: Operator `+` is unsupported between objects of type `None | Unknown` and `Literal[1]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/ptmalloc2.py:96:16: Operator `<=` is not supported for types `None` and `int`, in comparing `None | @Todo(GenericAlias instance)` with `Literal[7]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/ptmalloc2.py:97:25: Operator `+` is unsupported between objects of type `None | @Todo(GenericAlias instance)` and `Literal[1]`

bokeh (https://github.com/bokeh/bokeh)
- error[lint:invalid-base] /tmp/mypy_primer/projects/bokeh/src/bokeh/core/property/wrappers.py:174:49: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/bokeh/src/bokeh/core/property/wrappers.py:270:48: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/bokeh/src/bokeh/core/property/wrappers.py:312:49: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/bokeh/src/bokeh/util/compiler.py:73:16: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/bokeh/src/typings/bs4.pyi:8:17: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 1930 diagnostics
+ Found 1925 diagnostics

rotki (https://github.com/rotki/rotki)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/db/history_events.py:466:58: Attribute `prepare` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/globaldb/handler.py:281:52: Attribute `limit` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/globaldb/handler.py:283:21: Attribute `limit` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/globaldb/handler.py:284:22: Attribute `offset` on type `Unknown | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/db/history_events.py:466:58: Attribute `prepare` on type `@Todo(GenericAlias instance) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/globaldb/handler.py:281:52: Attribute `limit` on type `@Todo(GenericAlias instance) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/globaldb/handler.py:283:21: Attribute `limit` on type `@Todo(GenericAlias instance) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/globaldb/handler.py:284:22: Attribute `offset` on type `@Todo(GenericAlias instance) | None` is possibly unbound

zulip (https://github.com/zulip/zulip)
- error[lint:invalid-base] /tmp/mypy_primer/projects/zulip/zerver/webhooks/github/view.py:321:19: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 4442 diagnostics
+ Found 4441 diagnostics

jax (https://github.com/google/jax)
- error[lint:invalid-base] /tmp/mypy_primer/projects/jax/jax/experimental/_private_mm/mini_dime.py:76:21: Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 5717 diagnostics
+ Found 5716 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-23 07:54_

---

_Review requested from @carljm by @sharkdp on 2025-04-23 07:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-23 07:54_

---

_Review requested from @dcreager by @sharkdp on 2025-04-23 07:54_

---

_@AlexWaygood approved on 2025-04-23 08:36_

LGTM as a short-term fix!

---

_Merged by @sharkdp on 2025-04-23 08:39_

---

_Closed by @sharkdp on 2025-04-23 08:39_

---

_Branch deleted on 2025-04-23 08:39_

---
