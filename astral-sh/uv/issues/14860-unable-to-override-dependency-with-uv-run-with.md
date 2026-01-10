---
number: 14860
title: "Unable to override dependency with `uv run --with` after 0.8.0"
type: issue
state: closed
author: SumanthRH
labels:
  - bug
assignees: []
created_at: 2025-07-24T01:18:31Z
updated_at: 2025-07-24T12:08:18Z
url: https://github.com/astral-sh/uv/issues/14860
synced_at: 2026-01-10T01:25:49Z
---

# Unable to override dependency with `uv run --with` after 0.8.0

---

_Issue opened by @SumanthRH on 2025-07-24 01:18_

## Summary

Overriding a dependency from CLI with `--with` seems broken now after 0.8.0 release. Having this way of overriding a dependency is pretty useful (There are several reasons outlined in https://github.com/astral-sh/uv/issues/4824 ). I might have missed if there's a different way of doing this, so please let me know if so. 

## Setup

I have a simple project where I want to override the version of `ray` to test out the latest release:

```bash
repro/
|-- pyproject.toml
`-- uv.lock
```

the pyproject.toml file is:

```toml
[project]
name = "test-project"
version = "0.1.0"
requires-python = "==3.12.*"

dependencies = [
    "ray==2.44.0",
]
```

Here is the command I ran:

```bash
(base) ray@ip-10-0-125-178:~/default/repro$ uv run --verbose  -w ray==2.48 python -c "import ray; print(ray.__version__)"
DEBUG uv 0.8.2
DEBUG Found project root: `/home/ray/default/repro`
DEBUG No workspace root found, using project root
DEBUG Discovered project `test-project` at: /home/ray/default/repro
DEBUG Acquired lock for `/home/ray/default/repro`
DEBUG No Python version file found in workspace: /home/ray/default/repro
DEBUG Using Python request `==3.12.*` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG Searching for Python ==3.12.* in managed installations or search path
DEBUG Searching for managed installations at `/home/ray/.local/share/uv/python`
DEBUG Found `cpython-3.12.11-linux-x86_64-gnu` at `/home/ray/anaconda3/bin/python3` (first executable in the search path)
Using CPython 3.12.11 interpreter at: /home/ray/anaconda3/bin/python3
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /home/ray/anaconda3/bin/python3
DEBUG Released lock at `/tmp/uv-727a841f95c356f0.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-project @ file:///home/ray/default/repro
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 22 packages in 0.84ms
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: ray==2.44.0
DEBUG Registry requirement already cached: aiosignal==1.4.0
DEBUG Registry requirement already cached: click==8.2.1
DEBUG Registry requirement already cached: filelock==3.18.0
DEBUG Registry requirement already cached: frozenlist==1.7.0
DEBUG Registry requirement already cached: jsonschema==4.25.0
DEBUG Registry requirement already cached: msgpack==1.1.1
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: protobuf==6.31.1
DEBUG Registry requirement already cached: pyyaml==6.0.2
DEBUG Registry requirement already cached: requests==2.32.4
DEBUG Registry requirement already cached: typing-extensions==4.14.1
DEBUG Registry requirement already cached: attrs==25.3.0
DEBUG Registry requirement already cached: jsonschema-specifications==2025.4.1
DEBUG Registry requirement already cached: referencing==0.36.2
DEBUG Registry requirement already cached: rpds-py==0.26.0
DEBUG Registry requirement already cached: certifi==2025.7.14
DEBUG Registry requirement already cached: charset-normalizer==3.4.2
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: urllib3==2.5.0
Installed 20 packages in 75ms
 + aiosignal==1.4.0
 + attrs==25.3.0
 + certifi==2025.7.14
 + charset-normalizer==3.4.2
 + click==8.2.1
 + filelock==3.18.0
 + frozenlist==1.7.0
 + idna==3.10
 + jsonschema==4.25.0
 + jsonschema-specifications==2025.4.1
 + msgpack==1.1.1
 + packaging==25.0
 + protobuf==6.31.1
 + pyyaml==6.0.2
 + ray==2.44.0
 + referencing==0.36.2
 + requests==2.32.4
 + rpds-py==0.26.0
 + typing-extensions==4.14.1
 + urllib3==2.5.0
DEBUG Released lock at `/home/ray/default/repro/.venv/.lock`
DEBUG Using Python 3.12.11 interpreter at: /home/ray/default/repro/.venv/bin/python
DEBUG At least one requirement is not satisfied in the base environment: ray==2.48
DEBUG Syncing `--with` requirements to cached environment
DEBUG Assessing Python executable as base candidate: /home/ray/anaconda3/bin/python3
DEBUG Caching via base interpreter: `/home/ray/anaconda3/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.11
DEBUG Solving with target Python version: >=3.12.11
DEBUG Adding direct dependency: ray>=2.48, <2.48+
DEBUG Found fresh response for: https://pypi.org/simple/ray/
DEBUG Searching for a compatible version of ray (>=2.48, <2.48+)
DEBUG Selecting: ray==2.48.0 [compatible] (ray-2.48.0-cp312-cp312-manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/61/02/1894be2ab930b599de0f1f77f785b86c78bda4873c6c2dd65d1de5b40837/ray-2.48.0-cp312-cp312-manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency for ray==2.48.0: click>=7.0
DEBUG Adding transitive dependency for ray==2.48.0: filelock*
DEBUG Adding transitive dependency for ray==2.48.0: jsonschema*
DEBUG Adding transitive dependency for ray==2.48.0: msgpack>=1.0.0, <2.0.0
DEBUG Adding transitive dependency for ray==2.48.0: packaging*
DEBUG Adding transitive dependency for ray==2.48.0: protobuf>=3.15.3, <3.19.5 | >=3.19.5+
DEBUG Adding transitive dependency for ray==2.48.0: pyyaml*
DEBUG Adding transitive dependency for ray==2.48.0: requests*
DEBUG Found fresh response for: https://pypi.org/simple/click/
DEBUG Found fresh response for: https://pypi.org/simple/jsonschema/
DEBUG Searching for a compatible version of click (>=7.0)
DEBUG Selecting: click==8.2.1 [preference] (click-8.2.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4d/36/2a115987e2d8c300a974597416d9de88f2444426de9571f4b59b2cca3acc/filelock-3.18.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/32/10bb5764d90a8eee674e9dc6f4db6a0ab47c8c4d0d83c27f7c39ac415a4d/click-8.2.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.18.0 [preference] (filelock-3.18.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of jsonschema (*)
DEBUG Selecting: jsonschema==4.25.0 [preference] (jsonschema-4.25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/e4/56027c4a6b4ae70ca9de302488c5ca95ad4a39e190093d6c1a8ace08341b/requests-2.32.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/msgpack/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fe/54/c86cd8e011fe98803d7e382fd67c0df5ceab8d2b7ad8c5a81524f791551c/jsonschema-4.25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/pyyaml/
DEBUG Adding transitive dependency for jsonschema==4.25.0: attrs>=22.2.0
DEBUG Adding transitive dependency for jsonschema==4.25.0: jsonschema-specifications>=2023.3.6
DEBUG Adding transitive dependency for jsonschema==4.25.0: referencing>=0.28.4
DEBUG Adding transitive dependency for jsonschema==4.25.0: rpds-py>=0.7.1
DEBUG Searching for a compatible version of msgpack (>=1.0.0, <2.0.0)
DEBUG Selecting: msgpack==1.1.1 [preference] (msgpack-1.1.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4d/ec/fd869e2567cc9c01278a736cfd1697941ba0d4b81a43e0aa2e8d71dab208/msgpack-1.1.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/2b/614b4752f2e127db5cc206abc23a8c19678e92b23c3db30fc86ab731d3bd/PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==25.0 [preference] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/attrs/
DEBUG Found fresh response for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Found fresh response for: https://pypi.org/simple/referencing/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/01/0e/b27cdbaccf30b890c40ed1da9fd4a3593a5cf94dae54fb34f8a4b74fcd3f/jsonschema_specifications-2025.4.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/77/06/bb80f5f86020c4551da315d78b3ab75e8228f89f0162f2c3a819e407941a/attrs-25.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/b1/3baf80dc6d2b7bc27a95a67752d0208e410351e3feb4eb78de5f77454d8d/referencing-0.36.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/protobuf/
DEBUG Searching for a compatible version of protobuf (>=3.15.3, <3.19.5 | >=3.19.5+)
DEBUG Selecting: protobuf==6.31.1 [preference] (protobuf-6.31.1-cp39-abi3-manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fa/b1/b59d405d64d31999244643d88c45c8241c58f17cc887e73bcb90602327f8/protobuf-6.31.1-cp39-abi3-manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of pyyaml (*)
DEBUG Selecting: pyyaml==6.0.2 [preference] (PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.4 [preference] (requests-2.32.4-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.4: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.4: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.4: urllib3>=1.21.1, <3
DEBUG Adding transitive dependency for requests==2.32.4: certifi>=2017.4.17
DEBUG Searching for a compatible version of attrs (>=22.2.0)
DEBUG Selecting: attrs==25.3.0 [preference] (attrs-25.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jsonschema-specifications (>=2023.3.6)
DEBUG Selecting: jsonschema-specifications==2025.4.1 [preference] (jsonschema_specifications-2025.4.1-py3-none-any.whl)
DEBUG Adding transitive dependency for jsonschema-specifications==2025.4.1: referencing>=0.31.0
DEBUG Searching for a compatible version of referencing (>=0.31.0)
DEBUG Selecting: referencing==0.36.2 [preference] (referencing-0.36.2-py3-none-any.whl)
DEBUG Adding transitive dependency for referencing==0.36.2: attrs>=22.2.0
DEBUG Adding transitive dependency for referencing==0.36.2: rpds-py>=0.7.0
DEBUG Adding transitive dependency for referencing==0.36.2: typing-extensions{python_full_version < '3.13'}>=4.4.0
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/c2/fe1e52489ae3122415c51f387e221dd0773709bad6c6cdaa599e8a2c5185/urllib3-2.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/charset-normalizer/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8c/73/6ede2ec59bce19b3edf4209d70004253ec5f4e319f9a2e3f2f15601ed5f7/charset_normalizer-3.4.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/rpds-py/
DEBUG Searching for a compatible version of rpds-py (>=0.7.1)
DEBUG Selecting: rpds-py==0.26.0 [preference] (rpds_py-0.26.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2e/77/87d7bfabfc4e821caa35481a2ff6ae0b73e6a391bb6b343db2c91c2b9844/rpds_py-0.26.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.2 [preference] (charset_normalizer-3.4.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.10 [preference] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.5.0 [preference] (urllib3-2.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2025.7.14 [preference] (certifi-2025.7.14-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.4.0)
DEBUG Selecting: typing-extensions==4.14.1 [preference] (typing_extensions-4.14.1-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.14.1: typing-extensions==4.14.1
DEBUG Adding transitive dependency for typing-extensions==4.14.1: typing-extensions{python_full_version < '3.13'}==4.14.1
DEBUG Searching for a compatible version of typing-extensions (==4.14.1)
DEBUG Selecting: typing-extensions==4.14.1 [preference] (typing_extensions-4.14.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b5/00/d631e67a838026495268c2f6884f3711a15a9a2a96cd244fdaea53b823fb/typing_extensions-4.14.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.14.1)
DEBUG Selecting: typing-extensions==4.14.1 [preference] (typing_extensions-4.14.1-py3-none-any.whl)
DEBUG Tried 18 versions: attrs 1, certifi 1, charset-normalizer 1, click 1, filelock 1, idna 1, jsonschema 1, jsonschema-specifications 1, msgpack 1, packaging 1, protobuf 1, pyyaml 1, ray 1, referencing 1, requests 1, rpds-py 1, typing-extensions 1, urllib3 1
DEBUG marker environment resolution took 0.009s
Resolved 18 packages in 9ms
DEBUG Checking for Python environment at: `/home/ray/.cache/uv/archive-v0/5JOLt8CR8IZlBThx6NNca`
DEBUG Creating ephemeral environment at: `/home/ray/.cache/uv/builds-v0/.tmpa2RPIn`
DEBUG Using base executable for virtual environment: /home/ray/anaconda3/bin/python3
DEBUG Running `python -c import ray; print(ray.__version__)`
DEBUG Spawned child 123642 in process group 123580
2.44.0
DEBUG Command exited with code: 0
```

## Previous behaviour

The same command worked fine with 0.7.22: 

```bash
(base) ray@ip-10-0-125-178:~/default/repro$ curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.7.22/uv-
installer.sh | sh
downloading uv 0.7.22 x86_64-unknown-linux-gnu
no checksums to verify
installing to /home/ray/.local/bin
  uv
  uvx
everything's installed!
(base) ray@ip-10-0-125-178:~/default/repro$ uv self version
uv 0.7.22
(base) ray@ip-10-0-125-178:~/default/repro$ uv run  -w ray==2.48 python -c "import ray; print(ray.__version__)"
2.48.0
```

### Platform

Ubuntu 22.04.5, Linux 6.5.0-1024-aws x86_64 GNU/Linux

### Version

0.8.2

### Python version

3.12.11

---

_Label `bug` added by @SumanthRH on 2025-07-24 01:18_

---

_Comment by @zanieb on 2025-07-24 01:23_

Thanks for the report. I presume this was caused by https://github.com/astral-sh/uv/pull/14447

I'll need to take a closer look, but we should definitely prefer the `--with` layer over the project layer to maintain this capability.

---

_Assigned to @zanieb by @zanieb on 2025-07-24 01:24_

---

_Referenced in [astral-sh/uv#14863](../../astral-sh/uv/pulls/14863.md) on 2025-07-24 01:43_

---

_Closed by @charliermarsh on 2025-07-24 02:00_

---

_Comment by @tpgillam on 2025-07-24 07:51_

We ran into basically the same thing for `--with-editable` (where we override a project dependency with a local checkout that we're hacking on). Is it expected that #14863 should fix this case too? If not I'm happy to open a new issue / make a small repro etc.

---

_Comment by @charliermarsh on 2025-07-24 12:08_

I would expect it to be fixed by https://github.com/astral-sh/uv/pull/14863 too, but let me know if it's not resolved in the next release.

---
