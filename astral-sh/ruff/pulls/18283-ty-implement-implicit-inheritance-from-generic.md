```yaml
number: 18283
title: "[ty] Implement implicit inheritance from `Generic[]` for PEP-695 generic classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/695-generic-base
created_at: 2025-05-23T22:54:33Z
updated_at: 2025-05-26T19:40:18Z
url: https://github.com/astral-sh/ruff/pull/18283
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Implement implicit inheritance from `Generic[]` for PEP-695 generic classes

---

_@AlexWaygood_

## Summary

PEP-695 classes have `Generic` implicitly appended to the end of their `__bases__`:

```pycon
>>> class Foo[T]: ...
... 
>>> Foo.__mro__
(<class '__main__.Foo'>, <class 'typing.Generic'>, <class 'object'>)
>>> class Bar[T](int): ...
... 
>>> Bar.__mro__
(<class '__main__.Bar'>, <class 'int'>, <class 'typing.Generic'>, <class 'object'>)
```

This isn't something we fully model on `main`, leading us to infer inaccurate MROs for PEP-695 generic classes in some situations. This PR fixes that.

## Test Plan

mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-05-23 22:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-23 22:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-23 22:54_

---

_Label `ty` added by @AlexWaygood on 2025-05-23 22:54_

---

_Comment by @github-actions[bot] on 2025-05-23 22:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[invalid-argument-type] pytest_robotframework/_internal/robot/utils.py:158:15: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] pytest_robotframework/_internal/robot/utils.py:158:15: `Unknown` is not a valid argument to `Generic`

starlette (https://github.com/encode/starlette)
- error[invalid-argument-type] starlette/middleware/__init__.py:17:26: `ParamSpec` is not a valid argument to subscripted `typing.Protocol`
+ error[invalid-argument-type] starlette/middleware/__init__.py:17:26: `ParamSpec` is not a valid argument to `Protocol`

comtypes (https://github.com/enthought/comtypes)
- error[invalid-argument-type] comtypes/hints.pyi:150:33: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] comtypes/hints.pyi:150:33: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] comtypes/hints.pyi:160:28: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] comtypes/hints.pyi:160:28: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] comtypes/hints.pyi:174:34: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] comtypes/hints.pyi:174:34: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] comtypes/hints.pyi:183:29: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] comtypes/hints.pyi:183:29: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] comtypes/hints.pyi:196:34: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] comtypes/hints.pyi:196:34: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] comtypes/hints.pyi:205:29: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] comtypes/hints.pyi:205:29: `ParamSpec` is not a valid argument to `Generic`

asynq (https://github.com/quora/asynq)
- error[invalid-argument-type] asynq/decorators.pyi:35:70: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] asynq/decorators.pyi:35:70: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] asynq/decorators.pyi:39:58: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] asynq/decorators.pyi:39:58: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] asynq/decorators.pyi:70:62: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] asynq/decorators.pyi:70:62: `ParamSpec` is not a valid argument to `Generic`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-argument-type] src/hydra_zen/typing/_implementations.py:139:32: `ParamSpec` is not a valid argument to subscripted `typing.Protocol`
+ error[invalid-argument-type] src/hydra_zen/typing/_implementations.py:139:32: `ParamSpec` is not a valid argument to `Protocol`
- error[invalid-argument-type] src/hydra_zen/wrapper/_implementations.py:110:11: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] src/hydra_zen/wrapper/_implementations.py:110:11: `ParamSpec` is not a valid argument to `Generic`

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-argument-type] psycopg_pool/psycopg_pool/pool.py:38:22: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] psycopg_pool/psycopg_pool/pool.py:38:22: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] psycopg_pool/psycopg_pool/pool.py:814:21: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] psycopg_pool/psycopg_pool/pool.py:814:21: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] psycopg_pool/psycopg_pool/pool_async.py:38:27: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] psycopg_pool/psycopg_pool/pool_async.py:38:27: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] psycopg_pool/psycopg_pool/pool_async.py:868:21: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] psycopg_pool/psycopg_pool/pool_async.py:868:21: `Unknown` is not a valid argument to `Generic`

Expression (https://github.com/cognitedata/Expression)
- error[invalid-argument-type] expression/core/fn.py:14:16: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] expression/core/fn.py:14:16: `ParamSpec` is not a valid argument to `Generic`

mkdocs (https://github.com/mkdocs/mkdocs)
- error[invalid-argument-type] mkdocs/plugins.py:460:21: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mkdocs/plugins.py:460:21: `ParamSpec` is not a valid argument to `Generic`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-argument-type] mitmproxy/utils/signals.py:68:19: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mitmproxy/utils/signals.py:68:19: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] mitmproxy/utils/signals.py:81:20: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mitmproxy/utils/signals.py:81:20: `ParamSpec` is not a valid argument to `Generic`

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/core/frame.py:239:31: `@Todo(Inference of subscript on special form)` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] static_frame/core/frame.py:239:31: `@Todo(Inference of subscript on special form)` is not a valid argument to `Generic`
- error[invalid-argument-type] static_frame/core/index_hierarchy.py:234:33: `@Todo(Inference of subscript on special form)` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] static_frame/core/index_hierarchy.py:234:33: `@Todo(Inference of subscript on special form)` is not a valid argument to `Generic`

mypy (https://github.com/python/mypy)
- error[invalid-argument-type] mypy/typeshed/stdlib/asyncio/tasks.pyi:431:34: `Unknown` is not a valid argument to subscripted `typing.Protocol`
+ error[invalid-argument-type] mypy/typeshed/stdlib/asyncio/tasks.pyi:431:34: `Unknown` is not a valid argument to `Protocol`
- error[invalid-argument-type] mypy/typeshed/stdlib/asyncio/tasks.pyi:443:33: `Unknown` is not a valid argument to subscripted `typing.Protocol`
+ error[invalid-argument-type] mypy/typeshed/stdlib/asyncio/tasks.pyi:443:33: `Unknown` is not a valid argument to `Protocol`
- error[invalid-argument-type] mypy/typeshed/stdlib/builtins.pyi:139:20: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/builtins.pyi:139:20: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/builtins.pyi:156:19: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/builtins.pyi:156:19: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/builtins.pyi:1999:45: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/builtins.pyi:1999:45: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/contextlib.pyi:214:53: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/contextlib.pyi:214:53: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/functools.pyi:86:16: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/functools.pyi:86:16: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/functools.pyi:93:16: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/functools.pyi:93:16: `ParamSpec` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/unittest/case.pyi:73:52: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/unittest/case.pyi:73:52: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] mypy/typeshed/stdlib/weakref.pyi:189:16: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] mypy/typeshed/stdlib/weakref.pyi:189:16: `ParamSpec` is not a valid argument to `Generic`

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-argument-type] lib/streamlit/elements/widgets/button_group.py:94:25: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/button_group.py:94:25: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/elements/widgets/button_group.py:117:26: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/button_group.py:117:26: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/elements/widgets/button_group.py:153:24: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/button_group.py:153:24: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/elements/widgets/multiselect.py:65:24: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/multiselect.py:65:24: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/elements/widgets/radio.py:61:18: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/radio.py:61:18: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/elements/widgets/select_slider.py:76:25: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/select_slider.py:76:25: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/elements/widgets/selectbox.py:64:22: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/selectbox.py:64:22: `Unknown` is not a valid argument to `Generic`
- error[invalid-argument-type] lib/streamlit/testing/v1/element_tree.py:1120:22: `Unknown` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] lib/streamlit/testing/v1/element_tree.py:1120:22: `Unknown` is not a valid argument to `Generic`

zulip (https://github.com/zulip/zulip)
- error[invalid-argument-type] zerver/lib/cache.py:745:39: `ParamSpec` is not a valid argument to `typing.Generic`
+ error[invalid-argument-type] zerver/lib/cache.py:745:39: `ParamSpec` is not a valid argument to `Generic`

```
</details>


---

_@dcreager approved on 2025-05-24 13:41_

Excellent, thank you for catching this!

---

_Comment by @AlexWaygood on 2025-05-26 19:36_

This change means that we now naturally recognize classes such as `class Foo[T](Generic[T]): ...` at MRO-resolution time, so I was able to reimplement the diagnostic telling users off for doing that as an `MroError` variant. This enabled me to push some simplifications to the `GenericContext` struct (we no longer need to record whether the context came from `typing.Generic` or `typing.Protocol` inheritance), which in turn has led to some small speedups in the benchmarks 🎉 https://codspeed.io/astral-sh/ruff/branches/alex%2F695-generic-base

---

_Merged by @AlexWaygood on 2025-05-26 19:40_

---

_Closed by @AlexWaygood on 2025-05-26 19:40_

---

_Branch deleted on 2025-05-26 19:40_

---
