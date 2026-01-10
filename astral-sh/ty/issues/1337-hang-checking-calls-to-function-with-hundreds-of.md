---
number: 1337
title: hang checking calls to function with hundreds of overloads (started in 0.0.1-alpha.22)
type: issue
state: closed
author: galah92
labels:
  - performance
  - overloads
  - hang
assignees: []
created_at: 2025-10-12T09:37:55Z
updated_at: 2026-01-08T23:55:41Z
url: https://github.com/astral-sh/ty/issues/1337
synced_at: 2026-01-10T01:48:23Z
---

# hang checking calls to function with hundreds of overloads (started in 0.0.1-alpha.22)

---

_Issue opened by @galah92 on 2025-10-12 09:37_

### Summary

Looks like something related to the search path.
Attached run logs on the same codebase with `alpha.21` and `alpha.22`.

[0.0.1-alpha.21.log](https://github.com/user-attachments/files/22870529/0.0.1-alpha.21.log)
[0.0.1-alpha.22.log](https://github.com/user-attachments/files/22870528/0.0.1-alpha.22.log)

### Version

_No response_

---

_Label `hang` added by @AlexWaygood on 2025-10-12 09:42_

---

_Comment by @MichaReiser on 2025-10-12 16:20_

The 0.22 log only contains a single message saying that all checks passed. 

Would you mind running ty with `ty check -v` or even `ty check -vv`? Is the project available open source (we could then try to run ty ourselves).

---

_Label `needs-info` added by @MichaReiser on 2025-10-12 16:20_

---

_Comment by @galah92 on 2025-10-13 05:33_

My bad, attached the updated log:

- `uvx ty@0.0.1-alpha.21 check -vv 2> 0.0.1-alpha.21.log`: [0.0.1-alpha.21.log](https://github.com/user-attachments/files/22877221/0.0.1-alpha.21.log)
- `uvx ty@0.0.1-alpha.22 check -vv 2> 0.0.1-alpha.22.log`: [0.0.1-alpha.22.log](https://github.com/user-attachments/files/22877220/0.0.1-alpha.22.log)

You'll notice 0.22 has many `types_aiobotocore_* not found in search paths`.

---

_Comment by @MichaReiser on 2025-10-13 06:44_

I can see that the performance significantely recreased between 21 and 22 (from ~100ms to 4s). 

The [logs](https://www.diffchecker.com/YYNjpbZV/) otherwise look mostly the same, including the many failed module resolutions.

It's a bit hard to tell from the logs which file took long to type check. We should probably add some timing information to our logs to make it easier to track those down.

For the failed imports: Is `/Users/galah92/Downloads/VayyarHomeCloud/python` your virtual environment? Are the dependencies that fail to resolve installed into that environment?

---

_Comment by @galah92 on 2025-10-13 06:47_

> For the failed imports: Is `/Users/galah92/Downloads/VayyarHomeCloud/python` your virtual environment? Are the dependencies that fail to resolve installed into that environment?

Yes, that's the root of my repo. I cannot share the code unfortunately. All dependencies were resolved correctly, `alpha.21` seem to find them all.
Is there anything else I can provide that will help to debug this?

---

_Comment by @MichaReiser on 2025-10-13 07:07_

> Yes, that's the root of my repo. I cannot share the code unfortunately. All dependencies were resolved correctly, alpha.21 seem to find them all.

I'm not sure about that. I see about the same amount of unresolved import errors for both versions (at least from the logs you shared).

>  Is there anything else I can provide that will help to debug this?

I put up https://github.com/astral-sh/ruff/pull/20836 which should help debug this in the future. What you could try now is to run the following command (assuming unix):

```
TY_MAX_PARALLELISM=1 ty check -vv
```

This reduces concurrency to 1 thread. The timing events in the logs should then tell us which file takes long to type check (unless there are multiple once). Once you konw the file, you can try to binary-search your way to find the problematic code:

* Try to comment out half of the imports. 
  * Is it still slow? Try to comment out the other half of the imports... and so on
  * Is it now fast? Revert, now comment out the other half of the imports...
* After the imports, apply the same mechanics to the body of the file. Comment out half of the file's body. Is it still slow? Comment out the half of the remaining uncommented code... 


Hopefully, this narrows the code down to a single Class, method, or a few lines. You could then try to obfuscate the code before sharing or try to describe the problematic code (like what is it doing? Is it a large dictionary or type dict, is it a very large union? ...)

I'm sorry that there isn't an easier way. 

We're also aware of some cases where ty is hanging (e.g. does your code look similar to https://github.com/astral-sh/ty/issues/1111) that we hope to address as part of the next release. You could also consider waiting and reporting back with the new release (should be released sometimes this week)

---

_Comment by @MichaReiser on 2025-10-17 09:09_

We released a new ty version. Do you want to give it a try? It should now also log files that take long when using `-vv`. 

---

_Comment by @galah92 on 2025-10-17 17:15_

Just did, there's still slowness since `alpha.22` that's present in `alpha.23`.

[0.0.1-alpha.21.log](https://github.com/user-attachments/files/22976518/0.0.1-alpha.21.log)
[0.0.1-alpha.22.log](https://github.com/user-attachments/files/22976517/0.0.1-alpha.22.log)
[0.0.1-alpha.23.log](https://github.com/user-attachments/files/22976516/0.0.1-alpha.23.log)

---

_Comment by @MichaReiser on 2025-10-17 17:19_

Thanks for testing again. It seems that ty spends the majority of the within those files

```
2025-10-17 20:14:19.974382 INFO Checking file `/Users/galah92/Downloads/VayyarHomeCloud/python/care/core/fastapi_s3.py` took more than 100ms (2.789960458s)
2025-10-17 20:14:19.974846 INFO Checking file `/Users/galah92/Downloads/VayyarHomeCloud/python/care/conftest.py` took more than 100ms (2.860847125s)
```

Can you maybe tell me more about those files. Do they use specific third party libraries? Are there other langauge features that you make use of that you aren't using in any of the other files? Do they use very large literal types (e.g. dictionaries or lists)

---

_Comment by @galah92 on 2025-10-17 17:26_

Sure.

See [fastapi_s3.py](https://github.com/user-attachments/files/22976965/fastapi_s3.py)

As for `conftest.py`, these are the imports:
```python
import base64
import os
import random
import secrets
import uuid
from collections.abc import AsyncGenerator, AsyncIterator, Generator
from dataclasses import dataclass
from datetime import UTC, datetime, timedelta
from random import choice
from string import ascii_lowercase

import aioboto3
import pytest
import pytest_asyncio
from argon2 import PasswordHasher
from firebase_admin import auth  # type: ignore
from pytest_asyncio import is_async_test
from types_aiobotocore_s3.client import S3Client
```
There's nothing special there, it's mostly pytest fixtures for our specific business logic.

I believe the common thing between the two is the usage of `types_aiobotocore_s3`.

---

_Comment by @MichaReiser on 2025-10-21 08:47_

Thanks for the additional information. I tried to reproduce the hang with the `fastapi_s3.py` but I'm unable to reproduce the hang

```
2025-10-21 10:45:09.232145 DEBUG Version: 0.0.1-alpha.23 (5c51b8480 2025-10-16)
2025-10-21 10:45:09.23385 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-10-21 10:45:09.234047 DEBUG Searching for a project in '/Users/micha/astral/test/1337'
2025-10-21 10:45:09.234871 DEBUG Found project at '/Users/micha/astral/test/1337'
2025-10-21 10:45:09.234886 DEBUG Searching for a user-level configuration at `/Users/micha/.config/ty/ty.toml`
2025-10-21 10:45:09.238958 DEBUG Discovering virtual environment in `/Users/micha/astral/test/1337`
2025-10-21 10:45:09.238982 DEBUG Attempting to parse virtual environment metadata at '/Users/micha/astral/test/1337/.venv/pyvenv.cfg'
2025-10-21 10:45:09.239336 DEBUG Searching for site-packages directory in sys.prefix /Users/micha/astral/test/1337/.venv
2025-10-21 10:45:09.23993 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/micha/astral/test/1337/.venv/lib/python3.14/site-packages"})
2025-10-21 10:45:09.239989 DEBUG Searching for real stdlib directory in sys.prefix /Users/micha/.local/share/uv/python/cpython-3.14.0-macos-aarch64-none
2025-10-21 10:45:09.239995 DEBUG Resolved real stdlib path for this virtual environment is: /Users/micha/.local/share/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14
2025-10-21 10:45:09.240001 DEBUG Including `.` in `environment.root`
2025-10-21 10:45:09.240016 DEBUG Adding first-party search path `/Users/micha/astral/test/1337`
2025-10-21 10:45:09.240029 DEBUG Using vendored stdlib
2025-10-21 10:45:09.24093 DEBUG Adding site-packages search path `/Users/micha/astral/test/1337/.venv/lib/python3.14/site-packages`
2025-10-21 10:45:09.240936 INFO Python version: Python 3.14, platform: linux
2025-10-21 10:45:09.240968 DEBUG Adding new file root '/Users/micha/astral/test/1337/.venv/lib/python3.14/site-packages' of kind LibrarySearchPath
2025-10-21 10:45:09.243478 DEBUG Adding new file root '/Users/micha/astral/test/1337' of kind Project
2025-10-21 10:45:09.243997 DEBUG Starting main loop
2025-10-21 10:45:09.244005 DEBUG Waiting for next main loop message.
2025-10-21 10:45:09.244052 DEBUG Checking all files in project 'ty1337'
2025-10-21 10:45:09.248506 INFO Indexed 1 file(s) in 0.004s
2025-10-21 10:45:09.249358 DEBUG Checking file '/Users/micha/astral/test/1337/main.py'
2025-10-21 10:45:09.267588 DEBUG Resolving dynamic module resolution paths
2025-10-21 10:45:09.28292 DEBUG Checking all files took 0.034s
All checks passed!
2025-10-21 10:45:09.282993 DEBUG Exiting main loop
```

This is my `pyproject.toml`

```toml
[project]
name = "ty1337"
version = "0.0.1"
dependencies = [
    "aioboto3>=15.4.0",
    "fastapi>=0.119.1",
    "types-aiobotocore[s3]>=2.25.0",
]

[tool.ty]

```

And running ty

```bash
uvx ty@latest check -vv --python-platform linux --python-version 3.12
2025-10-21 10:46:48.424481 DEBUG Version: 0.0.1-alpha.23 (5c51b8480 2025-10-16)
2025-10-21 10:46:48.426117 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-10-21 10:46:48.42632 DEBUG Searching for a project in '/Users/micha/astral/test/1337'
2025-10-21 10:46:48.427035 DEBUG Found project at '/Users/micha/astral/test/1337'
2025-10-21 10:46:48.427051 DEBUG Searching for a user-level configuration at `/Users/micha/.config/ty/ty.toml`
2025-10-21 10:46:48.431019 DEBUG Discovering virtual environment in `/Users/micha/astral/test/1337`
2025-10-21 10:46:48.431043 DEBUG Attempting to parse virtual environment metadata at '/Users/micha/astral/test/1337/.venv/pyvenv.cfg'
2025-10-21 10:46:48.43128 DEBUG Searching for site-packages directory in sys.prefix /Users/micha/astral/test/1337/.venv
2025-10-21 10:46:48.431837 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/micha/astral/test/1337/.venv/lib/python3.14/site-packages"})
2025-10-21 10:46:48.431889 DEBUG Searching for real stdlib directory in sys.prefix /Users/micha/.local/share/uv/python/cpython-3.14.0-macos-aarch64-none
2025-10-21 10:46:48.431895 DEBUG Resolved real stdlib path for this virtual environment is: /Users/micha/.local/share/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14
2025-10-21 10:46:48.431901 DEBUG Including `.` in `environment.root`
2025-10-21 10:46:48.431916 DEBUG Adding first-party search path `/Users/micha/astral/test/1337`
2025-10-21 10:46:48.43193 DEBUG Using vendored stdlib
2025-10-21 10:46:48.432806 DEBUG Adding site-packages search path `/Users/micha/astral/test/1337/.venv/lib/python3.14/site-packages`
2025-10-21 10:46:48.432814 INFO Python version: Python 3.12, platform: linux
2025-10-21 10:46:48.432859 DEBUG Adding new file root '/Users/micha/astral/test/1337/.venv/lib/python3.14/site-packages' of kind LibrarySearchPath
2025-10-21 10:46:48.435221 DEBUG Adding new file root '/Users/micha/astral/test/1337' of kind Project
2025-10-21 10:46:48.435556 DEBUG Starting main loop
2025-10-21 10:46:48.435564 DEBUG Waiting for next main loop message.
2025-10-21 10:46:48.435606 DEBUG Checking all files in project 'ty1337'
2025-10-21 10:46:48.438795 INFO Indexed 1 file(s) in 0.003s
2025-10-21 10:46:48.439258 DEBUG Checking file '/Users/micha/astral/test/1337/main.py'
2025-10-21 10:46:48.449025 DEBUG Resolving dynamic module resolution paths
2025-10-21 10:46:48.462412 DEBUG Checking all files took 0.024s
All checks passed!
2025-10-21 10:46:48.462485 DEBUG Exiting main loop
```

What are the dependencies that you've installed?

---

_Comment by @galah92 on 2025-10-21 08:57_

```toml
[project]
name = "care"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "fastapi[all]>=0.116.1",
    "scalar-fastapi>=1.0.3",
    "jinja2-fragments>=1.9.0",
    "httpx>=0.28.0",
    "httpx-auth>=0.23.1",
    "argon2-cffi>=25.1.0",
    "google-cloud-bigquery>=3.33.0",
    "google-cloud-bigquery-storage>=2.31.0",
    "google-cloud-pubsub>=2.29.0",
    "pandas>=2.3.2",
    "scikit-learn>=1.6.1",
    "pyyaml>=6.0.2",
    "firebase-admin>=6.5.0",
    "asyncpg>=0.30.0",
    "db-dtypes>=1.2.0",
    "aioboto3==15.2.0",
    "protobuf>=6.31.1",
    "bcrypt>=5.0.0",
    "grpcio-tools>=1.70.0",
    "pyjwt>=2.10.1",
    "cryptography>=46.0.2",
    "twilio>=9.8.4",
]

[dependency-groups]
dev = [
    "ruff>=0.13.1",
    "ty==0.0.1a21",
    "mypy>=1.15.0",
    "types-protobuf>=6.30.2.20250516",
    "pytest>=8.3.4",
    "pytest-watcher>=0.4.3",
    "pytest-mock>=3.14.0",
    "pytest-env>=1.1.5",
    "pytest-asyncio>=0.24.0",
    "types-aioboto3>=15.2.0",
    "types-aiobotocore-s3>=2.24.3",
    "types-pyyaml>=6.0.12.20240808",
    "asyncpg-stubs>=0.30.0",
    "protobuf-protoc-bin>=31.1",
    "pandas-stubs>=2.3.2.250827",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Comment by @MichaReiser on 2025-10-21 09:06_

Thanks, that's helpful. I can reproduce with

```py
from collections.abc import  AsyncIterator

import aioboto3
from types_aiobotocore_s3.client import S3Client

async def create_storage_client(
    endpoint_url: str,
    access_key_id: str,
    secret_access_key: str,
    region_name: str,
) -> AsyncIterator[S3Client]:
    session = aioboto3.Session()
    async with session.client(
        "s3",
        endpoint_url=endpoint_url,
        aws_access_key_id=access_key_id,
        aws_secret_access_key=secret_access_key,
        region_name=region_name,
    ) as client:
        yield client

```

and 

```toml
[project]
name = "ty1337"
version = "0.0.1"
requires-python = ">=3.12"
dependencies = [
    "aioboto3==15.2.0",
    "types-aioboto3>=15.2.0",
    "types-aiobotocore-s3>=2.24.3",
]

[tool.ty]

```

Installing `types-aiobotocore[full]` does help with the many unresolved import warnings but it isn't the cause for ty being slow

---

_Label `needs-info` removed by @MichaReiser on 2025-10-21 09:06_

---

_Comment by @galah92 on 2025-10-21 09:11_

Great! If there are other ways for me to help please let me know.

---

_Comment by @MichaReiser on 2025-10-21 09:15_

An even simpler repro:

```py
from collections.abc import  AsyncIterator

import aioboto3
from types_aiobotocore_s3.client import S3Client

def create_storage_client(
    endpoint_url: str,
    access_key_id: str,
    secret_access_key: str,
    region_name: str,
) -> AsyncIterator[S3Client]:
    session = aioboto3.Session()
    client = session.client(
        "s3",
        endpoint_url=endpoint_url,
        aws_access_key_id=access_key_id,
        aws_secret_access_key=secret_access_key,
        region_name=region_name,
    )
        
```

And the culprint is `session.py` which has a ton of `client` overloads: https://gist.github.com/MichaReiser/6f3149be6dcea72608a4bb81f11dbed6


They all have the same signature except for the first literal. 

I think this is somewhat a known issue with the stubs

> ```bash
> # Lite version does not provide session.client/resource overloads
> # it is more RAM-friendly, but requires explicit type annotations
> python -m pip install 'types-aiobotocore-lite[s3]'
> ```
> https://pypi.org/project/types-aiobotocore-s3/#generate-locally-(recommended)

CC: @dhruvmanila (I don't think this is urgent)

---

_Label `overloads` added by @AlexWaygood on 2025-10-21 09:19_

---

_Comment by @MichaReiser on 2025-10-21 09:34_

Note: I'm not sure if it's overload-related, but it certainly looks very suspicious. But it might also be a combination of overloads and "something else"

---

_Comment by @dhruvmanila on 2025-10-22 08:17_

Wow, there are like ~400 overloads for the `client` function.

The example code that I've used for debugging is the one that Micha posted in the latest comment on this issue (https://github.com/astral-sh/ty/issues/1337#issuecomment-3425567191).

I added a tracing message similar to https://github.com/astral-sh/ruff/pull/20836 to see if there's any callable which takes a long time to type check (>= 100ms) and there isn't any.

So, I went back to see the commits related to the overload work between 0.0.1-alpha.21 and 0.0.1-alpha.22 and there are two commits, both of which doesn't result in the hang but the commit that does is somewhere between them:
1. https://github.com/astral-sh/ruff/commit/35ed55ec8c9e952609e2065467b01c58693f865a - the hang doesn't exists here
2. https://github.com/astral-sh/ruff/commit/4e94b22815c7979003d07a4931c9174285617c90 - the hang already exists before this commit, I tried running the code on the parent commit (https://github.com/astral-sh/ruff/commit/0639da255263219e8cf0289301661b748681db7c)

So, I did a git bisect between (1) and parent commit of (2) and found out that https://github.com/astral-sh/ruff/pull/20517 is the first commit when the example code started regressing (sorry @sharkdp!). This doesn't necessarily mean that it's that specific feature that resulted in the performance regression, it could very well be that ty is doing more work due to that commit which could be unnecessary. I'll need to look deeper but this is what I've so far.

---

_Comment by @sharkdp on 2025-10-22 12:23_

> So, I did a git bisect between (1) and parent commit of (2) and found out that [astral-sh/ruff#20517](https://github.com/astral-sh/ruff/pull/20517) is the first commit when the example code started regressing (sorry [@sharkdp](https://github.com/sharkdp)!). This doesn't necessarily mean that it's that specific feature that resulted in the performance regression, it could very well be that ty is doing more work due to that commit which could be unnecessary

Yes, there certainly is more work to do now, because https://github.com/astral-sh/ruff/pull/20517 makes all of those overloads generic. I am fairly confident that 20517 itself is not the root cause here, and that something else needs to be optimized. We could potentially try to [make these checks](https://github.com/astral-sh/ruff/blob/2c9433796ac7e73667fd6bede25ee79238dc6805/crates/ty_python_semantic/src/types/signatures.rs#L1289-L1300) more aggressive, such that they won't recognize the specific overloads in question here as being generic (if that is indeed the case). But that won't help in similar cases where the methods are actually generic.

---

_Comment by @galah92 on 2025-10-26 08:22_

`ty@0.0.1-alpha.24` Cut these times in half

With `alpha.23`:
```
2025-10-26 10:20:02.491294 INFO Checking file `care/core/fastapi_s3.py` took more than 100ms (2.722958458s)
2025-10-26 10:20:02.49173 INFO Checking file `care/conftest.py` took more than 100ms (2.698604625s)
```

With `alpha.24`:
```
2025-10-26 10:20:16.438164 INFO Checking file `care/core/fastapi_s3.py` took more than 100ms (1.195371708s)
2025-10-26 10:20:16.438582 INFO Checking file `care/conftest.py` took more than 100ms (1.301141083s)
```

---

_Renamed from "0.0.1-alpha.22 is hanging" to "hang checking calls to function with hundreds of overloads (started in 0.0.1-alpha.22)" by @carljm on 2025-10-29 20:12_

---

_Comment by @galah92 on 2025-11-02 09:31_

`alpha.25` has similar performance to `alpha.24`.

---

_Label `performance` added by @carljm on 2025-11-03 18:23_

---

_Comment by @galah92 on 2025-11-04 04:44_

I'm also bumped into a similar performance gap with the following:
Create a new project, add `transformers`, and change main to just
```python
from transformers import AutoTokenizer
```
That's it.
> 2025-11-04 06:43:09.953352 INFO Checking file `/Users/galah92/Downloads/bla/repro.py` took more than 100ms (1.354159292s)

---

_Comment by @MichaReiser on 2025-11-14 08:22_

@carljm FYI, I moved this into stable

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:22_

---

_Comment by @carljm on 2026-01-08 23:54_

> I'm also bumped into a similar performance gap with the following: Create a new project, add `transformers`, and change main to just
> 
> from transformers import AutoTokenizer

I'm not able to repro this; that checks instantly for me. Maybe something has changed in ty or in transformers?

---

_Comment by @carljm on 2026-01-08 23:55_

I'm going to close this as duplicate of #2400, since that appears to be the same issue and has a simple repro with a lot less discussion to wade through. But once that is fixed we should double-check the original repro here as well.

---

_Closed by @carljm on 2026-01-08 23:55_

---
