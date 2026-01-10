---
number: 3279
title: uv does not support keyring after 0.1.32
type: issue
state: closed
author: mvaldi
labels:
  - question
assignees: []
created_at: 2024-04-26T14:20:12Z
updated_at: 2025-01-14T13:40:40Z
url: https://github.com/astral-sh/uv/issues/3279
synced_at: 2026-01-10T01:23:26Z
---

# uv does not support keyring after 0.1.32

---

_Issue opened by @mvaldi on 2024-04-26 14:20_

Hi astral team, I love this repository and save me a lot of time in github actions.

I have a problem trying to install a private repository with gcp using uv with docker

This is the dockerfile that contain the error:

<details>

<summary>Dockerfile with error</summary>

```dockerfile
FROM python:3.10.14-bullseye

RUN pip install -U pip "uv==0.1.33" -q

ARG GOOGLE_APPLICATION_CREDENTIALS_PATH="./gcp.json"

COPY $GOOGLE_APPLICATION_CREDENTIALS_PATH /tmp/credentials.json
ENV GOOGLE_APPLICATION_CREDENTIALS /tmp/credentials.json

RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-package
```

</details>

it throw the follow error

<details>

<summary>Detailed error</summary>

```
 => ERROR [4/4] RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q &  2.9s 
------                                                                                               
 > [4/4] RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-package:
2.838 error: HTTP status client error (401 Unauthorized) for url (https://location-python.pkg.dev/project-id/package-name/simple/private-package/)
------
Dockerfile:11
--------------------
   9 |     
  10 |     # RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-package
  11 | >>> RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-package
  12 |     
--------------------
ERROR: failed to solve: process "/bin/sh -c uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-repository" did not complete successfully: exit code: 2
```
</details>

and this error appear for the next versions like

- `0.1.33`
- `0.1.34`
- `0.1.35`
- `0.1.36`
- `0.1.37`
- `0.1.38`

But if we use the uv version `0.1.32` it works

<details>

<summary>Dockerfile worked</summary>

```dockerfile
FROM python:3.10.14-bullseye

RUN pip install -U pip "uv==0.1.32" -q

ARG GOOGLE_APPLICATION_CREDENTIALS_PATH="./gcp.json"

COPY $GOOGLE_APPLICATION_CREDENTIALS_PATH /tmp/credentials.json
ENV GOOGLE_APPLICATION_CREDENTIALS /tmp/credentials.json

RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-package
```

</details>

so, something it change and break the feature

Thanks you!

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Closed by @mvaldi on 2024-04-26 14:20_

---

_Reopened by @mvaldi on 2024-04-26 14:20_

---

_Comment by @charliermarsh on 2024-04-26 14:25_

Can you include the uv logs from running with `--verbose`?

---

_Comment by @mvaldi on 2024-04-26 14:33_

Sure!

<details>

<summary>Detailed error with -v</summary>

```Dockerfile

FROM python:3.10.14-bullseye

RUN pip install -U pip "uv==0.1.33" -q

ARG GOOGLE_APPLICATION_CREDENTIALS_PATH="./gcp.json"

COPY $GOOGLE_APPLICATION_CREDENTIALS_PATH /tmp/credentials.json
ENV GOOGLE_APPLICATION_CREDENTIALS /tmp/credentials.json

RUN uv pip install --system keyring keyrings.google-artifactregistry-auth -q && uv pip install --system --keyring-provider subprocess --extra-index-url https://location-python.pkg.dev/project-id/package-name/simple private-package -v

```

```
1.163 DEBUG Starting interpreter discovery for default Python                     
1.163 DEBUG Cached interpreter info for Python 3.10.14, skipping probing: usr/local/bin/python3
1.163 DEBUG Using Python 3.10.14 environment at usr/local/bin/python3
1.163 DEBUG Trying to lock if free: tmp/uv-39ee7c309557c5e9.lock
1.164 DEBUG Using registry request timeout of 300s
1.164 DEBUG Solving with target Python version 3.10.14
1.164 DEBUG Adding direct dependency: private-package*
1.164 TRACE Fetching metadata for private-package from https://location-python.pkg.dev/project-id/package-name/simple/private-package/
1.165 TRACE No cache entry exists for /root/.cache/uv/simple-v7/65a3230ca73dc537/private-package.rkyv
1.165 DEBUG No cache entry for: https://location-python.pkg.dev/project-id/package-name/simple/prisma-workflows/
1.165 TRACE Sending fresh GET request for https://location-python.pkg.dev/project-id/package-name/simple/private-package/
1.165 TRACE Handling request for https://location-python.pkg.dev/project-id/package-name/simple/private-package/
1.165 TRACE No credentials on request, checking cache...
1.165 TRACE No credentials in cache.
1.165 TRACE Skipping keyring lookup for https://location-python.pkg.dev/project-id/package-name/simple/private-package/ with no username
1.165 DEBUG No credentials found for https://location-python.pkg.dev/project-id/package-name/simple/private-package/
2.091 error: HTTP status client error (401 Unauthorized) for url (https://location-python.pkg.dev/project-id/package-name/simple/private-package/)
```

</details>

---

_Comment by @zanieb on 2024-04-26 14:36_

Hi! You need to provide a username for keyring.

See the [0.1.33 breaking changes](https://github.com/astral-sh/uv/blob/main/CHANGELOG.md#0133)


---

_Label `question` added by @zanieb on 2024-04-26 14:36_

---

_Comment by @mvaldi on 2024-04-26 14:40_

wonderful! it worked, thanks you so much!

---

_Comment by @zanieb on 2024-04-26 14:41_

You're welcome! Sorry for the breaking change — it seemed incorrect for us hard-code the username for a specific service.

---

_Closed by @zanieb on 2024-04-26 14:41_

---

_Comment by @mvaldi on 2024-04-26 17:05_

For the following people who come asking the same question, this was the solution:

uv pip install --system --keyring-provider subprocess --extra-index-url https://<strong>`oauth2accesstoken@`</strong>location-python.pkg.dev/project-id/package-name/simple private-package



---

_Comment by @erikdao on 2025-01-14 13:40_

@mvaldi Thank you very much for pointing this out. It saved me!

---
