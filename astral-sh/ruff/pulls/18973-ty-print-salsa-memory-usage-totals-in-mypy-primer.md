```yaml
number: 18973
title: "[ty] Print salsa memory usage totals in mypy primer CI runs"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: ibraheem/memory-usage-dump
created_at: 2025-06-27T03:15:26Z
updated_at: 2025-06-28T19:09:52Z
url: https://github.com/astral-sh/ruff/pull/18973
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Print salsa memory usage totals in mypy primer CI runs

---

_Pull request opened by @ibraheemdev on 2025-06-27 03:15_

## Summary

Print the [new salsa memory usage dumps](https://github.com/astral-sh/ruff/pull/18928) in mypy primer CI runs to help us catch memory regressions. The numbers are rounded to the nearest 100 to avoid overly sensitive diffs. I don't really have a good sense for whether this is still too sensitive or maybe not sensitive enough. It also likely depends on the scale of the numbers, but rounding to the next power-of-2 felt like losing too much precision.

I'm also not sure about the exact numbers we want to expose, or if the current breakdown is too detailed. @MichaReiser mentioned cycle counts?

---

_Review requested from @carljm by @ibraheemdev on 2025-06-27 03:15_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-06-27 03:15_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-06-27 03:15_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-06-27 03:15_

---

_Review requested from @dcreager by @ibraheemdev on 2025-06-27 03:15_

---

_Label `internal` added by @ibraheemdev on 2025-06-27 03:15_

---

_Label `ty` added by @ibraheemdev on 2025-06-27 03:15_

---

_Comment by @github-actions[bot] on 2025-06-27 04:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~19MB
+     struct metadata = ~0MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~17MB

dacite (https://github.com/konradhalas/dacite)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~19MB
+     struct metadata = ~0MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~15MB

git-revise (https://github.com/mystor/git-revise)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~37MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~2MB
+     memo fields = ~34MB

janus (https://github.com/aio-libs/janus)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~28MB
+     struct metadata = ~0MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~23MB

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~49MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~45MB

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~49MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~3MB
+     memo fields = ~41MB

async-utils (https://github.com/mikeshardmind/async-utils)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~45MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~41MB

parso (https://github.com/davidhalter/parso)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~60MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~4MB
+     memo fields = ~49MB

pyp (https://github.com/hauntsaninja/pyp)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~45MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~41MB

zipp (https://github.com/jaraco/zipp)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~30MB
+     struct metadata = ~0MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~25MB

com2ann (https://github.com/ilevkivskyi/com2ann)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~41MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~34MB

paroxython (https://github.com/laowantong/paroxython)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~49MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~41MB

python-chess (https://github.com/niklasf/python-chess)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~8MB
+     memo fields = ~72MB

nionutils (https://github.com/nion-software/nionutils)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~66MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~5MB
+     memo fields = ~54MB

beartype (https://github.com/beartype/beartype)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~3MB
+     struct fields = ~4MB
+     memo metadata = ~9MB
+     memo fields = ~72MB

pegen (https://github.com/we-like-parsers/pegen)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~45MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~37MB

anyio (https://github.com/agronholm/anyio)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~4MB
+     memo metadata = ~6MB
+     memo fields = ~80MB

attrs (https://github.com/python-attrs/attrs)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~80MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~7MB
+     memo fields = ~60MB

aioredis (https://github.com/aio-libs/aioredis)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~66MB
+     struct metadata = ~1MB
+     struct fields = ~3MB
+     memo metadata = ~3MB
+     memo fields = ~60MB

more-itertools (https://github.com/more-itertools/more-itertools)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~34MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~3MB
+     memo fields = ~25MB

Expression (https://github.com/cognitedata/Expression)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~66MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~6MB
+     memo fields = ~54MB

pyinstrument (https://github.com/joerick/pyinstrument)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~66MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~4MB
+     memo fields = ~54MB

yarl (https://github.com/aio-libs/yarl)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~60MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~5MB
+     memo fields = ~49MB

koda-validate (https://github.com/keithasaurus/koda-validate)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~37MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~3MB
+     memo fields = ~30MB

operator (https://github.com/canonical/operator)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~106MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~8MB
+     memo fields = ~88MB

aiortc (https://github.com/aiortc/aiortc)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~6MB
+     memo fields = ~80MB

python-htmlgen (https://github.com/srittau/python-htmlgen)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~30MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~3MB
+     memo fields = ~25MB

graphql-core (https://github.com/graphql-python/graphql-core)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~156MB
+     struct metadata = ~6MB
+     struct fields = ~8MB
+     memo metadata = ~21MB
+     memo fields = ~129MB

dedupe (https://github.com/dedupeio/dedupe)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~80MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~5MB
+     memo fields = ~72MB

pyjwt (https://github.com/jpadilla/pyjwt)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~34MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~2MB
+     memo fields = ~28MB

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~80MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~3MB
+     memo fields = ~66MB

black (https://github.com/psf/black)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~142MB
+     struct metadata = ~4MB
+     struct fields = ~6MB
+     memo metadata = ~8MB
+     memo fields = ~117MB

svcs (https://github.com/hynek/svcs)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~80MB
+     struct metadata = ~1MB
+     struct fields = ~3MB
+     memo metadata = ~2MB
+     memo fields = ~66MB

comtypes (https://github.com/enthought/comtypes)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~117MB
+     struct metadata = ~4MB
+     struct fields = ~6MB
+     memo metadata = ~10MB
+     memo fields = ~97MB

starlette (https://github.com/encode/starlette)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~4MB
+     memo metadata = ~9MB
+     memo fields = ~80MB

dulwich (https://github.com/dulwich/dulwich)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~142MB
+     struct metadata = ~5MB
+     struct fields = ~7MB
+     memo metadata = ~14MB
+     memo fields = ~117MB

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~41MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~2MB
+     memo fields = ~34MB

alerta (https://github.com/alerta/alerta)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~117MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~9MB
+     memo fields = ~97MB

downforeveryone (https://github.com/rpdelaney/downforeveryone)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~37MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~34MB

kopf (https://github.com/nolar/kopf)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~4MB
+     memo metadata = ~8MB
+     memo fields = ~80MB

rich (https://github.com/Textualize/rich)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~142MB
+     struct metadata = ~5MB
+     struct fields = ~7MB
+     memo metadata = ~14MB
+     memo fields = ~106MB

trio (https://github.com/python-trio/trio)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~189MB
+     struct metadata = ~7MB
+     struct fields = ~8MB
+     memo metadata = ~19MB
+     memo fields = ~156MB

httpx-caching (https://github.com/johtso/httpx-caching)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~66MB
+     struct metadata = ~1MB
+     struct fields = ~3MB
+     memo metadata = ~2MB
+     memo fields = ~60MB

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~41MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~37MB

kornia (https://github.com/kornia/kornia)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~189MB
+     struct metadata = ~6MB
+     struct fields = ~8MB
+     memo metadata = ~23MB
+     memo fields = ~156MB

strawberry (https://github.com/strawberry-graphql/strawberry)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~129MB
+     struct metadata = ~5MB
+     struct fields = ~6MB
+     memo metadata = ~13MB
+     memo fields = ~106MB

pydantic (https://github.com/pydantic/pydantic)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~156MB
+     struct metadata = ~6MB
+     struct fields = ~7MB
+     memo metadata = ~17MB
+     memo fields = ~117MB

imagehash (https://github.com/JohannesBuchner/imagehash)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~49MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~1MB
+     memo fields = ~41MB

sockeye (https://github.com/awslabs/sockeye)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~9MB
+     memo fields = ~80MB

porcupine (https://github.com/Akuli/porcupine)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~117MB
+     struct metadata = ~4MB
+     struct fields = ~6MB
+     memo metadata = ~8MB
+     memo fields = ~97MB

nox (https://github.com/wntrblm/nox)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~80MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~4MB
+     memo fields = ~72MB

pybind11 (https://github.com/pybind/pybind11)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~8MB
+     memo fields = ~80MB

PyGithub (https://github.com/PyGithub/PyGithub)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~156MB
+     struct metadata = ~5MB
+     struct fields = ~7MB
+     memo metadata = ~17MB
+     memo fields = ~129MB

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~49MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~41MB

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~41MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~37MB

ppb-vector (https://github.com/ppb/ppb-vector)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~37MB
+     struct metadata = ~1MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~34MB

stone (https://github.com/dropbox/stone)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~3MB
+     struct fields = ~4MB
+     memo metadata = ~8MB
+     memo fields = ~72MB

websockets (https://github.com/aaugustin/websockets)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~6MB
+     memo fields = ~80MB

pylox (https://github.com/sco1/pylox)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~60MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~4MB
+     memo fields = ~54MB

mkosi (https://github.com/systemd/mkosi)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~129MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~9MB
+     memo fields = ~106MB

flake8 (https://github.com/pycqa/flake8)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~72MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~5MB
+     memo fields = ~66MB

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~72MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~4MB
+     memo fields = ~60MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~5MB
+     memo fields = ~72MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~207MB
+     struct metadata = ~7MB
+     struct fields = ~9MB
+     memo metadata = ~21MB
+     memo fields = ~171MB

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~106MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~4MB
+     memo fields = ~88MB

ignite (https://github.com/pytorch/ignite)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~207MB
+     struct metadata = ~7MB
+     struct fields = ~9MB
+     memo metadata = ~25MB
+     memo fields = ~171MB

asynq (https://github.com/quora/asynq)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~66MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~4MB
+     memo fields = ~54MB

poetry (https://github.com/python-poetry/poetry)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~207MB
+     struct metadata = ~7MB
+     struct fields = ~9MB
+     memo metadata = ~23MB
+     memo fields = ~171MB

schemathesis (https://github.com/schemathesis/schemathesis)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~156MB
+     struct metadata = ~6MB
+     struct fields = ~8MB
+     memo metadata = ~14MB
+     memo fields = ~129MB

xarray-dataclasses (https://github.com/astropenguin/xarray-dataclasses)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~1MB
+     struct metadata = ~0MB
+     struct fields = ~0MB
+     memo metadata = ~0MB
+     memo fields = ~1MB

pwndbg (https://github.com/pwndbg/pwndbg)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~207MB
+     struct metadata = ~7MB
+     struct fields = ~8MB
+     memo metadata = ~21MB
+     memo fields = ~171MB

mkdocs (https://github.com/mkdocs/mkdocs)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~129MB
+     struct metadata = ~4MB
+     struct fields = ~6MB
+     memo metadata = ~9MB
+     memo fields = ~106MB

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~3MB
+     struct fields = ~4MB
+     memo metadata = ~6MB
+     memo fields = ~72MB

dragonchain (https://github.com/dragonchain/dragonchain)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~8MB
+     memo fields = ~80MB

cloud-init (https://github.com/canonical/cloud-init)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~368MB
+     struct metadata = ~15MB
+     struct fields = ~19MB
+     memo metadata = ~45MB
+     memo fields = ~304MB

vision (https://github.com/pytorch/vision)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~368MB
+     struct metadata = ~13MB
+     struct fields = ~17MB
+     memo metadata = ~37MB
+     memo fields = ~276MB

alectryon (https://github.com/cpitclaudel/alectryon)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~14MB
+     struct metadata = ~0MB
+     struct fields = ~0MB
+     memo metadata = ~0MB
+     memo fields = ~13MB

urllib3 (https://github.com/urllib3/urllib3)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~156MB
+     struct metadata = ~5MB
+     struct fields = ~8MB
+     memo metadata = ~14MB
+     memo fields = ~129MB

apprise (https://github.com/caronc/apprise)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~304MB
+     struct metadata = ~9MB
+     struct fields = ~11MB
+     memo metadata = ~41MB
+     memo fields = ~251MB

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~117MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~4MB
+     memo fields = ~97MB

itsdangerous (https://github.com/pallets/itsdangerous)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~19MB
+     struct metadata = ~0MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~15MB

isort (https://github.com/pycqa/isort)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~5MB
+     memo fields = ~72MB

twine (https://github.com/pypa/twine)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~60MB
+     struct metadata = ~1MB
+     struct fields = ~3MB
+     memo metadata = ~2MB
+     memo fields = ~54MB

scrapy (https://github.com/scrapy/scrapy)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~251MB
+     struct metadata = ~9MB
+     struct fields = ~13MB
+     memo metadata = ~28MB
+     memo fields = ~189MB

pywin32 (https://github.com/mhammond/pywin32)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~445MB
+     struct metadata = ~19MB
+     struct fields = ~28MB
+     memo metadata = ~49MB
+     memo fields = ~368MB

python-sop (https://gitlab.com/dkg/python-sop)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~28MB
+     struct metadata = ~0MB
+     struct fields = ~1MB
+     memo metadata = ~1MB
+     memo fields = ~25MB

jinja (https://github.com/pallets/jinja)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~97MB
+     struct metadata = ~3MB
+     struct fields = ~5MB
+     memo metadata = ~9MB
+     memo fields = ~80MB

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~142MB
+     struct metadata = ~5MB
+     struct fields = ~7MB
+     memo metadata = ~13MB
+     memo fields = ~117MB

colour (https://github.com/colour-science/colour)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~405MB
+     struct metadata = ~13MB
+     struct fields = ~19MB
+     memo metadata = ~37MB
+     memo fields = ~334MB

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~106MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~5MB
+     memo fields = ~88MB

bandersnatch (https://github.com/pypa/bandersnatch)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~88MB
+     struct metadata = ~2MB
+     struct fields = ~4MB
+     memo metadata = ~6MB
+     memo fields = ~72MB

pyppeteer (https://github.com/pyppeteer/pyppeteer)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~80MB
+     struct metadata = ~2MB
+     struct fields = ~3MB
+     memo metadata = ~4MB
+     memo fields = ~72MB

paasta (https://github.com/yelp/paasta)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~207MB
+     struct metadata = ~8MB
+     struct fields = ~10MB
+     memo metadata = ~21MB
+     memo fields = ~156MB

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~276MB
+     struct metadata = ~8MB
+     struct fields = ~13MB
+     memo metadata = ~21MB
+     memo fields = ~228MB

altair (https://github.com/vega/altair)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~276MB
+     struct metadata = ~9MB
+     struct fields = ~14MB
+     memo metadata = ~23MB
+     memo fields = ~251MB

pytest (https://github.com/pytest-dev/pytest)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~251MB
+     struct metadata = ~9MB
+     struct fields = ~13MB
+     memo metadata = ~30MB
+     memo fields = ~189MB

freqtrade (https://github.com/freqtrade/freqtrade)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~334MB
+     struct metadata = ~10MB
+     struct fields = ~17MB
+     memo metadata = ~25MB
+     memo fields = ~276MB

werkzeug (https://github.com/pallets/werkzeug)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~171MB
+     struct metadata = ~6MB
+     struct fields = ~8MB
+     memo metadata = ~14MB
+     memo fields = ~142MB

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~304MB
+     struct metadata = ~11MB
+     struct fields = ~14MB
+     memo metadata = ~34MB
+     memo fields = ~228MB

AutoSplit (https://github.com/Toufool/AutoSplit)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~228MB
+     struct metadata = ~6MB
+     struct fields = ~11MB
+     memo metadata = ~8MB
+     memo fields = ~189MB

rclip (https://github.com/yurijmikhalevich/rclip)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~54MB
+     struct metadata = ~1MB
+     struct fields = ~2MB
+     memo metadata = ~2MB
+     memo fields = ~49MB

psycopg (https://github.com/psycopg/psycopg)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~228MB
+     struct metadata = ~8MB
+     struct fields = ~10MB
+     memo metadata = ~23MB
+     memo fields = ~171MB

optuna (https://github.com/optuna/optuna)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~334MB
+     struct metadata = ~10MB
+     struct fields = ~15MB
+     memo metadata = ~28MB
+     memo fields = ~276MB

django-stubs (https://github.com/typeddjango/django-stubs)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~189MB
+     struct metadata = ~6MB
+     struct fields = ~9MB
+     memo metadata = ~21MB
+     memo fields = ~142MB

cwltool (https://github.com/common-workflow-language/cwltool)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~251MB
+     struct metadata = ~8MB
+     struct fields = ~11MB
+     memo metadata = ~17MB
+     memo fields = ~228MB

openlibrary (https://github.com/internetarchive/openlibrary)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~228MB
+     struct metadata = ~8MB
+     struct fields = ~10MB
+     memo metadata = ~23MB
+     memo fields = ~171MB

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~142MB
+     struct metadata = ~6MB
+     struct fields = ~8MB
+     memo metadata = ~17MB
+     memo fields = ~106MB

discord.py (https://github.com/Rapptz/discord.py)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~251MB
+     struct metadata = ~8MB
+     struct fields = ~11MB
+     memo metadata = ~25MB
+     memo fields = ~207MB

sphinx (https://github.com/sphinx-doc/sphinx)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~304MB
+     struct metadata = ~10MB
+     struct fields = ~14MB
+     memo metadata = ~34MB
+     memo fields = ~228MB

bokeh (https://github.com/bokeh/bokeh)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~251MB
+     struct metadata = ~9MB
+     struct fields = ~13MB
+     memo metadata = ~25MB
+     memo fields = ~207MB

aiohttp (https://github.com/aio-libs/aiohttp)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~142MB
+     struct metadata = ~5MB
+     struct fields = ~6MB
+     memo metadata = ~11MB
+     memo fields = ~117MB

static-frame (https://github.com/static-frame/static-frame)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~405MB
+     struct metadata = ~14MB
+     struct fields = ~19MB
+     memo metadata = ~49MB
+     memo fields = ~304MB

streamlit (https://github.com/streamlit/streamlit)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~304MB
+     struct metadata = ~11MB
+     struct fields = ~14MB
+     memo metadata = ~30MB
+     memo fields = ~251MB

meson (https://github.com/mesonbuild/meson)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~405MB
+     struct metadata = ~15MB
+     struct fields = ~19MB
+     memo metadata = ~54MB
+     memo fields = ~334MB

prefect (https://github.com/PrefectHQ/prefect)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~652MB
+     struct metadata = ~23MB
+     struct fields = ~30MB
+     memo metadata = ~66MB
+     memo fields = ~539MB

rotki (https://github.com/rotki/rotki)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~593MB
+     struct metadata = ~23MB
+     struct fields = ~25MB
+     memo metadata = ~80MB
+     memo fields = ~445MB

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~789MB
+     struct metadata = ~30MB
+     struct fields = ~37MB
+     memo metadata = ~106MB
+     memo fields = ~593MB

zulip (https://github.com/zulip/zulip)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~789MB
+     struct metadata = ~28MB
+     struct fields = ~37MB
+     memo metadata = ~97MB
+     memo fields = ~652MB

manticore (https://github.com/trailofbits/manticore)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~652MB
+     struct metadata = ~21MB
+     struct fields = ~30MB
+     memo metadata = ~80MB
+     memo fields = ~539MB

materialize (https://github.com/MaterializeInc/materialize)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~539MB
+     struct metadata = ~17MB
+     struct fields = ~28MB
+     memo metadata = ~37MB
+     memo fields = ~445MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~652MB
+     struct metadata = ~23MB
+     struct fields = ~30MB
+     memo metadata = ~80MB
+     memo fields = ~539MB

scipy (https://github.com/scipy/scipy)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~1271MB
+     struct metadata = ~41MB
+     struct fields = ~54MB
+     memo metadata = ~156MB
+     memo fields = ~955MB

sympy (https://github.com/sympy/sympy)
+ =======SALSA SUMMARY=======
+ TOTAL MEMORY USAGE: ~1862MB
+     struct metadata = ~54MB
+     struct fields = ~66MB
+     memo metadata = ~276MB
+     memo fields = ~1538MB

```
</details>


---

_Comment by @MichaReiser on 2025-06-27 05:59_

The code looks good to me but I think the numbers aren't very meaningful anymore. Could we use some form of bucketing (based on magnitude, otherwise based on their size) and then output the result in MB or GB (2 digits?). Maybe something like this

```py
def scalable_memory_round(mb_value):
    """
    Memory rounding that scales properly into GB range
    """
    if mb_value == 0:
        return 0
    elif mb_value < 1:
        return 1                          # 1MB minimum
    elif mb_value < 5:
        return round(mb_value)            # 1MB precision
    elif mb_value < 50:
        return round(mb_value / 5) * 5    # 5MB precision  
    elif mb_value < 200:
        return round(mb_value / 10) * 10  # 10MB precision
    elif mb_value < 1000:
        return round(mb_value / 25) * 25  # 25MB precision
    else:                 # 1-5GB range
        return round(mb_value / 100) * 100  # 100MB precision

# Test with GB-range values:
test_values = [
    # Small values
    0.5, 2.3, 8.7, 15.2, 47.8, 156.3, 
    # Medium values  
    300, 750, 1500,
    # GB range values
    1200, 1850, 2100, 3500, 5200, 8750, 12000, 25000
]

print("Original → Rounded")
for val in test_values:
    rounded = scalable_memory_round(val)
    gb_orig = val / 1000
    gb_rounded = rounded / 1000
    print(f"{val:6.1f}MB ({gb_orig:4.1f}GB) → {rounded:6.0f}MB ({gb_rounded:4.1f}GB)")
```

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-27 07:07_

---

_Label `internal` removed by @MichaReiser on 2025-06-27 07:24_

---

_Label `testing` added by @MichaReiser on 2025-06-27 07:24_

---

_Comment by @sharkdp on 2025-06-27 07:32_

> Could we use some form of bucketing (based on magnitude, otherwise based on their size) and then output the result in MB or GB (2 digits?). Maybe something like this

I guess the generalization of this idea would be logarithmic rounding: `2 ** round(math.log2(mb_value))`.

In general, all of these approaches can still lead to noisy diffs, though. Even if the memory usage increases just a tiny bit, we can always fall into the next rounding bucket. So what we would really want to do instead of diffing the rounded results, would be to build the actual difference and compare that to the total. And then warn if |new - old|/new exceeds some threshold, in both directions. It's not possible to do this using mypy_primer, but it's plausible that we could build this into ecosystem-analyzer. We talked about runtime diffs in the past as well. I think it's perfectly fine to start with this, though.

---

_Comment by @MichaReiser on 2025-06-27 07:56_

> Even if the memory usage increases just a tiny bit, we can always fall into the next rounding bucket.

Yes, that's true. Our goal isn't to avoid noise entirely. We primarily aim to find an approach where projects keep falling into the same bucket unless there's a real regression. 

I also reached out to the codspeed team to understand if they have a way to surface custom benchmark metrics. Unfortunately, they don't but I think that would be the ideal solution (because it also allows us to track the value over time)

---

_Comment by @ibraheemdev on 2025-06-27 14:16_

> I guess the generalization of this idea would be logarithmic rounding: 2 ** round(math.log2(mb_value)).

We could use a fractional base and round-to-nearest to have a percentage threshold in either direction, but I'm not sure what a good percentage is. Maybe 5%? It has the same bucketing problem but the numbers should be more useful.

---

_Closed by @ibraheemdev on 2025-06-27 14:16_

---

_Reopened by @ibraheemdev on 2025-06-27 14:16_

---

_Comment by @MichaReiser on 2025-06-27 14:18_

> We could use a fractional base and round-to-nearest to have a percentage threshold in either direction, but I'm not sure what a good percentage is. Maybe 5%?

I suggest picking something and then seeing how noisy it turns out to be in practice. This is something we can easily iterate on.

---

_Review request for @carljm removed by @carljm on 2025-06-27 14:46_

---

_@MichaReiser approved on 2025-06-28 06:26_

---

_Merged by @ibraheemdev on 2025-06-28 19:09_

---

_Closed by @ibraheemdev on 2025-06-28 19:09_

---

_Branch deleted on 2025-06-28 19:09_

---
