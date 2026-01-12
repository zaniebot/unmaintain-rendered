```yaml
number: 22291
title: "[ty] Add support for dynamic `type()` classes"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/dyn
created_at: 2025-12-29T21:45:15Z
updated_at: 2026-01-12T17:53:12Z
url: https://github.com/astral-sh/ruff/pull/22291
synced_at: 2026-01-12T18:23:35Z
```

# [ty] Add support for dynamic `type()` classes

---

_@charliermarsh_

## Summary

This PR adds support for dynamic classes created via `type()`. The core of the change is that `ClassLiteral` is now an enum:

```rust
pub enum ClassLiteral<'db> {
    /// A class defined via a `class` statement.
    Stmt(StmtClassLiteral<'db>),
    /// A class created via the functional form `type(name, bases, dict)`.
    Functional(FunctionalClassLiteral<'db>),
}
```

And, in turn, various methods on `ClassLiteral` like `body_scope` now return `Option` or similar (and callers must adjust to that change in signature).

Over time, we can expand the enum to include functional namedtuples, etc. (I already have this working in a separate branch, and I believe it slots in well.)

(I'd love help with the names -- I think `StmtClassLiteral` is kind of lame. Maybe `DeclarativeClassLiteral`?)

Closes https://github.com/astral-sh/ty/issues/740.


---

_Label `ty` added by @charliermarsh on 2025-12-29 21:45_

---

_Comment by @charliermarsh on 2025-12-29 21:45_

(Not ready for review.)

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 21:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 21:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 5 diagnostics
+ Found 4 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 6 diagnostics
+ Found 5 diagnostics

bidict (https://github.com/jab/bidict)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ bidict/_base.py:147:47: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[BT@_make_inv_cls]`

git-revise (https://github.com/mystor/git-revise)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2 diagnostics
+ Found 1 diagnostic

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 4 diagnostics
+ Found 3 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 36 diagnostics
+ Found 35 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 49 diagnostics
+ Found 48 diagnostics

attrs (https://github.com/python-attrs/attrs)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 628 diagnostics
+ Found 627 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 31 diagnostics
+ Found 30 diagnostics

packaging (https://github.com/pypa/packaging)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 24 diagnostics
+ Found 23 diagnostics

dacite (https://github.com/konradhalas/dacite)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 23 diagnostics
+ Found 22 diagnostics

anyio (https://github.com/agronholm/anyio)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 94 diagnostics
+ Found 93 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 11 diagnostics
+ Found 10 diagnostics

parso (https://github.com/davidhalter/parso)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 200 diagnostics
+ Found 199 diagnostics

janus (https://github.com/aio-libs/janus)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 4 diagnostics
+ Found 3 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 9 diagnostics
+ Found 8 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 6 diagnostics
+ Found 5 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 24 diagnostics
+ Found 23 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 44 diagnostics
+ Found 43 diagnostics

kornia (https://github.com/kornia/kornia)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 326 diagnostics
+ Found 324 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 173 diagnostics
+ Found 172 diagnostics

spack (https://github.com/spack/spack)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ lib/spack/spack/builder.py:134:17: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__init__]`
- lib/spack/spack/test/packages.py:417:9: error[unresolved-attribute] Object of type `type` has no attribute `name`
- Found 4319 diagnostics
+ Found 4318 diagnostics

python-sop (https://gitlab.com/dkg/python-sop)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2 diagnostics
+ Found 1 diagnostic

async-utils (https://github.com/mikeshardmind/async-utils)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 10 diagnostics
+ Found 9 diagnostics

pip (https://github.com/pypa/pip)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 604 diagnostics
+ Found 603 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 3 diagnostics
+ Found 2 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 81 diagnostics
+ Found 80 diagnostics

stone (https://github.com/dropbox/stone)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 253 diagnostics
+ Found 252 diagnostics

isort (https://github.com/pycqa/isort)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 36 diagnostics
+ Found 35 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- src/werkzeug/test.py:815:32: error[invalid-assignment] Object of type `type` is not assignable to `type[Response] | None`
+ src/werkzeug/test.py:817:32: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Response] & ~type[TestResponse]`
- Found 387 diagnostics
+ Found 386 diagnostics

asynq (https://github.com/quora/asynq)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 218 diagnostics
+ Found 217 diagnostics

websockets (https://github.com/aaugustin/websockets)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 35 diagnostics
+ Found 34 diagnostics

jinja (https://github.com/pallets/jinja)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 181 diagnostics
+ Found 180 diagnostics

paasta (https://github.com/yelp/paasta)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1103 diagnostics
+ Found 1102 diagnostics

yarl (https://github.com/aio-libs/yarl)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 47 diagnostics
+ Found 46 diagnostics

twine (https://github.com/pypa/twine)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 10 diagnostics
+ Found 9 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 195 diagnostics
+ Found 194 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 641 diagnostics
+ Found 640 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 22 diagnostics
+ Found 21 diagnostics

black (https://github.com/psf/black)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 55 diagnostics
+ Found 54 diagnostics

pylint (https://github.com/pycqa/pylint)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 217 diagnostics
+ Found 216 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- src/_pytest/_py/error.py:78:13: error[invalid-assignment] Invalid subscript assignment with key of type `int` and value of type `type` on object of type `dict[int, type[Error]]`
- src/_pytest/_py/error.py:79:20: error[invalid-return-type] Return type does not match returned value: expected `type[Error]`, found `type`
- Found 426 diagnostics
+ Found 423 diagnostics

DateType (https://github.com/glyph/DateType)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 9 diagnostics
+ Found 8 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1786 diagnostics
+ Found 1785 diagnostics

beartype (https://github.com/beartype/beartype)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 498 diagnostics
+ Found 497 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 26 diagnostics
+ Found 25 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 38 diagnostics
+ Found 37 diagnostics

starlette (https://github.com/encode/starlette)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 217 diagnostics
+ Found 216 diagnostics

alerta (https://github.com/alerta/alerta)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 556 diagnostics
+ Found 555 diagnostics

kopf (https://github.com/nolar/kopf)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 268 diagnostics
+ Found 267 diagnostics

downforeveryone (https://github.com/rpdelaney/downforeveryone)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 6 diagnostics
+ Found 5 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 449 diagnostics
+ Found 448 diagnostics

ignite (https://github.com/pytorch/ignite)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2037 diagnostics
+ Found 2036 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 28 diagnostics
+ Found 27 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 417 diagnostics
+ Found 416 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 19 diagnostics
+ Found 18 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 232 diagnostics
+ Found 231 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 3 diagnostics
+ Found 2 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 300 diagnostics
+ Found 299 diagnostics

rich (https://github.com/Textualize/rich)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 346 diagnostics
+ Found 345 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 217 diagnostics
+ Found 216 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 53 diagnostics
+ Found 52 diagnostics

pylox (https://github.com/sco1/pylox)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 49 diagnostics
+ Found 48 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 22 diagnostics
+ Found 21 diagnostics

mkosi (https://github.com/systemd/mkosi)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 78 diagnostics
+ Found 77 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ tests/test_runserver_logs.py:17:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aiohttp_std'>.Logger`
+ tests/test_runserver_logs.py:39:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aiohttp_debugtoolbar'>.Logger`
+ tests/test_runserver_logs.py:61:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aux_logger'>.Logger`
+ tests/test_runserver_logs.py:84:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aux_logger_livereload'>.Logger`
+ tests/test_runserver_logs.py:99:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_extra'>.Logger`
- Found 31 diagnostics
+ Found 35 diagnostics

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 32 diagnostics
+ Found 31 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 28 diagnostics
+ Found 27 diagnostics

imagehash (https://github.com/JohannesBuchner/imagehash)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 11 diagnostics
+ Found 10 diagnostics

flake8 (https://github.com/pycqa/flake8)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 39 diagnostics
+ Found 38 diagnostics

poetry (https://github.com/python-poetry/poetry)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 979 diagnostics
+ Found 978 diagnostics

nox (https://github.com/wntrblm/nox)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 25 diagnostics
+ Found 24 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 279 diagnostics
+ Found 278 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 241 diagnostics
+ Found 240 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
- Found 329 diagnostics
+ Found 328 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 15 diagnostics
+ Found 14 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 437 diagnostics
+ Found 436 diagnostics

alectryon (https://github.com/cpitclaudel/alectryon)
+ All checks passed!
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1 diagnostic

pandera (https://github.com/pandera-dev/pandera)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ pandera/api/dataframe/model.py:306:55: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__] & <Protocol with members '__parameters__'>`
- pandera/api/dataframe/model.py:308:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `type`
+ pandera/api/dataframe/model.py:308:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `<class '<unknown>'>`
+ pandera/api/pyspark/model.py:211:55: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__] & <Protocol with members '__parameters__'>`
- pandera/api/pyspark/model.py:213:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `type`
+ pandera/api/pyspark/model.py:213:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `<class '<unknown>'>`
- Found 1579 diagnostics
+ Found 1580 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- Found 3160 diagnostics
+ Found 3159 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 307 diagnostics
+ Found 306 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 244 diagnostics
+ Found 243 diagnostics

Expression (https://github.com/cognitedata/Expression)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 206 diagnostics
+ Found 205 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2147 diagnostics
+ Found 2146 diagnostics

artigraph (https://github.com/artigraph/artigraph)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 150 diagnostics
+ Found 149 diagnostics

optuna (https://github.com/optuna/optuna)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 578 diagnostics
+ Found 577 diagnostics

antidote (https://github.com/Finistere/antidote)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 248 diagnostics
+ Found 247 diagnostics

mypy (https://github.com/python/mypy)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1741 diagnostics
+ Found 1740 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ psycopg/psycopg/types/json.py:140:26: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Loader]`

comtypes (https://github.com/enthought/comtypes)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 401 diagnostics
+ Found 400 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 682 diagnostics
+ Found 681 diagnostics

operator (https://github.com/canonical/operator)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 133 diagnostics
+ Found 132 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 135 diagnostics
+ Found 134 diagnostics

pyppeteer (https://github.com/pyppeteer/pyppeteer)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 88 diagnostics
+ Found 87 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 225 diagnostics
+ Found 224 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 549 diagnostics
+ Found 548 diagnostics

build (https://github.com/pypa/build)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 50 diagnostics
+ Found 49 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 346 diagnostics
+ Found 345 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 406 diagnostics
+ Found 405 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1157 diagnostics
+ Found 1156 diagnostics

cibuildwheel (https://github.com/pypa/cibuildwheel)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 29 diagnostics
+ Found 28 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 29 diagnostics
+ Found 28 diagnostics

trio (https://github.com/python-trio/trio)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 483 diagnostics
+ Found 482 diagnostics

pyproject-metadata (https://github.com/pypa/pyproject-metadata)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 5 diagnostics
+ Found 4 diagnostics

xarray-dataclasses (https://github.com/astropenguin/xarray-dataclasses)
+ All checks passed!
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1 diagnostic

pyodide (https://github.com/pyodide/pyodide)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 936 diagnostics
+ Found 935 diagnostics

meson (https://github.com/mesonbuild/meson)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2155 diagnostics
+ Found 2154 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 448 diagnostics
+ Found 447 diagnostics

vision (https://github.com/pytorch/vision)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1410 diagnostics
+ Found 1409 diagnostics

apprise (https://github.com/caronc/apprise)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2648 diagnostics
+ Found 2647 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 422 diagnostics
+ Found 421 diagnostics

svcs (https://github.com/hynek/svcs)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 11 diagnostics
+ Found 10 diagnostics

django-test-migrations (https://github.com/wemake-services/django-test-migrations)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 3 diagnostics
+ Found 2 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 258 diagnostics
+ Found 257 diagnostics

manticore (https://github.com/trailofbits/manticore)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 11070 diagnostics
+ Found 11069 diagnostics

rclip (https://github.com/yurijmikhalevich/rclip)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 14 diagnostics
+ Found 13 diagnostics

numpy-stl (https://github.com/WoLpH/numpy-stl)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 23 diagnostics
+ Found 22 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 59 diagnostics
+ Found 58 diagnostics

archinstall (https://github.com/archlinux/archinstall)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 58 diagnostics
+ Found 57 diagnostics

setuptools (https://github.com/pypa/setuptools)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ setuptools/tests/test_editable_install.py:1206:53: error[unresolved-attribute] Object of type `SimulatedErr` has no attribute `__notes__`
- setuptools/tests/test_editable_install.py:1204:24: error[invalid-argument-type] Argument to function `raises` is incorrect: Argument type `object` does not satisfy upper bound `BaseException` of type variable `E`
- setuptools/tests/test_editable_install.py:1204:24: error[invalid-argument-type] Argument to function `raises` is incorrect: Expected `type[BaseException] | tuple[type[BaseException], ...]`, found `type`
- Found 1267 diagnostics
+ Found 1265 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5373 diagnostics
+ Found 5372 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2039 diagnostics
+ Found 2038 diagnostics

xarray (https://github.com/pydata/xarray)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1762 diagnostics
+ Found 1761 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 522 diagnostics
+ Found 521 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1235 diagnostics
+ Found 1232 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ strawberry/types/base.py:341:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

altair (https://github.com/vega/altair)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1062 diagnostics
+ Found 1061 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1180 diagnostics
+ Found 1179 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1322 diagnostics
+ Found 1321 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 1486 diagnostics
+ Found 1485 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 8474 diagnostics
+ Found 8473 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 181 diagnostics
+ Found 180 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 48 diagnostics
+ Found 47 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 64 diagnostics
+ Found 63 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 445 diagnostics
+ Found 444 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 22 diagnostics
+ Found 21 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ hydpy/core/testtools.py:1376:47: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[T@make_abc_testable]`
+ hydpy/core/testtools.py:1377:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- hydpy/core/testtools.py:1378:12: error[invalid-return-type] Return type does not match returned value: expected `type[T@make_abc_testable]`, found `type`
+ hydpy/core/testtools.py:1378:12: error[invalid-return-type] Return type does not match returned value: expected `type[T@make_abc_testable]`, found `<class '<unknown>'>`
- Found 664 diagnostics
+ Found 665 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 878 diagnostics
+ Found 877 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2713 diagnostics
+ Found 2712 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 6 diagnostics
+ Found 5 diagnostics

jax (https://github.com/google/jax)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2806 diagnostics
+ Found 2805 diagnostics

CPython (peg_generator) (https://github.com/python/cpython)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 47 diagnostics
+ Found 46 diagnostics

cryptography (https://github.com/pyca/cryptography)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 37 diagnostics
+ Found 36 diagnostics

ibis (https://github.com/ibis-project/ibis)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- ibis/examples/__init__.py:122:9: error[unresolved-attribute] Object of type `type` has no attribute `fetch`
- Found 4609 diagnostics
+ Found 4607 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`

streamlit (https://github.com/streamlit/streamlit)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 104 diagnostics
+ Found 103 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 3793 diagnostics
+ Found 3792 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2436 diagnostics
+ Found 2435 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 515 diagnostics
+ Found 514 diagnostics

sympy (https://github.com/sympy/sympy)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 15598 diagnostics
+ Found 15597 diagnostics

zulip (https://github.com/zulip/zulip)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 3673 diagnostics
+ Found 3672 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 5171 diagnostics
+ Found 5170 diagnostics

rotki (https://github.com/rotki/rotki)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 2105 diagnostics
+ Found 2104 diagnostics

colour (https://github.com/colour-science/colour)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 417 diagnostics
+ Found 416 diagnostics

core (https://github.com/home-assistant/core)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`

scipy (https://github.com/scipy/scipy)
- /home/runner/.config/ty/ty.toml:9:1: warning[unknown-rule] Unknown rule `unsupported-dynamic-base`
- Found 8082 diagnostics
+ Found 8081 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~159MB
+ TOTAL MEMORY USAGE: ~167MB


```

</details>




---

_Comment by @charliermarsh on 2025-12-29 22:56_

I think the scrapy, black, and aiohttp-devtools changes are true positives.

---

_Comment by @codspeed-hq[bot] on 2025-12-29 22:59_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **degrade performance by 5.11%**




`❌ 1` regressed benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn?utm_source=github&utm_medium=comment-v2&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 10 s | 10.6 s | -5.11% |
---

<sub>Comparing <code>charlie/dyn</code> (5a177bb) with <code>main</code> (e4ba293)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @carljm by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-29 23:35_

---

_Converted to draft by @charliermarsh on 2025-12-29 23:36_

---

_@charliermarsh reviewed on 2025-12-29 23:49_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/functional_class.rs`:54 on 2025-12-29 23:49_

(Need to remove this.)

---

_Marked ready for review by @charliermarsh on 2025-12-30 03:24_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2025-12-30 03:24_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 03:30_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unknown-rule` | 0 | 156 | 0 |
| `invalid-return-type` | 4 | 1 | 9 |
| `invalid-argument-type` | 5 | 2 | 3 |
| `invalid-assignment` | 0 | 2 | 5 |
| `unsupported-dynamic-base` | 7 | 0 | 0 |
| `unresolved-attribute` | 1 | 2 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| **Total** | **19** | **163** | **17** |


**[Full report with detailed diff](https://aae4324d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://aae4324d.ty-ecosystem-ext.pages.dev/timing))



---

_Converted to draft by @charliermarsh on 2025-12-30 16:28_

---

_Marked ready for review by @charliermarsh on 2025-12-30 17:12_

---

_Comment by @AlexWaygood on 2025-12-30 17:24_

I haven't looked too hard at this yet, but is adding a new `Type` variant definitely the best way of doing this? We've talked in the past about making `ClassLiteral` (the wrapped data inside the `Type::ClassLiteral` variant) an enum. In almost all respects, types created using three-argument `type()` calls should be treated the same way as classes created using `class` statements (the only difference is that its definition in the AST will be an `ExprCall` rather than a `StmtClassDef`), and that design would better reflect that, I think.

We should also try to make our design here extensible for other classes-created-by-function-calls: functional namedtuples, functional typeddicts, and functional enums.

---

_Comment by @AlexWaygood on 2025-12-30 17:28_

For a previous attempt to make `ClassLiteral` an enum, you can see https://github.com/astral-sh/ruff/pull/19998. We eventually realised that that wasn't the correct way to go to implement `NewType`, but it still might be for three-argument `type()`, functional typeddicts, functional namedtuples and functional enums.

---

_Comment by @charliermarsh on 2025-12-30 17:34_

I can give that a try. I initially tried to add another variant to `ClassType`, but there was so much coupling to the class AST node that it ended up not being worth it in my assessment.

---

_Comment by @charliermarsh on 2025-12-30 17:35_

(The context around functional namedtuples, functional typeddicts, and functional enums is also very useful, thank you.)

---

_Converted to draft by @charliermarsh on 2025-12-30 19:12_

---

_Marked ready for review by @charliermarsh on 2025-12-31 20:57_

---

_Converted to draft by @charliermarsh on 2025-12-31 21:16_

---

_Marked ready for review by @charliermarsh on 2026-01-01 00:15_

---

_Comment by @carljm on 2026-01-06 04:15_

This needs a rebase.

---

_Comment by @charliermarsh on 2026-01-06 14:48_

Should be rebased now. It was a little gnarly, sorry for any fragments...

---

_@charliermarsh reviewed on 2026-01-06 14:48_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:2114 on 2026-01-06 14:48_

I'm not a huge fan of `Stmt` vs. `Functional`. I would almost prefer `Declarative` vs. `Functional`.

---

_Comment by @carljm on 2026-01-08 01:42_

Ecosystem report looks like mostly good news, a few quick notes:

1. It looks like this PR now allows inheritance from `type[...]` (SubclassOf) types. Is that intentional? I think it might be OK -- we've intentionally not done it so far, but I'm not sure it can actually cause unsoundness. Mypy and Pyrefly seem to allow it, Pyright does not.
2. The new diagnostics in `src/prefect/flow_engine.py` look a bit concerning. I don't think those are non-determinism, and it looks like we are now failing to specialize away a type variable from a method call. I think we need to understand what's happening here.

---

_Comment by @charliermarsh on 2026-01-08 01:47_

Which new diagnostics are you referring to? I only see these removed diagnostics: https://dee97dfa.ty-ecosystem-ext.pages.dev/diff

<img width="876" height="594" alt="Screenshot 2026-01-07 at 8 47 00 PM" src="https://github.com/user-attachments/assets/276f08f3-3569-4603-badf-1563ec32513a" />


---

_Comment by @charliermarsh on 2026-01-08 01:47_

I assume I'm looking in the wrong place.

---

_Comment by @carljm on 2026-01-08 01:57_

Oh interesting! I was still looking at the pre rebase ecosystem report. And it was exactly those same diagnostics now showing as removed that were showing as new in that report. So maybe they are non deterministic. In any case, no need to worry about them. 

---

_Comment by @MichaReiser on 2026-01-08 07:36_

> . In any case, no need to worry about them.

Shouldn't we worry about any new non-deterministic diagnostics to avoid making ty even more non-deterministic? It's obviously possible that the they're non-deterministic because of an existing issue but it might be worth to scan through the code to see if there's anything that might introduce new non-determinism (are we iterating over a `FxHashMap`, any non-deterministic cycle handling...)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:982 on 2026-01-08 07:37_

Nit: You could write this as:


```suggestion
            .and_then(|class| crate::types::enums::enum_metadata(db, instance.class_literal(db)?))
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 07:43_

I find `as_stmt` somewhat confusing because I immediately associate it with `ast::Stmt`. 

I think I would go with:

* `ClassDefinition` which is the terminology used in the [Python documentation](https://docs.python.org/3/reference/compound_stmts.html#class)
* `Static` and `Dynamic`, `dynamic` is the terminology used in the [`type` documentation](https://docs.python.org/3/library/functions.html#type) *This is essentially a dynamic form of the [class](https://docs.python.org/3/reference/compound_stmts.html#class) statement.*

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3262 on 2026-01-08 07:45_

```suggestion
                    .and_then(|(class_literal, _)| {
                    	let metadata = enum_metadata(db, class_literal)?;
                    	let index = *metadata.members.get_index(0)?;
                    	Some(Place::bound(index))
                  	})
                  	.unwrap_or(Place::Undefined)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3371 on 2026-01-08 07:45_

```suggestion
                        .and_then(|class| Some(class.stmt_class_literal(db)?.0)),
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8996 on 2026-01-08 07:46_

```suggestion
                    Some(Type::ClassLiteral(
                    	instance
                        .class(db)
                        .stmt_class_literal(db)?
                        .0
                        .into()
                    ))
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/arguments.rs`:409 on 2026-01-08 07:47_

```suggestion
            if let Some((class_literal, _)) = class.stmt_class_literal(db) &&
            	let Some(enum_members) = enum_member_literals(db, class_literal, None) {
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:91 on 2026-01-08 07:49_

That's a lot of monomorphized code. Does the code need to be generic over `I`? Are there parts that we can move into non-generic functions?

I also think this should come after the `ClassLiteral` definition as it seems more an implementation detail

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:682 on 2026-01-08 07:50_

```suggestion
        self.as_stmt()?.known(db)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4738 on 2026-01-08 07:53_

Could we use `deref` here? (so that salsa returns `&[ClassBase]`) or are there cases where we need to know that it's a boxed slice?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4784 on 2026-01-08 07:55_

```suggestion
        let (mut candidate_base, rest) = bases.split_first().unwrap();

        // Reconcile with other bases' metaclasses.
        for base in rest {
```

---

_@MichaReiser reviewed on 2026-01-08 07:57_

---

_@charliermarsh reviewed on 2026-01-08 14:43_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 14:43_

Yeah I like this too. @carljm -- does that seem reasonable? (Just want to avoid multiple rounds of renaming if possible.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:981 on 2026-01-08 14:51_

we'll probably have to use a `match` here in the future when we add support for functional enums, so is it worth switching to use a `match` here now? It might lead to less code churn in the future

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 14:52_

> I find `as_stmt` somewhat confusing because I immediately associate it with `ast::Stmt`.

I think that's sort of the point? One kind of class is created using a `class` statement, and the other is created using a call expression.

But I also like your proposed terminology!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5489 on 2026-01-08 14:56_

what's the motivation for making this a separate branch in the `match`? It still looks like the branch arm does the same thing as the following one to me

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6543 on 2026-01-08 14:58_

we should be able to provide a `TypeDefinition` for dynamic `type()` classes too -- you might have to add a new variant to the `TypeDefinition` enum, but that shouldn't be too hard

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12725 on 2026-01-08 15:00_

in the long term we won't be able to count on all `enum_class`es having `StmtClassDef` nodes in the AST, due to the functional syntax for creating enum classes. (But no change requested; this seems fine for now.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12953 on 2026-01-08 15:01_

why is this taking a `StmtClassLiteral` rather than a `ClassLiteral`? All kinds of class-literals have MROs, right?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/bound_super.rs`:1 on 2026-01-08 15:10_

most of the changes to this file *look* unnecessary -- but I think that's just because you need to fix some merge conflicts, and they'll probably go away from the diff after a rebase?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1396 on 2026-01-08 15:15_

```suggestion
                                    .map(|s| Name::new(s.value(db)));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:755 on 2026-01-08 15:23_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:763 on 2026-01-08 15:24_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:578 on 2026-01-08 15:32_

is this correct? Couldn't you create a protocol class using the `type()` function?

```py
from typing import Protocol

X = type("X", (Protocol,), {})
```

though I don't know why you would, since you wouldn't be able to define any protocol members. Maybe we should just explicitly ban that, and emit a diagnostic for that? (Fine to defer that to a followup.)

There are lots of assumptions everywhere that you can't define a generic class using the `type()` function, so we should probably try to emit a diagnostic for

```py
from typing import Generic, TypeVar

T = TypeVar("T")

X = type("X", (Generic[T],), {})
```

Interestingly, all type checkers reject "dynamic generic classes" like the one above, but mypy _does_ support defining generic functional namedtuples/typeddicts:

```py
from typing import Generic, NamedTuple, TypeVar, TypedDict

T = TypeVar("T")

X = NamedTuple("X", [("a", T)])
Y = TypedDict("Y", {"some-key": T})

def _(obj: X[int], obj2: Y[int]):
    reveal_type(obj)  # revealed: tuple[builtins.int, fallback=__main__.X[builtins.int]]
    reveal_type(obj2)  # revealed: TypedDict('__main__.Y', {'some-key': builtins.int})
```

the second case here is important, actually, because it's the only way to define a generic `TypedDict` with invalid-identifier keys.

https://mypy-play.net/?mypy=latest&python=3.12&gist=5f45711ace3174d8a87b2c4d14920f57

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:825 on 2026-01-08 15:32_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:837 on 2026-01-08 15:33_

Ideally, functional classes would store a reference to the `ExprCall` `Definition` that was used to define them, and return that `Definition` here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:855 on 2026-01-08 15:34_

```suggestion
            Self::Functional(_) => {
                Type::instance(db, ClassType::NonGeneric(self))
            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:875 on 2026-01-08 15:34_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:896 on 2026-01-08 15:35_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4694 on 2026-01-08 15:40_

Hmm, but pyright's behaviour seems incorrect here? Ideally we would treat these as distinct types, the same as we do for different class-literals that have the same name. Storing the `Definition` on the `FunctionalClassLiteral` struct, as I suggested above, might help with that.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4893 on 2026-01-08 15:42_

I would add `#[salsa::tracked]` to the `impl` block above and then put this method in that `impl` block. Just because `#[salsa::tracked]` is added to an `impl` block doesn't mean that all methods in that `impl` block need to be themselves decorated with `#[salsa::tracked]`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4774 on 2026-01-08 15:43_

```suggestion
        // If no bases, metaclass is `type`.
        // To dynamically create a class with no bases that has a custom metaclass,
        // you have to invoke that metaclass rather than `type()`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:321 on 2026-01-08 15:46_

At some point we need to add support for the fact that all `Protocol` classes actually have `_ProtocolMeta` as their metaclass (https://github.com/astral-sh/ty/issues/1204); you could possibly add a TODO here about that

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:373 on 2026-01-08 15:48_

```suggestion
                let Some((class_literal, specialization)) = class.stmt_class_literal(db) else {
                    // Functional classes can't have cyclic MRO since their bases must
                    // already exist at creation time. Unlike statement classes, we do not
                    // permit functional classes to have forward references in their
                    // bases list.
                    return false;
                };
```

Do you have any tests for attempts to create cyclic MROs using `type()` in stub files? If not, you could add this as a test (make sure you make the mdtest a `.pyi` file!):

```py
X = type("X", (X,), {})
```

(you don't panic on it, which is good 😄)

---

_@AlexWaygood reviewed on 2026-01-08 15:51_

Wow, thanks for taking this on!!

---

_Comment by @jelle-openai on 2026-01-08 18:06_

> but I'm not sure it can actually cause unsoundness

```python
def func(x: int) -> str:
    class Base1:
        pass
    
    class Base2:
        def meth(self, x: int) -> str:
            return str(x)
    
    class Sneaky(Base1):
        def meth(self, x: int) -> int:
            return x
            
    t: type[Base1] = Sneaky
    
    class Child(t, Base2):
        pass
    
    c = Child()
    return c.meth(x)
```

Pyrefly seems to allow this and it is unsound. Mypy rejects the inheritance from `t`, not sure what you were doing that made it accept it.

---

_Comment by @carljm on 2026-01-08 20:01_

Thanks @jelle-openai ! I didn't spend very long thinking about it, and knew I was probably missing something. I was thinking that Liskov checking should make it safe, but of course that doesn't apply when subclasses introduce new methods/attributes.

I agree this means we shouldn't allow this, so whatever is changing in this PR to allow it, let's not make that change (even if it shows up in the ecosystem as a thing people try to do.)

> Mypy rejects the inheritance from `t`, not sure what you were doing that made it accept it.

Don't have the playground I used anymore, but probably it was the usual -- forgetting that mypy doesn't check all code by default.




---

_Comment by @carljm on 2026-01-08 20:02_

(This PR needs conflict resolution again, with the bound-super PR that landed.)

---

_Comment by @charliermarsh on 2026-01-08 20:02_

Sounds good -- I will rebase and address some of the initial feedback tonight.

---

_Comment by @carljm on 2026-01-08 20:04_

> Shouldn't we worry about any new non-deterministic diagnostics to avoid making ty even more non-deterministic?

Yes, I guess I was thinking "the non-determinism in prefect isn't new", but if these particular diagnostics are really new (not totally sure, but I don't think I remember this block of added/removed diagnostics) we should try to avoid adding them.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 20:05_

I also like @MichaReiser 's proposed terminology!

---

_@carljm reviewed on 2026-01-08 20:05_

---

_Comment by @AlexWaygood on 2026-01-08 20:06_

> > Shouldn't we worry about any new non-deterministic diagnostics to avoid making ty even more non-deterministic?
> 
> Yes, I guess I was thinking "the non-determinism in prefect isn't new", but if these particular diagnostics are really new (not totally sure, but I don't think I remember this block of added/removed diagnostics) we should try to avoid adding them.

I've seen the diagnostics in `src/prefect/flow_engine.py` come and go on quite a few other PRs; there's nothing new about those flakes in particular

---

_Comment by @AlexWaygood on 2026-01-08 20:08_

I agree with Carl and Jelle that we definitely shouldn't allow this (we don't on `main`, and that's deliberate):

```py
class Foo: ...

def f(x: type[Foo]):
    class Y(x): ...
```

---

_@AlexWaygood reviewed on 2026-01-08 20:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:263 on 2026-01-08 20:13_

huh? Not sure I understand this comment. For a class defined as `X = type("X", (A, B), {})`, surely the explicit bases of `X` are `[A, B]`?

---

_Comment by @charliermarsh on 2026-01-08 22:21_

Reducing the diff quite a bit here (hopefully) by using `ClassLiteral` in more places (i.e., pushing the match on `ClassLiteral` lower).

---

_@charliermarsh reviewed on 2026-01-08 22:40_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/mro.rs`:263 on 2026-01-08 22:40_

Sorry, this comment is clearly wrong. (But we only ever call this particular method with a statement-based class.) I will clean up.

---

_Converted to draft by @charliermarsh on 2026-01-08 23:00_

---

_Comment by @charliermarsh on 2026-01-08 23:00_

Converting to draft while I work through the feedback.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/ty_walltime.rs`:166 on 2026-01-09 01:17_

You can stick to round numbers here and give us a buffer, so we don't have to bump this on every small change.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:118 on 2026-01-09 01:22_

I kinda think this behavior isn't great, and I'd rather have mypy/pyrefly's behavior (where all attributes are allowed, with type `Unknown`), or better, just attributes named in the namespace dict are allowed, with type `Unknown`. (Or we could even infer types from the assigned elements in the namespace dict?)

Not saying we should do it in this PR, though. And we probably don't even need a TODO for it here, we can wait for someone to file an issue.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:162 on 2026-01-09 01:29_

This test demonstrates (and the comment in it explains) the opposite of what the prose above it claims.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:169 on 2026-01-09 01:30_

And this test isn't really about disjointness at all, just subtyping

---

_@charliermarsh reviewed on 2026-01-09 01:38_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:612 on 2026-01-09 01:38_

I _think_ we want to omit dynamic classes here.

---

_@carljm reviewed on 2026-01-09 01:39_

Holding off on more code review since this is currently in draft and getting revisions, but I did take a look at the tests quick.

---

_@charliermarsh reviewed on 2026-01-09 01:56_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:1 on 2026-01-09 01:56_

I'm not very confident in what's in here.

---

_Marked ready for review by @charliermarsh on 2026-01-09 02:27_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-09 02:27_

---

_Comment by @charliermarsh on 2026-01-09 02:29_

Okay, remarking as ready for review:

- I renamed to `StaticClassLiteral` and `DynamicClassLiteral`.
- I removed a variety of special-casings (like for display, etc.) that were unnecessary and wrong. (Likely Claude fragments from iterating on this a few times.)
- I implemented `Definition` and span tracking for dynamic class literals. This enabled me to remove a lot of special-casing that was no longer necessary as it further unified the `ClassLiteral` interface.

The pieces in which I have lowest confidence:

- The actual call parsing and inference in `builder.rs`, and the fall-through handling.
- The MRO handling for dynamic classes.


---

_@charliermarsh reviewed on 2026-01-09 15:57_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:118 on 2026-01-09 15:57_

Doing this in https://github.com/astral-sh/ruff/pull/22480 (in part because it actually helps the code generalize a bit more).

---

_@AlexWaygood reviewed on 2026-01-09 17:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-09 17:10_

Okay this was a great call @MichaReiser -- I find the code so much more readable now we've switched to this terminology!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:1 on 2026-01-09 17:23_

There are so many new tests here that it might be worth moving all `type()`-related assertions in this file to a new, standalone, `type.md` mdtest suite

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12720 on 2026-01-09 19:16_

looks like a spurious change

```suggestion
        self.enum_class(db).to_non_generic_instance(db)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3258 on 2026-01-09 19:19_

can be this if you rebase on `main`

```suggestion
                    .unwrap_or_default()
```

though idk if that's actually any clearer at the end of the day 😆

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1138 on 2026-01-09 19:21_

Why must this specifically be a static class-literal? In theory we could support this, right? (We don't have to do it in this PR, but you could add a test and/or a TODO comment)

```py
from dataclasses import dataclass

T = type("T", (), {})
T = dataclass(T)
```

If we made this change, `CodeGeneratorKind::from_class` would need to be updated so that it accepted a `ClassLiteral` rather than a `StaticClassLiteral`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:580 on 2026-01-09 19:32_

```suggestion
    /// Returns whether this class is a `TypedDict`.
    ///
    // TODO: We should emit a diagnostic if a functional class (created via `type()`) attempts
    // to inherit from `TypedDict`. To create a functional TypedDict, you should invoke
    // `TypedDict` as a function, not `type`. See also: `generic_context`, `is_protocol`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:556 on 2026-01-09 19:38_

The code here is correct but the comments are wrong:
- Functional types _can_ be tuple subclasses: your branch gets this right!

  ```py
  from ty_extensions import reveal_mro

  X = type("X", (tuple[int, str],), {})
  reveal_mro(X)  # Revealed MRO: (<class 'X'>, <class 'tuple[int, str]'>, <class 'Sequence[int | str]'>, <class 'Reversible[int | str]'>, <class 'Collection[int | str]'>, <class 'Iterable[int | str]'>, <class 'Container[int | str]'>, typing.Protocol, typing.Generic, <class 'object'>)
  ```

- But this method isn't actually asking "Is this class a tuple subclass?", it's asking "Is this the tuple class exactly?"

```suggestion
    /// Returns whether this class is `builtins.tuple` exactly
    pub(crate) fn is_tuple(self, db: &'db dyn Db) -> bool {
        match self {
            Self::Static(class) => class.is_tuple(db),
            Self::Dynamic(_) => false,
        }
    }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:653 on 2026-01-09 19:40_

this will need to be revised in https://github.com/astral-sh/ruff/pull/22480, when we start adding support for dynamic classes to define attributes via the namespace parameter

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:628 on 2026-01-09 19:44_

this API now feels like a bit of a footgun, because I think you usually want to go via higher-level APIs for attribute access, etc. (which will work for both static classes and dynamic classes) rather than asking for the body scope. Considering that it is only used in one place, I'd be inclined to get rid of it:

```diff
diff --git a/crates/ty_python_semantic/src/place.rs b/crates/ty_python_semantic/src/place.rs
index 1eb7e0b511..6b44f328af 100644
--- a/crates/ty_python_semantic/src/place.rs
+++ b/crates/ty_python_semantic/src/place.rs
@@ -1606,7 +1606,9 @@ mod implicit_globals {
     use crate::place::{Definedness, PlaceAndQualifiers};
     use crate::semantic_index::symbol::Symbol;
     use crate::semantic_index::{place_table, use_def_map};
-    use crate::types::{KnownClass, MemberLookupPolicy, Parameter, Parameters, Signature, Type};
+    use crate::types::{
+        ClassLiteral, KnownClass, MemberLookupPolicy, Parameter, Parameters, Signature, Type,
+    };
     use ruff_python_ast::PythonVersion;
 
     use super::{DefinedPlace, Place, place_from_declarations};
@@ -1756,10 +1758,11 @@ mod implicit_globals {
             return smallvec::SmallVec::default();
         };
 
-        let Some(module_type_scope) = module_type.body_scope(db) else {
+        let ClassLiteral::Static(module_type) = module_type else {
             return smallvec::SmallVec::default();
         };
-        let module_type_symbol_table = place_table(db, module_type_scope);
+
+        let module_type_symbol_table = place_table(db, module_type.body_scope(db));
 
         module_type_symbol_table
             .symbols()
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index d67e927f39..5cf85f120c 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -621,11 +621,6 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns the body scope of this class, if it's a statement class.
-    pub(crate) fn body_scope(self, db: &'db dyn Db) -> Option<ScopeId<'db>> {
-        self.as_static().map(|class| class.body_scope(db))
-    }
-
     /// Returns the definition of this class, if available.
     pub(crate) fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
         match self {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:721 on 2026-01-09 19:47_

we may want to revise this in https://github.com/astral-sh/ruff/pull/22480, because a dynamic class could provide `__slots__` in the namespace dictionary, which would make it a disjoint base:

```pycon
>>> X = type("X", (), {"__slots__": ("a",)})
>>> class Foo(int, X): ...
... 
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    class Foo(int, X): ...
TypeError: multiple bases have instance lay-out conflict
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:348 on 2026-01-09 20:01_

when we hit this situation for static class literals, we still infer the class as a class-literal, but (after emitting a diagnostic to tell the user off about it) we insert `Unknown` into the MRO. This has the nice property that we will allow arbitrary attributes to be accessed on instances of the class: the user gets one (and exactly one) diagnostic about the bad class definition, rather than getting many other diagnostics about unresolved attributes on instances of the class:

```py
from ty_extensions import reveal_mro

def f(x: type[int]):
    class Foo(x): ...  # error: [unsupported-base]

    reveal_mro(Foo)  # revealed: (<class 'Foo'>, Unknown, <class 'object'>)
```

Should we just do the same for dynamic class-literals?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:1 on 2026-01-09 20:02_

Can you also add a test that attributes defined on `builtins.type` are accessible on dynamic classes? It looks like you get this behaviour correct on your branch, which is great:

```py
T = type("T", (), {})

# inherited from `builtins.type`:
reveal_type(T.__dictoffset__)  # revealed: int
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4519 on 2026-01-09 20:09_

`type[]` types are different from class-literal types: `type[Foo]` means "the class object `Foo` or any subclass of `Foo`"; `<class 'Foo'>` means "the class object `Foo`, and only that exact class object (no subclasses)"

```suggestion
/// The type of `Foo` would be `<class 'Foo'>` where `Foo` is a `DynamicClassLiteral` with:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4678 on 2026-01-09 20:10_

nit

```suggestion
/// This matches Pyright's behavior. For example:
///
/// ```python
/// Foo = type("Foo", (), {"attr": 42})
/// Foo().attr  # Error: no attribute 'attr'
/// ```
///
/// Supporting namespace dict attributes would require parsing dict literals and tracking
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4688 on 2026-01-09 20:10_

```suggestion
/// same name and bases:
///
/// ```python
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-09 20:14_

We may need to put some warnings in the doc-comments for some of these fields that they shouldn't be accessed when we're inferring types for a different module to the one the class was defined in? I think doing so would lead to overly aggressive cache invalidation. @MichaReiser will be able to say whether I'm spouting nonsense here or not

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4596 on 2026-01-09 20:18_

```suggestion
#[salsa::tracked]
impl<'db> DynamicClassLiteral<'db> {
    /// Returns a [`Span`] with the range of the `type()` call expression.
    ///
    /// See [`Self::header_range`] for more details.
    pub(super) fn header_span(self, db: &'db dyn Db) -> Span {
        Span::from(self.file(db)).with_range(self.header_range(db))
    }

    /// Returns the range of the `type()` call expression that created this class.
    pub(super) fn header_range(self, db: &'db dyn Db) -> TextRange {
        let module = parsed_module(db, self.file(db)).load(db);
        let node = module.get_by_index(self.node_index(db));
        node.range()
    }

```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4624 on 2026-01-09 20:21_

this isn't quite correct: we have this behaviour on your branch:

```py
class Foo: ...
Bar = type("Bar", (), {})

reveal_type(Foo.__class__)  # <class 'type'>
reveal_type(Bar.__class__)  # type
```

so whereas for the static class, we return "the class `type` itself", for the functional class we return "instance of `type`". We should be consistent here and return "the class `type` itself" in both cases:

```suggestion
            return Ok(KnownClass::Type.to_class_literal(db));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4889 on 2026-01-09 20:23_

nit: I'd find this more readable

```suggestion
        let result = MroLookup::new(db, self.iter_mro(db)).class_member(
            name,
            policy,
            None,  // No inherited generic context.
            false,  // Functional classes are never `object`.
        );
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4961 on 2026-01-09 20:33_

you can do this without the `unreachable!()` if you add another intermediate type:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index d67e927f39..d773c7daef 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -488,7 +488,7 @@ impl<'db> ClassLiteral<'db> {
                 // Functional classes don't have inherited generic context and are never `object`.
                 let result = MroLookup::new(db, mro_iter).class_member(name, policy, None, false);
                 match result {
-                    ClassMemberResult::Done { .. } => result.finalize(db),
+                    ClassMemberResult::Done(result) => result.finalize(db),
                     ClassMemberResult::TypedDict => KnownClass::TypedDictFallback
                         .to_class_literal(db)
                         .find_name_in_mro_with_policy(db, name, policy)
@@ -2634,7 +2634,7 @@ impl<'db> StaticClassLiteral<'db> {
         );
 
         match result {
-            ClassMemberResult::Done { .. } => result.finalize(db),
+            ClassMemberResult::Done(result) => result.finalize(db),
 
             ClassMemberResult::TypedDict => KnownClass::TypedDictFallback
                 .to_class_literal(db)
@@ -4713,7 +4713,7 @@ impl<'db> DynamicClassLiteral<'db> {
         );
 
         match result {
-            ClassMemberResult::Done { .. } => result.finalize(db),
+            ClassMemberResult::Done(result) => result.finalize(db),
             ClassMemberResult::TypedDict => {
                 // Simplified `TypedDict` handling without type mapping.
                 KnownClass::TypedDictFallback
@@ -4848,10 +4848,10 @@ impl<'db, I: Iterator<Item = ClassBase<'db>>> MroLookup<'db, I> {
             }
         }
 
-        ClassMemberResult::Done {
+        ClassMemberResult::Done(CompletedMemberLookup {
             lookup_result,
             dynamic_type,
-        }
+        })
     }
 
     /// Look up an instance member by iterating through the MRO.
@@ -4942,50 +4942,46 @@ impl<'db, I: Iterator<Item = ClassBase<'db>>> MroLookup<'db, I> {
 /// Result of class member lookup from MRO iteration.
 pub(super) enum ClassMemberResult<'db> {
     /// Found the member or exhausted the MRO.
-    Done {
-        lookup_result: LookupResult<'db>,
-        dynamic_type: Option<Type<'db>>,
-    },
+    Done(CompletedMemberLookup<'db>),
     /// Encountered a `TypedDict` base.
     TypedDict,
 }
 
-impl<'db> ClassMemberResult<'db> {
+pub(super) struct CompletedMemberLookup<'db> {
+    lookup_result: LookupResult<'db>,
+    dynamic_type: Option<Type<'db>>,
+}
+
+impl<'db> CompletedMemberLookup<'db> {
     /// Finalize the lookup result by handling dynamic type intersection.
     pub(super) fn finalize(self, db: &'db dyn Db) -> PlaceAndQualifiers<'db> {
-        match self {
-            ClassMemberResult::TypedDict => {
-                // Caller should handle TypedDict case before calling finalize
-                unreachable!("finalize called on TypedDict result")
-            }
-            ClassMemberResult::Done {
-                lookup_result,
-                dynamic_type,
-            } => match (PlaceAndQualifiers::from(lookup_result), dynamic_type) {
-                (symbol_and_qualifiers, None) => symbol_and_qualifiers,
+        match (
+            PlaceAndQualifiers::from(self.lookup_result),
+            self.dynamic_type,
+        ) {
+            (symbol_and_qualifiers, None) => symbol_and_qualifiers,
 
-                (
-                    PlaceAndQualifiers {
-                        place: Place::Defined(DefinedPlace { ty, .. }),
-                        qualifiers,
-                    },
-                    Some(dynamic),
-                ) => Place::bound(
-                    IntersectionBuilder::new(db)
-                        .add_positive(ty)
-                        .add_positive(dynamic)
-                        .build(),
-                )
-                .with_qualifiers(qualifiers),
+            (
+                PlaceAndQualifiers {
+                    place: Place::Defined(DefinedPlace { ty, .. }),
+                    qualifiers,
+                },
+                Some(dynamic),
+            ) => Place::bound(
+                IntersectionBuilder::new(db)
+                    .add_positive(ty)
+                    .add_positive(dynamic)
+                    .build(),
+            )
+            .with_qualifiers(qualifiers),
 
-                (
-                    PlaceAndQualifiers {
-                        place: Place::Undefined,
-                        qualifiers,
-                    },
-                    Some(dynamic),
-                ) => Place::bound(dynamic).with_qualifiers(qualifiers),
-            },
+            (
+                PlaceAndQualifiers {
+                    place: Place::Undefined,
+                    qualifiers,
+                },
+                Some(dynamic),
+            ) => Place::bound(dynamic).with_qualifiers(qualifiers),
         }
     }
 }
```

---

_@AlexWaygood reviewed on 2026-01-09 20:33_

Sorry, just a partial review but I gotta go!

---

_@charliermarsh reviewed on 2026-01-09 21:12_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:628 on 2026-01-09 21:12_

Smart!

---

_@charliermarsh reviewed on 2026-01-09 21:18_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:4889 on 2026-01-09 21:18_

Rust un-formats that away sadly :(

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/ty_walltime.rs`:174 on 2026-01-10 02:07_

I suspect we don't need this? No significant number of new diagnostics on pandas in the current iteration of the PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/type.md`:70 on 2026-01-10 02:11_

nit: should we match the code and call these "dynamic classes" throughout the tests? It seems clearer to me than "functional classes".

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/type.md`:474 on 2026-01-10 02:22_

There's an issue here that shows up in pywin32 in the ecosystem:

```py
from not_found import Bar, Baz  # import from unknown module to get values of type Unknown

class Child(Bar, Baz):  # no diagnostic here, on main or this PR
    pass

X = type("X", (Bar, Baz), {})  # duplicate-bases diagnostic here on this PR
```

We don't emit a duplicate-bases diagnostic on `Child`, but in this PR we do on `X`. There shouldn't be a diagnostic on either one -- as far as we know (and should assume), `Bar` and `Baz` are separate types and it's perfectly valid to inherit from both; we just don't know what types they are.

The way this is handled in the static-class MRO logic is that we don't emit duplicate-base errors until after we've tried and failed to linearize an MRO -- and multiple `Unknown` or `Any` bases doesn't fail linearization. Might be ideal to see if we can use the same approach for dynamic classes?

If not, the other approach would be to manually avoid emitting this diagnostic if the "dupes" are `Type::Dynamic`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/type.md`:523 on 2026-01-10 02:23_

This means we aren't treating the bases of a dynamic class as forward references (deferred evaluation) in a stub file?

I think that's fine -- nobody should be using `type(...)` in a stub file anyway.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1123 on 2026-01-10 02:25_

Should there be a TODO here that this should also work for dynamic class literal types?

---

_@carljm reviewed on 2026-01-10 02:26_

I also didn't make it through a complete review before needing to head out, but I left what I've got so far.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:523 on 2026-01-10 10:49_

Yeah, I specifically asked Charlie to add this test to demonstrate that we wouldn't panic on cyclic definitions in stub files 😄

---

_@AlexWaygood reviewed on 2026-01-10 10:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:404 on 2026-01-10 12:23_

but note that that `weird_other_arg` would actually be _mandatory_ if you created a dynamic class that had `Base` as a superclass, and `Base.__init_subclass__` required a `weird_other_arg` argument to be passed to it:

```pycon
>>> class Base:
...     def __init_subclass__(cls, weird_other_arg: int): ...
...     
>>> X = type("X", (Base,), {}, weird_other_arg=42)
>>> Y = type("X", (Base,), {})
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    Y = type("X", (Base,), {})
TypeError: Base.__init_subclass__() missing 1 required positional argument: 'weird_other_arg'
```

We don't handle this properly yet for static classes (https://github.com/astral-sh/ty/issues/1541), though we have an open PR to fix that (https://github.com/astral-sh/ruff/pull/22185). Maybe we should just land that PR and then rebase this on top of that...


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-10 12:27_

What happens if you create a type with a dynamic name?

```py
def f(x: str):
    X = type(x, (), {})
    reveal_type(X)]
```

I think our ideal behaviour there would maybe be that we'd still infer it as a class-literal type, but in our display we'd show something like `<class '<unknown>'> @ main.py:42`...?

---

What happens if you create a type with dynamic bases?

```py
from ty_extensions import reveal_mro

def f(bases: tuple[type, ...]):
    X = type("X", bases, {})
    reveal_type(X)
    reveal_mro(X)
```

I think our ideal behaviour there would maybe be that we'd still infer it as a class-literal type, but the revealed MRO would be `(<class 'X'>, Unknown, <class 'object'>)`, which would lead us to treat instances of the class highly dynamically (they would have all possible attributes available on them)

---

What happens for things like this?

```py
def f(*args, **kwargs):
    A = type(*args, **kwargs)
    reveal_type(A)

    B = type("B", *args, **kwargs)
    reveal_type(B)

    C = type("C", (), *args, **kwargs)
    reveal_type(C)

    D = type("D", (), {}, **kwargs)
    reveal_type(D)
```

For `A` and `B`, `type[Any]` might lead to the fewest false-positive errors. For `C` and `D`, it feels like we could infer class-literal types...?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:382 on 2026-01-10 12:36_

Why are these commented out? They all pass for me locally 😆

```suggestion
# Inherited from `builtins.type`:
reveal_type(T.__dictoffset__)  # revealed: int
reveal_type(T.__name__)  # revealed: str
reveal_type(T.__bases__)  # revealed: tuple[type, ...]
reveal_type(T.__mro__)  # revealed: tuple[type, ...]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:6132 on 2026-01-10 12:43_

```suggestion
        let class_literal = self.to_class_literal(db).as_class_literal()?.as_static()?;
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:7111 on 2026-01-10 12:57_

```suggestion
                    // Check for metaclass conflicts
                    if let Err(FunctionalMetaclassConflict {
                        metaclass1,
                        base1,
                        metaclass2,
                        base2,
                    }) = dynamic_class.try_metaclass(db)
                        && let Some(builder) =
                            context.report_lint(&CONFLICTING_METACLASS, call_expression)
                    {
                        builder.into_diagnostic(format_args!(
                            "The metaclass of a derived class (`{class}`) \
                            must be a subclass of the metaclasses of all its bases, \
                            but `{metaclass1}` (metaclass of base class `{base1}`) \
                            and `{metaclass2}` (metaclass of base class `{base2}`) \
                            have no subclass relationship",
                            class = dynamic_class.name(db),
                            metaclass1 = metaclass1.name(db),
                            base1 = base1.display(db),
                            metaclass2 = metaclass2.name(db),
                            base2 = base2.display(db),
                        ));
                    }
```

But given that this is the exact same message that we emit for static classes with metaclass conflicts in `infer/builder.rs`, maybe we could factor out a standalone `report_*` function in `types/diagnostic.rs`, to avoid the repetition?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5173 on 2026-01-10 12:59_

```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
pub(super) enum InstanceMemberResult<'db> {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:6974 on 2026-01-10 13:00_

```suggestion
                                        "Duplicate base class{maybe_s} {dupes} in class `{class}`",
                                        maybe_s = if duplicates.len() == 1 { "" } else { "es" },
                                        dupes = duplicates.iter().map(|base| base.display(db)).join(", "),
                                        class = dynamic_class.name(db),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-10 13:12_

I'd also love some x-failing tests for the various diagnostics that we plan to add in followups:
- `Protocol` in the bases list
- `TypedDict` in the bases list
- `NamedTuple` in the bases list
- `Generic[T]` in the bases list
- `Enum` (or any `Enum` subclass) in the bases list

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:167 on 2026-01-10 13:15_

oof, splitting that type annotation over three lines is ugly 😆 Let's just import `SmallVec` here?

```diff
diff --git a/crates/ty_python_semantic/src/types/enums.rs b/crates/ty_python_semantic/src/types/enums.rs
index 2bb0e4e4d1..f5067cc675 100644
--- a/crates/ty_python_semantic/src/types/enums.rs
+++ b/crates/ty_python_semantic/src/types/enums.rs
@@ -1,5 +1,6 @@
 use ruff_python_ast::name::Name;
 use rustc_hash::FxHashMap;
+use smallvec::SmallVec;
 
 use crate::{
     Db, FxIndexMap,
@@ -162,23 +163,22 @@ pub(crate) fn enum_metadata<'db>(
                                     {
                                         Type::string_literal(db, &name.to_lowercase())
                                     } else {
-                                        let custom_mixins: smallvec::SmallVec<
-                                            [Option<KnownClass>; 1],
-                                        > = class
-                                            .iter_mro(db, None)
-                                            .skip(1)
-                                            .filter_map(ClassBase::into_class)
-                                            .filter(|class| {
-                                                !Type::from(*class).is_subtype_of(
-                                                    db,
-                                                    KnownClass::Enum.to_subclass_of(db),
-                                                )
-                                            })
-                                            .map(|class| class.known(db))
-                                            .filter(|class| {
-                                                !matches!(class, Some(KnownClass::Object))
-                                            })
-                                            .collect();
+                                        let custom_mixins: SmallVec<[Option<KnownClass>; 1]> =
+                                            class
+                                                .iter_mro(db, None)
+                                                .skip(1)
+                                                .filter_map(ClassBase::into_class)
+                                                .filter(|class| {
+                                                    !Type::from(*class).is_subtype_of(
+                                                        db,
+                                                        KnownClass::Enum.to_subclass_of(db),
+                                                    )
+                                                })
+                                                .map(|class| class.known(db))
+                                                .filter(|class| {
+                                                    !matches!(class, Some(KnownClass::Object))
+                                                })
+                                                .collect();
 
                                         // `IntEnum`s have the same `auto()` behaviour to enums inheriting from `(int, Enum)`,
                                         // and `IntEnum`s also have `int` in their MROs, so both cases are handled here.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:39 on 2026-01-10 13:21_

```suggestion
            // Dynamic classes cannot be `TypedDict`s or protocols and they cannot be
            // the class `builtins.tuple`.
```

But I think it would be more future-proof to do an explicit `match` here over the `ClassLiteral` variant rather than calling `.static_class_literal(db)`. In the future we intend to add another `ClassLiteral` variant corresponding to functional `TypeDict`s; for that variant, we will want to go down the same codepath as the `class_literal.is_typed_dict(db).then(|| Type::typed_dict(class))` bit of this method a few lines below.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/list_members.rs`:1 on 2026-01-10 13:22_

Can we add some tests to https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md that demonstrate which members our autocomplete machinery considers to be available on instances of dynamic classes?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:51 on 2026-01-10 13:25_

We will want to change this in https://github.com/astral-sh/ruff/pull/22480 so that it accepts dynamic classes too: if we allow dynamic classes to define attributes in their namespace dictionary, we should also check whether those attributes are valid overrides of attributes in their superclasses.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:612 on 2026-01-10 13:29_

It looks like most of these checks either don't apply to dynamic classes, or we have them independently implemented elsewhere for dynamic classes. So I think this is fine. But we should rename the method to `check_static_class_definitions, and mention in the docstring that we only iterate over class definitions created using `class` statements.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-10 13:32_

What happens if you provide an explicit annotation when creating a dynamic class? I don't actually care _too_ much about our behaviour here right now, because we intend to change how this stuff works more broadly pretty soon in https://github.com/astral-sh/ty/issues/136. But let's add a test for it:

```py
T: type = type("T", (), {})
reveal_type(T)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5419 on 2026-01-10 13:34_

```suggestion
                        // Only handle 3-arg type() calls specially to capture the definition.
                        // 1-arg type() calls are handled by normal call binding.
                        Some(KnownClass::Type)
                            if call_expr.arguments.args.len() == 3
                                && call_expr.arguments.keywords.is_empty() =>
                        {
                            // Try to extract the functional class with definition.
                            // Fall back to regular call binding if extraction fails
                            // (e.g., for cyclic references where names aren't resolved yet).
                            self.infer_functional_type_expression(call_expr, definition)
                                .unwrap_or_else(|| {
                                    self.infer_call_expression_impl(call_expr, callable_type, tcx)
                                })
                        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6057 on 2026-01-10 13:36_

we should check whether we can infer the name as a string-literal _type_ rather than checking whether it's a string-literal _node_, so that we can support things like

```py
name = "some very very long name"
T = type(name, (), {})
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6075 on 2026-01-10 13:44_

I think here you want to do something like this:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index b3cacb5663..96c3243ad0 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -6077,10 +6077,13 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             KnownClass::Object.try_to_class_literal(db).unwrap().into();
 
         let base_classes: Option<Box<[ClassBase<'db>]>> = bases_type
-            .exact_tuple_instance_spec(db)
-            .and_then(|tuple_spec| {
-                tuple_spec
-                    .fixed_elements()
+            .tuple_instance_spec(db)
+            .as_deref()
+            .and_then(|spec|spec.as_fixed_length())
+            .and_then(|tuple| {
+                tuple
+                    .elements_slice()
+                    .iter()
                     .enumerate()
                     .map(|(idx, base)| {
                         // First try the standard conversion.
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index dfc484cabd..2bd955432b 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -1313,6 +1313,13 @@ pub enum Tuple<T> {
 }
 
 impl<T> Tuple<T> {
+    pub(crate) fn as_fixed_length(&self) -> Option<&FixedLengthTuple<T>> {
+        match self {
+            Tuple::Fixed(tuple) => Some(tuple),
+            Tuple::Variable(_) => None,
+        }
+    }
+
     pub(crate) const fn homogeneous(element: T) -> Self {
         Self::Variable(VariableLengthTuple::homogeneous(element))
     }
```

There's two changes here:
- Use `tuple_instance_spec()` rather than `exact_tuple_instance_spec()`. Tuple subclasses should be fine here. We could actually add a test for this:
- Make sure that the tuple is a fixed-length tuple. `Tuple::fixed_elements()` doesn't do this for you: if it's a tuple like `tuple[int, *tuple[str, ...]]`, it will give you the `int` element (the "fixed element"), then stop.

---

_@charliermarsh reviewed on 2026-01-10 13:46_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:70 on 2026-01-10 13:46_

Thank you, sorry. These are missed renames.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6153 on 2026-01-10 13:47_

It looks like there's quite a lot of duplication between this and the method above. Can we not have them both call into a `infer_functional_type_call_impl` method or something?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:94 on 2026-01-10 13:53_

I've been meaning for a while now to move this special casing to `class.rs`, where it would make more sense. Since this code is moving anyway in this PR, you could make that change here (or in a standalone PR and then rebase this on top of it):

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 32029a5617..ded5d5d9bc 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -2280,20 +2280,24 @@ impl<'db> StaticClassLiteral<'db> {
         let class_definition =
             semantic_index(db, self.file(db)).expect_single_definition(class_stmt);
 
-        if self.is_known(db, KnownClass::VersionInfo) {
-            let tuple_type = TupleType::new(db, &TupleSpec::version_info_spec(db))
-                .expect("sys.version_info tuple spec should always be a valid tuple");
-
-            Box::new([
-                definition_expression_type(db, class_definition, &class_stmt.bases()[0]),
-                Type::from(tuple_type.to_class_type(db)),
-            ])
-        } else {
-            class_stmt
+        match self.known(db) {
+            Some(KnownClass::VersionInfo) => {
+                let tuple_type = TupleType::new(db, &TupleSpec::version_info_spec(db))
+                    .expect("sys.version_info tuple spec should always be a valid tuple");
+
+                // Special-case `NotImplementedType`: typeshed says that it inherits from `Any`,
+                // but this causes more problems than it fixes.
+                Box::new([
+                    definition_expression_type(db, class_definition, &class_stmt.bases()[0]),
+                    Type::from(tuple_type.to_class_type(db)),
+                ])
+            }
+            Some(KnownClass::NotImplementedType) => Box::new([]),
+            _ => class_stmt
                 .bases()
                 .iter()
                 .map(|base_node| definition_expression_type(db, class_definition, base_node))
-                .collect()
+                .collect(),
         }
     }
 
diff --git a/crates/ty_python_semantic/src/types/mro.rs b/crates/ty_python_semantic/src/types/mro.rs
index eb6e05e58c..9f60c590ac 100644
--- a/crates/ty_python_semantic/src/types/mro.rs
+++ b/crates/ty_python_semantic/src/types/mro.rs
@@ -8,7 +8,7 @@ use crate::Db;
 use crate::types::class_base::ClassBase;
 use crate::types::generics::Specialization;
 use crate::types::{
-    ClassLiteral, ClassType, DynamicClassLiteral, KnownClass, KnownInstanceType, SpecialFormType,
+    ClassLiteral, ClassType, DynamicClassLiteral, KnownInstanceType, SpecialFormType,
     StaticClassLiteral, Type,
 };
 
@@ -86,13 +86,6 @@ impl<'db> Mro<'db> {
         }
 
         let class = class_literal.apply_optional_specialization(db, specialization);
-
-        // Special-case `NotImplementedType`: typeshed says that it inherits from `Any`,
-        // but this causes more problems than it fixes.
-        if class_literal.is_known(db, KnownClass::NotImplementedType) {
-            return Ok(Self::from([ClassBase::Class(class), ClassBase::object(db)]));
-        }
-
         let original_bases = class_literal.explicit_bases(db);
 
         match original_bases {
```

---

_@AlexWaygood reviewed on 2026-01-10 13:53_

This looks very good, and I think it's close! My main comment on this pass is I think we could do with a bunch more tests

---

_@charliermarsh reviewed on 2026-01-10 15:55_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:404 on 2026-01-10 15:55_

(Adding a TODO for now though happy to rebase on that if you want to merge it!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:523 on 2026-01-10 16:48_

```suggestion
                // Dynamic classes don't have inherited generic context and are never `object`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:544 on 2026-01-10 16:48_

```suggestion
    /// For static classes, this applies default type arguments.
    /// For dynamic classes, this returns a non-generic class type.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:563 on 2026-01-10 16:49_

```suggestion
    // TODO: We should emit a diagnostic if a dynamic class (created via `type()`) attempts
    // to inherit from `Generic[T]`, since dynamic classes can't be generic. See also: `is_protocol`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:571 on 2026-01-10 16:49_

```suggestion
    // TODO: We should emit a diagnostic if a dynamic class (created via `type()`) attempts
    // to inherit from `Protocol`, since dynamic classes can't be protocols. See also: `generic_context`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:578 on 2026-01-10 16:49_

```suggestion
    // TODO: We should emit a diagnostic if a dynamic class (created via `type()`) attempts
    // to inherit from `TypedDict`. To create a functional TypedDict, you should invoke
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:620 on 2026-01-10 16:49_

```suggestion
    /// For static classes, this is the class name and any arguments passed to the `class` statement.
    /// For dynamic classes, this is the entire `type()` call expression.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:769 on 2026-01-10 16:50_

```suggestion
    /// Dynamic classes cannot be `TypedDicts`, so this delegates to the Stmt variant.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:804 on 2026-01-10 16:50_

```suggestion
    fn from(dynamic: DynamicClassLiteral<'db>) -> Self {
        ClassLiteral::Dynamic(dynamic)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:946 on 2026-01-10 16:50_

```suggestion
    /// For static classes, returns `TypeDefinition::StaticClass`.
    /// For dynamic classes, returns `TypeDefinition::DynamicClass` if a definition is available.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1653 on 2026-01-10 16:50_

```suggestion
        // Dynamic classes don't have a generic context.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1877 on 2026-01-10 16:51_

```suggestion
    fn from(dynamic: DynamicClassLiteral<'db>) -> Type<'db> {
        Type::ClassLiteral(dynamic.into())
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2590 on 2026-01-10 16:51_

```suggestion
            // For dynamic classes, we can't get a StaticClassLiteral, so use self for tracking.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2645 on 2026-01-10 16:51_

```suggestion
            // For dynamic classes, we can't get a StaticClassLiteral, so use self for tracking.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2672 on 2026-01-10 16:51_

```suggestion
        // Dynamic classes don't have dataclass transformer params.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3685 on 2026-01-10 16:51_

```suggestion
                    // Dynamic classes don't have fields (no class body).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4622 on 2026-01-10 16:52_

outdated

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4695 on 2026-01-10 16:52_

```suggestion
    /// Get the metaclass of this dynamic class.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4706 on 2026-01-10 16:52_

```suggestion
    /// Try to get the metaclass of this dynamic class.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4776 on 2026-01-10 16:53_

```suggestion
    /// Iterate over the MRO of this dynamic class using C3 linearization.
    ///
    /// The MRO includes the dynamic class itself as the first element, followed
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4802 on 2026-01-10 16:53_

```suggestion
    /// - No inherited generic context (dynamic classes aren't generic).
    /// - `is_self_object = false` (dynamic classes are never `object`).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4832 on 2026-01-10 16:54_

```suggestion
            false, // Dynamic classes are never `object`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4847 on 2026-01-10 16:54_

```suggestion
    /// Try to compute the MRO for this dynamic class.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4857 on 2026-01-10 16:54_

```suggestion
/// Error for metaclass conflicts in dynamic classes.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4861 on 2026-01-10 16:55_

Should be renamed to `DynamicMetaclassConflict`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:379 on 2026-01-10 16:55_

```suggestion
                    // Dynamic classes can't have cyclic MRO since their bases must
                    // already exist at creation time. Unlike statement classes, we do not
                    // permit dynamic classes to have forward references in their
                    // bases list.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:317 on 2026-01-10 16:56_

```suggestion
    /// Attempt to resolve the MRO of a dynamic class (created via `type(name, bases, dict)`).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:322 on 2026-01-10 16:56_

parameter should be renamed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:389 on 2026-01-10 16:56_

```suggestion
    /// Compute a fallback MRO for a dynamic class when `of_dynamic_class` fails.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:394 on 2026-01-10 16:57_

parameter should be renamed

---

_Converted to draft by @charliermarsh on 2026-01-10 17:00_

---

_Comment by @charliermarsh on 2026-01-10 17:00_

(Back to draft while I address feedback.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:501 on 2026-01-10 17:00_

```suggestion
            ClassLiteral::Dynamic(dynamic) => {
                ClassBase::Class(ClassType::NonGeneric(dynamic.into()))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:688 on 2026-01-10 17:02_

```suggestion
/// Error kinds for dynamic class MRO computation.
///
/// These mirror the relevant variants from `MroErrorKind` for static classes.
```

---

_Comment by @AlexWaygood on 2026-01-10 17:21_

I think you're using `SubsequentMroElements::Owned()` more than you need to -- you could do this:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 2ac60dce6c..00ca535dc3 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -4848,19 +4848,10 @@ impl<'db> DynamicClassLiteral<'db> {
     ///
     /// Returns `Ok(Mro)` if successful, or `Err(FunctionalMroError)` if there's
     /// an error (duplicate bases or C3 linearization failure).
+    #[salsa::tracked(returns(ref), heap_size=ruff_memory_usage::heap_size)]
     pub(crate) fn try_mro(self, db: &'db dyn Db) -> Result<Mro<'db>, FunctionalMroError<'db>> {
         Mro::of_dynamic_class(db, self)
     }
-
-    /// Compute and cache the MRO for this functional class.
-    ///
-    /// Uses C3 linearization when possible, falling back to sequential iteration
-    /// with deduplication when there's an error (duplicate bases or C3 merge failure).
-    #[salsa::tracked(heap_size = ruff_memory_usage::heap_size)]
-    pub(crate) fn mro(self, db: &'db dyn Db) -> Mro<'db> {
-        self.try_mro(db)
-            .unwrap_or_else(|_| Mro::functional_fallback(db, self))
-    }
 }
 
 /// Error for metaclass conflicts in functional classes.
diff --git a/crates/ty_python_semantic/src/types/mro.rs b/crates/ty_python_semantic/src/types/mro.rs
index 9677b27603..9bb923b40a 100644
--- a/crates/ty_python_semantic/src/types/mro.rs
+++ b/crates/ty_python_semantic/src/types/mro.rs
@@ -435,6 +435,15 @@ impl<'db> FromIterator<ClassBase<'db>> for Mro<'db> {
     }
 }
 
+impl<'db> IntoIterator for Mro<'db> {
+    type Item = ClassBase<'db>;
+    type IntoIter = std::vec::IntoIter<ClassBase<'db>>;
+
+    fn into_iter(self) -> Self::IntoIter {
+        self.0.into_iter()
+    }
+}
+
 /// Iterator that yields elements of a class's MRO.
 ///
 /// We avoid materialising the *full* MRO unless it is actually necessary:
@@ -537,9 +546,14 @@ impl<'db> MroIterator<'db> {
                     SubsequentMroElements::Borrowed(full_mro_iter)
                 }
                 ClassLiteral::Dynamic(functional) => {
-                    let mro = functional.mro(self.db);
-                    let elements: Vec<_> = mro.iter().skip(1).copied().collect();
-                    SubsequentMroElements::Owned(elements.into_iter())
+                    let mut full_mro_iter = match functional.try_mro(self.db) {
+                        Ok(mro) => SubsequentMroElements::Borrowed(mro.iter()),
+                        Err(_) => SubsequentMroElements::Owned(
+                            Mro::functional_fallback(self.db, functional).into_iter(),
+                        ),
+                    };
+                    full_mro_iter.next();
+                    full_mro_iter
                 }
             })
     }
@@ -684,7 +698,7 @@ fn c3_merge(mut sequences: Vec<VecDeque<ClassBase>>) -> Option<Mro> {
 /// Error kinds for functional class MRO computation.
 ///
 /// These mirror the relevant variants from `MroErrorKind` for regular classes.
-#[derive(Debug, Clone)]
+#[derive(Debug, Clone, PartialEq, Eq, get_size2::GetSize, salsa::Update)]
 pub(crate) enum FunctionalMroError<'db> {
     /// The class has duplicate bases in its bases tuple.
     DuplicateBases(Box<[ClassBase<'db>]>),
```

But I'm a bit confused about why `Mro::functional_fallback` is as complex as it is, and why the fallback isn't stored on the error returned `DynamicClassLiteral::try_mro`. If we computed and stored the fallback MRO in a way more similar to what we do for static classes, we might not need the `SubsequentMroElements` enum at all.

---

_@charliermarsh reviewed on 2026-01-10 17:47_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6153 on 2026-01-10 17:47_

Added some shared helpers...

---

_Comment by @charliermarsh on 2026-01-10 17:47_

Yeah you're right. I think the functional MRO went through several iterations and I must've solved whatever required the enum along the way. Thanks!

---

_@charliermarsh reviewed on 2026-01-10 17:48_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:5424 on 2026-01-10 17:48_

This seems wrong, looking into it.

---

_@charliermarsh reviewed on 2026-01-10 17:49_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6228 on 2026-01-10 17:49_

(I suspect this is _not_ what you had in mind @AlexWaygood.)

---

_@charliermarsh reviewed on 2026-01-10 18:30_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:5424 on 2026-01-10 18:30_

Still not sure I have the ideal approach here.

---

_Marked ready for review by @charliermarsh on 2026-01-10 18:48_

---

_@charliermarsh reviewed on 2026-01-10 18:53_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:678 on 2026-01-10 18:53_

I think we said we wanted to ban this as a known simplification @AlexWaygood -- is that right?

---

_@charliermarsh reviewed on 2026-01-10 18:59_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:678 on 2026-01-10 18:59_

Ah no -- it was inheriting from `Protocol` itself.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:678 on 2026-01-10 19:00_

yeah, inheriting from protocol classes is fine, it just doesn't seem necessary to support creating a _new_ `Protocol` class using `type()` (which you'd do by including `Protocol` itself in the bases list)

---

_@AlexWaygood reviewed on 2026-01-10 19:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:211 on 2026-01-11 11:08_

```suggestion
Dynamic classes can be used as the pivot class in `super()` calls:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:259 on 2026-01-11 11:08_

```suggestion
# Child instances are subtypes of `Parent` instances.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:262 on 2026-01-11 11:09_

```suggestion
takes_parent(child)  # No error - `ChildCls` is a subtype of `Parent`
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:400 on 2026-01-11 11:11_

```suggestion
reveal_type(type("Foo", ()))  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:404 on 2026-01-11 11:17_

```suggestion
# TODO: the keyword arguments for `Foo`/`Bar`/`Baz` here are invalid
# (you cannot pass `metaclass=` to `type()`, and none of them have
# base classes with `__init_subclass__` methods),
# but `type[Unknown]` would be better than `Unknown` here
#
# error: [no-matching-overload] "No overload of class `type` matches arguments"
reveal_type(type("Foo", (), {}, weird_other_arg=42))  # revealed: Unknown
# error: [no-matching-overload] "No overload of class `type` matches arguments"
reveal_type(type("Bar", (int,), {}, weird_other_arg=42))  # revealed: Unknown
# error: [no-matching-overload] "No overload of class `type` matches arguments"
reveal_type(type("Baz", (), {}, metaclass=type))  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:439 on 2026-01-11 11:20_

I still think this should cause us to emit a diagnostic. However, having seen the ecosystem report, I think perhaps it should be a different error code to `unsupported-base` (maybe `unsupported-dynamic-base`?). It seems fairly common for users to create dynamic classes with `type[]` types as a base class, and the whole point of dynamic classes is after all that you can create them... dynamically. Making it a different error code to the one we use for unsupported bases in static class definitions will allow users to switch off only the diagnostic regarding dynamic classes, if they find it really noisy. (And maybe we should actually have it disabled by default, since it's quite strict.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:526 on 2026-01-11 11:23_

It might be fun to add this test too:

```py
def make_classes(name1: str, name2: str):
    cls1 = type(name1, (), {})
    cls2 = type(name2, (), {})

    def inner(x: cls1): ...

    # error: [invalid-argument-type] "Argument to function `inner` is incorrect: Expected `main.<locals of function 'make_classes'>.<unknown> @ main.py:2`, found `main.<locals of function 'make_classes'>.<unknown> @ main.py:3`"
    inner(cls2())
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:565 on 2026-01-11 11:24_

```suggestion
When `bases` is a module-level variable holding a tuple of class literals, we can extract the base
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:717 on 2026-01-11 11:40_

It seems like it is _possible_ to create an empty enum using `type()` if you try realy really hard. But yeah, we don't need to support this 😆

```pycon
>>> import enum as e
>>> type("E", (e.Enum,), e.EnumDict())
<enum 'E'>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:702 on 2026-01-11 11:43_

It doesn't need to be a keyword argument. Even if it's a positional-or-keyword argument in the `__init_subclass__` signature, that makes it a required keyword argument when subclassing that class (because there's no other way to pass arguments to a superclass's `__init_subclass__` method except by passing keyword arguments in the class statement or a dynamic `type()` call).

```suggestion
When a base class defines `__init_subclass__` with required arguments, those should be
passed to `type()`. This is not yet supported:

```py
class Base:
    def __init_subclass__(cls, required_arg: str, **kwargs):
        super().__init_subclass__(**kwargs)
        cls.config = required_arg
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1 on 2026-01-11 11:45_

If you do it as

```py
from dataclasses import dataclass

X = dataclass(type("X", (), {}))
```

do we store a `Definition` for the `X` class there? I think it's okay if we don't (this should come up very rarely), I'm just curious

I guess ideally we'd add some dedicated tests for goto-definition in https://github.com/astral-sh/ruff/blob/main/crates/ty_ide/src/goto_type_definition.rs, which would include edge cases like this, but it's fine for that to be a followup. I realise that this PR is dragging on a bit!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:931 on 2026-01-11 11:49_

```suggestion
# TODO: these should pass -- namespace dict attributes are not yet available for autocomplete
static_assert(has_member(DynamicWithDict, "custom_attr"))  # error: [static-assert-error]
static_assert(has_member(DynamicWithDict(), "custom_attr"))  # error: [static-assert-error]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:966 on 2026-01-11 11:50_

```suggestion
# TODO: these should pass; instance members should be available
static_assert(has_member(instance, "base_attr"))  # error: [static-assert-error]
static_assert(has_member(instance, "__repr__"))  # error: [static-assert-error]
static_assert(has_member(instance, "__hash__"))  # error: [static-assert-error]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:603 on 2026-01-11 11:52_

I would also be interested in some tests where the `*args` and/or `**kwargs` arguments "fill up" an unknown number of parameters in the `type()` call, e.g.

```py
def f(*args, **kwargs):
    A = type(*args, **kwargs)
    reveal_type(A)

    B = type("B", *args, **kwargs)
    reveal_type(B)

    C = type("C", (), *args, **kwargs)
    reveal_type(C)

    D = type("D", (), {}, **kwargs)
    reveal_type(D)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:605 on 2026-01-11 12:00_

this doesn't look correct -- I think we should do something like this here:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 3ab1659f9e..48c4049704 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -596,12 +596,11 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns the metaclass instance type for this class.
+    /// Return a type representing "the set of all instances of the metaclass of this class".
     pub(crate) fn metaclass_instance_type(self, db: &'db dyn Db) -> Type<'db> {
-        match self {
-            Self::Static(class) => class.metaclass_instance_type(db),
-            Self::Dynamic(class) => class.metaclass(db),
-        }
+        self.metaclass(db)
+            .to_instance(db)
+            .expect("`Type::to_instance()` should always return `Some()` when called on the type of a metaclass")
     }
 
     /// Returns whether this class is type-check only.
@@ -2585,14 +2584,6 @@ impl<'db> StaticClassLiteral<'db> {
             .unwrap_or_else(|_| SubclassOfType::subclass_of_unknown())
     }
 
-    /// Return a type representing "the set of all instances of the metaclass of this class".
-    pub(super) fn metaclass_instance_type(self, db: &'db dyn Db) -> Type<'db> {
-        self
-            .metaclass(db)
-            .to_instance(db)
-            .expect("`Type::to_instance()` should always return `Some()` when called on the type of a metaclass")
-    }
-
     /// Return the metaclass of this class, or an error if the metaclass cannot be inferred.
     #[salsa::tracked(cycle_initial=try_metaclass_cycle_initial,
         heap_size=ruff_memory_usage::heap_size,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:634 on 2026-01-11 12:02_

It's fine not to support dynamic classes being marked as deprecated, but in that case we should probably emit a diagnostic if somebody tries to do something like

```py
from warnings import deprecated

X = deprecated(type("X", (), {}))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:641 on 2026-01-11 12:02_

same here, I guess ideally we'd emit a diagnostic for this if we don't want to support it

```py
from typing import final

X = final(type("X", (), {}))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-11 12:06_

We should also emit a diagnostic (but don't currently on this branch) if you try to use an `@final` class as the base class for a dynamic class, e.g.

```py
from typing import final

@final
class Base: ...

type("X", (Base,), {})
```

This is maybe a counter-argument to https://github.com/astral-sh/ruff/pull/22291/files#r2678657516 -- if we looped through all dynamic class definitions in `TypeInferenceBuilder::check_class_definitions` as well as all the static class definitions, it might have been harder to forget to implement this check... but I'm guess there's a lot of stuff in that method that currently assumes that we have a `StmtClassDef` AST node, so it may be that what you have right now is the best design.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1742 on 2026-01-11 12:09_

It seems like we should also emit a diagnostic for

```py
from ty_extensions import get_protocol_members

get_protocol_members(type("X", (), {}))
```

?

But this isn't high-priority, since `get_protocol_members` is mostly an internal debugging tool

---

_Comment by @AlexWaygood on 2026-01-11 13:13_

@charliermarsh -- I pushed a commit fixing up and simplifying some of the `type()` expression parsing in `infer/builder.rs`, hope that's okay!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:668 on 2026-01-11 13:18_

Is the reason why this is a separate struct to `MroError` is because the kinds of errors that could occur for dynamic class definitions is only a subset of the kinds of errors that could occur for static class definitions? If so, maybe it's worth saying that in the struct doc-comment?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:682 on 2026-01-11 13:19_

```suggestion
/// These mirror the relevant variants from `MroErrorKind` for static classes.
```

---

_@AlexWaygood reviewed on 2026-01-11 13:22_

---

_Comment by @AlexWaygood on 2026-01-11 13:24_

From my perspective, I think this is basically ready to go now! My remaining concerns are https://github.com/astral-sh/ruff/pull/22291#discussion_r2677500504 (which I'd like @MichaReiser's or @carljm's opinion on) and https://github.com/astral-sh/ruff/pull/22291#discussion_r2679453580.

---

_@charliermarsh reviewed on 2026-01-11 13:42_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-11 13:42_

I believe I did this in https://github.com/astral-sh/ruff/pull/22499. I can revisit the approach in that review (RE whether it's worth using check_class_definitions), if that works?

---

_@AlexWaygood reviewed on 2026-01-11 13:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-11 13:48_

yeah sure!

---

_@AlexWaygood reviewed on 2026-01-11 14:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:643 on 2026-01-11 14:08_

```suggestion
    # TODO: `type[Unknown]` would cause fewer false positives
    reveal_type(B)  # revealed: <class 'str'>

    # Has string and tuple, but unknown additional args
    C = type("C", (), *args, **kwargs)
    # TODO: `type[Unknown]` would cause fewer false positives
    reveal_type(C)  # revealed: type

    # All three positional args provided, only **kwargs unknown
    D = type("D", (), {}, **kwargs)
    # TODO: `type[Unknown]` would cause fewer false positives
    reveal_type(D)  # revealed: type
```

---

_@AlexWaygood reviewed on 2026-01-11 14:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:633 on 2026-01-11 14:28_

I think here as well, `type[Unknown]` would cause fewer false positives (so I'd add a TODO here) -- it's ambiguous here whether we should pick the first overload (which would lead to us inferring `<class 'str'>` or the second overload (which would lead to us inferring a dynamic class-literal type). But either `<class 'str'>` or a dynamic class-literal type are both assignable to `type[Unknown]`, and it's a forgiving type that allows you to access most attributes on it.

---

_@charliermarsh reviewed on 2026-01-11 14:54_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:633 on 2026-01-11 14:54_

(Adding TODOs, going to tackle these in a separate PR.)

---

_@MichaReiser reviewed on 2026-01-12 15:10_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 15:10_

Including `NodeIndex` here is inherently problematic for incrementality. The `NodeIndex` changes if any new node is inserted before the class definition, which results in a new interned `StaticClassLiteral`. The result of this is that any cached data on the `StaticClassLiteral`s identity goes out the window when someone make changes above the `type` call expression (even in entirely unrelated scopes). So this is roughly as bad as holding on to the AST node itself (from an incrementality perspective).


The way we normally avoid this is by holding on to the `Definition` which gives access to the AST node (but in a way that doesn't affect the ID of `Definition`). 

What do we use this information for? Could we compute it using `definition` instead?

Alex is also right that we should avoid reading this field in cross-module queries, but, unfortunately, the problem is even more severe.


---

_@charliermarsh reviewed on 2026-01-12 15:20_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 15:20_

We can't really use `Definition` here, since these can be created without a `Definition` -- they don't always bind to a name. For example:

```python
class Url(
    typing.NamedTuple(
        "Url",
        [
            ("scheme", typing.Optional[str]),
            ("auth", typing.Optional[str]),
            ("host", typing.Optional[str]),
            ("port", typing.Optional[int]),
            ("path", typing.Optional[str]),
            ("query", typing.Optional[str]),
            ("fragment", typing.Optional[str]),
        ],
    )
):
    ...
```

The dynamically-created `typing.NamedTuple` doesn't have a definition.

---

_@charliermarsh reviewed on 2026-01-12 15:21_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 15:21_

These are, admittedly, rare though -- in case that affects how we'd think about the performance impact.

---

_@charliermarsh reviewed on 2026-01-12 15:22_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:439 on 2026-01-12 15:22_

(I added this! It's a separate diagnostic, disabled by default.)

---

_@MichaReiser reviewed on 2026-01-12 15:28_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 15:28_

I'll try to have a look tomorrow. What do we use it for? Is it for diagnostics?

We could also consider to create `Definition`s for those always but I assume the problem is that we don't know the `NamedTuple` until it is too late. 

I've to look at where we intern this dynamic types. An alternative would be to use a tracked struct over an interned struct because tracked struct's allow you to exclude a field from its identity. However, their identity is also defined differently. It is based on value equality and by which query created the tracked struct. That means, two `NamedTuple` with exact the same values but from different call sites would resolve to different salsa ids. I don't know if that's a problem.

Although I'm slightly scared that they're broken in fixpoint iteration. 

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 15:28_

Let me see if I could store an enum with `Expression` here instead for those use-cases?

---

_@charliermarsh reviewed on 2026-01-12 15:28_

---

_@AlexWaygood reviewed on 2026-01-12 15:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:845 on 2026-01-12 15:28_

You need to add the new error code to https://github.com/astral-sh/ruff/blob/e4ba29392bb768965ba16ba86aba7d482e4f2ea8/.github/mypy-primer-ty.toml#L4-L8 or it'll be disabled by default in mypy_primer/ecosystem-analyzer runs.

Also, could we state clearly in the docs why it's disabled by default? (Because it won't fail at runtime, and may be noisy on codebases that are using `type()` in highly dynamic ways.)

You also need to run `cargo dev generate-all` to regenerate `ty.schema.json` and the generated docs files.

---

_@AlexWaygood reviewed on 2026-01-12 15:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 15:34_

> We could also consider to create `Definition`s for those always but I assume the problem is that we don't know the `NamedTuple` until it is too late.

I think it would probably be too expensive to create a `Definition` for every call expression, but it might be possible to only create `Definition`s for call expressions inside the `ast::Argument`s for `class` statements? That would probably cover almost all use cases.

@charliermarsh I also think it would be fine to defer "inline" dynamic classes for now -- this PR is huge! We should probably focus on getting something over the line at this point, so that we can iterate on it. Saying that we currently only support `type()` classes when they're on the right-hand side of an assignment statement seems like a reasonable way to cut scope for now.

---

_Comment by @charliermarsh on 2026-01-12 17:08_

I dropped support for "dangling" `type(...)` expressions for now. I attempted to retain all existing tests with appropriate TODOs.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:613 on 2026-01-12 17:26_

```suggestion
from ty_extensions import reveal_mro

class Base: ...

# Unpacking a tuple for bases
bases_tuple = (Base,)
Cls1 = type("Cls1", (*bases_tuple,), {})
reveal_type(Cls1)  # revealed: <class 'Cls1'>
reveal_mro(Cls1)  # revealed: (<class 'Cls1'>, @Todo(StarredExpression), <class 'object'>)
```

(The `@Todo` type will be fixed by https://github.com/astral-sh/ruff/pull/22503)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:679 on 2026-01-12 17:34_

could be useful to add a `reveal_mro` call here too

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:696 on 2026-01-12 17:34_

and here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:710 on 2026-01-12 17:34_

and here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:846 on 2026-01-12 17:46_

It could also be good to say explicitly that this is the same rule as `unsupported-base`, but applied to classes created using `type()` rather than classes created using `class` statements

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6051 on 2026-01-12 17:48_

we may want to check whether any of these were starred expressions and return `None` if so, e.g.

```py
a = (1, 2, 3)
b = (3, 4, 5)
c = (6, 7, 8)

X = type(*a, *b, *c)
```

---

_@AlexWaygood approved on 2026-01-12 17:49_

LGTM! (But maybe give @carljm a chance to have another once-over if he wants to, before landing 😄)

---

_@charliermarsh reviewed on 2026-01-12 17:50_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 17:50_

> Including NodeIndex here is inherently problematic for incrementality. The NodeIndex changes if any new node is inserted before the class definition, which results in a new interned StaticClassLiteral. The result of this is that any cached data on the StaticClassLiterals identity goes out the window when someone make changes above the type call expression (even in entirely unrelated scopes). So this is roughly as bad as holding on to the AST node itself (from an incrementality perspective).

As an aside: I think this comment is using `StaticClassLiteral` when it should be `DynamicClassLiteral`?

---

_@AlexWaygood reviewed on 2026-01-12 17:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-12 17:53_

yes

---
