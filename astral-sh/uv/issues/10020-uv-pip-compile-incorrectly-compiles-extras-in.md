```yaml
number: 10020
title: uv pip compile incorrectly compiles extras in requirements.txt
type: issue
state: closed
author: mumin91
labels:
  - question
assignees: []
created_at: 2024-12-19T06:58:26Z
updated_at: 2024-12-19T13:36:33Z
url: https://github.com/astral-sh/uv/issues/10020
synced_at: 2026-01-12T16:00:05Z
```

# uv pip compile incorrectly compiles extras in requirements.txt

---

_@mumin91_

### Version Information
- UV version: uv 0.5.10 (37b11ddb2 2024-12-17)
- OS: macOS 14.7.1
- Python: 3.12.7

### Steps to Reproduce
1. Create a new project:
```
mkdir uv_test
cd uv_test
```
2. Initiate UV project:
```
uv init .
```
3. Add sentry-sdk with fastapi extras:
```
uv add 'sentry-sdk[fastapi]'
```

4. This creates a pyproject.toml with:
```
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "sentry-sdk[fastapi]>=2.19.2",
]
```
5. Generate `requirements.txt`. Using `--versbose` for debug perpose:
```
uv pip compile pyproject.toml -o requirements.txt --verbose
```
the output:
```
DEBUG uv 0.5.10 (37b11ddb2 2024-12-17)
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/muminur.rahman/Desktop/uv_test/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.7 interpreter at .venv/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Found static `requires-dist` for: /Users/muminur.rahman/Desktop/uv_test
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: sentry-sdk[fastapi]>=2.19.2
DEBUG Found fresh response for: https://pypi.org/simple/sentry-sdk/
DEBUG Searching for a compatible version of sentry-sdk[fastapi] (>=2.19.2)
DEBUG Selecting: sentry-sdk==2.19.2 [preference] (sentry_sdk-2.19.2-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for sentry-sdk==2.19.2: sentry-sdk==2.19.2
DEBUG Adding transitive dependency for sentry-sdk==2.19.2: sentry-sdk[fastapi]==2.19.2
DEBUG Searching for a compatible version of sentry-sdk[fastapi] (==2.19.2)
DEBUG Selecting: sentry-sdk==2.19.2 [preference] (sentry_sdk-2.19.2-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/4d/74597bb6bcc23abc774b8901277652c61331a9d4d0a8d1bdb20679b9bbcb/sentry_sdk-2.19.2-py2.py3-none-any.whl.metadata
DEBUG Adding transitive dependency for sentry-sdk==2.19.2: fastapi>=0.79.0
DEBUG Searching for a compatible version of sentry-sdk (==2.19.2)
DEBUG Selecting: sentry-sdk==2.19.2 [preference] (sentry_sdk-2.19.2-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for sentry-sdk==2.19.2: urllib3>=1.26.11
DEBUG Adding transitive dependency for sentry-sdk==2.19.2: certifi*
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Found fresh response for: https://pypi.org/simple/fastapi/
DEBUG Searching for a compatible version of fastapi (>=0.79.0)
DEBUG Selecting: fastapi==0.115.6 [preference] (fastapi-0.115.6-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a5/32/8f6669fc4798494966bf446c8c4a162e0b5d893dff088afddf76414f70e1/certifi-2024.12.14-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ce/d9/5f4c13cecde62396b0d3fe530a50ccea91e7dfc1ccf0e09c228841bb5ba8/urllib3-2.2.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/52/b3/7e4df40e585df024fac2f80d1a2d579c854ac37109675db2b0cc22c0bb9e/fastapi-0.115.6-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for fastapi==0.115.6: starlette>=0.40.0, <0.42.0
DEBUG Adding transitive dependency for fastapi==0.115.6: pydantic>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <2.0.0 | >=2.0.0+, <2.0.1 | >=2.0.1+, <2.1.0 | >=2.1.0+, <3.0.0
DEBUG Adding transitive dependency for fastapi==0.115.6: typing-extensions>=4.8.0
DEBUG Searching for a compatible version of urllib3 (>=1.26.11)
DEBUG Selecting: urllib3==2.2.3 [preference] (urllib3-2.2.3-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (*)
DEBUG Selecting: certifi==2024.12.14 [preference] (certifi-2024.12.14-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/starlette/
DEBUG Searching for a compatible version of starlette (>=0.40.0, <0.42.0)
DEBUG Selecting: starlette==0.41.3 [preference] (starlette-0.41.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/96/00/2b325970b3060c7cecebab6d295afe763365822b1306a12eeab198f74323/starlette-0.41.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for starlette==0.41.3: anyio>=3.4.0, <5
DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
DEBUG Found fresh response for: https://pypi.org/simple/anyio/
DEBUG Searching for a compatible version of pydantic (>=1.7.4, <1.8 | >=1.8+, <1.8.1 | >=1.8.1+, <2.0.0 | >=2.0.0+, <2.0.1 | >=2.0.1+, <2.1.0 | >=2.1.0+, <3.0.0)
DEBUG Selecting: pydantic==2.10.4 [preference] (pydantic-2.10.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/7a/4daaf3b6c08ad7ceffea4634ec206faeff697526421c20f07628c7372156/anyio-4.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f3/26/3e1bbe954fde7ee22a6e7d31582c642aad9e84ffe4b5fb61e63b87cd326f/pydantic-2.10.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for pydantic==2.10.4: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.10.4: pydantic-core>=2.27.2, <2.27.2+
DEBUG Adding transitive dependency for pydantic==2.10.4: typing-extensions>=4.12.2
DEBUG Searching for a compatible version of typing-extensions (>=4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of anyio (>=3.4.0, <5)
DEBUG Selecting: anyio==4.7.0 [preference] (anyio-4.7.0-py3-none-any.whl)
DEBUG Adding transitive dependency for anyio==4.7.0: idna>=2.8
DEBUG Adding transitive dependency for anyio==4.7.0: sniffio>=1.1
DEBUG Adding transitive dependency for anyio==4.7.0: typing-extensions{python_full_version < '3.13'}>=4.5
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.5)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions==4.12.2
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions{python_full_version < '3.13'}==4.12.2
DEBUG Found fresh response for: https://pypi.org/simple/annotated-types/
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [preference] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/sniffio/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/pydantic-core/
DEBUG Searching for a compatible version of pydantic-core (>=2.27.2, <2.27.2+)
DEBUG Selecting: pydantic-core==2.27.2 [preference] (pydantic_core-2.27.2-cp312-cp312-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d3/f3/c97e80721735868313c58b89d2de85fa80fe8dfeeed84dc51598b92a135e/pydantic_core-2.27.2-cp312-cp312-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for pydantic-core==2.27.2: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of idna (>=2.8)
DEBUG Selecting: idna==3.10 [preference] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of sniffio (>=1.1)
DEBUG Selecting: sniffio==1.3.1 [preference] (sniffio-1.3.1-py3-none-any.whl)
DEBUG Tried 12 versions: annotated-types 1, anyio 1, certifi 1, fastapi 1, idna 1, pydantic 1, pydantic-core 1, sentry-sdk 1, sniffio 1, starlette 1, typing-extensions 1, urllib3 1
DEBUG marker environment resolution took 0.014s
Resolved 12 packages in 18ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml -o requirements.txt
annotated-types==0.7.0
    # via pydantic
anyio==4.7.0
    # via starlette
certifi==2024.12.14
    # via sentry-sdk
fastapi==0.115.6
    # via sentry-sdk
idna==3.10
    # via anyio
pydantic==2.10.4
    # via fastapi
pydantic-core==2.27.2
    # via pydantic
sentry-sdk==2.19.2
    # via uv-test (pyproject.toml)
sniffio==1.3.1
    # via anyio
starlette==0.41.3
    # via fastapi
typing-extensions==4.12.2
    # via
    #   anyio
    #   fastapi
    #   pydantic
    #   pydantic-core
urllib3==2.2.3
    # via sentry-sdk
```


### Expected Behavior
The generated `requirements.txt` should include `sentry-sdk[fastapi]==2.19.2` to maintain the extras specification.

### Actual Behavior
The generated `requirements.txt` includes just `sentry-sdk==2.19.2` without the `[fastapi]` extra specification.


### Additional Context
- Using `pip-tools` to generate `requirements.txt` correctly maintains the extras syntax
- While all dependencies are correctly resolved and installed, the lack of extras syntax in the `requirements.txt` causing issues where the extra is required. 

---

_Renamed from "uv pip compile does not add extra option to requirements.txt" to "uv pip compile incorrectly compiles extras in requirements.txt" by @mumin91 on 2024-12-19 07:44_

---

_Comment by @charliermarsh on 2024-12-19 13:36_

You're looking for the `--no-strip-extras` flag. For reference, this has no practical impact on behavior. The file functions exactly the same way with our without the extras, which is why they're omitted. It's only a superficial difference (i.e., a difference in appearance).

---

_Closed by @charliermarsh on 2024-12-19 13:36_

---

_Label `question` added by @charliermarsh on 2024-12-19 13:36_

---
