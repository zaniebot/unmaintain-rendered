```yaml
number: 18089
title: "[ty] Change layout of extra verbose output and respect `--color` for verbose output"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: micha/logging
created_at: 2025-05-14T08:42:52Z
updated_at: 2025-05-15T07:58:01Z
url: https://github.com/astral-sh/ruff/pull/18089
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Change layout of extra verbose output and respect `--color` for verbose output

---

_@MichaReiser_

## Summary

This makes two improvements to our tracing logging:

1. It disables ANSI if stderr isn't a terminal or `--color never` is specified
2. It replaces `tracing-tree` with the pretty formatter. tracing-tree is prone to panicking during unwinding due to a poisoned lock (https://github.com/davidbarsky/tracing-tree/issues/85). I also find it doesn't render nicely once multi threading is involved and deeply nested spans can be hard to read. The pretty format is a bit more verbose but I sort of like it. I can spli this change out if it proves controversial

```
  2025-05-14T08:39:04.479874Z TRACE ty_python_semantic::module_resolver::resolver: Resolved module `typing` to `vendored://stdlib/typing.pyi`
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:47 on ThreadId(10)
    in ty_python_semantic::module_resolver::resolver::resolve_module with name=typing
    in ty_python_semantic::types::infer::infer_scope_types with scope=Id(1c00) file=File(System("/Users/micha/astral/test/tomllib/_types.py"))
    in ty_python_semantic::types::check_types with file=File(System("/Users/micha/astral/test/tomllib/_types.py"))
    in ty_project::check_file with file=file(Id(c06))
    in ty_project::Project::check

  2025-05-14T08:39:04.479889Z TRACE ruff_db::files: Adding file '/Users/micha/astral/test/__future__-stubs.py'
    at crates/ruff_db/src/files.rs:90 on ThreadId(4)
    in ty_python_semantic::module_resolver::resolver::resolve_module with name=__future__
    in ty_python_semantic::types::infer::infer_scope_types with scope=Id(2000) file=File(System("/Users/micha/astral/test/tomllib/_re.py"))
    in ty_python_semantic::types::check_types with file=File(System("/Users/micha/astral/test/tomllib/_re.py"))
    in ty_project::check_file with file=file(Id(c08))
    in ty_project::Project::check
```

## Test Plan

Verified that piping the logs to a file doesn't contain any ansi escpaes. Verified that the output is still colored when stderr isn't redirected


---

_Review requested from @carljm by @MichaReiser on 2025-05-14 08:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-14 08:42_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-14 08:42_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-14 08:42_

---

_Label `cli` added by @MichaReiser on 2025-05-14 08:44_

---

_Label `ty` added by @MichaReiser on 2025-05-14 08:44_

---

_Comment by @github-actions[bot] on 2025-05-14 08:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
janus (https://github.com/aio-libs/janus)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

com2ann (https://github.com/ilevkivskyi/com2ann)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dacite (https://github.com/konradhalas/dacite)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

parso (https://github.com/davidhalter/parso)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

zipp (https://github.com/jaraco/zipp)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

paroxython (https://github.com/laowantong/paroxython)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

async-utils (https://github.com/mikeshardmind/async-utils)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

nionutils (https://github.com/nion-software/nionutils)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pegen (https://github.com/we-like-parsers/pegen)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

python-chess (https://github.com/niklasf/python-chess)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

beartype (https://github.com/beartype/beartype)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyp (https://github.com/hauntsaninja/pyp)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyinstrument (https://github.com/joerick/pyinstrument)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

bidict (https://github.com/jab/bidict)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

more-itertools (https://github.com/more-itertools/more-itertools)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

git-revise (https://github.com/mystor/git-revise)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

python-htmlgen (https://github.com/srittau/python-htmlgen)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

attrs (https://github.com/python-attrs/attrs)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

python-sop (https://gitlab.com/dkg/python-sop)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

anyio (https://github.com/agronholm/anyio)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

starlette (https://github.com/encode/starlette)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aioredis (https://github.com/aio-libs/aioredis)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

kornia (https://github.com/kornia/kornia)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

koda-validate (https://github.com/keithasaurus/koda-validate)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyjwt (https://github.com/jpadilla/pyjwt)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

downforeveryone (https://github.com/rpdelaney/downforeveryone)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

kopf (https://github.com/nolar/kopf)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

httpx-caching (https://github.com/johtso/httpx-caching)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

itsdangerous (https://github.com/pallets/itsdangerous)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

alerta (https://github.com/alerta/alerta)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

strawberry (https://github.com/strawberry-graphql/strawberry)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

porcupine (https://github.com/Akuli/porcupine)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

graphql-core (https://github.com/graphql-python/graphql-core)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

ignite (https://github.com/pytorch/ignite)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pylox (https://github.com/sco1/pylox)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

trio (https://github.com/python-trio/trio)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pybind11 (https://github.com/pybind/pybind11)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

yarl (https://github.com/aio-libs/yarl)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

ppb-vector (https://github.com/ppb/ppb-vector)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mkosi (https://github.com/systemd/mkosi)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pydantic (https://github.com/pydantic/pydantic)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

imagehash (https://github.com/JohannesBuchner/imagehash)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

stone (https://github.com/dropbox/stone)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

flake8 (https://github.com/pycqa/flake8)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

websockets (https://github.com/aaugustin/websockets)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

black (https://github.com/psf/black)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

nox (https://github.com/wntrblm/nox)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

alectryon (https://github.com/cpitclaudel/alectryon)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

asynq (https://github.com/quora/asynq)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

sockeye (https://github.com/awslabs/sockeye)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

cki-lib (https://gitlab.com/cki-project/cki-lib)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

xarray-dataclasses (https://github.com/astropenguin/xarray-dataclasses)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

urllib3 (https://github.com/urllib3/urllib3)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

poetry (https://github.com/python-poetry/poetry)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

tornado (https://github.com/tornadoweb/tornado)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aiortc (https://github.com/aiortc/aiortc)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dragonchain (https://github.com/dragonchain/dragonchain)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

Expression (https://github.com/cognitedata/Expression)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

bandersnatch (https://github.com/pypa/bandersnatch)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

paasta (https://github.com/yelp/paasta)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

psycopg (https://github.com/psycopg/psycopg)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

schema_salad (https://github.com/common-workflow-language/schema_salad)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mkdocs (https://github.com/mkdocs/mkdocs)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

vision (https://github.com/pytorch/vision)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dulwich (https://github.com/dulwich/dulwich)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

werkzeug (https://github.com/pallets/werkzeug)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

twine (https://github.com/pypa/twine)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

isort (https://github.com/pycqa/isort)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

jinja (https://github.com/pallets/jinja)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

scrapy (https://github.com/scrapy/scrapy)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

colour (https://github.com/colour-science/colour)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyppeteer (https://github.com/pyppeteer/pyppeteer)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

comtypes (https://github.com/enthought/comtypes)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pwndbg (https://github.com/pwndbg/pwndbg)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

boostedblob (https://github.com/hauntsaninja/boostedblob)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dedupe (https://github.com/dedupeio/dedupe)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

altair (https://github.com/vega/altair)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

operator (https://github.com/canonical/operator)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

static-frame (https://github.com/static-frame/static-frame)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

meson (https://github.com/mesonbuild/meson)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mypy (https://github.com/python/mypy)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

optuna (https://github.com/optuna/optuna)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

cwltool (https://github.com/common-workflow-language/cwltool)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

arviz (https://github.com/arviz-devs/arviz)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

AutoSplit (https://github.com/Toufool/AutoSplit)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

rclip (https://github.com/yurijmikhalevich/rclip)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

PyGithub (https://github.com/PyGithub/PyGithub)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

django-stubs (https://github.com/typeddjango/django-stubs)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pytest (https://github.com/pytest-dev/pytest)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

openlibrary (https://github.com/internetarchive/openlibrary)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

rich (https://github.com/Textualize/rich)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

cloud-init (https://github.com/canonical/cloud-init)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

sphinx (https://github.com/sphinx-doc/sphinx)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

bokeh (https://github.com/bokeh/bokeh)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aiohttp (https://github.com/aio-libs/aiohttp)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

apprise (https://github.com/caronc/apprise)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

streamlit (https://github.com/streamlit/streamlit)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pycryptodome (https://github.com/Legrandin/pycryptodome)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

rotki (https://github.com/rotki/rotki)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

zulip (https://github.com/zulip/zulip)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

scipy (https://github.com/scipy/scipy)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

materialize (https://github.com/MaterializeInc/materialize)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

sympy (https://github.com/sympy/sympy)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

manticore (https://github.com/trailofbits/manticore)
- WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

```
</details>


---

_Comment by @github-actions[bot] on 2025-05-14 08:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @danielhollas on 2025-05-14 12:08_

> Verified that piping the logs to a file doesn't contain any ansi escpaes. Verified that the output is still colored when stderr isn't redirected

btw: In case this is unexpected I noticed that on the `-v` and `-vv` logging levels there's no color even when not redirected:

![image](https://github.com/user-attachments/assets/94d3924c-ed07-4dd8-8ef5-5aceacca297f)


---

_Comment by @Skylion007 on 2025-05-14 14:08_

> > Verified that piping the logs to a file doesn't contain any ansi escpaes. Verified that the output is still colored when stderr isn't redirected
> 
> btw: In case this is unexpected I noticed that on the `-v` and `-vv` logging levels there's no color even when not redirected:
> 
> ![image](https://private-user-images.githubusercontent.com/9539441/443648938-94d3924c-ed07-4dd8-8ef5-5aceacca297f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDcyMzE5MTMsIm5iZiI6MTc0NzIzMTYxMywicGF0aCI6Ii85NTM5NDQxLzQ0MzY0ODkzOC05NGQzOTI0Yy1lZDA3LTRkZDgtOGVmNS01YWNlYWNjYTI5N2YucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUxNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MTRUMTQwNjUzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzVkZjRhYTg4NGY4Nzc3ZGM3NTQ0MjQzZTA1MDJmZTZlYzU1Yzk0NzhjNTQxZmRhMDg2YjRiYzczMDhiOTY2OSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.at3XEN4t3Vf7lt_ZEfG7IgbnhlQqS8ZKeOr0XqEK0r8)

Does your env have CLICOLOR envvar set somewhere maybe?

---

_@carljm approved on 2025-05-14 20:01_

---

_Closed by @MichaReiser on 2025-05-15 07:08_

---

_Reopened by @MichaReiser on 2025-05-15 07:08_

---

_Renamed from "[ty] Logging improvements" to "[ty] Change extra-verbose log layout and respect `--color` option" by @MichaReiser on 2025-05-15 07:51_

---

_Renamed from "[ty] Change extra-verbose log layout and respect `--color` option" to "[ty] Change extra-verbose log layout and respect `--color` option for tracing output" by @MichaReiser on 2025-05-15 07:52_

---

_Renamed from "[ty] Change extra-verbose log layout and respect `--color` option for tracing output" to "[ty] Change layout of extra verbose output and respect `--color` for verbose output" by @MichaReiser on 2025-05-15 07:53_

---

_Merged by @MichaReiser on 2025-05-15 07:57_

---

_Closed by @MichaReiser on 2025-05-15 07:57_

---

_Branch deleted on 2025-05-15 07:58_

---
