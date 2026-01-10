```yaml
number: 17983
title: "[ty] Add a warning about pre-release status to the CLI"
type: pull_request
state: merged
author: ntBre
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: brent/ty-cli-warning
created_at: 2025-05-09T14:06:25Z
updated_at: 2025-05-09T17:42:37Z
url: https://github.com/astral-sh/ruff/pull/17983
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Add a warning about pre-release status to the CLI

---

_Pull request opened by @ntBre on 2025-05-09 14:06_

Summary
--

This was suggested on Discord, I hope this is roughly what we had in mind. I took the message from the ty README, but I'm more than happy to update it. Otherwise I just tried to mimic the appearance of the `ruff analyze graph` warning (although I'm realizing now the whole text is bold for ruff).

Test Plan
--

New warnings in the CLI tests. I thought this might be undesirable but it looks like uv did the same thing (https://github.com/astral-sh/uv/pull/6166).

![image](https://github.com/user-attachments/assets/e5e56a49-02ab-4c5f-9c38-716e4008d6e6)


---

_Label `cli` added by @ntBre on 2025-05-09 14:06_

---

_Label `ty` added by @ntBre on 2025-05-09 14:06_

---

_@ntBre reviewed on 2025-05-09 14:07_

---

_Review comment by @ntBre on `crates/ty/src/main.rs`:9 on 2025-05-09 14:07_

Do we need a scope here to drop the lock? It worked fine even if I returned an error from `run` but I wasn't sure.

---

_Comment by @github-actions[bot] on 2025-05-09 14:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

janus (https://github.com/aio-libs/janus)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

paroxython (https://github.com/laowantong/paroxython)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

com2ann (https://github.com/ilevkivskyi/com2ann)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

async-utils (https://github.com/mikeshardmind/async-utils)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

zipp (https://github.com/jaraco/zipp)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

parso (https://github.com/davidhalter/parso)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

bidict (https://github.com/jab/bidict)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

git-revise (https://github.com/mystor/git-revise)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

python-chess (https://github.com/niklasf/python-chess)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pegen (https://github.com/we-like-parsers/pegen)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

beartype (https://github.com/beartype/beartype)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyinstrument (https://github.com/joerick/pyinstrument)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

attrs (https://github.com/python-attrs/attrs)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

nionutils (https://github.com/nion-software/nionutils)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

python-sop (https://gitlab.com/dkg/python-sop)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

python-htmlgen (https://github.com/srittau/python-htmlgen)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyp (https://github.com/hauntsaninja/pyp)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

starlette (https://github.com/encode/starlette)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

more-itertools (https://github.com/more-itertools/more-itertools)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aioredis (https://github.com/aio-libs/aioredis)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

kornia (https://github.com/kornia/kornia)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyjwt (https://github.com/jpadilla/pyjwt)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

kopf (https://github.com/nolar/kopf)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

anyio (https://github.com/agronholm/anyio)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

koda-validate (https://github.com/keithasaurus/koda-validate)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

httpx-caching (https://github.com/johtso/httpx-caching)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

strawberry (https://github.com/strawberry-graphql/strawberry)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

trio (https://github.com/python-trio/trio)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

alerta (https://github.com/alerta/alerta)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pylox (https://github.com/sco1/pylox)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

porcupine (https://github.com/Akuli/porcupine)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

ppb-vector (https://github.com/ppb/ppb-vector)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

itsdangerous (https://github.com/pallets/itsdangerous)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

sockeye (https://github.com/awslabs/sockeye)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pybind11 (https://github.com/pybind/pybind11)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

ignite (https://github.com/pytorch/ignite)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

black (https://github.com/psf/black)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

nox (https://github.com/wntrblm/nox)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mkosi (https://github.com/systemd/mkosi)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

downforeveryone (https://github.com/rpdelaney/downforeveryone)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

flake8 (https://github.com/pycqa/flake8)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

imagehash (https://github.com/JohannesBuchner/imagehash)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pydantic (https://github.com/pydantic/pydantic)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

PyGithub (https://github.com/PyGithub/PyGithub)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dragonchain (https://github.com/dragonchain/dragonchain)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

tornado (https://github.com/tornadoweb/tornado)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

alectryon (https://github.com/cpitclaudel/alectryon)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

stone (https://github.com/dropbox/stone)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

isort (https://github.com/pycqa/isort)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

bandersnatch (https://github.com/pypa/bandersnatch)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

yarl (https://github.com/aio-libs/yarl)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mkdocs (https://github.com/mkdocs/mkdocs)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

asynq (https://github.com/quora/asynq)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

poetry (https://github.com/python-poetry/poetry)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

websockets (https://github.com/aaugustin/websockets)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

urllib3 (https://github.com/urllib3/urllib3)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

twine (https://github.com/pypa/twine)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

Expression (https://github.com/cognitedata/Expression)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pyppeteer (https://github.com/pyppeteer/pyppeteer)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

paasta (https://github.com/yelp/paasta)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

psycopg (https://github.com/psycopg/psycopg)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

jinja (https://github.com/pallets/jinja)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pwndbg (https://github.com/pwndbg/pwndbg)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dulwich (https://github.com/dulwich/dulwich)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

xarray-dataclasses (https://github.com/astropenguin/xarray-dataclasses)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

werkzeug (https://github.com/pallets/werkzeug)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

comtypes (https://github.com/enthought/comtypes)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

operator (https://github.com/canonical/operator)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dedupe (https://github.com/dedupeio/dedupe)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

vision (https://github.com/pytorch/vision)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aiortc (https://github.com/aiortc/aiortc)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

meson (https://github.com/mesonbuild/meson)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

arviz (https://github.com/arviz-devs/arviz)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mypy (https://github.com/python/mypy)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

scrapy (https://github.com/scrapy/scrapy)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

rich (https://github.com/Textualize/rich)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`

colour (https://github.com/colour-science/colour)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pytest (https://github.com/pytest-dev/pytest)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

graphql-core (https://github.com/graphql-python/graphql-core)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

AutoSplit (https://github.com/Toufool/AutoSplit)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

cwltool (https://github.com/common-workflow-language/cwltool)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

rclip (https://github.com/yurijmikhalevich/rclip)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

altair (https://github.com/vega/altair)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

cloud-init (https://github.com/canonical/cloud-init)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

optuna (https://github.com/optuna/optuna)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

apprise (https://github.com/caronc/apprise)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

static-frame (https://github.com/static-frame/static-frame)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

aiohttp (https://github.com/aio-libs/aiohttp)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

django-stubs (https://github.com/typeddjango/django-stubs)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

bokeh (https://github.com/bokeh/bokeh)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

sphinx (https://github.com/sphinx-doc/sphinx)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

openlibrary (https://github.com/internetarchive/openlibrary)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

streamlit (https://github.com/streamlit/streamlit)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

rotki (https://github.com/rotki/rotki)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

zulip (https://github.com/zulip/zulip)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

materialize (https://github.com/MaterializeInc/materialize)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

scipy (https://github.com/scipy/scipy)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

sympy (https://github.com/sympy/sympy)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

manticore (https://github.com/trailofbits/manticore)
+ WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

```
</details>


---

_@MichaReiser reviewed on 2025-05-09 14:10_

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:17 on 2025-05-09 14:10_

I'd suggest to use `tracing::warn!` here. It's what we use for other warning messages (similar to ruff's `warn!` macro)

---

_@ntBre reviewed on 2025-05-09 14:12_

---

_Review comment by @ntBre on `crates/ty/src/main.rs`:17 on 2025-05-09 14:12_

Sounds good. Do we still want the colors? My first draft was just this message in `tracing::warn` but it didn't look as nice.

And `setup_tracing` is called in `run_check`, so should I only warn in there?

---

_@MichaReiser reviewed on 2025-05-09 14:15_

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:17 on 2025-05-09 14:15_

Hmm, tracing warn should use colors.

https://github.com/astral-sh/ruff/blob/fa628018b28fa018d7a0e109223186172ef91275/crates/ty/src/logging.rs#L210-L220

Maybe not if you use `-vvv` 

---

_@ntBre reviewed on 2025-05-09 15:09_

---

_Review comment by @ntBre on `crates/ty/src/main.rs`:17 on 2025-05-09 15:09_

This is what I'm seeing without any `-v`:

![image](https://github.com/user-attachments/assets/32add1e0-e3d7-4f6e-8b02-aed5d6a064a5)

I actually _only_ see color on the warning (and other logging) with `-vvv`, not with any of the lower verbosity levels. Is that expected?

(updating the snapshots then I'll push the `tracing::warn` version)

---

_@MichaReiser reviewed on 2025-05-09 15:16_

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:17 on 2025-05-09 15:16_

It's all nice in orange for me :)

---

_@MichaReiser approved on 2025-05-09 15:16_

Thank you

---

_Marked ready for review by @ntBre on 2025-05-09 16:02_

---

_Review requested from @carljm by @ntBre on 2025-05-09 16:02_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-09 16:02_

---

_Review requested from @sharkdp by @ntBre on 2025-05-09 16:02_

---

_Review requested from @dcreager by @ntBre on 2025-05-09 16:02_

---

_Merged by @ntBre on 2025-05-09 17:42_

---

_Closed by @ntBre on 2025-05-09 17:42_

---

_Branch deleted on 2025-05-09 17:42_

---
