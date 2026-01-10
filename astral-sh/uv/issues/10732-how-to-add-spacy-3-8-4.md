---
number: 10732
title: how to add spacy==3.8.4
type: issue
state: closed
author: jacobweiss2305
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-01-18T13:29:40Z
updated_at: 2025-01-20T19:02:51Z
url: https://github.com/astral-sh/uv/issues/10732
synced_at: 2026-01-10T01:24:57Z
---

# how to add spacy==3.8.4

---

_Issue opened by @jacobweiss2305 on 2025-01-18 13:29_

How do I add spacy with uv? Steps to reproduce:

Step 1: create .venv `uv venv .venv`

Step 2: activate .venv

Step 3: add spacy `uv add spacy `


I am getting this error:

```
Resolved 135 packages in 96ms                                                                                                                                                                                                               
error: Distribution `spacy==3.8.4 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
``

---

_Comment by @charliermarsh on 2025-01-18 15:24_

What is your uv version, Python version, and Python platform (e.g., Linux ARM)?

---

_Label `question` added by @charliermarsh on 2025-01-18 15:24_

---

_Label `needs-mre` added by @charliermarsh on 2025-01-18 15:24_

---

_Comment by @jacobweiss2305 on 2025-01-19 12:31_

uv version: `uv 0.5.11 (c4d0caaee 2024-12-19)`

python version: python 3.10.11

Platform: Windows machine

---

_Comment by @jacobweiss2305 on 2025-01-19 12:43_

I think i found a workaround `uv pip install spacy`

---

_Comment by @charliermarsh on 2025-01-19 16:19_

Are you certain that `uv add spacy --python 3.10` fails? I would expect it to select `spacy-3.8.4-cp310-cp310-win_amd64.whl`.

---

_Comment by @charliermarsh on 2025-01-20 03:41_

Do you mind removing the `uv.lock` and then sharing the `--verbose` logs for the failing command?

---

_Comment by @jacobweiss2305 on 2025-01-20 16:54_

Okay so `uv add spacy --python 3.10`  works

```
(.venv) PS C:\Users\jawei> uv pip list
(.venv) PS C:\Users\jawei> uv add spacy
error: No `pyproject.toml` found in current directory or any parent directory
(.venv) PS C:\Users\jawei> uv init
Initialized project `jawei`
(.venv) PS C:\Users\jawei> uv add spacy
Resolved 44 packages in 827ms
error: Distribution `spacy==3.8.4 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
(.venv) PS C:\Users\jawei> uv add spacy --python 3.10
Using CPython 3.10.11 interpreter at: AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.10_qbz5n2kfra8p0\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 44 packages in 35ms
Prepared 24 packages in 4.32s
Installed 42 packages in 722ms
 + annotated-types==0.7.0
 + blis==1.2.0
 + catalogue==2.0.10
 + certifi==2024.12.14
 + charset-normalizer==3.4.1
 + click==8.1.8
 + cloudpathlib==0.20.0
 + colorama==0.4.6
 + confection==0.1.5
 + cymem==2.0.11
 + idna==3.10
 + jinja2==3.1.5
 + langcodes==3.5.0
 + language-data==1.3.0
 + marisa-trie==1.2.1
 + markdown-it-py==3.0.0
 + markupsafe==3.0.2
 + mdurl==0.1.2
 + murmurhash==1.0.12
 + numpy==2.2.2
 + packaging==24.2
 + preshed==3.0.9
 + pydantic==2.10.5
 + pydantic-core==2.27.2
 + pygments==2.19.1
 + requests==2.32.3
 + rich==13.9.4
 + setuptools==75.8.0
 + shellingham==1.5.4
 + smart-open==7.1.0
 + spacy==3.8.4
 + spacy-legacy==3.0.12
 + spacy-loggers==1.0.5
 + srsly==2.5.1
 + thinc==8.3.4
 + tqdm==4.67.1
 + typer==0.15.1
 + typing-extensions==4.12.2
 + urllib3==2.3.0
 + wasabi==1.1.3
 + weasel==0.4.1
 + wrapt==1.17.2
(.venv) PS C:\Users\jawei>
```

I also created a fresh environment and ran `uv add spacy --verbose`. Here were the logs:

```
(.venv) PS C:\Users\jawei\test-spacy> uv add spacy --verbose
DEBUG uv 0.5.21 (3478c068b 2025-01-17)
DEBUG Found project root: `C:\Users\jawei\test-spacy`
DEBUG Found workspace root: `C:\Users\jawei`
DEBUG Adding root workspace member: `C:\Users\jawei`
DEBUG Adding current workspace member: `C:\Users\jawei\test-spacy`
DEBUG Reading Python requests from version file at `C:\Users\jawei\.python-version`
DEBUG Using Python request `3.9` from version file at `C:\Users\jawei\.python-version`
warning: `VIRTUAL_ENV=.venv` does not match the project environment path `C:\Users\jawei\.venv` and will be ignored
DEBUG The virtual environment's Python version satisfies `3.9`
DEBUG Using request timeout of 30s
DEBUG Found static `requires-dist` for: C:\Users\jawei\
DEBUG Found workspace root: `C:\Users\jawei\`
DEBUG Adding current workspace member: `C:\Users\jawei\`
DEBUG Adding discovered workspace member: `C:\Users\jawei\test-spacy`
DEBUG Found static `requires-dist` for: C:\Users\jawei\test-spacy
DEBUG Found workspace root: `C:\Users\jawei`
DEBUG Adding root workspace member: `C:\Users\jawei`
DEBUG Adding current workspace member: `C:\Users\jawei\test-spacy`
DEBUG Ignoring existing lockfile due to mismatched requirements for: `test-spacy==0.1.0`
  Requested: {Requirement { name: PackageName("spacy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
  Existing: {}
DEBUG Found static `pyproject.toml` for: jawei @ file:///C:/Users/jawei
DEBUG Found static `pyproject.toml` for: test-spacy @ file:///C:/Users/jawei/test-spacy
DEBUG Found workspace root: `C:\Users\jawei`
DEBUG Adding current workspace member: `C:\Users\jawei`
DEBUG Adding discovered workspace member: `C:\Users\jawei\test-spacy`
DEBUG Found workspace root: `C:\Users\jawei`
DEBUG Adding root workspace member: `C:\Users\jawei`
DEBUG Adding current workspace member: `C:\Users\jawei\test-spacy`
DEBUG Solving with installed Python version: 3.9.21
DEBUG Solving with target Python version: >=3.9
DEBUG Narrowed `requires-python` bound to: >=3.9
DEBUG Narrowed `requires-python` bound to: >=3.9
DEBUG Solving split (python_full_version >= '3.10') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Unbounded)) })
DEBUG Adding direct dependency: jawei*
DEBUG Adding direct dependency: test-spacy*
DEBUG Searching for a compatible version of jawei @ file:///C:/Users/jawei (*)
DEBUG Adding direct dependency: spacy>=3.8.4
DEBUG Searching for a compatible version of test-spacy @ file:///C:/Users/jawei/test-spacy (*)
DEBUG Adding direct dependency: spacy*
DEBUG Found fresh response for: https://pypi.org/simple/spacy/
DEBUG Searching for a compatible version of spacy (>=3.8.4)
DEBUG Selecting: spacy==3.8.4 [preference] (spacy-3.8.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e8/51/c0862063e8338a2cc769e787f0448c92a87ac87abfe2987ecc84d8246f51/spacy-3.8.4-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Adding transitive dependency for spacy==3.8.4: catalogue>=2.0.6, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: jinja2*
DEBUG Adding transitive dependency for spacy==3.8.4: langcodes>=3.2.0, <4.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: murmurhash>=0.28.0, <1.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: numpy{python_full_version >= '3.9'}>=1.19.0
DEBUG Adding transitive dependency for spacy==3.8.4: packaging>=20.0
DEBUG Adding transitive dependency for spacy==3.8.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: setuptools*
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-legacy>=3.0.11, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-loggers>=1.0.0, <2.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: thinc>=8.3.4, <8.4.0
DEBUG Adding transitive dependency for spacy==3.8.4: tqdm>=4.38.0, <5.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: wasabi>=0.9.1, <1.2.0
DEBUG Adding transitive dependency for spacy==3.8.4: weasel>=0.1.0, <0.5.0
DEBUG Found fresh response for: https://pypi.org/simple/catalogue/
DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
DEBUG Found fresh response for: https://pypi.org/simple/cymem/
DEBUG Found fresh response for: https://pypi.org/simple/murmurhash/
DEBUG Found fresh response for: https://pypi.org/simple/langcodes/
DEBUG Found fresh response for: https://pypi.org/simple/preshed/
DEBUG Found fresh response for: https://pypi.org/simple/spacy-loggers/
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/spacy-legacy/
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Found fresh response for: https://pypi.org/simple/weasel/
DEBUG Found fresh response for: https://pypi.org/simple/wasabi/
DEBUG Found fresh response for: https://pypi.org/simple/typer/
DEBUG Found fresh response for: https://pypi.org/simple/tqdm/
DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
DEBUG Found fresh response for: https://pypi.org/simple/srsly/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bd/0f/2ba5fbcd631e3e88689309dbe978c5769e883e4b84ebfe7da30b43275c5a/jinja2-3.1.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/74/4c/bc0a79c7b0ebec63256ac547e2cecbae73badcd26e874231ff901665e8fc/murmurhash-1.0.12-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6d/55/f453f2b2f560e057f20eb2acdaafbf6488d72a6e8a36a4aef30f6053a51c/cymem-2.0.11-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9e/96/d32b941a501ab566a16358d68b6eb4e4acc373fab3c3c4d7d9e649f7b4bb/catalogue-2.0.10-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.0)
DEBUG Found fresh response for: https://pypi.org/simple/thinc/
DEBUG Selecting: numpy==2.2.2 [preference] (numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c3/6b/068c2ea7a712bf805c62445bd9e9c06d7340358ef2824150eceac027444b/langcodes-3.5.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for numpy==2.2.2: numpy==2.2.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/38/7f/a7d3eeaee67ecebbe51866c1aae6310e34cefa0a64821aed963a0a167b51/preshed-3.0.9-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Adding transitive dependency for numpy==2.2.2: numpy{python_full_version >= '3.9'}==2.2.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/33/78/d1a1a026ef3af911159398c939b1509d5c36fe524c7b644f34a5146c4e16/spacy_loggers-1.0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of catalogue (>=2.0.6, <2.1.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Selecting: catalogue==2.0.10 [preference] (catalogue-2.0.10-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c3/55/12e842c70ff8828e34e543a2c7176dac4da006ca6901c9e8b43efab8bc6b/spacy_legacy-3.0.12-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of cymem (>=2.0.2, <2.1.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2a/87/abd57374044e1f627f0a905ac33c1a7daab35a3a815abfea4e1bafd3fdb1/weasel-0.4.1-py3-none-any.whl.metadata
DEBUG Selecting: cymem==2.0.11 [preference] (cymem-2.0.11-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/9b/335f9764261e915ed497fcdeb11df5dfd6f7bf257d4a6a2a686d80da4d54/requests-2.32.3-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.5 [preference] (jinja2-3.1.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jinja2==3.1.5: markupsafe>=2.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/26/82663c79010b28eddf29dcdd0ea723439535fa917fce5905885c0e9ba562/pydantic-2.10.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of langcodes (>=3.2.0, <4.0.0)
DEBUG Selecting: langcodes==3.5.0 [preference] (langcodes-3.5.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/cc/0a838ba5ca64dc832aa43f727bd586309846b0ffb2ce52422543e6075e8a/typer-0.15.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for langcodes==3.5.0: language-data>=1.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/06/7c/34330a89da55610daa5f245ddce5aab81244321101614751e7537f125133/wasabi-1.1.3-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of murmurhash (>=0.28.0, <1.1.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/37/08/448bcc87bb93bc19fccf70c2f0f993ac42aa41d5f44a19c60d00186aea09/srsly-2.5.1-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Selecting: murmurhash==1.0.12 [preference] (murmurhash-1.0.12-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/69/8a/b9dc7678803429e4a3bc9ba462fa3dd9066824d3c607490235c6a796be5a/setuptools-75.8.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of numpy (==2.2.2)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/c8/13db2e346d2e199f679fc3f620da53af561ea74b43b38e5b4a0a79a12860/thinc-8.3.4-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Selecting: numpy==2.2.2 [preference] (numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/2a/69033dc22d981ad21325314f8357438078f5c28310a6d89fb3833030ec8a/numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.2.2)
DEBUG Found fresh response for: https://pypi.org/simple/language-data/
DEBUG Selecting: numpy==2.2.2 [preference] (numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of packaging (>=20.0)
DEBUG Selecting: packaging==24.2 [preference] (packaging-24.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5d/e9/5a5ffd9b286db82be70d677d0a91e4d58f7912bb8dd026ddeeb4abe70679/language_data-1.3.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of preshed (>=3.0.2, <3.1.0)
DEBUG Selecting: preshed==3.0.9 [preference] (preshed-3.0.9-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/markupsafe/
DEBUG Adding transitive dependency for preshed==3.0.9: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for preshed==3.0.9: murmurhash>=0.28.0, <1.1.0
DEBUG Searching for a compatible version of pydantic (>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0)
DEBUG Selecting: pydantic==2.10.5 [preference] (pydantic-2.10.5-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic==2.10.5: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.10.5: pydantic-core>=2.27.2, <2.27.2+
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/04/90/d08277ce111dd22f77149fd1a5d4653eeb3b3eaacbdfcbae5afb2600eebd/MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl.metadata
DEBUG Adding transitive dependency for pydantic==2.10.5: typing-extensions>=4.12.2
DEBUG Found fresh response for: https://pypi.org/simple/annotated-types/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/pydantic-core/
DEBUG Searching for a compatible version of pydantic-core (>=2.27.2, <2.27.2+)
DEBUG Selecting: pydantic-core==2.27.2 [preference] (pydantic_core-2.27.2-cp310-cp310-macosx_10_12_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/bc/fed5f74b5d802cf9a03e83f60f18864e90e3aed7223adaca5ffb7a8d8d64/pydantic_core-2.27.2-cp310-cp310-macosx_10_12_x86_64.whl.metadata
DEBUG Adding transitive dependency for pydantic-core==2.27.2: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of requests (>=2.13.0, <3.0.0)
DEBUG Selecting: requests==2.32.3 [preference] (requests-2.32.3-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.8.0 [preference] (setuptools-75.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-legacy (>=3.0.11, <3.1.0)
DEBUG Selecting: spacy-legacy==3.0.12 [preference] (spacy_legacy-3.0.12-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Searching for a compatible version of spacy-loggers (>=1.0.0, <2.0.0)
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Selecting: spacy-loggers==1.0.5 [preference] (spacy_loggers-1.0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Searching for a compatible version of srsly (>=2.4.3, <3.0.0)
DEBUG Selecting: srsly==2.5.1 [preference] (srsly-2.5.1-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for srsly==2.5.1: catalogue>=2.0.3, <2.1.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a5/32/8f6669fc4798494966bf446c8c4a162e0b5d893dff088afddf76414f70e1/certifi-2024.12.14-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of thinc (>=8.3.4, <8.4.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c8/19/4ec628951a74043532ca2cf5d97b7b14863931476d117c471e8e2b1eb39f/urllib3-2.3.0-py3-none-any.whl.metadata
DEBUG Selecting: thinc==8.3.4 [preference] (thinc-8.3.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/charset-normalizer/
DEBUG Adding transitive dependency for thinc==8.3.4: blis>=1.2.0, <1.3.0
DEBUG Adding transitive dependency for thinc==8.3.4: catalogue>=2.0.4, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: confection>=0.0.1, <1.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: murmurhash>=1.0.2, <1.1.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0d/58/5580c1716040bc89206c77d8f74418caf82ce519aae06450393ca73475d1/charset_normalizer-3.4.1-cp310-cp310-macosx_10_9_universal2.whl.metadata
DEBUG Adding transitive dependency for thinc==8.3.4: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: packaging>=20.0
DEBUG Adding transitive dependency for thinc==8.3.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: setuptools*
DEBUG Adding transitive dependency for thinc==8.3.4: srsly>=2.4.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: wasabi>=0.8.1, <1.2.0
DEBUG Searching for a compatible version of tqdm (>=4.38.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [preference] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Found fresh response for: https://pypi.org/simple/confection/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/00/3106b1854b45bd0474ced037dfe6b73b90fe68a68968cef47c23de3d43d2/confection-0.1.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/colorama/
DEBUG Found fresh response for: https://pypi.org/simple/blis/
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of typer (>=0.3.0, <1.0.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/54/ff/c55d9d42a622b95fca27f82d4674cd19ad86941dc893f0898ebcccdab105/blis-1.2.0-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Selecting: typer==0.15.1 [preference] (typer-0.15.1-py3-none-any.whl)
DEBUG Adding transitive dependency for typer==0.15.1: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.1: rich>=10.11.0
DEBUG Adding transitive dependency for typer==0.15.1: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.1: typing-extensions>=3.7.4.3
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of wasabi (>=0.9.1, <1.2.0)
DEBUG Selecting: wasabi==1.1.3 [preference] (wasabi-1.1.3-py3-none-any.whl)
DEBUG Adding transitive dependency for wasabi==1.1.3: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}>=0.4.6
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (>=0.4.6)
DEBUG Found fresh response for: https://pypi.org/simple/click/
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/shellingham/
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Found fresh response for: https://pypi.org/simple/rich/
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of weasel (>=0.1.0, <0.5.0)
DEBUG Selecting: weasel==0.4.1 [preference] (weasel-0.4.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for weasel==0.4.1: cloudpathlib>=0.7.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: confection>=0.0.4, <0.2.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for weasel==0.4.1: packaging>=20.0
DEBUG Adding transitive dependency for weasel==0.4.1: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: smart-open>=5.2.1, <8.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: wasabi>=0.9.1, <1.2.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [preference] (MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of language-data (>=1.2)
DEBUG Selecting: language-data==1.3.0 [preference] (language_data-1.3.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/cloudpathlib/
DEBUG Adding transitive dependency for language-data==1.3.0: marisa-trie>=1.1.0
DEBUG Found fresh response for: https://pypi.org/simple/smart-open/
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [preference] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1f/6e/b64600156934dab14cc8b403095a9ea8bd722aad2e775673c68346b76220/cloudpathlib-0.20.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=4.12.2)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7a/18/9a8d9f01957aa1f8bbc5676d54c2e33102d247e146c1a3679d3bd5cc2e3a/smart_open-7.1.0-py3-none-any.whl.metadata
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.12.14 [preference] (certifi-2024.12.14-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.1 [preference] (charset_normalizer-3.4.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Found fresh response for: https://pypi.org/simple/marisa-trie/
DEBUG Selecting: idna==3.10 [preference] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.3.0 [preference] (urllib3-2.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of blis (>=1.2.0, <1.3.0)
DEBUG Selecting: blis==1.2.0 [preference] (blis-1.2.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for blis==1.2.0: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e4/83/ccf5b33f2123f3110705c608f8e0caa82002626511aafafc58f82e50d322/marisa_trie-1.2.1-cp310-cp310-macosx_10_9_universal2.whl.metadata
DEBUG Searching for a compatible version of confection (>=0.0.4, <0.2.0)
DEBUG Selecting: confection==0.1.5 [preference] (confection-0.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for confection==0.1.5: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for confection==0.1.5: srsly>=2.4.0, <3.0.0
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.8 [preference] (click-8.1.8-py3-none-any.whl)
DEBUG Adding transitive dependency for click==8.1.8: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of rich (>=10.11.0)
DEBUG Selecting: rich==13.9.4 [preference] (rich-13.9.4-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for rich==13.9.4: typing-extensions{python_full_version < '3.11'}>=4.0.0, <5.0
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (>=4.0.0, <5.0)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions==4.12.2
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions{python_full_version < '3.11'}==4.12.2
DEBUG Found fresh response for: https://pypi.org/simple/markdown-it-py/
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (==4.12.2)
DEBUG Found fresh response for: https://pypi.org/simple/pygments/
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [preference] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of cloudpathlib (>=0.7.0, <1.0.0)
DEBUG Selecting: cloudpathlib==0.20.0 [preference] (cloudpathlib-0.20.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for cloudpathlib==0.20.0: typing-extensions{python_full_version < '3.11'}>4
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of smart-open (>=5.2.1, <8.0.0)
DEBUG Selecting: smart-open==7.1.0 [preference] (smart_open-7.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for smart-open==7.1.0: wrapt*
DEBUG Searching for a compatible version of marisa-trie (>=1.1.0)
DEBUG Selecting: marisa-trie==1.2.1 [preference] (marisa_trie-1.2.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Adding transitive dependency for marisa-trie==1.2.1: setuptools*
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [preference] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.1 [preference] (pygments-2.19.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/mdurl/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/wrapt/
DEBUG Searching for a compatible version of wrapt (*)
DEBUG Selecting: wrapt==1.17.2 [preference] (wrapt-1.17.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/d1/1daec934997e8b160040c78d7b31789f19b122110a75eca3d4e8da0049e1/wrapt-1.17.2-cp310-cp310-macosx_10_9_universal2.whl.metadata
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [preference] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Tried 44 versions: annotated-types 1, blis 1, catalogue 1, certifi 1, charset-normalizer 1, click 1, cloudpathlib 1, colorama 1, confection 1, cymem 1, idna 1, jawei 1, jinja2 1, langcodes 1, language-data 1, marisa-trie 1, markdown-it-py 1, markupsafe 1, mdurl 1, murmurhash 1, numpy 1, packaging 1, preshed 1, pydantic 1, pydantic-core 1, pygments 1, requests 1, rich 1, setuptools 1, shellingham 1, smart-open 1, spacy 1, spacy-legacy 1, spacy-loggers 1, srsly 1, test-spacy 1, thinc 1, tqdm 1, typer 1, typing-extensions 1, urllib3 1, wasabi 1, weasel 1, wrapt 1
DEBUG split `python_full_version >= '3.10'` resolution took 0.027s
DEBUG Solving split (python_full_version == '3.9.*') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }]), range: RequiresPythonRange(LowerBound(Included("3.9")), UpperBound(Excluded("3.10"))) })
DEBUG Adding direct dependency: jawei*
DEBUG Adding direct dependency: test-spacy*
DEBUG Searching for a compatible version of jawei @ file:///C:/Users/jawei (*)
DEBUG Adding direct dependency: spacy>=3.8.4
DEBUG Searching for a compatible version of test-spacy @ file:///C:/Users/jawei/test-spacy (*)
DEBUG Adding direct dependency: spacy*
DEBUG Searching for a compatible version of spacy (>=3.8.4)
DEBUG Selecting: spacy==3.8.4 [preference] (spacy-3.8.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for spacy==3.8.4: catalogue>=2.0.6, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: jinja2*
DEBUG Adding transitive dependency for spacy==3.8.4: langcodes>=3.2.0, <4.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: murmurhash>=0.28.0, <1.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: numpy{python_full_version >= '3.9'}>=1.19.0
DEBUG Adding transitive dependency for spacy==3.8.4: packaging>=20.0
DEBUG Adding transitive dependency for spacy==3.8.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: setuptools*
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-legacy>=3.0.11, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-loggers>=1.0.0, <2.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: thinc>=8.3.4, <8.4.0
DEBUG Adding transitive dependency for spacy==3.8.4: tqdm>=4.38.0, <5.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: wasabi>=0.9.1, <1.2.0
DEBUG Adding transitive dependency for spacy==3.8.4: weasel>=0.1.0, <0.5.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.0)
DEBUG Selecting: numpy==2.0.2 [preference] (numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for numpy==2.0.2: numpy==2.0.2
DEBUG Adding transitive dependency for numpy==2.0.2: numpy{python_full_version >= '3.9'}==2.0.2
DEBUG Searching for a compatible version of catalogue (>=2.0.6, <2.1.0)
DEBUG Selecting: catalogue==2.0.10 [preference] (catalogue-2.0.10-py3-none-any.whl)
DEBUG Searching for a compatible version of cymem (>=2.0.2, <2.1.0)
DEBUG Selecting: cymem==2.0.11 [preference] (cymem-2.0.11-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/21/91/3495b3237510f79f5d81f2508f9f13fea78ebfdf07538fc7444badda173d/numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.5 [preference] (jinja2-3.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.5: markupsafe>=2.0
DEBUG Searching for a compatible version of langcodes (>=3.2.0, <4.0.0)
DEBUG Selecting: langcodes==3.5.0 [preference] (langcodes-3.5.0-py3-none-any.whl)
DEBUG Adding transitive dependency for langcodes==3.5.0: language-data>=1.2
DEBUG Searching for a compatible version of murmurhash (>=0.28.0, <1.1.0)
DEBUG Selecting: murmurhash==1.0.12 [preference] (murmurhash-1.0.12-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of numpy (==2.0.2)
DEBUG Selecting: numpy==2.0.2 [preference] (numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.0.2)
DEBUG Selecting: numpy==2.0.2 [preference] (numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of packaging (>=20.0)
DEBUG Selecting: packaging==24.2 [preference] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of preshed (>=3.0.2, <3.1.0)
DEBUG Selecting: preshed==3.0.9 [preference] (preshed-3.0.9-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for preshed==3.0.9: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for preshed==3.0.9: murmurhash>=0.28.0, <1.1.0
DEBUG Searching for a compatible version of pydantic (>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0)
DEBUG Selecting: pydantic==2.10.5 [preference] (pydantic-2.10.5-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic==2.10.5: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.10.5: pydantic-core>=2.27.2, <2.27.2+
DEBUG Adding transitive dependency for pydantic==2.10.5: typing-extensions>=4.12.2
DEBUG Searching for a compatible version of pydantic-core (>=2.27.2, <2.27.2+)
DEBUG Selecting: pydantic-core==2.27.2 [preference] (pydantic_core-2.27.2-cp310-cp310-macosx_10_12_x86_64.whl)
DEBUG Adding transitive dependency for pydantic-core==2.27.2: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of requests (>=2.13.0, <3.0.0)
DEBUG Selecting: requests==2.32.3 [preference] (requests-2.32.3-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.8.0 [preference] (setuptools-75.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-legacy (>=3.0.11, <3.1.0)
DEBUG Selecting: spacy-legacy==3.0.12 [preference] (spacy_legacy-3.0.12-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-loggers (>=1.0.0, <2.0.0)
DEBUG Selecting: spacy-loggers==1.0.5 [preference] (spacy_loggers-1.0.5-py3-none-any.whl)
DEBUG Searching for a compatible version of srsly (>=2.4.3, <3.0.0)
DEBUG Selecting: srsly==2.5.1 [preference] (srsly-2.5.1-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for srsly==2.5.1: catalogue>=2.0.3, <2.1.0
DEBUG Searching for a compatible version of thinc (>=8.3.4, <8.4.0)
DEBUG Selecting: thinc==8.3.4 [preference] (thinc-8.3.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for thinc==8.3.4: blis>=1.2.0, <1.3.0
DEBUG Adding transitive dependency for thinc==8.3.4: catalogue>=2.0.4, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: confection>=0.0.1, <1.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: murmurhash>=1.0.2, <1.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: packaging>=20.0
DEBUG Adding transitive dependency for thinc==8.3.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: setuptools*
DEBUG Adding transitive dependency for thinc==8.3.4: srsly>=2.4.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: wasabi>=0.8.1, <1.2.0
DEBUG Searching for a compatible version of tqdm (>=4.38.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [preference] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of typer (>=0.3.0, <1.0.0)
DEBUG Selecting: typer==0.15.1 [preference] (typer-0.15.1-py3-none-any.whl)
DEBUG Adding transitive dependency for typer==0.15.1: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.1: rich>=10.11.0
DEBUG Adding transitive dependency for typer==0.15.1: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.1: typing-extensions>=3.7.4.3
DEBUG Searching for a compatible version of wasabi (>=0.9.1, <1.2.0)
DEBUG Selecting: wasabi==1.1.3 [preference] (wasabi-1.1.3-py3-none-any.whl)
DEBUG Adding transitive dependency for wasabi==1.1.3: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}>=0.4.6
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (>=0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of weasel (>=0.1.0, <0.5.0)
DEBUG Selecting: weasel==0.4.1 [preference] (weasel-0.4.1-py3-none-any.whl)
DEBUG Adding transitive dependency for weasel==0.4.1: cloudpathlib>=0.7.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: confection>=0.0.4, <0.2.0
DEBUG Adding transitive dependency for weasel==0.4.1: packaging>=20.0
DEBUG Adding transitive dependency for weasel==0.4.1: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: smart-open>=5.2.1, <8.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: wasabi>=0.9.1, <1.2.0
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [preference] (MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of language-data (>=1.2)
DEBUG Selecting: language-data==1.3.0 [preference] (language_data-1.3.0-py3-none-any.whl)
DEBUG Adding transitive dependency for language-data==1.3.0: marisa-trie>=1.1.0
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [preference] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.12.14 [preference] (certifi-2024.12.14-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.1 [preference] (charset_normalizer-3.4.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.10 [preference] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.3.0 [preference] (urllib3-2.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of blis (>=1.2.0, <1.3.0)
DEBUG Selecting: blis==1.2.0 [preference] (blis-1.2.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for blis==1.2.0: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Searching for a compatible version of confection (>=0.0.4, <0.2.0)
DEBUG Selecting: confection==0.1.5 [preference] (confection-0.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for confection==0.1.5: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for confection==0.1.5: srsly>=2.4.0, <3.0.0
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.8 [preference] (click-8.1.8-py3-none-any.whl)
DEBUG Adding transitive dependency for click==8.1.8: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of rich (>=10.11.0)
DEBUG Selecting: rich==13.9.4 [preference] (rich-13.9.4-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for rich==13.9.4: typing-extensions{python_full_version < '3.11'}>=4.0.0, <5.0
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (>=4.0.0, <5.0)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions==4.12.2
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions{python_full_version < '3.11'}==4.12.2
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (==4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [preference] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of cloudpathlib (>=0.7.0, <1.0.0)
DEBUG Selecting: cloudpathlib==0.20.0 [preference] (cloudpathlib-0.20.0-py3-none-any.whl)
DEBUG Adding transitive dependency for cloudpathlib==0.20.0: typing-extensions{python_full_version < '3.11'}>4
DEBUG Searching for a compatible version of smart-open (>=5.2.1, <8.0.0)
DEBUG Selecting: smart-open==7.1.0 [preference] (smart_open-7.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for smart-open==7.1.0: wrapt*
DEBUG Searching for a compatible version of marisa-trie (>=1.1.0)
DEBUG Selecting: marisa-trie==1.2.1 [preference] (marisa_trie-1.2.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Adding transitive dependency for marisa-trie==1.2.1: setuptools*
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [preference] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.1 [preference] (pygments-2.19.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (*)
DEBUG Selecting: wrapt==1.17.2 [preference] (wrapt-1.17.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [preference] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Tried 44 versions: annotated-types 1, blis 1, catalogue 1, certifi 1, charset-normalizer 1, click 1, cloudpathlib 1, colorama 1, confection 1, cymem 1, idna 1, jawei 1, jinja2 1, langcodes 1, language-data 1, marisa-trie 1, markdown-it-py 1, markupsafe 1, mdurl 1, murmurhash 1, numpy 1, packaging 1, preshed 1, pydantic 1, pydantic-core 1, pygments 1, requests 1, rich 1, setuptools 1, shellingham 1, smart-open 1, spacy 1, spacy-legacy 1, spacy-loggers 1, srsly 1, test-spacy 1, thinc 1, tqdm 1, typer 1, typing-extensions 1, urllib3 1, wasabi 1, weasel 1, wrapt 1
DEBUG split `python_full_version == '3.9.*'` resolution took 0.009s
INFO Solved your requirements for 2 environments
DEBUG Distinct solution for split (python_full_version >= '3.10') with 44 packages
DEBUG Distinct solution for split (python_full_version == '3.9.*') with 44 packages
Resolved 45 packages in 42ms
DEBUG Using request timeout of 30s
DEBUG Found static `requires-dist` for: C:\Users\jawei\
DEBUG Found workspace root: `C:\Users\jawei\`
DEBUG Adding current workspace member: `C:\Users\jawei\`
DEBUG Adding discovered workspace member: `C:\Users\jawei\test-spacy`
DEBUG Found static `requires-dist` for: C:\Users\jawei\test-spacy
DEBUG Found workspace root: `C:\Users\jawei`
DEBUG Adding root workspace member: `C:\Users\jawei`
DEBUG Adding current workspace member: `C:\Users\jawei\test-spacy`
DEBUG Ignoring existing lockfile due to mismatched requirements for: `test-spacy==0.1.0`
  Requested: {Requirement { name: PackageName("spacy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8.4" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("spacy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
DEBUG Found static `pyproject.toml` for: test-spacy @ file:///C:/Users/jawei/test-spacy
DEBUG Found workspace root: `C:\Users\jawei`
DEBUG Adding root workspace member: `C:\Users\jawei`
DEBUG Adding current workspace member: `C:\Users\jawei\test-spacy`
DEBUG Solving with installed Python version: 3.9.21
DEBUG Solving with target Python version: >=3.9
DEBUG Narrowed `requires-python` bound to: >=3.9
DEBUG Narrowed `requires-python` bound to: >=3.9
DEBUG Solving split (python_full_version >= '3.10') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Unbounded)) })
DEBUG Adding direct dependency: jawei*
DEBUG Adding direct dependency: test-spacy*
DEBUG Searching for a compatible version of jawei @ file:///C:/Users/jawei (*)
DEBUG Adding direct dependency: spacy>=3.8.4
DEBUG Searching for a compatible version of test-spacy @ file:///C:/Users/jawei/test-spacy (*)
DEBUG Adding direct dependency: spacy>=3.8.4
DEBUG Searching for a compatible version of spacy (>=3.8.4)
DEBUG Selecting: spacy==3.8.4 [preference] (spacy-3.8.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for spacy==3.8.4: catalogue>=2.0.6, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: jinja2*
DEBUG Adding transitive dependency for spacy==3.8.4: langcodes>=3.2.0, <4.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: murmurhash>=0.28.0, <1.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: numpy{python_full_version >= '3.9'}>=1.19.0
DEBUG Adding transitive dependency for spacy==3.8.4: packaging>=20.0
DEBUG Adding transitive dependency for spacy==3.8.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: setuptools*
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-legacy>=3.0.11, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-loggers>=1.0.0, <2.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: thinc>=8.3.4, <8.4.0
DEBUG Adding transitive dependency for spacy==3.8.4: tqdm>=4.38.0, <5.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: wasabi>=0.9.1, <1.2.0
DEBUG Adding transitive dependency for spacy==3.8.4: weasel>=0.1.0, <0.5.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.0)
DEBUG Selecting: numpy==2.2.2 [preference] (numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for numpy==2.2.2: numpy==2.2.2
DEBUG Adding transitive dependency for numpy==2.2.2: numpy{python_full_version >= '3.9'}==2.2.2
DEBUG Searching for a compatible version of catalogue (>=2.0.6, <2.1.0)
DEBUG Selecting: catalogue==2.0.10 [preference] (catalogue-2.0.10-py3-none-any.whl)
DEBUG Searching for a compatible version of cymem (>=2.0.2, <2.1.0)
DEBUG Selecting: cymem==2.0.11 [preference] (cymem-2.0.11-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.5 [preference] (jinja2-3.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.5: markupsafe>=2.0
DEBUG Searching for a compatible version of langcodes (>=3.2.0, <4.0.0)
DEBUG Selecting: langcodes==3.5.0 [preference] (langcodes-3.5.0-py3-none-any.whl)
DEBUG Adding transitive dependency for langcodes==3.5.0: language-data>=1.2
DEBUG Searching for a compatible version of murmurhash (>=0.28.0, <1.1.0)
DEBUG Selecting: murmurhash==1.0.12 [preference] (murmurhash-1.0.12-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of numpy (==2.2.2)
DEBUG Selecting: numpy==2.2.2 [preference] (numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.2.2)
DEBUG Selecting: numpy==2.2.2 [preference] (numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of packaging (>=20.0)
DEBUG Selecting: packaging==24.2 [preference] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of preshed (>=3.0.2, <3.1.0)
DEBUG Selecting: preshed==3.0.9 [preference] (preshed-3.0.9-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for preshed==3.0.9: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for preshed==3.0.9: murmurhash>=0.28.0, <1.1.0
DEBUG Searching for a compatible version of pydantic (>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0)
DEBUG Selecting: pydantic==2.10.5 [preference] (pydantic-2.10.5-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic==2.10.5: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.10.5: pydantic-core>=2.27.2, <2.27.2+
DEBUG Adding transitive dependency for pydantic==2.10.5: typing-extensions>=4.12.2
DEBUG Searching for a compatible version of pydantic-core (>=2.27.2, <2.27.2+)
DEBUG Selecting: pydantic-core==2.27.2 [preference] (pydantic_core-2.27.2-cp310-cp310-macosx_10_12_x86_64.whl)
DEBUG Adding transitive dependency for pydantic-core==2.27.2: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of requests (>=2.13.0, <3.0.0)
DEBUG Selecting: requests==2.32.3 [preference] (requests-2.32.3-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.8.0 [preference] (setuptools-75.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-legacy (>=3.0.11, <3.1.0)
DEBUG Selecting: spacy-legacy==3.0.12 [preference] (spacy_legacy-3.0.12-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-loggers (>=1.0.0, <2.0.0)
DEBUG Selecting: spacy-loggers==1.0.5 [preference] (spacy_loggers-1.0.5-py3-none-any.whl)
DEBUG Searching for a compatible version of srsly (>=2.4.3, <3.0.0)
DEBUG Selecting: srsly==2.5.1 [preference] (srsly-2.5.1-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for srsly==2.5.1: catalogue>=2.0.3, <2.1.0
DEBUG Searching for a compatible version of thinc (>=8.3.4, <8.4.0)
DEBUG Selecting: thinc==8.3.4 [preference] (thinc-8.3.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for thinc==8.3.4: blis>=1.2.0, <1.3.0
DEBUG Adding transitive dependency for thinc==8.3.4: catalogue>=2.0.4, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: confection>=0.0.1, <1.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: murmurhash>=1.0.2, <1.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: packaging>=20.0
DEBUG Adding transitive dependency for thinc==8.3.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: setuptools*
DEBUG Adding transitive dependency for thinc==8.3.4: srsly>=2.4.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: wasabi>=0.8.1, <1.2.0
DEBUG Searching for a compatible version of tqdm (>=4.38.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [preference] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of typer (>=0.3.0, <1.0.0)
DEBUG Selecting: typer==0.15.1 [preference] (typer-0.15.1-py3-none-any.whl)
DEBUG Adding transitive dependency for typer==0.15.1: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.1: rich>=10.11.0
DEBUG Adding transitive dependency for typer==0.15.1: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.1: typing-extensions>=3.7.4.3
DEBUG Searching for a compatible version of wasabi (>=0.9.1, <1.2.0)
DEBUG Selecting: wasabi==1.1.3 [preference] (wasabi-1.1.3-py3-none-any.whl)
DEBUG Adding transitive dependency for wasabi==1.1.3: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}>=0.4.6
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (>=0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of weasel (>=0.1.0, <0.5.0)
DEBUG Selecting: weasel==0.4.1 [preference] (weasel-0.4.1-py3-none-any.whl)
DEBUG Adding transitive dependency for weasel==0.4.1: cloudpathlib>=0.7.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: confection>=0.0.4, <0.2.0
DEBUG Adding transitive dependency for weasel==0.4.1: packaging>=20.0
DEBUG Adding transitive dependency for weasel==0.4.1: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: smart-open>=5.2.1, <8.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: wasabi>=0.9.1, <1.2.0
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [preference] (MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of language-data (>=1.2)
DEBUG Selecting: language-data==1.3.0 [preference] (language_data-1.3.0-py3-none-any.whl)
DEBUG Adding transitive dependency for language-data==1.3.0: marisa-trie>=1.1.0
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [preference] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.12.14 [preference] (certifi-2024.12.14-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.1 [preference] (charset_normalizer-3.4.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.10 [preference] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.3.0 [preference] (urllib3-2.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of blis (>=1.2.0, <1.3.0)
DEBUG Selecting: blis==1.2.0 [preference] (blis-1.2.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for blis==1.2.0: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Searching for a compatible version of confection (>=0.0.4, <0.2.0)
DEBUG Selecting: confection==0.1.5 [preference] (confection-0.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for confection==0.1.5: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for confection==0.1.5: srsly>=2.4.0, <3.0.0
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.8 [preference] (click-8.1.8-py3-none-any.whl)
DEBUG Adding transitive dependency for click==8.1.8: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of rich (>=10.11.0)
DEBUG Selecting: rich==13.9.4 [preference] (rich-13.9.4-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for rich==13.9.4: typing-extensions{python_full_version < '3.11'}>=4.0.0, <5.0
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (>=4.0.0, <5.0)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions==4.12.2
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions{python_full_version < '3.11'}==4.12.2
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (==4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [preference] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of cloudpathlib (>=0.7.0, <1.0.0)
DEBUG Selecting: cloudpathlib==0.20.0 [preference] (cloudpathlib-0.20.0-py3-none-any.whl)
DEBUG Adding transitive dependency for cloudpathlib==0.20.0: typing-extensions{python_full_version < '3.11'}>4
DEBUG Searching for a compatible version of smart-open (>=5.2.1, <8.0.0)
DEBUG Selecting: smart-open==7.1.0 [preference] (smart_open-7.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for smart-open==7.1.0: wrapt*
DEBUG Searching for a compatible version of marisa-trie (>=1.1.0)
DEBUG Selecting: marisa-trie==1.2.1 [preference] (marisa_trie-1.2.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Adding transitive dependency for marisa-trie==1.2.1: setuptools*
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [preference] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.1 [preference] (pygments-2.19.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (*)
DEBUG Selecting: wrapt==1.17.2 [preference] (wrapt-1.17.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [preference] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Tried 44 versions: annotated-types 1, blis 1, catalogue 1, certifi 1, charset-normalizer 1, click 1, cloudpathlib 1, colorama 1, confection 1, cymem 1, idna 1, jawei 1, jinja2 1, langcodes 1, language-data 1, marisa-trie 1, markdown-it-py 1, markupsafe 1, mdurl 1, murmurhash 1, numpy 1, packaging 1, preshed 1, pydantic 1, pydantic-core 1, pygments 1, requests 1, rich 1, setuptools 1, shellingham 1, smart-open 1, spacy 1, spacy-legacy 1, spacy-loggers 1, srsly 1, test-spacy 1, thinc 1, tqdm 1, typer 1, typing-extensions 1, urllib3 1, wasabi 1, weasel 1, wrapt 1
DEBUG split `python_full_version >= '3.10'` resolution took 0.009s
DEBUG Solving split (python_full_version == '3.9.*') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }]), range: RequiresPythonRange(LowerBound(Included("3.9")), UpperBound(Excluded("3.10"))) })
DEBUG Adding direct dependency: jawei*
DEBUG Adding direct dependency: test-spacy*
DEBUG Searching for a compatible version of jawei @ file:///C:/Users/jawei (*)
DEBUG Adding direct dependency: spacy>=3.8.4
DEBUG Searching for a compatible version of test-spacy @ file:///C:/Users/jawei/test-spacy (*)
DEBUG Adding direct dependency: spacy>=3.8.4
DEBUG Searching for a compatible version of spacy (>=3.8.4)
DEBUG Selecting: spacy==3.8.4 [preference] (spacy-3.8.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for spacy==3.8.4: catalogue>=2.0.6, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: jinja2*
DEBUG Adding transitive dependency for spacy==3.8.4: langcodes>=3.2.0, <4.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: murmurhash>=0.28.0, <1.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: numpy{python_full_version >= '3.9'}>=1.19.0
DEBUG Adding transitive dependency for spacy==3.8.4: packaging>=20.0
DEBUG Adding transitive dependency for spacy==3.8.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: setuptools*
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-legacy>=3.0.11, <3.1.0
DEBUG Adding transitive dependency for spacy==3.8.4: spacy-loggers>=1.0.0, <2.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: thinc>=8.3.4, <8.4.0
DEBUG Adding transitive dependency for spacy==3.8.4: tqdm>=4.38.0, <5.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for spacy==3.8.4: wasabi>=0.9.1, <1.2.0
DEBUG Adding transitive dependency for spacy==3.8.4: weasel>=0.1.0, <0.5.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.0)
DEBUG Selecting: numpy==2.0.2 [preference] (numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for numpy==2.0.2: numpy==2.0.2
DEBUG Adding transitive dependency for numpy==2.0.2: numpy{python_full_version >= '3.9'}==2.0.2
DEBUG Searching for a compatible version of catalogue (>=2.0.6, <2.1.0)
DEBUG Selecting: catalogue==2.0.10 [preference] (catalogue-2.0.10-py3-none-any.whl)
DEBUG Searching for a compatible version of cymem (>=2.0.2, <2.1.0)
DEBUG Selecting: cymem==2.0.11 [preference] (cymem-2.0.11-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.5 [preference] (jinja2-3.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.5: markupsafe>=2.0
DEBUG Searching for a compatible version of langcodes (>=3.2.0, <4.0.0)
DEBUG Selecting: langcodes==3.5.0 [preference] (langcodes-3.5.0-py3-none-any.whl)
DEBUG Adding transitive dependency for langcodes==3.5.0: language-data>=1.2
DEBUG Searching for a compatible version of murmurhash (>=0.28.0, <1.1.0)
DEBUG Selecting: murmurhash==1.0.12 [preference] (murmurhash-1.0.12-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of numpy (==2.0.2)
DEBUG Selecting: numpy==2.0.2 [preference] (numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.0.2)
DEBUG Selecting: numpy==2.0.2 [preference] (numpy-2.0.2-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of packaging (>=20.0)
DEBUG Selecting: packaging==24.2 [preference] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of preshed (>=3.0.2, <3.1.0)
DEBUG Selecting: preshed==3.0.9 [preference] (preshed-3.0.9-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for preshed==3.0.9: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for preshed==3.0.9: murmurhash>=0.28.0, <1.1.0
DEBUG Searching for a compatible version of pydantic (>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0)
DEBUG Selecting: pydantic==2.10.5 [preference] (pydantic-2.10.5-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic==2.10.5: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.10.5: pydantic-core>=2.27.2, <2.27.2+
DEBUG Adding transitive dependency for pydantic==2.10.5: typing-extensions>=4.12.2
DEBUG Searching for a compatible version of pydantic-core (>=2.27.2, <2.27.2+)
DEBUG Selecting: pydantic-core==2.27.2 [preference] (pydantic_core-2.27.2-cp310-cp310-macosx_10_12_x86_64.whl)
DEBUG Adding transitive dependency for pydantic-core==2.27.2: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of requests (>=2.13.0, <3.0.0)
DEBUG Selecting: requests==2.32.3 [preference] (requests-2.32.3-py3-none-any.whl)
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.8.0 [preference] (setuptools-75.8.0-py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-legacy (>=3.0.11, <3.1.0)
DEBUG Selecting: spacy-legacy==3.0.12 [preference] (spacy_legacy-3.0.12-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of spacy-loggers (>=1.0.0, <2.0.0)
DEBUG Selecting: spacy-loggers==1.0.5 [preference] (spacy_loggers-1.0.5-py3-none-any.whl)
DEBUG Searching for a compatible version of srsly (>=2.4.3, <3.0.0)
DEBUG Selecting: srsly==2.5.1 [preference] (srsly-2.5.1-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for srsly==2.5.1: catalogue>=2.0.3, <2.1.0
DEBUG Searching for a compatible version of thinc (>=8.3.4, <8.4.0)
DEBUG Selecting: thinc==8.3.4 [preference] (thinc-8.3.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for thinc==8.3.4: blis>=1.2.0, <1.3.0
DEBUG Adding transitive dependency for thinc==8.3.4: catalogue>=2.0.4, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: confection>=0.0.1, <1.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: cymem>=2.0.2, <2.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: murmurhash>=1.0.2, <1.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: packaging>=20.0
DEBUG Adding transitive dependency for thinc==8.3.4: preshed>=3.0.2, <3.1.0
DEBUG Adding transitive dependency for thinc==8.3.4: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: setuptools*
DEBUG Adding transitive dependency for thinc==8.3.4: srsly>=2.4.0, <3.0.0
DEBUG Adding transitive dependency for thinc==8.3.4: wasabi>=0.8.1, <1.2.0
DEBUG Searching for a compatible version of tqdm (>=4.38.0, <5.0.0)
DEBUG Selecting: tqdm==4.67.1 [preference] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of typer (>=0.3.0, <1.0.0)
DEBUG Selecting: typer==0.15.1 [preference] (typer-0.15.1-py3-none-any.whl)
DEBUG Adding transitive dependency for typer==0.15.1: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.1: rich>=10.11.0
DEBUG Adding transitive dependency for typer==0.15.1: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.1: typing-extensions>=3.7.4.3
DEBUG Searching for a compatible version of wasabi (>=0.9.1, <1.2.0)
DEBUG Selecting: wasabi==1.1.3 [preference] (wasabi-1.1.3-py3-none-any.whl)
DEBUG Adding transitive dependency for wasabi==1.1.3: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}>=0.4.6
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (>=0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{python_full_version >= '3.7' and sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of weasel (>=0.1.0, <0.5.0)
DEBUG Selecting: weasel==0.4.1 [preference] (weasel-0.4.1-py3-none-any.whl)
DEBUG Adding transitive dependency for weasel==0.4.1: cloudpathlib>=0.7.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: confection>=0.0.4, <0.2.0
DEBUG Adding transitive dependency for weasel==0.4.1: packaging>=20.0
DEBUG Adding transitive dependency for weasel==0.4.1: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: requests>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: smart-open>=5.2.1, <8.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: srsly>=2.4.3, <3.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: typer>=0.3.0, <1.0.0
DEBUG Adding transitive dependency for weasel==0.4.1: wasabi>=0.9.1, <1.2.0
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [preference] (MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of language-data (>=1.2)
DEBUG Selecting: language-data==1.3.0 [preference] (language_data-1.3.0-py3-none-any.whl)
DEBUG Adding transitive dependency for language-data==1.3.0: marisa-trie>=1.1.0
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [preference] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.12.14 [preference] (certifi-2024.12.14-py3-none-any.whl)
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.4.1 [preference] (charset_normalizer-3.4.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.10 [preference] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.3.0 [preference] (urllib3-2.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of blis (>=1.2.0, <1.3.0)
DEBUG Selecting: blis==1.2.0 [preference] (blis-1.2.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for blis==1.2.0: numpy{python_full_version >= '3.9'}>=1.19.0, <3.0.0
DEBUG Searching for a compatible version of confection (>=0.0.4, <0.2.0)
DEBUG Selecting: confection==0.1.5 [preference] (confection-0.1.5-py3-none-any.whl)
DEBUG Adding transitive dependency for confection==0.1.5: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <3.0.0
DEBUG Adding transitive dependency for confection==0.1.5: srsly>=2.4.0, <3.0.0
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{python_full_version >= '3.7' and sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.8 [preference] (click-8.1.8-py3-none-any.whl)
DEBUG Adding transitive dependency for click==8.1.8: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of rich (>=10.11.0)
DEBUG Selecting: rich==13.9.4 [preference] (rich-13.9.4-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for rich==13.9.4: typing-extensions{python_full_version < '3.11'}>=4.0.0, <5.0
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (>=4.0.0, <5.0)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions==4.12.2
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions{python_full_version < '3.11'}==4.12.2
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (==4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [preference] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of cloudpathlib (>=0.7.0, <1.0.0)
DEBUG Selecting: cloudpathlib==0.20.0 [preference] (cloudpathlib-0.20.0-py3-none-any.whl)
DEBUG Adding transitive dependency for cloudpathlib==0.20.0: typing-extensions{python_full_version < '3.11'}>4
DEBUG Searching for a compatible version of smart-open (>=5.2.1, <8.0.0)
DEBUG Selecting: smart-open==7.1.0 [preference] (smart_open-7.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for smart-open==7.1.0: wrapt*
DEBUG Searching for a compatible version of marisa-trie (>=1.1.0)
DEBUG Selecting: marisa-trie==1.2.1 [preference] (marisa_trie-1.2.1-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Adding transitive dependency for marisa-trie==1.2.1: setuptools*
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [preference] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.1 [preference] (pygments-2.19.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (*)
DEBUG Selecting: wrapt==1.17.2 [preference] (wrapt-1.17.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [preference] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Tried 44 versions: annotated-types 1, blis 1, catalogue 1, certifi 1, charset-normalizer 1, click 1, cloudpathlib 1, colorama 1, confection 1, cymem 1, idna 1, jawei 1, jinja2 1, langcodes 1, language-data 1, marisa-trie 1, markdown-it-py 1, markupsafe 1, mdurl 1, murmurhash 1, numpy 1, packaging 1, preshed 1, pydantic 1, pydantic-core 1, pygments 1, requests 1, rich 1, setuptools 1, shellingham 1, smart-open 1, spacy 1, spacy-legacy 1, spacy-loggers 1, srsly 1, test-spacy 1, thinc 1, tqdm 1, typer 1, typing-extensions 1, urllib3 1, wasabi 1, weasel 1, wrapt 1
DEBUG split `python_full_version == '3.9.*'` resolution took 0.008s
INFO Solved your requirements for 2 environments
DEBUG Distinct solution for split (python_full_version >= '3.10') with 44 packages
DEBUG Distinct solution for split (python_full_version == '3.9.*') with 44 packages
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
error: Distribution `spacy==3.8.4 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

---

_Comment by @charliermarsh on 2025-01-20 17:24_

It looks like `uv add spacy` is using Python 3.9. SpaCy does not publish Python 3.9 wheels: https://pypi.org/project/spacy/#files. So you should use `3.10` in your `.python-version` file, rather than `3.9`.

See:

```
Using Python request `3.9` from version file at `C:\Users\jawei\.python-version`
```

---

_Comment by @jacobweiss2305 on 2025-01-20 19:02_

Thanks

---

_Closed by @jacobweiss2305 on 2025-01-20 19:02_

---
