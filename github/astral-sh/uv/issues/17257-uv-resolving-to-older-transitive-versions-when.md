---
number: 17257
title: uv resolving to older transitive versions when multiple valid solutions exist
type: issue
state: open
author: juanitosvq
labels:
  - question
assignees: []
created_at: 2025-12-29T22:03:09Z
updated_at: 2026-01-05T11:41:28Z
url: https://github.com/astral-sh/uv/issues/17257
synced_at: 2026-01-07T13:12:19-06:00
---

# uv resolving to older transitive versions when multiple valid solutions exist

---

_Issue opened by @juanitosvq on 2025-12-29 22:03_

### Summary

Hi, I am seeing an odd behaviour when installing the following dependencies together:
```
    "trcli==1.12.5",
    "jsonschema==4.19.2",
```

`trcli` depends on `"openapi-spec-validator>=0.5.0,<1.0.0"`, and when installed by itself, it will bring v0.7.2, which is the newest available at the time of writing the ticket.

When I have both `trcli` and `jsonschema`, the `openapi-spec-validator` that gets installed is v0.5.5, but as far as I can see the v0.7.2 is still a valid version for both packages. In fact, `jsonschema` by itself won't install that `openapi-spec-validator` package at all. Which is why I find this behaviour confusing. I've tried with other tools (pipenv, pdm, poetry) and they all choose to install v0.7.2.

I am trying to make sense of `uv`'s verbose output, but I can't see anything definitive.

Also, I looked through existing issues and couldn't find anything similar, but I may have missed something relevant.

### Platform

macOS 26 (Darwin 25.2.0), arm64 (Apple Silicon)

### Version

uv 0.9.18 (0cee76417 2025-12-16)

### Python version

Python 3.12.12

---

_Label `bug` added by @juanitosvq on 2025-12-29 22:03_

---

_Comment by @woodruffw on 2025-12-29 22:09_

Hi @juanitosvq, thanks for the report. Could you share those verbose logs? They might be able to help us trace through the resolver here.

---

_Comment by @juanitosvq on 2025-12-29 22:14_

@woodruffw here they are:

**pyproject.toml**
```
[project]
version = "1.0.0"
name = "test-project"
requires-python = "~=3.12.10"
dependencies = [
    "trcli==1.12.5", 
    "jsonschema==4.19.2",
]
```

**Output of `uv lock --upgrade --verbose`**:
```
DEBUG uv 0.9.18 (0cee76417 2025-12-16)
DEBUG Acquired shared lock for `/Users/jcabrera/.cache/uv`
DEBUG Found workspace root: `/Users/jcabrera/work/python/franz`
DEBUG Adding root workspace member: `/Users/jcabrera/work/python/franz`
DEBUG No Python version file found in workspace: /Users/jcabrera/work/python/franz
DEBUG Using Python request `>=3.12.10, <3.13` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.12.10, <3.13`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to `--upgrade`
DEBUG Found static `pyproject.toml` for: test-project @ file:///Users/jcabrera/work/python/franz
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.12
DEBUG Solving with target Python version: >=3.12.10, <3.13
DEBUG Adding direct dependency: test-project*
DEBUG Adding direct dependency: test-project:dev*
DEBUG Searching for a compatible version of test-project @ file:///Users/jcabrera/work/python/franz (*)
DEBUG Adding direct dependency: jsonschema>=4.19.2, <4.19.2+
DEBUG Adding direct dependency: trcli>=1.12.5, <1.12.5+
DEBUG Searching for a compatible version of test-project @ file:///Users/jcabrera/work/python/franz (*)
DEBUG Adding direct dependency: test-project:dev==1.0.0
DEBUG Searching for a compatible version of test-project @ file:///Users/jcabrera/work/python/franz (==1.0.0)
DEBUG Found stale response for: https://pypi.org/simple/trcli/
DEBUG Sending revalidation request for: https://pypi.org/simple/trcli/
DEBUG Found stale response for: https://pypi.org/simple/jsonschema/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema/
DEBUG Found not-modified response for: https://pypi.org/simple/trcli/
DEBUG Searching for a compatible version of jsonschema (>=4.19.2, <4.19.2+)
DEBUG Selecting: jsonschema==4.19.2 [compatible] (jsonschema-4.19.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ce/aa/d1bd0b5ec568a903cc3ebcb6b096ab65c1d971c8a01ca3bf3cf788c3c646/jsonschema-4.19.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema==4.19.2: attrs>=22.2.0
DEBUG Adding transitive dependency for jsonschema==4.19.2: jsonschema-specifications>=2023.3.6
DEBUG Adding transitive dependency for jsonschema==4.19.2: referencing>=0.28.4
DEBUG Adding transitive dependency for jsonschema==4.19.2: rpds-py>=0.7.1
DEBUG Searching for a compatible version of trcli (>=1.12.5, <1.12.5+)
DEBUG Selecting: trcli==1.12.5 [compatible] (trcli-1.12.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/9d/650f8dcb5566373604bce167683f9d62374b81422aadfbf9dac97f0d2dbc/trcli-1.12.5-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for trcli==1.12.5: beartype>=0.17.0, <1.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: click>=8.1.0, <8.2.2
DEBUG Adding transitive dependency for trcli==1.12.5: humanfriendly>=10.0.0, <11.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: junitparser>=3.1.0, <4.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: openapi-spec-validator>=0.5.0, <1.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: prance*
DEBUG Adding transitive dependency for trcli==1.12.5: pyserde>=0.12.dev0, <0.13.dev0
DEBUG Adding transitive dependency for trcli==1.12.5: pyyaml>=6.0.0, <7.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: requests>=2.31.0, <3.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: tqdm>=4.65.0, <5.0.0
DEBUG Found stale response for: https://pypi.org/simple/attrs/
DEBUG Sending revalidation request for: https://pypi.org/simple/attrs/
DEBUG Found stale response for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Found stale response for: https://pypi.org/simple/junitparser/
DEBUG Sending revalidation request for: https://pypi.org/simple/junitparser/
DEBUG Found stale response for: https://pypi.org/simple/referencing/
DEBUG Sending revalidation request for: https://pypi.org/simple/referencing/
DEBUG Found stale response for: https://pypi.org/simple/click/
DEBUG Sending revalidation request for: https://pypi.org/simple/click/
DEBUG Found stale response for: https://pypi.org/simple/humanfriendly/
DEBUG Sending revalidation request for: https://pypi.org/simple/humanfriendly/
DEBUG Found stale response for: https://pypi.org/simple/openapi-spec-validator/
DEBUG Sending revalidation request for: https://pypi.org/simple/openapi-spec-validator/
DEBUG Found stale response for: https://pypi.org/simple/prance/
DEBUG Sending revalidation request for: https://pypi.org/simple/prance/
DEBUG Found stale response for: https://pypi.org/simple/beartype/
DEBUG Sending revalidation request for: https://pypi.org/simple/beartype/
DEBUG Found stale response for: https://pypi.org/simple/tqdm/
DEBUG Sending revalidation request for: https://pypi.org/simple/tqdm/
DEBUG Found stale response for: https://pypi.org/simple/pyserde/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyserde/
DEBUG Found stale response for: https://pypi.org/simple/requests/
DEBUG Sending revalidation request for: https://pypi.org/simple/requests/
DEBUG Found stale response for: https://pypi.org/simple/pyyaml/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyyaml/
DEBUG Found stale response for: https://pypi.org/simple/rpds-py/
DEBUG Sending revalidation request for: https://pypi.org/simple/rpds-py/
DEBUG Found not-modified response for: https://pypi.org/simple/attrs/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Searching for a compatible version of attrs (>=22.2.0)
DEBUG Selecting: attrs==25.4.0 [compatible] (attrs-25.4.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/45/1a4ed80516f02155c51f51e8cedb3c1902296743db0bbc66608a0db2814f/jsonschema_specifications-2025.9.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/2a/7cc015f5b9f5db42b7d48157e23356022889fc354a2813c15934b7cb5c0e/attrs-25.4.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of jsonschema-specifications (>=2023.3.6)
DEBUG Selecting: jsonschema-specifications==2025.9.1 [compatible] (jsonschema_specifications-2025.9.1-py3-none-any.whl)
DEBUG Adding transitive dependency for jsonschema-specifications==2025.9.1: referencing>=0.31.0
DEBUG Found not-modified response for: https://pypi.org/simple/junitparser/
DEBUG Found not-modified response for: https://pypi.org/simple/referencing/
DEBUG Searching for a compatible version of referencing (>=0.31.0)
DEBUG Selecting: referencing==0.37.0 [compatible] (referencing-0.37.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/f9/321d566c9f2af81fdb4bb3d5900214116b47be9e26b82219da8b818d9da9/junitparser-3.2.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2c/58/ca301544e1fa93ed4f80d724bf5b194f6e4b945841c5bfd555878eea9fcb/referencing-0.37.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for referencing==0.37.0: attrs>=22.2.0
DEBUG Adding transitive dependency for referencing==0.37.0: rpds-py>=0.7.0
DEBUG Adding transitive dependency for referencing==0.37.0: typing-extensions{python_full_version < '3.13'}>=4.4.0
DEBUG Found stale response for: https://pypi.org/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/pyyaml/
DEBUG Found not-modified response for: https://pypi.org/simple/requests/
DEBUG Found not-modified response for: https://pypi.org/simple/humanfriendly/
DEBUG Found not-modified response for: https://pypi.org/simple/click/
DEBUG Found not-modified response for: https://pypi.org/simple/pyserde/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/33/422b98d2195232ca1826284a76852ad5a86fe23e31b009c9886b2d0fb8b2/pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/aa/9b/d94ec40c6274eb8e83ac25628cc6cf240617580d0effe129292a0bd0ba94/pyserde-0.12.7-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f0/0f/310fb31e39e2d734ccaa2c0fb981ee41f7bd5056ce9bc29b2248bd569169/humanfriendly-10.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/32/10bb5764d90a8eee674e9dc6f4db6a0ab47c8c4d0d83c27f7c39ac415a4d/click-8.2.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/beartype/
DEBUG Found not-modified response for: https://pypi.org/simple/tqdm/
DEBUG Found not-modified response for: https://pypi.org/simple/rpds-py/
DEBUG Found not-modified response for: https://pypi.org/simple/prance/
DEBUG Searching for a compatible version of rpds-py (>=0.7.1)
DEBUG Selecting: rpds-py==0.30.0 [compatible] (rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/71/cc/18245721fa7747065ab478316c7fea7c74777d07f37ae60db2e84f8172e8/beartype-0.22.9-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a9/a8/fc509e514c708f43102542cdcbc2f42dc49f7a159f90f56d072371629731/prance-25.4.8.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/03/e7/98a2f4ac921d82f33e03f3835f5bf3a4a40aa1bfdc57975e74a97b2b4bdd/rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl.metadata
DEBUG Searching for a compatible version of beartype (>=0.17.0, <1.0.0)
DEBUG Selecting: beartype==0.22.9 [compatible] (beartype-0.22.9-py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.1.0, <8.2.2)
DEBUG Selecting: click==8.2.1 [compatible] (click-8.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for click==8.2.1: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of humanfriendly (>=10.0.0, <11.0.0)
DEBUG Selecting: humanfriendly==10.0 [compatible] (humanfriendly-10.0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for humanfriendly==10.0: pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'}*
DEBUG Searching for a compatible version of junitparser (>=3.1.0, <4.0.0)
DEBUG Selecting: junitparser==3.2.0 [compatible] (junitparser-3.2.0-py2.py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/colorama/
DEBUG Sending revalidation request for: https://pypi.org/simple/colorama/
DEBUG Found stale response for: https://pypi.org/simple/pyreadline3/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyreadline3/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/18/67/36e9267722cc04a6b9f15c7f3441c2363321a3ea07da7ae0c0707beb2a9c/typing_extensions-4.15.0-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/colorama/
DEBUG Found not-modified response for: https://pypi.org/simple/pyreadline3/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/dc/491b7661614ab97483abf2056be1deee4dc2490ecbf7bff9ab5cdbac86e1/pyreadline3-3.5.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/openapi-spec-validator/
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.7.2 [compatible] (openapi_spec_validator-0.7.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/27/dd/b3fd642260cb17532f66cc1e8250f3507d1e580483e209dc1e9d13bd980d/openapi_spec_validator-0.7.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: jsonschema>=4.18.0, <5.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: jsonschema-path>=0.3.1, <0.4.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: openapi-schema-validator>=0.6.0, <0.7.0
DEBUG Searching for a compatible version of prance (*)
DEBUG Selecting: prance==25.4.8.0 [compatible] (prance-25.4.8.0-py3-none-any.whl)
DEBUG Adding transitive dependency for prance==25.4.8.0: chardet>=5.2
DEBUG Adding transitive dependency for prance==25.4.8.0: packaging>=24.2
DEBUG Adding transitive dependency for prance==25.4.8.0: requests>=2.32.3
DEBUG Adding transitive dependency for prance==25.4.8.0: ruamel-yaml>=0.18.10
DEBUG Searching for a compatible version of pyserde (>=0.12.dev0, <0.13.dev0)
DEBUG Selecting: pyserde==0.12.7 [compatible] (pyserde-0.12.7-py3-none-any.whl)
DEBUG Adding transitive dependency for pyserde==0.12.7: casefy*
DEBUG Adding transitive dependency for pyserde==0.12.7: jinja2*
DEBUG Adding transitive dependency for pyserde==0.12.7: typing-inspect>=0.4.0
DEBUG Searching for a compatible version of pyyaml (>=6.0.0, <7.0.0)
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of requests (>=2.32.3, <3.0.0)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.5: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.5: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.5: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.5: urllib3>=1.21.1, <3
DEBUG Searching for a compatible version of tqdm (>=4.65.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.4.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.15.0: typing-extensions==4.15.0
DEBUG Adding transitive dependency for typing-extensions==4.15.0: typing-extensions{python_full_version < '3.13'}==4.15.0
DEBUG Searching for a compatible version of typing-extensions (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/chardet/
DEBUG Sending revalidation request for: https://pypi.org/simple/chardet/
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Found stale response for: https://pypi.org/simple/jsonschema-path/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema-path/
DEBUG Found stale response for: https://pypi.org/simple/typing-inspect/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-inspect/
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/openapi-schema-validator/
DEBUG Sending revalidation request for: https://pypi.org/simple/openapi-schema-validator/
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (*)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Adding transitive dependency for pyreadline3==3.5.4: pyreadline3==3.5.4
DEBUG Adding transitive dependency for pyreadline3==3.5.4: pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'}==3.5.4
DEBUG Searching for a compatible version of pyreadline3 (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/casefy/
DEBUG Sending revalidation request for: https://pypi.org/simple/casefy/
DEBUG Found stale response for: https://pypi.org/simple/idna/
DEBUG Sending revalidation request for: https://pypi.org/simple/idna/
DEBUG Found stale response for: https://pypi.org/simple/jinja2/
DEBUG Sending revalidation request for: https://pypi.org/simple/jinja2/
DEBUG Found stale response for: https://pypi.org/simple/urllib3/
DEBUG Sending revalidation request for: https://pypi.org/simple/urllib3/
DEBUG Found stale response for: https://pypi.org/simple/certifi/
DEBUG Sending revalidation request for: https://pypi.org/simple/certifi/
DEBUG Found stale response for: https://pypi.org/simple/lazy-object-proxy/
DEBUG Sending revalidation request for: https://pypi.org/simple/lazy-object-proxy/
DEBUG Found stale response for: https://pypi.org/simple/charset-normalizer/
DEBUG Sending revalidation request for: https://pypi.org/simple/charset-normalizer/
DEBUG Found stale response for: https://pypi.org/simple/ruamel-yaml/
DEBUG Sending revalidation request for: https://pypi.org/simple/ruamel-yaml/
DEBUG Found not-modified response for: https://pypi.org/simple/chardet/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/38/6f/f5fbc992a329ee4e0f288c1fe0e2ad9485ed064cac731ed2fe47dcc38cbf/chardet-5.2.0-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/urllib3/
DEBUG Found not-modified response for: https://pypi.org/simple/certifi/
DEBUG Found not-modified response for: https://pypi.org/simple/idna/
DEBUG Found not-modified response for: https://pypi.org/simple/charset-normalizer/
DEBUG Found not-modified response for: https://pypi.org/simple/jinja2/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema-path/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-inspect/
DEBUG Found not-modified response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of jsonschema-path (>=0.3.1, <0.4.0)
DEBUG Selecting: jsonschema-path==0.3.4 [compatible] (jsonschema_path-0.3.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6d/b9/4095b668ea3678bf6a0af005527f39de12fb026516fb3df17495a733b7f8/urllib3-2.6.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/7d/9bc192684cea499815ff478dfcdc13835ddf401365057044fb721ec6bddb/certifi-2025.11.12-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f3/85/1637cd4af66fa687396e757dec650f28025f2a2f5a5531a3208dc0ec43f2/charset_normalizer-3.4.4-cp312-cp312-macosx_10_13_universal2.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/65/f3/107a22063bf27bdccf2024833d3445f4eea42b2e598abfbd46f6a63b6cb0/typing_inspect-0.9.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/lazy-object-proxy/
DEBUG Found not-modified response for: https://pypi.org/simple/openapi-schema-validator/
DEBUG Found not-modified response for: https://pypi.org/simple/casefy/
DEBUG Found not-modified response for: https://pypi.org/simple/ruamel-yaml/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/58/3485da8cb93d2f393bce453adeef16896751f14ba3e2024bc21dc9597646/jsonschema_path-0.3.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: referencing<0.37.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: requests>=2.31.0, <3.0.0
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (jsonschema-path, referencing)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.4.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (*)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3 (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of prance (*)
DEBUG Selecting: prance==25.4.8.0 [compatible] (prance-25.4.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pyserde (>=0.12.dev0, <0.13.dev0)
DEBUG Selecting: pyserde==0.12.7 [compatible] (pyserde-0.12.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pyyaml (>=6.0.0, <7.0.0)
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of requests (>=2.32.3, <3.0.0)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Searching for a compatible version of tqdm (>=4.65.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Searching for a compatible version of jsonschema-path (>=0.3.1, <0.3.4 | >0.3.4, <0.4.0)
DEBUG Selecting: jsonschema-path==0.3.3 [compatible] (jsonschema_path-0.3.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/21/c6/ad0fba32775ae749016829dace42ed80f4407b171da41313d1a3a5f102e4/openapi_schema_validator-0.6.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/af/fe/b6045c782f1fd1ae317d2a6ca1884857ce5c20f59befe6ab25a8603c43a7/ruamel_yaml-0.18.17-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f1/6a/1766f8c163951a3c9aeb30a4e6f5de9b2eed8389e3906c4cf30fcb475be6/casefy-1.1.0-py3-none-any.whl.metadata
DEBUG Found stale response for: https://pypi.org/simple/pathable/
DEBUG Sending revalidation request for: https://pypi.org/simple/pathable/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/53/b0/69237e85976916b2e37586b7ddc48b9547fc38b440e25103d084b2b02ab3/jsonschema_path-0.3.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0d/1b/b5f5bd6bda26f1e15cd3232b223892e4498e34ec70a7f4f11c401ac969f1/lazy_object_proxy-1.12.0-cp312-cp312-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for jsonschema-path==0.3.3: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.3: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-path==0.3.3: referencing>=0.28.0, <0.36.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.3: requests>=2.31.0, <3.0.0
DEBUG Recording dependency conflict of jsonschema-path==0.3.3 from incompatibility of (jsonschema-path, referencing)
DEBUG Searching for a compatible version of jsonschema-path (>=0.3.1, <0.3.3 | >0.3.3, <0.3.4 | >0.3.4, <0.4.0)
DEBUG Selecting: jsonschema-path==0.3.2 [compatible] (jsonschema_path-0.3.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7f/5a/f405ced79c55191e460fc6d17a14845fddf09f601e39cfcab28cc1d3ff1c/jsonschema_path-0.3.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-path==0.3.2: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.2: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-path==0.3.2: referencing>=0.28.0, <0.32.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.2: requests>=2.31.0, <3.0.0
DEBUG Recording dependency conflict of jsonschema-path==0.3.2 from incompatibility of (jsonschema-path, referencing)
DEBUG Searching for a compatible version of jsonschema-path (>=0.3.1, <0.3.2 | >0.3.2, <0.3.3 | >0.3.3, <0.3.4 | >0.3.4, <0.4.0)
DEBUG Selecting: jsonschema-path==0.3.1 [compatible] (jsonschema_path-0.3.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/73/92/2234549efe32f6275c945d04f2da1392a47f5cd8e31ce9430366de6d4290/jsonschema_path-0.3.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-path==0.3.1: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.1: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-path==0.3.1: referencing>=0.28.0, <0.31.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.1: requests>=2.31.0, <3.0.0
DEBUG Recording dependency conflict of jsonschema-path==0.3.1 from incompatibility of (jsonschema-path, referencing)
DEBUG Searching for a compatible version of jsonschema-path (>0.3.1, <0.3.2 | >0.3.2, <0.3.3 | >0.3.3, <0.3.4 | >0.3.4, <0.4.0)
DEBUG No compatible version found for: jsonschema-path
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (referencing, jsonschema-path)
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (referencing, jsonschema-path)
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (referencing, jsonschema-path)
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (referencing, jsonschema-path)
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (referencing, jsonschema-path)
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (referencing, openapi-spec-validator)
DEBUG Recording unit propagation conflict of openapi-spec-validator from incompatibility of (referencing, openapi-spec-validator)
DEBUG Package jsonschema-path has too many conflicts (affected), prioritizing
DEBUG Package referencing has too many conflicts (culprit), deprioritizing and backtracking
DEBUG Backtracked 1 decisions
DEBUG Searching for a compatible version of referencing (>=0.31.0)
DEBUG Selecting: referencing==0.37.0 [compatible] (referencing-0.37.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.4.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of rpds-py (>=0.7.1)
DEBUG Selecting: rpds-py==0.30.0 [compatible] (rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl)
DEBUG Searching for a compatible version of beartype (>=0.17.0, <1.0.0)
DEBUG Selecting: beartype==0.22.9 [compatible] (beartype-0.22.9-py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.1.0, <8.2.2)
DEBUG Selecting: click==8.2.1 [compatible] (click-8.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of humanfriendly (>=10.0.0, <11.0.0)
DEBUG Selecting: humanfriendly==10.0 [compatible] (humanfriendly-10.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (*)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3 (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of junitparser (>=3.1.0, <4.0.0)
DEBUG Selecting: junitparser==3.2.0 [compatible] (junitparser-3.2.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.7.1 [compatible] (openapi_spec_validator-0.7.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2b/4d/e744fff95aaf3aeafc968d5ba7297c8cda0d1ecb8e3acd21b25adae4d835/openapi_spec_validator-0.7.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.1: jsonschema>=4.18.0, <5.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.1: jsonschema-path>=0.3.1, <0.4.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.1: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.1: openapi-schema-validator>=0.6.0, <0.7.0
DEBUG Recording dependency conflict of openapi-spec-validator==0.7.1 from incompatibility of (openapi-spec-validator, jsonschema-path)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.7.1 | >0.7.1, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.7.0 [compatible] (openapi_spec_validator-0.7.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/dd/96eec5b41ef0a0cd63226d275f5c160ce2241d51ab5b8da98e9d6aa18283/openapi_spec_validator-0.7.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.0: jsonschema>=4.18.0, <5.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.0: jsonschema-spec>=0.2.3, <0.3.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.0: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.0: openapi-schema-validator>=0.6.0, <0.7.0
DEBUG Searching for a compatible version of prance (*)
DEBUG Selecting: prance==25.4.8.0 [compatible] (prance-25.4.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pyserde (>=0.12.dev0, <0.13.dev0)
DEBUG Selecting: pyserde==0.12.7 [compatible] (pyserde-0.12.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pyyaml (>=6.0.0, <7.0.0)
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of requests (>=2.32.3, <3.0.0)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Searching for a compatible version of tqdm (>=4.65.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Searching for a compatible version of lazy-object-proxy (>=1.7.1, <2.0.0)
DEBUG Selecting: lazy-object-proxy==1.12.0 [compatible] (lazy_object_proxy-1.12.0-cp312-cp312-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of openapi-schema-validator (>=0.6.0, <0.7.0)
DEBUG Selecting: openapi-schema-validator==0.6.3 [compatible] (openapi_schema_validator-0.6.3-py3-none-any.whl)
DEBUG Adding transitive dependency for openapi-schema-validator==0.6.3: jsonschema>=4.19.1, <5.0.0
DEBUG Adding transitive dependency for openapi-schema-validator==0.6.3: jsonschema-specifications>=2023.5.2
DEBUG Adding transitive dependency for openapi-schema-validator==0.6.3: rfc3339-validator*
DEBUG Searching for a compatible version of chardet (>=5.2)
DEBUG Selecting: chardet==5.2.0 [compatible] (chardet-5.2.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of ruamel-yaml (>=0.18.10)
DEBUG Selecting: ruamel-yaml==0.18.17 [compatible] (ruamel_yaml-0.18.17-py3-none-any.whl)
DEBUG Adding transitive dependency for ruamel-yaml==0.18.17: ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'}>=0.2.15
DEBUG Searching for a compatible version of casefy (*)
DEBUG Selecting: casefy==1.1.0 [compatible] (casefy-1.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.6 [compatible] (jinja2-3.1.6-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.6: markupsafe>=2.0
DEBUG Searching for a compatible version of typing-inspect (>=0.4.0)
DEBUG Selecting: typing-inspect==0.9.0 [compatible] (typing_inspect-0.9.0-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-inspect==0.9.0: mypy-extensions>=0.3.0
DEBUG Adding transitive dependency for typing-inspect==0.9.0: typing-extensions>=3.7.4
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2025.11.12 [compatible] (certifi-2025.11.12-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.4 [compatible] (charset_normalizer-3.4.4-cp312-cp312-macosx_10_13_universal2.whl)
DEBUG Found stale response for: https://pypi.org/simple/jsonschema-spec/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema-spec/
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.11 [compatible] (idna-3.11-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.6.2 [compatible] (urllib3-2.6.2-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/rfc3339-validator/
DEBUG Sending revalidation request for: https://pypi.org/simple/rfc3339-validator/
DEBUG Found stale response for: https://pypi.org/simple/mypy-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/mypy-extensions/
DEBUG Found stale response for: https://pypi.org/simple/ruamel-yaml-clib/
DEBUG Sending revalidation request for: https://pypi.org/simple/ruamel-yaml-clib/
DEBUG Found stale response for: https://pypi.org/simple/markupsafe/
DEBUG Sending revalidation request for: https://pypi.org/simple/markupsafe/
DEBUG Found not-modified response for: https://pypi.org/simple/pathable/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema-spec/
DEBUG Searching for a compatible version of jsonschema-spec (>=0.2.3, <0.3.0)
DEBUG Selecting: jsonschema-spec==0.2.4 [compatible] (jsonschema_spec-0.2.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d9/a2/7759a4268e1d6d74559de8fb5be6c77d621b822ae64d28ab4f7467c22f63/jsonschema_spec-0.2.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-spec==0.2.4: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-spec==0.2.4: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-spec==0.2.4: referencing>=0.28.0, <0.31.0
DEBUG Adding transitive dependency for jsonschema-spec==0.2.4: requests>=2.31.0, <3.0.0
DEBUG Recording dependency conflict of jsonschema-spec==0.2.4 from incompatibility of (jsonschema-spec, referencing)
DEBUG Searching for a compatible version of jsonschema-spec (>=0.2.3, <0.2.4 | >0.2.4, <0.3.0)
DEBUG Selecting: jsonschema-spec==0.2.3 [compatible] (jsonschema_spec-0.2.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6a/c3/9ccf904bb987d1925a5c0c7a482d6d132f62669902338f2aafc179345448/jsonschema_spec-0.2.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-spec==0.2.3: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-spec==0.2.3: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-spec==0.2.3: referencing>=0.28.0, <0.30.0
DEBUG Adding transitive dependency for jsonschema-spec==0.2.3: requests>=2.31.0, <3.0.0
DEBUG Recording dependency conflict of jsonschema-spec==0.2.3 from incompatibility of (jsonschema-spec, referencing)
DEBUG Searching for a compatible version of jsonschema-spec (>0.2.3, <0.2.4 | >0.2.4, <0.3.0)
DEBUG No compatible version found for: jsonschema-spec
DEBUG Recording unit propagation conflict of jsonschema-spec from incompatibility of (referencing, jsonschema-spec)
DEBUG Recording unit propagation conflict of jsonschema-spec from incompatibility of (referencing, jsonschema-spec)
DEBUG Recording unit propagation conflict of jsonschema-spec from incompatibility of (referencing, jsonschema-spec)
DEBUG Recording unit propagation conflict of jsonschema-spec from incompatibility of (referencing, openapi-spec-validator)
DEBUG Recording unit propagation conflict of openapi-spec-validator from incompatibility of (referencing, openapi-spec-validator)
DEBUG Package jsonschema-spec has too many conflicts (affected), prioritizing
DEBUG Searching for a compatible version of referencing (>=0.31.0)
DEBUG Selecting: referencing==0.37.0 [compatible] (referencing-0.37.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.4.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of rpds-py (>=0.7.1)
DEBUG Selecting: rpds-py==0.30.0 [compatible] (rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl)
DEBUG Searching for a compatible version of beartype (>=0.17.0, <1.0.0)
DEBUG Selecting: beartype==0.22.9 [compatible] (beartype-0.22.9-py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.1.0, <8.2.2)
DEBUG Selecting: click==8.2.1 [compatible] (click-8.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of humanfriendly (>=10.0.0, <11.0.0)
DEBUG Selecting: humanfriendly==10.0 [compatible] (humanfriendly-10.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (*)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3 (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of junitparser (>=3.1.0, <4.0.0)
DEBUG Selecting: junitparser==3.2.0 [compatible] (junitparser-3.2.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.7.0 | >0.7.0, <0.7.1 | >0.7.1, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.6.0 [compatible] (openapi_spec_validator-0.6.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/24/32/c4272a50c125b225bedc730306e28aad9563ad85794c16de41c05b6135ca/openapi_spec_validator-0.6.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.6.0: jsonschema>=4.18.0, <5.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.6.0: jsonschema-spec>=0.2.3, <0.3.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.6.0: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.6.0: openapi-schema-validator>=0.6.0, <0.7.0
DEBUG Recording dependency conflict of openapi-spec-validator==0.6.0 from incompatibility of (openapi-spec-validator, jsonschema-spec)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.6.0 | >0.6.0, <0.7.0 | >0.7.0, <0.7.1 | >0.7.1, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.5.7 [compatible] (openapi_spec_validator-0.5.7-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a8/68/05df6b11a082ee09b8b5319b3c8aeda58b8d3bccf89f201c45e8fe8a91d2/openapi_spec_validator-0.5.7-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.7: jsonschema>=4.0.0, <4.18.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.7: jsonschema-spec>=0.1.1, <0.2.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.7: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.7: openapi-schema-validator>=0.4.2, <0.5.0
DEBUG Recording dependency conflict of openapi-spec-validator==0.5.7 from incompatibility of (openapi-spec-validator, jsonschema)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.5.7 | >0.5.7, <0.6.0 | >0.6.0, <0.7.0 | >0.7.0, <0.7.1 | >0.7.1, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.5.6 [compatible] (openapi_spec_validator-0.5.6-py3-none-any.whl)
DEBUG Prefetched 5 `openapi-spec-validator` versions
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/fa/d7c3cddd55e38a4b9b1c14a847cb6257f75ee35c0ce5324c97c792d3f3ca/openapi_spec_validator-0.5.6-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/69/cf/8cd3453059b1956ebd21d7f6a373bc7fa89f0df1c12a88a152fddca95267/openapi_spec_validator-0.5.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.6: jsonschema>=4.0.0, <4.18.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.6: jsonschema-spec>=0.1.1, <0.2.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.6: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.6: openapi-schema-validator>=0.4.2, <0.5.0
DEBUG Recording dependency conflict of openapi-spec-validator==0.5.6 from incompatibility of (openapi-spec-validator, jsonschema)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.5.6 | >0.5.6, <0.5.7 | >0.5.7, <0.6.0 | >0.6.0, <0.7.0 | >0.7.0, <0.7.1 | >0.7.1, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.5.5 [compatible] (openapi_spec_validator-0.5.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5f/c4/2134d5ac7489d128fa34d0a3776df98971544f04b33beb9ec1bd6c08440d/openapi_spec_validator-0.5.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2f/d0/a9bb1909fe66ebf700931b6542cef46707342086390930117a1289a4530c/openapi_spec_validator-0.5.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/52/13/b75c7ee6adc3a776d2b54cf529c6f6b3e6dcff5a4980737cecdbf35bf7dd/openapi_spec_validator-0.5.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.5: jsonschema>=4.0.0, <5.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.5: jsonschema-spec>=0.1.1, <0.2.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.5: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.5.5: openapi-schema-validator>=0.4.2, <0.5.0
DEBUG Searching for a compatible version of jsonschema-spec (>=0.1.1, <0.2.0)
DEBUG Selecting: jsonschema-spec==0.1.6 [compatible] (jsonschema_spec-0.1.6-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8d/94/9cb35371ac834d56ba2bd8870cdab1be25dc664f3a7387f8057ac091916b/openapi_schema_validator-0.4.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/52/18/0c62b011d64158c2420594002f58b3ad30a8e982bf8e6b4f078df9e95b9b/jsonschema_spec-0.1.6-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-spec==0.1.6: jsonschema>=4.0.0, <4.18.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.6: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.6: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-spec==0.1.6: requests>=2.31.0, <3.0.0
DEBUG Recording dependency conflict of jsonschema-spec==0.1.6 from incompatibility of (jsonschema-spec, jsonschema)
DEBUG Searching for a compatible version of jsonschema-spec (>=0.1.1, <0.1.6 | >0.1.6, <0.2.0)
DEBUG Selecting: jsonschema-spec==0.1.5 [compatible] (jsonschema_spec-0.1.5-py3-none-any.whl)
DEBUG Found not-modified response for: https://pypi.org/simple/rfc3339-validator/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f5/0f/ca68b7096777d8c501624a6bbcf42b06f126d060109098e0071e2a8e1f6d/jsonschema_spec-0.1.5-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/markupsafe/
DEBUG Adding transitive dependency for jsonschema-spec==0.1.5: jsonschema>=4.0.0, <4.18.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.5: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.5: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-spec==0.1.5: requests>=2.31.0, <3.0.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.5: typing-extensions<4.6.0
DEBUG Recording dependency conflict of jsonschema-spec==0.1.5 from incompatibility of (jsonschema-spec, jsonschema)
DEBUG Searching for a compatible version of jsonschema-spec (>=0.1.1, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.2.0)
DEBUG Selecting: jsonschema-spec==0.1.4 [compatible] (jsonschema_spec-0.1.4-py3-none-any.whl)
DEBUG Found not-modified response for: https://pypi.org/simple/mypy-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/ruamel-yaml-clib/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7b/44/4e421b96b67b2daff264473f7465db72fbdf36a07e05494f50300cc7b0c6/rfc3339_validator-0.1.4-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/c4/ae6ef0d61df90f9e5a059e4ff8f29031702e96281b61bd9276c8bcd1e9e9/jsonschema_spec-0.1.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-spec==0.1.4: jsonschema>=4.0.0, <4.18.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.4: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.4: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-spec==0.1.4: typing-extensions>=4.3.0, <5.0.0
DEBUG Recording dependency conflict of jsonschema-spec==0.1.4 from incompatibility of (jsonschema-spec, jsonschema)
DEBUG Package jsonschema has too many conflicts (culprit), deprioritizing and backtracking
DEBUG Backtracked 20 decisions
DEBUG Searching for a compatible version of trcli (>=1.12.5, <1.12.5+)
DEBUG Selecting: trcli==1.12.5 [compatible] (trcli-1.12.5-py3-none-any.whl)
DEBUG Searching for a compatible version of jsonschema (>=4.19.2, <4.19.2+)
DEBUG Selecting: jsonschema==4.19.2 [compatible] (jsonschema-4.19.2-py3-none-any.whl)
DEBUG Searching for a compatible version of referencing (>=0.28.4)
DEBUG Selecting: referencing==0.37.0 [compatible] (referencing-0.37.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.4.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of attrs (>=22.2.0)
DEBUG Selecting: attrs==25.4.0 [compatible] (attrs-25.4.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jsonschema-specifications (>=2023.3.6)
DEBUG Selecting: jsonschema-specifications==2025.9.1 [compatible] (jsonschema_specifications-2025.9.1-py3-none-any.whl)
DEBUG Searching for a compatible version of rpds-py (>=0.7.1)
DEBUG Selecting: rpds-py==0.30.0 [compatible] (rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl)
DEBUG Searching for a compatible version of beartype (>=0.17.0, <1.0.0)
DEBUG Selecting: beartype==0.22.9 [compatible] (beartype-0.22.9-py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.1.0, <8.2.2)
DEBUG Selecting: click==8.2.1 [compatible] (click-8.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of humanfriendly (>=10.0.0, <11.0.0)
DEBUG Selecting: humanfriendly==10.0 [compatible] (humanfriendly-10.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (*)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3 (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of junitparser (>=3.1.0, <4.0.0)
DEBUG Selecting: junitparser==3.2.0 [compatible] (junitparser-3.2.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <0.5.6 | >0.5.6, <0.5.7 | >0.5.7, <0.6.0 | >0.6.0, <0.7.0 | >0.7.0, <0.7.1 | >0.7.1, <0.7.2 | >0.7.2, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.5.5 [compatible] (openapi_spec_validator-0.5.5-py3-none-any.whl)
DEBUG Searching for a compatible version of jsonschema-spec (>=0.1.1, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.2.0)
DEBUG Selecting: jsonschema-spec==0.1.3 [compatible] (jsonschema_spec-0.1.3-py3-none-any.whl)
DEBUG Prefetched 4 `jsonschema-spec` versions
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/72/147da192e38635ada20e0a2e1a51cf8823d2119ce8883f7053879c2199b5/markupsafe-3.0.3-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8d/9c/9d42976ba3a39e9ad9c8d6cb526bf496d6def9d1eae1ce055f6ceea748cc/jsonschema_spec-0.1.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/79/7b/2c79738432f5c924bef5071f933bcc9efd0473bac3b4aa584a6f7c1c8df8/mypy_extensions-1.1.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-spec==0.1.3: jsonschema>=4.0.0, <5.0.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.3: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-spec==0.1.3: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-spec==0.1.3: typing-extensions>=4.3.0, <5.0.0
DEBUG Searching for a compatible version of prance (*)
DEBUG Selecting: prance==25.4.8.0 [compatible] (prance-25.4.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pyserde (>=0.12.dev0, <0.13.dev0)
DEBUG Selecting: pyserde==0.12.7 [compatible] (pyserde-0.12.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pyyaml (>=6.0.0, <7.0.0)
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of requests (>=2.32.3, <3.0.0)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Searching for a compatible version of tqdm (>=4.65.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Searching for a compatible version of lazy-object-proxy (>=1.7.1, <2.0.0)
DEBUG Selecting: lazy-object-proxy==1.12.0 [compatible] (lazy_object_proxy-1.12.0-cp312-cp312-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of openapi-schema-validator (>=0.4.2, <0.5.0)
DEBUG Selecting: openapi-schema-validator==0.4.4 [compatible] (openapi_schema_validator-0.4.4-py3-none-any.whl)
DEBUG Adding transitive dependency for openapi-schema-validator==0.4.4: jsonschema>=4.0.0, <4.18.0
DEBUG Adding transitive dependency for openapi-schema-validator==0.4.4: rfc3339-validator*
DEBUG Recording dependency conflict of openapi-schema-validator==0.4.4 from incompatibility of (openapi-schema-validator, jsonschema)
DEBUG Searching for a compatible version of openapi-schema-validator (>=0.4.2, <0.4.4 | >0.4.4, <0.5.0)
DEBUG Selecting: openapi-schema-validator==0.4.3 [compatible] (openapi_schema_validator-0.4.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/72/4b/5fde11a0722d676e469d3d6f78c6a17591b9c7e0072ca359801c4bd17eee/ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ee/b8/8773d7b77398f5133ddfa3a618727682560c468f7d799ac6799133c37704/jsonschema_spec-0.1.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a4/fa/c61bd49682f3f49ff8ac079c472e95ffc460ecf8f54a904769fb1314afa5/jsonschema_spec-0.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f0/dc/3c0cfed493e23bf3c7561b3041b97f90f75ffe364de5d22b9eb9cb3b42a6/jsonschema_spec-0.1.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/eb/b6260b31b1a96386c0a880edebe26f89669098acea8e0318bff6adb378fd/pathable-0.4.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/54/47/2137d826dc99edf035a0ad462c3fe70ea0ccd96c1c11d06d07c29d793428/openapi_schema_validator-0.4.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-schema-validator==0.4.3: jsonschema>=4.0.0, <5.0.0
DEBUG Adding transitive dependency for openapi-schema-validator==0.4.3: rfc3339-validator*
DEBUG Searching for a compatible version of chardet (>=5.2)
DEBUG Selecting: chardet==5.2.0 [compatible] (chardet-5.2.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of ruamel-yaml (>=0.18.10)
DEBUG Selecting: ruamel-yaml==0.18.17 [compatible] (ruamel_yaml-0.18.17-py3-none-any.whl)
DEBUG Searching for a compatible version of casefy (*)
DEBUG Selecting: casefy==1.1.0 [compatible] (casefy-1.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.6 [compatible] (jinja2-3.1.6-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-inspect (>=0.4.0)
DEBUG Selecting: typing-inspect==0.9.0 [compatible] (typing_inspect-0.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2025.11.12 [compatible] (certifi-2025.11.12-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.4 [compatible] (charset_normalizer-3.4.4-cp312-cp312-macosx_10_13_universal2.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.11 [compatible] (idna-3.11-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.6.2 [compatible] (urllib3-2.6.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pathable (>=0.4.1, <0.5.0)
DEBUG Selecting: pathable==0.4.4 [compatible] (pathable-0.4.4-py3-none-any.whl)
DEBUG Searching for a compatible version of rfc3339-validator (*)
DEBUG Selecting: rfc3339-validator==0.1.4 [compatible] (rfc3339_validator-0.1.4-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for rfc3339-validator==0.1.4: six*
DEBUG Searching for a compatible version of ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'} (>=0.2.15)
DEBUG Selecting: ruamel-yaml-clib==0.2.15 [compatible] (ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Adding transitive dependency for ruamel-yaml-clib==0.2.15: ruamel-yaml-clib==0.2.15
DEBUG Adding transitive dependency for ruamel-yaml-clib==0.2.15: ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'}==0.2.15
DEBUG Searching for a compatible version of ruamel-yaml-clib (==0.2.15)
DEBUG Selecting: ruamel-yaml-clib==0.2.15 [compatible] (ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'} (==0.2.15)
DEBUG Selecting: ruamel-yaml-clib==0.2.15 [compatible] (ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.3 [compatible] (markupsafe-3.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of mypy-extensions (>=0.3.0)
DEBUG Selecting: mypy-extensions==1.1.0 [compatible] (mypy_extensions-1.1.0-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/six/
DEBUG Sending revalidation request for: https://pypi.org/simple/six/
DEBUG Found not-modified response for: https://pypi.org/simple/six/
DEBUG Searching for a compatible version of six (*)
DEBUG Selecting: six==1.17.0 [compatible] (six-1.17.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl.metadata
DEBUG Tried 56 versions: openapi-spec-validator 7, jsonschema-spec 6, jsonschema-path 4, openapi-schema-validator 3, attrs 1, beartype 1, casefy 1, certifi 1, chardet 1, charset-normalizer 1, click 1, colorama 1, humanfriendly 1, idna 1, jinja2 1, jsonschema 1, jsonschema-specifications 1, junitparser 1, lazy-object-proxy 1, markupsafe 1, mypy-extensions 1, packaging 1, pathable 1, prance 1, pyreadline3 1, pyserde 1, pyyaml 1, referencing 1, requests 1, rfc3339-validator 1, rpds-py 1, ruamel-yaml 1, ruamel-yaml-clib 1, six 1, test-project 1, tqdm 1, trcli 1, typing-extensions 1, typing-inspect 1, urllib3 1
DEBUG all marker environments resolution took 0.189s
Resolved 39 packages in 201ms
DEBUG Released lock at `/Users/jcabrera/.cache/uv/.lock`
```

---

_Comment by @woodruffw on 2025-12-29 22:35_

Thanks @juanitosvq! 

I haven't fully gone through it yet, but I think this is where the resolution starts to diverge from your expectation:

```
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: referencing<0.37.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: requests>=2.31.0, <3.0.0
DEBUG Recording unit propagation conflict of jsonschema-path from incompatibility of (jsonschema-path, referencing)
```

Essentially, we select `jsonschema-path==0.3.4` as a candidate upwards in the resolution, which introduces a `referencing<0.37.0` constraint. But earlier we selected:

```
DEBUG Adding transitive dependency for jsonschema-specifications==2025.9.1: referencing>=0.31.0
DEBUG Found not-modified response for: https://pypi.org/simple/junitparser/
DEBUG Found not-modified response for: https://pypi.org/simple/referencing/
DEBUG Searching for a compatible version of referencing (>=0.31.0)
DEBUG Selecting: referencing==0.37.0 [compatible] (referencing-0.37.0-py3-none-any.whl)
```

So, our top level candidate selection is `referencing==0.37.0`. uv then tries to walk back `jsonschema-path`'s versions to find a version that's compatible with that version of `referencing`, but it failed to find one in the valid range:

```
DEBUG Package jsonschema-path has too many conflicts (affected), prioritizing
DEBUG Package referencing has too many conflicts (culprit), deprioritizing and backtracking
DEBUG Backtracked 1 decisions
```

What I'm confused by here is why uv doesn't try `referencing==0.36` or other compatible candidates -- I suspect there would be a valid solve there.

@juanitosvq two requests:

1. Could you do a `uv pip freeze` to show me the full resolution set you're currently seeing?
2. After that, could you add `referencing==0.36` as a top-level constraint and see if that affects the resolution?

---

_Comment by @juanitosvq on 2025-12-29 22:57_

Thanks for the quick response, @woodruffw !

> 1. Could you do a uv pip freeze to show me the full resolution set you're currently seeing?


```
attrs==25.4.0
beartype==0.22.9
casefy==1.1.0
certifi==2025.11.12
chardet==5.2.0
charset-normalizer==3.4.4
click==8.2.1
-e file:///Users/jcabrera/work/python/franz
humanfriendly==10.0
idna==3.11
jinja2==3.1.6
jsonschema==4.19.2
jsonschema-spec==0.1.3
jsonschema-specifications==2025.9.1
junitparser==3.2.0
lazy-object-proxy==1.12.0
markupsafe==3.0.3
mypy-extensions==1.1.0
openapi-schema-validator==0.4.3
openapi-spec-validator==0.5.5
packaging==25.0
pathable==0.4.4
prance==25.4.8.0
pyserde==0.12.7
pyyaml==6.0.3
referencing==0.37.0
requests==2.32.5
rfc3339-validator==0.1.4
rpds-py==0.30.0
ruamel-yaml==0.18.17
ruamel-yaml-clib==0.2.15
six==1.17.0
tqdm==4.67.1
trcli==1.12.5
typing-extensions==4.15.0
typing-inspect==0.9.0
urllib3==2.6.2
```

> 2. After that, could you add referencing==0.36 as a top-level constraint and see if that affects the resolution?

Pinning `referencing==0.36` does pick the newest version of the package, v0.7.2:
```
attrs==25.4.0
beartype==0.22.9
casefy==1.1.0
certifi==2025.11.12
chardet==5.2.0
charset-normalizer==3.4.4
click==8.2.1
-e file:///Users/jcabrera/work/python/franz
humanfriendly==10.0
idna==3.11
jinja2==3.1.6
jsonschema==4.19.2
jsonschema-path==0.3.4
jsonschema-specifications==2025.9.1
junitparser==3.2.0
lazy-object-proxy==1.12.0
markupsafe==3.0.3
mypy-extensions==1.1.0
openapi-schema-validator==0.6.3
openapi-spec-validator==0.7.2
packaging==25.0
pathable==0.4.4
prance==25.4.8.0
pyserde==0.12.7
pyyaml==6.0.3
referencing==0.36.0
requests==2.32.5
rfc3339-validator==0.1.4
rpds-py==0.30.0
ruamel-yaml==0.18.17
ruamel-yaml-clib==0.2.15
six==1.17.0
tqdm==4.67.1
trcli==1.12.5
typing-extensions==4.15.0
typing-inspect==0.9.0
urllib3==2.6.2
```

In case it helps, the verbose output in this case is:
```
DEBUG uv 0.9.18 (0cee76417 2025-12-16)
DEBUG Acquired shared lock for `/Users/jcabrera/.cache/uv`
DEBUG Found workspace root: `/Users/jcabrera/work/python/franz`
DEBUG Adding root workspace member: `/Users/jcabrera/work/python/franz`
DEBUG No Python version file found in workspace: /Users/jcabrera/work/python/franz
DEBUG Using Python request `>=3.12.10, <3.13` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.12.10, <3.13`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to `--upgrade`
DEBUG Found static `pyproject.toml` for: test-project @ file:///Users/jcabrera/work/python/franz
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.12
DEBUG Solving with target Python version: >=3.12.10, <3.13
DEBUG Adding direct dependency: test-project*
DEBUG Adding direct dependency: test-project:dev*
DEBUG Searching for a compatible version of test-project @ file:///Users/jcabrera/work/python/franz (*)
DEBUG Adding direct dependency: jsonschema>=4.19.2, <4.19.2+
DEBUG Adding direct dependency: referencing>=0.36, <0.36+
DEBUG Adding direct dependency: trcli>=1.12.5, <1.12.5+
DEBUG Searching for a compatible version of test-project @ file:///Users/jcabrera/work/python/franz (*)
DEBUG Adding direct dependency: test-project:dev==1.0.0
DEBUG Searching for a compatible version of test-project @ file:///Users/jcabrera/work/python/franz (==1.0.0)
DEBUG Found stale response for: https://pypi.org/simple/jsonschema/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema/
DEBUG Found stale response for: https://pypi.org/simple/referencing/
DEBUG Sending revalidation request for: https://pypi.org/simple/referencing/
DEBUG Found stale response for: https://pypi.org/simple/trcli/
DEBUG Sending revalidation request for: https://pypi.org/simple/trcli/
DEBUG Found not-modified response for: https://pypi.org/simple/referencing/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema/
DEBUG Found not-modified response for: https://pypi.org/simple/trcli/
DEBUG Searching for a compatible version of jsonschema (>=4.19.2, <4.19.2+)
DEBUG Selecting: jsonschema==4.19.2 [compatible] (jsonschema-4.19.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/30/2f/a969d8bb4b86c2f1308cf020fba2f81a6c9c719b53a34b2ae83da2713629/referencing-0.36.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/9d/650f8dcb5566373604bce167683f9d62374b81422aadfbf9dac97f0d2dbc/trcli-1.12.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ce/aa/d1bd0b5ec568a903cc3ebcb6b096ab65c1d971c8a01ca3bf3cf788c3c646/jsonschema-4.19.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema==4.19.2: attrs>=22.2.0
DEBUG Adding transitive dependency for jsonschema==4.19.2: jsonschema-specifications>=2023.3.6
DEBUG Adding transitive dependency for jsonschema==4.19.2: referencing>=0.28.4
DEBUG Adding transitive dependency for jsonschema==4.19.2: rpds-py>=0.7.1
DEBUG Searching for a compatible version of referencing (>=0.36, <0.36+)
DEBUG Selecting: referencing==0.36.0 [compatible] (referencing-0.36.0-py3-none-any.whl)
DEBUG Adding transitive dependency for referencing==0.36.0: attrs>=22.2.0
DEBUG Adding transitive dependency for referencing==0.36.0: rpds-py>=0.7.0
DEBUG Adding transitive dependency for referencing==0.36.0: typing-extensions{python_full_version < '3.13'}*
DEBUG Searching for a compatible version of trcli (>=1.12.5, <1.12.5+)
DEBUG Selecting: trcli==1.12.5 [compatible] (trcli-1.12.5-py3-none-any.whl)
DEBUG Adding transitive dependency for trcli==1.12.5: beartype>=0.17.0, <1.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: click>=8.1.0, <8.2.2
DEBUG Adding transitive dependency for trcli==1.12.5: humanfriendly>=10.0.0, <11.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: junitparser>=3.1.0, <4.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: openapi-spec-validator>=0.5.0, <1.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: prance*
DEBUG Adding transitive dependency for trcli==1.12.5: pyserde>=0.12.dev0, <0.13.dev0
DEBUG Adding transitive dependency for trcli==1.12.5: pyyaml>=6.0.0, <7.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: requests>=2.31.0, <3.0.0
DEBUG Adding transitive dependency for trcli==1.12.5: tqdm>=4.65.0, <5.0.0
DEBUG Found stale response for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Found stale response for: https://pypi.org/simple/attrs/
DEBUG Sending revalidation request for: https://pypi.org/simple/attrs/
DEBUG Found stale response for: https://pypi.org/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-extensions/
DEBUG Found stale response for: https://pypi.org/simple/beartype/
DEBUG Sending revalidation request for: https://pypi.org/simple/beartype/
DEBUG Found stale response for: https://pypi.org/simple/junitparser/
DEBUG Sending revalidation request for: https://pypi.org/simple/junitparser/
DEBUG Found stale response for: https://pypi.org/simple/humanfriendly/
DEBUG Sending revalidation request for: https://pypi.org/simple/humanfriendly/
DEBUG Found stale response for: https://pypi.org/simple/requests/
DEBUG Sending revalidation request for: https://pypi.org/simple/requests/
DEBUG Found stale response for: https://pypi.org/simple/click/
DEBUG Sending revalidation request for: https://pypi.org/simple/click/
DEBUG Found stale response for: https://pypi.org/simple/openapi-spec-validator/
DEBUG Sending revalidation request for: https://pypi.org/simple/openapi-spec-validator/
DEBUG Found stale response for: https://pypi.org/simple/pyserde/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyserde/
DEBUG Found stale response for: https://pypi.org/simple/prance/
DEBUG Sending revalidation request for: https://pypi.org/simple/prance/
DEBUG Found stale response for: https://pypi.org/simple/tqdm/
DEBUG Sending revalidation request for: https://pypi.org/simple/tqdm/
DEBUG Found stale response for: https://pypi.org/simple/pyyaml/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyyaml/
DEBUG Found stale response for: https://pypi.org/simple/rpds-py/
DEBUG Sending revalidation request for: https://pypi.org/simple/rpds-py/
DEBUG Found not-modified response for: https://pypi.org/simple/attrs/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema-specifications/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-extensions/
DEBUG Searching for a compatible version of attrs (>=22.2.0)
DEBUG Selecting: attrs==25.4.0 [compatible] (attrs-25.4.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/45/1a4ed80516f02155c51f51e8cedb3c1902296743db0bbc66608a0db2814f/jsonschema_specifications-2025.9.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/2a/7cc015f5b9f5db42b7d48157e23356022889fc354a2813c15934b7cb5c0e/attrs-25.4.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/18/67/36e9267722cc04a6b9f15c7f3441c2363321a3ea07da7ae0c0707beb2a9c/typing_extensions-4.15.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of jsonschema-specifications (>=2023.3.6)
DEBUG Selecting: jsonschema-specifications==2025.9.1 [compatible] (jsonschema_specifications-2025.9.1-py3-none-any.whl)
DEBUG Adding transitive dependency for jsonschema-specifications==2025.9.1: referencing>=0.31.0
DEBUG Found not-modified response for: https://pypi.org/simple/beartype/
DEBUG Found not-modified response for: https://pypi.org/simple/requests/
DEBUG Found not-modified response for: https://pypi.org/simple/click/
DEBUG Found not-modified response for: https://pypi.org/simple/tqdm/
DEBUG Found not-modified response for: https://pypi.org/simple/rpds-py/
DEBUG Found not-modified response for: https://pypi.org/simple/pyyaml/
DEBUG Found not-modified response for: https://pypi.org/simple/junitparser/
DEBUG Found not-modified response for: https://pypi.org/simple/prance/
DEBUG Found not-modified response for: https://pypi.org/simple/openapi-spec-validator/
DEBUG Found not-modified response for: https://pypi.org/simple/pyserde/
DEBUG Found not-modified response for: https://pypi.org/simple/humanfriendly/
DEBUG Searching for a compatible version of rpds-py (>=0.7.1)
DEBUG Selecting: rpds-py==0.30.0 [compatible] (rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/71/cc/18245721fa7747065ab478316c7fea7c74777d07f37ae60db2e84f8172e8/beartype-0.22.9-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/32/10bb5764d90a8eee674e9dc6f4db6a0ab47c8c4d0d83c27f7c39ac415a4d/click-8.2.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/33/422b98d2195232ca1826284a76852ad5a86fe23e31b009c9886b2d0fb8b2/pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/f9/321d566c9f2af81fdb4bb3d5900214116b47be9e26b82219da8b818d9da9/junitparser-3.2.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a9/a8/fc509e514c708f43102542cdcbc2f42dc49f7a159f90f56d072371629731/prance-25.4.8.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/03/e7/98a2f4ac921d82f33e03f3835f5bf3a4a40aa1bfdc57975e74a97b2b4bdd/rpds_py-0.30.0-cp312-cp312-macosx_10_12_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/27/dd/b3fd642260cb17532f66cc1e8250f3507d1e580483e209dc1e9d13bd980d/openapi_spec_validator-0.7.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/aa/9b/d94ec40c6274eb8e83ac25628cc6cf240617580d0effe129292a0bd0ba94/pyserde-0.12.7-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (*)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.15.0: typing-extensions==4.15.0
DEBUG Adding transitive dependency for typing-extensions==4.15.0: typing-extensions{python_full_version < '3.13'}==4.15.0
DEBUG Searching for a compatible version of typing-extensions (==4.15.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f0/0f/310fb31e39e2d734ccaa2c0fb981ee41f7bd5056ce9bc29b2248bd569169/humanfriendly-10.0-py2.py3-none-any.whl.metadata
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.15.0)
DEBUG Selecting: typing-extensions==4.15.0 [compatible] (typing_extensions-4.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of beartype (>=0.17.0, <1.0.0)
DEBUG Selecting: beartype==0.22.9 [compatible] (beartype-0.22.9-py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.1.0, <8.2.2)
DEBUG Selecting: click==8.2.1 [compatible] (click-8.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for click==8.2.1: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of humanfriendly (>=10.0.0, <11.0.0)
DEBUG Selecting: humanfriendly==10.0 [compatible] (humanfriendly-10.0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for humanfriendly==10.0: pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'}*
DEBUG Searching for a compatible version of junitparser (>=3.1.0, <4.0.0)
DEBUG Selecting: junitparser==3.2.0 [compatible] (junitparser-3.2.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of openapi-spec-validator (>=0.5.0, <1.0.0)
DEBUG Selecting: openapi-spec-validator==0.7.2 [compatible] (openapi_spec_validator-0.7.2-py3-none-any.whl)
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: jsonschema>=4.18.0, <5.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: jsonschema-path>=0.3.1, <0.4.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: lazy-object-proxy>=1.7.1, <2.0.0
DEBUG Adding transitive dependency for openapi-spec-validator==0.7.2: openapi-schema-validator>=0.6.0, <0.7.0
DEBUG Searching for a compatible version of prance (*)
DEBUG Selecting: prance==25.4.8.0 [compatible] (prance-25.4.8.0-py3-none-any.whl)
DEBUG Adding transitive dependency for prance==25.4.8.0: chardet>=5.2
DEBUG Adding transitive dependency for prance==25.4.8.0: packaging>=24.2
DEBUG Adding transitive dependency for prance==25.4.8.0: requests>=2.32.3
DEBUG Adding transitive dependency for prance==25.4.8.0: ruamel-yaml>=0.18.10
DEBUG Searching for a compatible version of pyserde (>=0.12.dev0, <0.13.dev0)
DEBUG Selecting: pyserde==0.12.7 [compatible] (pyserde-0.12.7-py3-none-any.whl)
DEBUG Adding transitive dependency for pyserde==0.12.7: casefy*
DEBUG Adding transitive dependency for pyserde==0.12.7: jinja2*
DEBUG Adding transitive dependency for pyserde==0.12.7: typing-inspect>=0.4.0
DEBUG Found stale response for: https://pypi.org/simple/colorama/
DEBUG Sending revalidation request for: https://pypi.org/simple/colorama/
DEBUG Searching for a compatible version of pyyaml (>=6.0.0, <7.0.0)
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Found stale response for: https://pypi.org/simple/pyreadline3/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyreadline3/
DEBUG Searching for a compatible version of requests (>=2.32.3, <3.0.0)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.5: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.5: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.5: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.5: urllib3>=1.21.1, <3
DEBUG Searching for a compatible version of tqdm (>=4.65.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Found stale response for: https://pypi.org/simple/jsonschema-path/
DEBUG Sending revalidation request for: https://pypi.org/simple/jsonschema-path/
DEBUG Found stale response for: https://pypi.org/simple/openapi-schema-validator/
DEBUG Sending revalidation request for: https://pypi.org/simple/openapi-schema-validator/
DEBUG Found stale response for: https://pypi.org/simple/lazy-object-proxy/
DEBUG Sending revalidation request for: https://pypi.org/simple/lazy-object-proxy/
DEBUG Found stale response for: https://pypi.org/simple/chardet/
DEBUG Sending revalidation request for: https://pypi.org/simple/chardet/
DEBUG Found stale response for: https://pypi.org/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.org/simple/packaging/
DEBUG Found stale response for: https://pypi.org/simple/jinja2/
DEBUG Sending revalidation request for: https://pypi.org/simple/jinja2/
DEBUG Found stale response for: https://pypi.org/simple/typing-inspect/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-inspect/
DEBUG Found stale response for: https://pypi.org/simple/casefy/
DEBUG Sending revalidation request for: https://pypi.org/simple/casefy/
DEBUG Found stale response for: https://pypi.org/simple/urllib3/
DEBUG Sending revalidation request for: https://pypi.org/simple/urllib3/
DEBUG Found stale response for: https://pypi.org/simple/idna/
DEBUG Sending revalidation request for: https://pypi.org/simple/idna/
DEBUG Found stale response for: https://pypi.org/simple/certifi/
DEBUG Sending revalidation request for: https://pypi.org/simple/certifi/
DEBUG Found stale response for: https://pypi.org/simple/charset-normalizer/
DEBUG Sending revalidation request for: https://pypi.org/simple/charset-normalizer/
DEBUG Found stale response for: https://pypi.org/simple/ruamel-yaml/
DEBUG Sending revalidation request for: https://pypi.org/simple/ruamel-yaml/
DEBUG Found not-modified response for: https://pypi.org/simple/colorama/
DEBUG Found not-modified response for: https://pypi.org/simple/pyreadline3/
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/dc/491b7661614ab97483abf2056be1deee4dc2490ecbf7bff9ab5cdbac86e1/pyreadline3-3.5.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (*)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Adding transitive dependency for pyreadline3==3.5.4: pyreadline3==3.5.4
DEBUG Adding transitive dependency for pyreadline3==3.5.4: pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'}==3.5.4
DEBUG Searching for a compatible version of pyreadline3 (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Searching for a compatible version of pyreadline3{python_full_version >= '3.8' and sys_platform == 'win32'} (==3.5.4)
DEBUG Selecting: pyreadline3==3.5.4 [compatible] (pyreadline3-3.5.4-py3-none-any.whl)
DEBUG Found not-modified response for: https://pypi.org/simple/jinja2/
DEBUG Found not-modified response for: https://pypi.org/simple/chardet/
DEBUG Found not-modified response for: https://pypi.org/simple/packaging/
DEBUG Found not-modified response for: https://pypi.org/simple/jsonschema-path/
DEBUG Found not-modified response for: https://pypi.org/simple/certifi/
DEBUG Found not-modified response for: https://pypi.org/simple/charset-normalizer/
DEBUG Found not-modified response for: https://pypi.org/simple/casefy/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-inspect/
DEBUG Found not-modified response for: https://pypi.org/simple/lazy-object-proxy/
DEBUG Found not-modified response for: https://pypi.org/simple/urllib3/
DEBUG Found not-modified response for: https://pypi.org/simple/idna/
DEBUG Found not-modified response for: https://pypi.org/simple/ruamel-yaml/
DEBUG Found not-modified response for: https://pypi.org/simple/openapi-schema-validator/
DEBUG Searching for a compatible version of jsonschema-path (>=0.3.1, <0.4.0)
DEBUG Selecting: jsonschema-path==0.3.4 [compatible] (jsonschema_path-0.3.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/38/6f/f5fbc992a329ee4e0f288c1fe0e2ad9485ed064cac731ed2fe47dcc38cbf/chardet-5.2.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/7d/9bc192684cea499815ff478dfcdc13835ddf401365057044fb721ec6bddb/certifi-2025.11.12-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/58/3485da8cb93d2f393bce453adeef16896751f14ba3e2024bc21dc9597646/jsonschema_path-0.3.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f3/85/1637cd4af66fa687396e757dec650f28025f2a2f5a5531a3208dc0ec43f2/charset_normalizer-3.4.4-cp312-cp312-macosx_10_13_universal2.whl.metadata
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: pathable>=0.4.1, <0.5.0
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: pyyaml>=5.1
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: referencing<0.37.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f1/6a/1766f8c163951a3c9aeb30a4e6f5de9b2eed8389e3906c4cf30fcb475be6/casefy-1.1.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jsonschema-path==0.3.4: requests>=2.31.0, <3.0.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/65/f3/107a22063bf27bdccf2024833d3445f4eea42b2e598abfbd46f6a63b6cb0/typing_inspect-0.9.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of lazy-object-proxy (>=1.7.1, <2.0.0)
DEBUG Selecting: lazy-object-proxy==1.12.0 [compatible] (lazy_object_proxy-1.12.0-cp312-cp312-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0d/1b/b5f5bd6bda26f1e15cd3232b223892e4498e34ec70a7f4f11c401ac969f1/lazy_object_proxy-1.12.0-cp312-cp312-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6d/b9/4095b668ea3678bf6a0af005527f39de12fb026516fb3df17495a733b7f8/urllib3-2.6.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of openapi-schema-validator (>=0.6.0, <0.7.0)
DEBUG Selecting: openapi-schema-validator==0.6.3 [compatible] (openapi_schema_validator-0.6.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/af/fe/b6045c782f1fd1ae317d2a6ca1884857ce5c20f59befe6ab25a8603c43a7/ruamel_yaml-0.18.17-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/21/c6/ad0fba32775ae749016829dace42ed80f4407b171da41313d1a3a5f102e4/openapi_schema_validator-0.6.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for openapi-schema-validator==0.6.3: jsonschema>=4.19.1, <5.0.0
DEBUG Adding transitive dependency for openapi-schema-validator==0.6.3: jsonschema-specifications>=2023.5.2
DEBUG Adding transitive dependency for openapi-schema-validator==0.6.3: rfc3339-validator*
DEBUG Searching for a compatible version of chardet (>=5.2)
DEBUG Selecting: chardet==5.2.0 [compatible] (chardet-5.2.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of ruamel-yaml (>=0.18.10)
DEBUG Selecting: ruamel-yaml==0.18.17 [compatible] (ruamel_yaml-0.18.17-py3-none-any.whl)
DEBUG Adding transitive dependency for ruamel-yaml==0.18.17: ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'}>=0.2.15
DEBUG Searching for a compatible version of casefy (*)
DEBUG Selecting: casefy==1.1.0 [compatible] (casefy-1.1.0-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/pathable/
DEBUG Sending revalidation request for: https://pypi.org/simple/pathable/
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.6 [compatible] (jinja2-3.1.6-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.6: markupsafe>=2.0
DEBUG Searching for a compatible version of typing-inspect (>=0.4.0)
DEBUG Selecting: typing-inspect==0.9.0 [compatible] (typing_inspect-0.9.0-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/rfc3339-validator/
DEBUG Sending revalidation request for: https://pypi.org/simple/rfc3339-validator/
DEBUG Adding transitive dependency for typing-inspect==0.9.0: mypy-extensions>=0.3.0
DEBUG Adding transitive dependency for typing-inspect==0.9.0: typing-extensions>=3.7.4
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2025.11.12 [compatible] (certifi-2025.11.12-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.4 [compatible] (charset_normalizer-3.4.4-cp312-cp312-macosx_10_13_universal2.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.11 [compatible] (idna-3.11-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.6.2 [compatible] (urllib3-2.6.2-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/ruamel-yaml-clib/
DEBUG Sending revalidation request for: https://pypi.org/simple/ruamel-yaml-clib/
DEBUG Found stale response for: https://pypi.org/simple/mypy-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/mypy-extensions/
DEBUG Found stale response for: https://pypi.org/simple/markupsafe/
DEBUG Sending revalidation request for: https://pypi.org/simple/markupsafe/
DEBUG Found not-modified response for: https://pypi.org/simple/markupsafe/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/72/147da192e38635ada20e0a2e1a51cf8823d2119ce8883f7053879c2199b5/markupsafe-3.0.3-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/mypy-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/ruamel-yaml-clib/
DEBUG Found not-modified response for: https://pypi.org/simple/pathable/
DEBUG Found not-modified response for: https://pypi.org/simple/rfc3339-validator/
DEBUG Searching for a compatible version of pathable (>=0.4.1, <0.5.0)
DEBUG Selecting: pathable==0.4.4 [compatible] (pathable-0.4.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/79/7b/2c79738432f5c924bef5071f933bcc9efd0473bac3b4aa584a6f7c1c8df8/mypy_extensions-1.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/72/4b/5fde11a0722d676e469d3d6f78c6a17591b9c7e0072ca359801c4bd17eee/ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7b/44/4e421b96b67b2daff264473f7465db72fbdf36a07e05494f50300cc7b0c6/rfc3339_validator-0.1.4-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/eb/b6260b31b1a96386c0a880edebe26f89669098acea8e0318bff6adb378fd/pathable-0.4.4-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of rfc3339-validator (*)
DEBUG Selecting: rfc3339-validator==0.1.4 [compatible] (rfc3339_validator-0.1.4-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for rfc3339-validator==0.1.4: six*
DEBUG Searching for a compatible version of ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'} (>=0.2.15)
DEBUG Selecting: ruamel-yaml-clib==0.2.15 [compatible] (ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Adding transitive dependency for ruamel-yaml-clib==0.2.15: ruamel-yaml-clib==0.2.15
DEBUG Adding transitive dependency for ruamel-yaml-clib==0.2.15: ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'}==0.2.15
DEBUG Searching for a compatible version of ruamel-yaml-clib (==0.2.15)
DEBUG Selecting: ruamel-yaml-clib==0.2.15 [compatible] (ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of ruamel-yaml-clib{python_full_version < '3.15' and platform_python_implementation == 'CPython'} (==0.2.15)
DEBUG Selecting: ruamel-yaml-clib==0.2.15 [compatible] (ruamel_yaml_clib-0.2.15-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.3 [compatible] (markupsafe-3.0.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of mypy-extensions (>=0.3.0)
DEBUG Selecting: mypy-extensions==1.1.0 [compatible] (mypy_extensions-1.1.0-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/six/
DEBUG Sending revalidation request for: https://pypi.org/simple/six/
DEBUG Found not-modified response for: https://pypi.org/simple/six/
DEBUG Searching for a compatible version of six (*)
DEBUG Selecting: six==1.17.0 [compatible] (six-1.17.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl.metadata
DEBUG Tried 39 versions: attrs 1, beartype 1, casefy 1, certifi 1, chardet 1, charset-normalizer 1, click 1, colorama 1, humanfriendly 1, idna 1, jinja2 1, jsonschema 1, jsonschema-path 1, jsonschema-specifications 1, junitparser 1, lazy-object-proxy 1, markupsafe 1, mypy-extensions 1, openapi-schema-validator 1, openapi-spec-validator 1, packaging 1, pathable 1, prance 1, pyreadline3 1, pyserde 1, pyyaml 1, referencing 1, requests 1, rfc3339-validator 1, rpds-py 1, ruamel-yaml 1, ruamel-yaml-clib 1, six 1, test-project 1, tqdm 1, trcli 1, typing-extensions 1, typing-inspect 1, urllib3 1
DEBUG all marker environments resolution took 0.172s
Resolved 39 packages in 176ms
DEBUG Released lock at `/Users/jcabrera/.cache/uv/.lock`
```



---

_Comment by @woodruffw on 2025-12-30 00:06_

Thanks @juanitosvq, that's very helpful context!

Out of curiosity: when you're locking your dependencies, what minimum Python version are you specifying? In other words, do you have a minimum Python in your `pyproject.toml` that's different from the one you're running with (3.12)?

(This is supported and not _typically_ an issue, but it can cause resolution differences during universal resolution. So I'm curious if that's a factor here 🙂)

---

_Comment by @juanitosvq on 2025-12-30 16:18_

@woodruffw it should be the same version:
```
[project]
version = "1.0.0"
name = "test-project"
requires-python = "~=3.12.10"
```

```
❯ python --version
Python 3.12.12
```

---

_Comment by @woodruffw on 2025-12-30 18:19_

Thanks! That's good, one less free variable.

I *suspect* what's happening here is that uv's solver prefers "shallow" backtracking where possible, under the theory that deeper transitive dependencies are more common and therefore backtracking on them is more prone to (re)invalidating other candidates. Consequently we try and backtrack on `jsonschema-path` rather than on `referencing`. However @konstin may have more insight into this when he's back 🙂 

(In general divergent solutions aren't a bug in uv, since there's no general ground truth for what the 'best' solve for an arbitrary set of requirements is. But there _might_ be a heuristic we can adjust in this kind of case.)

---

_Comment by @konstin on 2026-01-05 11:41_

It's possible to get both resolutions from uv too, in pip mode it incidentally has the same resolution as pip:

```
$ uv pip install trcli==1.12.5 jsonschema==4.19.2 
$ uv sync  
 - certifi==2026.1.4
 + certifi==2025.11.12
 - jsonschema-path==0.3.4
 + jsonschema-spec==0.1.3
 - openapi-schema-validator==0.6.3
 + openapi-schema-validator==0.4.3
 - openapi-spec-validator==0.7.2
 + openapi-spec-validator==0.5.5
 - referencing==0.36.2
 + referencing==0.37.0
```

From uv's perspective, both of these resolutions are valid: There's no way for it to decide if it should prefer openapi-spec-validator or referencing. Both have been declared as compatible, so both solutions should work with all packages involved (if not, the bounds should be adjusted: Packaging need to rely on the existing metadata).

pip and poetry don't make the guarantee for a specific resolution either, they just happen to prefer a different package than uv, which may also change in a newer version of pip or poetry. The one guarantee that uv makes is that running the same command in the same environment is stable, it won't switch between different resolutions unless something external changes.

To use a specific resolution, you can either adjust the bounds of the dependencies, or use dependency constraints (https://docs.astral.sh/uv/reference/settings/#constraint-dependencies) to force additional requirements on dependencies.

---

_Label `bug` removed by @konstin on 2026-01-05 11:41_

---

_Label `question` added by @konstin on 2026-01-05 11:41_

---
