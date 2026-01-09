---
number: 9439
title: "`maturin pep517 build-wheel` fails when adding pydantic 2.4.2 on python 3.13"
type: issue
state: closed
author: v-zubarev
labels:
  - question
assignees: []
created_at: 2024-11-26T11:57:29Z
updated_at: 2024-11-26T14:26:28Z
url: https://github.com/astral-sh/uv/issues/9439
synced_at: 2026-01-07T13:12:18-06:00
---

# `maturin pep517 build-wheel` fails when adding pydantic 2.4.2 on python 3.13

---

_Issue opened by @v-zubarev on 2024-11-26 11:57_

Hi! 

I'm running this on:
* Ubuntu 22.04.5 LTS (Linux 5.15.167.4-microsoft-standard-WSL2). 
* uv 0.5.4 (Homebrew 2024-11-20)

How to reproduce:

```shell
uv init test-uv-pydantic-313
cd test-uv-pydantic-313/
uv venv -p 3.13
uv add "pydantic==2.4.2"
```

Output:

```shell
~/python/_test/test-uv-pydantic-313$ uv add "pydantic==2.4.2" --verbose
DEBUG uv 0.5.4 (Homebrew 2024-11-20)
DEBUG Found project root: `/home/vzubarev/python/_test/test-uv-pydantic-313`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/home/vzubarev/python/_test/test-uv-pydantic-313/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.13`
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-uv-pydantic-313 @ file:///home/vzubarev/python/_test/test-uv-pydantic-313
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched `requires-dist` for: `test-uv-pydantic-313==0.1.0`
  Expected: {Requirement { name: PackageName("pydantic"), extras: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.4.2" }]), index: None, conflict: None }, origin: None }}
  Actual: {}
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: test-uv-pydantic-313*
DEBUG Searching for a compatible version of test-uv-pydantic-313 @ file:///home/vzubarev/python/_test/test-uv-pydantic-313 (*)
DEBUG Adding transitive dependency for test-uv-pydantic-313==0.1.0: pydantic>=2.4.2, <2.4.2+
DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
DEBUG Searching for a compatible version of pydantic (>=2.4.2, <2.4.2+)
DEBUG Selecting: pydantic==2.4.2 [compatible] (pydantic-2.4.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/73/66/0a72c9fcde42e5650c8d8d5c5c1873b9a3893018020c77ca8eb62708b923/pydantic-2.4.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for pydantic==2.4.2: annotated-types>=0.4.0
DEBUG Adding transitive dependency for pydantic==2.4.2: pydantic-core>=2.10.1, <2.10.1+
DEBUG Adding transitive dependency for pydantic==2.4.2: typing-extensions>=4.6.1
DEBUG Found fresh response for: https://pypi.org/simple/annotated-types/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Searching for a compatible version of annotated-types (>=0.4.0)
DEBUG Selecting: annotated-types==0.7.0 [compatible] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/pydantic-core/
DEBUG Searching for a compatible version of pydantic-core (>=2.10.1, <2.10.1+)
DEBUG Selecting: pydantic-core==2.10.1 [compatible] (pydantic_core-2.10.1.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/33/50/e9ed10546bc169f49240e1bd5642fe2235a60bccfd5ec9e8f3dadee8932e/pydantic_core-2.10.1-cp310-cp310-macosx_10_7_x86_64.whl.metadata
DEBUG Adding transitive dependency for pydantic-core==2.10.1: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of typing-extensions (>=4.6.1, <4.7.0 | >=4.7.0+)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Tried 5 versions: annotated-types 1, pydantic 1, pydantic-core 1, test-uv-pydantic-313 1, typing-extensions 1
DEBUG all marker environments resolution took 0.011s
Resolved 5 packages in 13ms
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: pydantic==2.4.2
DEBUG Requirement already cached: annotated-types==0.7.0
DEBUG Identified uncached distribution: pydantic-core==2.10.1
DEBUG Requirement already cached: typing-extensions==4.12.2
DEBUG Acquired lock for `/home/vzubarev/.cache/uv/sdists-v6/pypi/pydantic-core/2.10.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/af/31/8e466c6ed47cddf23013d2f2ccf3fdb5b908ffa1d5c444150c41690d6eca/pydantic_core-2.10.1.tar.gz
DEBUG Building: pydantic-core==2.10.1
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: maturin>=1, <2
DEBUG Adding direct dependency: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/maturin/
DEBUG Searching for a compatible version of maturin (>=1, <2)
DEBUG Selecting: maturin==1.7.4 [compatible] (maturin-1.7.4-py3-none-manylinux_2_12_x86_64.manylinux2010_x86_64.musllinux_1_1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/97/5e2bfbcf42725ba5f64310423edcf00d90e684a61d55dd0a26b2313a44b6/maturin-1.7.4-py3-none-manylinux_2_12_x86_64.manylinux2010_x86_64.musllinux_1_1_x86_64.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=4.6.0, <4.7.0 | >=4.7.0+)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Tried 2 versions: maturin 1, typing-extensions 1
DEBUG marker environment resolution took 0.003s
DEBUG Installing in typing-extensions==4.12.2, maturin==1.7.4 in /home/vzubarev/.cache/uv/builds-v0/.tmpUy7Q9O
DEBUG Requirement already cached: typing-extensions==4.12.2
DEBUG Requirement already cached: maturin==1.7.4
DEBUG Installing build requirements: typing-extensions==4.12.2, maturin==1.7.4
DEBUG Creating PEP 517 build environment
DEBUG Calling `maturin.get_requires_for_build_wheel()`
DEBUG Calling `maturin.build_wheel("/home/vzubarev/.cache/uv/builds-v0/.tmpW2InKK", {}, None)`
DEBUG Running `maturin pep517 build-wheel -i /home/vzubarev/.cache/uv/builds-v0/.tmpUy7Q9O/bin/python --compatibility off`
DEBUG 💥 maturin failed
DEBUG   Caused by: Cargo metadata failed. Does your crate compile with `cargo build`?
DEBUG   Caused by: failed to start `cargo metadata`: Permission denied (os error 13)
DEBUG   Caused by: Permission denied (os error 13)
DEBUG Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/home/vzubarev/.cache/uv/builds-v0/.tmpUy7Q9O/bin/python', '--compatibility', 'off'] returned non-zero exit status 1
DEBUG Released lock at `/home/vzubarev/.cache/uv/sdists-v6/pypi/pydantic-core/2.10.1/.lock`
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
  × Failed to download and build `pydantic-core==2.10.1`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i /home/vzubarev/.cache/uv/builds-v0/.tmpUy7Q9O/bin/python --compatibility
      off`

      [stderr]
      💥 maturin failed
        Caused by: Cargo metadata failed. Does your crate compile with `cargo build`?
        Caused by: failed to start `cargo metadata`: Permission denied (os error 13)
        Caused by: Permission denied (os error 13)
      Error: command ['maturin', 'pep517', 'build-wheel', '-i',
      '/home/vzubarev/.cache/uv/builds-v0/.tmpUy7Q9O/bin/python', '--compatibility', 'off'] returned non-zero exit
      status 1

  help: `pydantic-core` (v2.10.1) was included because `test-uv-pydantic-313` (v0.1.0) depends on `pydantic`
        (v2.4.2) which depends on `pydantic-core`
```

What I've tried:
* it works with Python 3.12.7.
* it works with latest Pydantic + Python 3.13.
* `--no-cache` doesn't help.

I don't know the root cause or if this older version of Pydantic is even supported in 3.13, but that was not the displayed error, maybe the error text may be improved? This is by no means a deal-breaker for us, it's just something I wanted to report just in case. Thanks for the great tool!

---

_Comment by @FishAlchemist on 2024-11-26 12:34_

There is no wheel for pydantic 2.4.2 that supports Python 3.13, and the included pydantic-core 2.10.1 also doesn't have a wheel for Python 3.13.
And if you try to compile from source code,  pydantic-core v2 is written in Rust. On my Windows computer, even pip failed to compile successfully, so I don't think this issue is related to uv.

---

_Comment by @charliermarsh on 2024-11-26 14:26_

Yeah this seems like a problem with building Pydantic from source. I believe it's unrelated to uv.

---

_Closed by @charliermarsh on 2024-11-26 14:26_

---

_Label `question` added by @charliermarsh on 2024-11-26 14:26_

---
