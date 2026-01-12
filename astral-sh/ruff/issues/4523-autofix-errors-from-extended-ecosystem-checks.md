```yaml
number: 4523
title: Autofix errors from extended ecosystem checks
type: issue
state: closed
author: konstin
labels:
  - fixes
assignees: []
created_at: 2023-05-19T09:14:20Z
updated_at: 2023-09-04T18:09:44Z
url: https://github.com/astral-sh/ruff/issues/4523
synced_at: 2026-01-12T15:54:44Z
```

# Autofix errors from extended ecosystem checks

---

_@konstin_

I wrote a script that tries to `--select ALL --fix` against a large number of repositories (#4326). Currently, it's ~3k repositories, even though a good fraction of those is forks.

Each entry is an autofix failure with the rules causing it, that means e.g. `ruff --select COM812,Q000,RUF001,UP009,UP025 --fix third_party/python/idna/idna/uts46data.py` in [bolucat/Firefox](https://github.com/bolucat/Firefox) will fail and print an error.

Each entry needs to be minimized to the specific rule and snippet that causes the failure and reported as an issue; I did this for the f-string failures (all fixed by #4487 and #4488) and for three of the ones below (#4519, #4520, #4522). I normally start by bisecting the rule codes until only one remains, then i look up what the code does and try to minimizing the file contents by removing regions that are likely unrelated to the rule until i finally play around with the remaining for to get the most minimal reproduction.

| Repository | File | Rules | Issue |
| --- | --- | --- | --- |
| bolucat/Firefox | third_party/python/idna/idna/uts46data.py | COM812,Q000,RUF001,UP009,UP025 | #4519 |
| bolucat/Firefox | third_party/python/pip/pip/_vendor/idna/uts46data.py | COM812,F401,I001,Q000,RUF001,UP006,UP007,UP009 | |
| demisto/content | Packs/FeedIntel471/Integrations/Intel471MalwareIndicator/Intel471MalwareIndicator.py | ANN204,COM812,D200,D212,D400,D406,D407,D410,D411,D415,EM101,EM102,F401,I001,Q000,RUF010,SIM102,UP006,UP007 | #4520 |
| demisto/content | Packs/FeedOffice365/Integrations/FeedOffice365/FeedOffice365.py | ANN204,COM812,D200,D212,D400,D406,D407,D411,D415,EM102,F401,Q000,RUF010,SIM201,UP006,UP007,UP035 | |
| demisto/content | Packs/SymantecEndpointProtection/Integrations/SymantecEndpointProtection_V2/SymantecEndpointProtection_V2.py | C417,COM812,I001,Q000,Q001,RET503,SIM118,SIM210,UP030,UP032,W605 | |
| harupy/ruff | crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/docstring.py | D200,D207,D208,D209,D210,D212,D400,D403,D415,PIE790,Q002,W291,W293,W605 | |
| lamoboos223/synapse-archive | env/lib/python3.10/site-packages/docutils/utils/math/math2html.py | ANN204,B007,C408,COM819,D204,D209,D212,D400,D415,E711,E713,E731,ERA001,F841,PIE790, PIE810,PLR1711,PLR1722,Q000,RET501,RET502,RET503,RUF001,RUF003,RUF005,SIM108,SIM201,UP004,UP009,UP025,UP036 | |
| lamoboos223/synapse-archive | env/lib/python3.10/site-packages/idna/uts46data.py | COM812,F401,I001,Q000,RUF001,UP006,UP007,UP009 | |
| lamoboos223/synapse-archive | env/lib/python3.10/site-packages/pip/_vendor/idna/uts46data.py | COM812,F401,I001,Q000,RUF001,UP006,UP007,UP009 | |
| lamoboos223/synapse-archive | env/lib/python3.10/site-packages/prometheus_client/decorator.py | ANN204,C408,D200,D202,D212,D400,D415,Q000,SIM108,UP004,UP010,UP036 | |
| lamoboos223/synapse-archive | env/lib/python3.10/site-packages/pydantic/typing.py | B010,D200,D212,D400,D415,ERA001,I001,Q000,SIM110,UP006,UP007,UP036 | |
| LefterisJP/rotkehlchen | rotkehlchen/chain/ethereum/modules/uniswap/v3/utils.py | D212,D400,D415,EM102,Q000,RUF010 | |
| pandabuilder/pandachaika | core/base/utilities.py | C419,COM812,D200,D210,D400,D415,EM103,ERA001,F401,I001,Q000, RUF001,RUF010,SIM103,SIM105,SIM108,SIM110,UP007,UP008,UP031,UP032 | |
| pylint-dev/pylint | doc/data/messages/m/missing-format-attribute/good.py | UP030,UP032 | #4525 |
| pylint-dev/pylint | tests/functional/a/assignment/assignment_expression.py | E731 | |
| ReflexAI/BentoML-fork | src/bentoml/_internal/types.py | B009,D200,D212,D415,EM101,EM102,I001,UP006,UP007,UP034,UP036 | #4522 |
| samkenxstream/turnkey-triumph-326606_SamKenX-snapcraft | snapcraft_legacy/internal/repo/snaps.py | ANN204,C408,C419,COM812,D209,D212,I001,SIM118,UP006,UP007,UP032,UP035 | |
| sarvex/onnxruntime | onnxruntime/python/tools/transformers/onnx_model_unet.py | ANN204,COM812,D200,D400,D407,D415,PT018 | |
| shapiromatron/hawc | hawc/apps/hawc_admin/actions/media.py | C408,COM812,D400,D407,D415 | |
| snapcore/snapcraft | snapcraft_legacy/internal/repo/snaps.py | ANN204,C408,C419,COM812,D209,D212,I001,SIM118,UP006,UP007,UP032,UP035 | |
| sylvestre/gecko-dev | third_party/python/idna/idna/uts46data.py | COM812,Q000,RUF001,UP009,UP025 | |
| sylvestre/gecko-dev | third_party/python/pip/pip/_vendor/idna/uts46data.py | COM812,F401,I001,Q000,RUF001,UP006,UP007,UP009 | |
| tomvodi/rotki | rotkehlchen/chain/ethereum/modules/uniswap/v3/utils.py | D212,D400,D415,EM102,Q000,RUF010 | |
| WeblateOrg/weblate | weblate/checks/data.py | RUF001 | |

---

_Label `autofix` added by @konstin on 2023-05-19 09:14_

---

_Comment by @JonathanPlasse on 2023-05-19 09:33_

We could run this script before each release to check if we did not introduce regressions.

---

_Comment by @JonathanPlasse on 2023-05-19 09:35_

I would like to work on

Repository | File |  
-- | -- | --
pylint-dev/pylint | doc/data/messages/m/missing-format-attribute/good.py | UP030,UP032

---

_Comment by @konstin on 2023-05-19 09:55_

> We could run this script before each release to check if we did not introduce regressions.

The main problem is that this is a 53GB directory from 3k git clones which is too much for e.g. [github action runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources), and i expect this to grow with growing adoption of ruff and better scraping scripts. So i can run this locally, but it's not feasible to automate this (yet).

What i would do (low prio) is the following:
 * Remove the forks from https://github.com/akx/ruff-usage-aggregate/blob/master/data/known-github-tomls.jsonl (while caching the results what is or isn't a fork so we don't do 3k api requests each time) (done)
 * Add https://dev.to/hugovk/how-to-search-5000-python-projects-31gk to the dataset
 * Streamline and document the scripts so everybody can run them (giving a machine with docker and sufficient disk space)

---

_Comment by @qarmin on 2023-08-28 20:24_

Recently via pypi I downloaded 23k latest source code archives of this projects(quite old list)(https://github.com/Silentsoul04/hoply/blob/9786f4c120fde6e48b26c7b85e42ed219fefe3dc/examples/pypi/top25k.txt) - [links.txt](https://github.com/astral-sh/ruff/files/12457884/links.txt).

This method is faster to use and takes a lot of less disk space(22 350 packages with total size 19,7 GB), but I'm unable to easily update package versions

Quite strange, that with `--fix --select ALL` I got only 1 panic when testing ~1 million files(I reported also a lot of different errors, so issues in first post are probably quite outdated)


---

_Comment by @konstin on 2023-09-04 12:44_

> Recently via pypi I downloaded 23k latest source code archives of this projects(quite old list)(https://github.com/Silentsoul04/hoply/blob/9786f4c120fde6e48b26c7b85e42ed219fefe3dc/examples/pypi/top25k.txt) - [links.txt](https://github.com/astral-sh/ruff/files/12457884/links.txt).

> This method is faster to use and takes a lot of less disk space(22 350 packages with total size 19,7 GB), but I'm unable to easily update package versions

That's really cool! do you have a script how you ran that?

> Quite strange, that with --fix --select ALL I got only 1 panic when testing ~1 million files(I reported also a lot of different errors, so issues in first post are probably quite outdated)

The vast majority of the panics in the OP were from failed f-string fixes. We have since disabled this kind of f-string fixes (e.g. https://github.com/astral-sh/ruff/pull/4488).

I'll close this issue since it has become quite outdated. We should instead create new issues for specific bugs (such as https://github.com/astral-sh/ruff/issues?q=is%3Aissue+is%3Aopen+label%3Afuzzer are).

---

_Closed by @konstin on 2023-09-04 12:44_

---

_Comment by @qarmin on 2023-09-04 18:09_

- Names of all pypi packages(without popularity level) can be found here - https://pypi.org/simple/
- Each archive with source code can be downloaded by creating url with package name inside - https://pypi.org/pypi/pydantic/json


I created tool automatically download packages, unpack it, deduplicate etc. - https://github.com/qarmin/download_python_repos/ (it was created just for my internal purposes, so it may be quite hard to use)

Almost 2 millions of random files from packages are available here - https://github.com/qarmin/Automated-Fuzzer/releases and are used in ci to find regressions 

---
