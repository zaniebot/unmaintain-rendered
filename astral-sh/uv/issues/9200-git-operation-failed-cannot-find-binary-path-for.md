---
number: 9200
title: "Git operation failed cannot find binary path for git dependencies while doing `uv sync` in dockerfile"
type: issue
state: closed
author: fortminors
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-11-18T15:14:47Z
updated_at: 2024-11-18T17:41:23Z
url: https://github.com/astral-sh/uv/issues/9200
synced_at: 2026-01-10T01:24:37Z
---

# Git operation failed cannot find binary path for git dependencies while doing `uv sync` in dockerfile

---

_Issue opened by @fortminors on 2024-11-18 15:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am unable to use uv to install dependencies inside dockerfile during build, while having git dependencies.
I've created a fork of uv-docker-example repo at https://github.com/fortminors/uv-docker-example that reproduces the issue.
In particular, I've added httpx package as a source:
```toml
dependencies = [
    "httpx",
    "fastapi[standard]>=0.112.2",
]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx" }
```

Then I do `uv sync` to install the project locally and create the .lock file, followed by `docker build --progress plain -t uvex .`
I get the following output

```bash
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 1.26kB done
#1 DONE 0.0s

#2 [internal] load metadata for ghcr.io/astral-sh/uv:python3.11-bookworm-slim
#2 DONE 0.6s

#3 [internal] load .dockerignore
#3 transferring context: 106B done
#3 DONE 0.0s

#4 [stage-0 1/5] FROM ghcr.io/astral-sh/uv:python3.11-bookworm-slim@sha256:0329d3af09651d5fc7b7cce76844a2292ad1299d7972e3ceb8bc1d4f82485c63
#4 DONE 0.0s

#5 [stage-0 2/5] WORKDIR /app
#5 CACHED

#6 [internal] load build context
#6 transferring context: 4.31kB done
#6 DONE 0.0s

#7 [stage-0 3/5] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --verbose --frozen --no-install-project --no-dev
#7 0.110 DEBUG uv 0.5.2
#7 0.110 DEBUG Found project root: `/app`
#7 0.110 DEBUG No workspace root found, using project root
#7 0.110 DEBUG Using Python request `>=3.11` from `requires-python` metadata
#7 0.110 DEBUG Searching for Python >=3.11 in managed installations or search path
#7 0.110 DEBUG Searching for managed installations at `/root/.local/share/uv/python`
#7 0.112 DEBUG Found `cpython-3.11.10-linux-x86_64-gnu` at `/usr/local/bin/python3` (search path)
#7 0.112 Using CPython 3.11.10 interpreter at: /usr/local/bin/python3
#7 0.112 Creating virtual environment at: .venv
#7 0.116 DEBUG Omitting `uv-docker-example` from resolution due to `--no-install-project`
#7 0.117 DEBUG Using request timeout of 30s
#7 0.117 DEBUG Requirement already cached: fastapi==0.115.5
#7 0.117 DEBUG Identified uncached distribution: httpx @ git+https://github.com/encode/httpx@7b19cd5f4b749064c6452892a5e58a3758da1d1b
#7 0.117 DEBUG Requirement already cached: pydantic==2.8.2
#7 0.117 DEBUG Requirement already cached: starlette==0.41.2
#7 0.117 DEBUG Requirement already cached: typing-extensions==4.12.2
#7 0.117 DEBUG Requirement already cached: email-validator==2.2.0
#7 0.117 DEBUG Requirement already cached: fastapi-cli==0.0.5
#7 0.117 DEBUG Requirement already cached: jinja2==3.1.4
#7 0.117 DEBUG Requirement already cached: python-multipart==0.0.9
#7 0.117 DEBUG Requirement already cached: uvicorn==0.32.0
#7 0.117 DEBUG Requirement already cached: anyio==4.4.0
#7 0.117 DEBUG Requirement already cached: certifi==2024.8.30
#7 0.117 DEBUG Requirement already cached: httpcore==1.0.5
#7 0.117 DEBUG Requirement already cached: idna==3.8
#7 0.117 DEBUG Requirement already cached: annotated-types==0.7.0
#7 0.117 DEBUG Requirement already cached: pydantic-core==2.20.1
#7 0.117 DEBUG Requirement already cached: dnspython==2.6.1
#7 0.117 DEBUG Requirement already cached: typer==0.12.5
#7 0.117 DEBUG Requirement already cached: markupsafe==2.1.5
#7 0.117 DEBUG Requirement already cached: click==8.1.7
#7 0.117 DEBUG Requirement already cached: h11==0.14.0
#7 0.118 DEBUG Requirement already cached: httptools==0.6.1
#7 0.118 DEBUG Requirement already cached: python-dotenv==1.0.1
#7 0.118 DEBUG Requirement already cached: pyyaml==6.0.2
#7 0.118 DEBUG Requirement already cached: uvloop==0.20.0
#7 0.118 DEBUG Requirement already cached: watchfiles==0.23.0
#7 0.118 DEBUG Requirement already cached: websockets==13.0
#7 0.118 DEBUG Requirement already cached: sniffio==1.3.1
#7 0.118 DEBUG Requirement already cached: rich==13.8.0
#7 0.118 DEBUG Requirement already cached: shellingham==1.5.4
#7 0.118 DEBUG Requirement already cached: markdown-it-py==3.0.0
#7 0.118 DEBUG Requirement already cached: pygments==2.18.0
#7 0.118 DEBUG Requirement already cached: mdurl==0.1.2
#7 0.118 DEBUG Fetching source distribution from Git: https://github.com/encode/httpx
#7 0.118 DEBUG Acquired lock for `https://github.com/encode/httpx`
#7 0.118 DEBUG Updating Git source `https://github.com/encode/httpx`
#7 0.118 DEBUG Released lock at `/root/.cache/uv/git-v0/locks/4c0b1441d92956e1`
#7 0.119   × Failed to download and build `httpx @
#7 0.119   │ git+https://github.com/encode/httpx@7b19cd5f4b749064c6452892a5e58a3758da1d1b`
#7 0.119   ├─▶ Git operation failed
#7 0.119   ╰─▶ cannot find binary path
#7 0.119   help: `httpx` was included because `uv-docker-example==0.1.0` depends
#7 0.119         on `httpx`
#7 ERROR: process "/bin/sh -c uv sync --verbose --frozen --no-install-project --no-dev" did not complete successfully: exit code: 1
------
 > [stage-0 3/5] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --verbose --frozen --no-install-project --no-dev:
0.118 DEBUG Fetching source distribution from Git: https://github.com/encode/httpx
0.118 DEBUG Acquired lock for `https://github.com/encode/httpx`
0.118 DEBUG Updating Git source `https://github.com/encode/httpx`
0.118 DEBUG Released lock at `/root/.cache/uv/git-v0/locks/4c0b1441d92956e1`
0.119   × Failed to download and build `httpx @
0.119   │ git+https://github.com/encode/httpx@7b19cd5f4b749064c6452892a5e58a3758da1d1b`
0.119   ├─▶ Git operation failed
0.119   ╰─▶ cannot find binary path
0.119   help: `httpx` was included because `uv-docker-example==0.1.0` depends
0.119         on `httpx`
------
Dockerfile:14
--------------------
  13 |     # Install the project's dependencies using the lockfile and settings
  14 | >>> RUN --mount=type=cache,target=/root/.cache/uv \
  15 | >>>     --mount=type=bind,source=uv.lock,target=uv.lock \
  16 | >>>     --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
  17 | >>>     uv sync --verbose --frozen --no-install-project --no-dev
  18 |     
--------------------
ERROR: failed to solve: process "/bin/sh -c uv sync --verbose --frozen --no-install-project --no-dev" did not complete successfully: exit code: 1
```

I've checked it produces the same error no matter what package I put there with a link to repository. The logs above indicate that the package is uncached, even though uv cache is mounted in dockerfile, which seems weird.

```bash
user:~/uv-docker-example$ uv --version
uv 0.5.2
user:~/uv-docker-example$ whereis uv
uv: /home/user/.cargo/bin/uv /home/user/.local/bin/uv
```

```bash
user:~/uv-docker-example$ cat /etc/*-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.5 LTS"
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

---

_Comment by @charliermarsh on 2024-11-18 15:16_

I think it's struggling to find the `git` binary. Is Git installed in the container?

---

_Label `question` added by @charliermarsh on 2024-11-18 15:24_

---

_Comment by @fortminors on 2024-11-18 15:25_

@charliermarsh You're right, thank you! Missed that, thought it was installed in the base image

---

_Closed by @fortminors on 2024-11-18 15:25_

---

_Comment by @charliermarsh on 2024-11-18 15:27_

That "cannot find binary path" error message is pretty bad. I wonder if we can do better (it might be an "opaque" error that we can't detect).

---

_Comment by @fortminors on 2024-11-18 15:33_

It would be great if you could improve it, I initially thought it was looking for some binary inside the package and couldn't find it.
I've had this line in my original Dockerfile:
```Dockerfile
RUN apt-get update --quiet && apt-get install --quiet --no-install-recommends --assume-yes build-essential git ca-certificates
```
So I thought I was installing git, but I missed apt-get upgrade and -y options, so it should be that:

```Dockerfile
RUN apt-get update --quiet && apt-get upgrade -y && apt-get install --quiet --no-install-recommends --assume-yes -y build-essential git ca-certificates
```

---

_Comment by @charliermarsh on 2024-11-18 15:34_

Yeah that's a very reasonable thing to think given the error message. I'll re-open and we can take a look.

---

_Reopened by @charliermarsh on 2024-11-18 15:34_

---

_Label `question` removed by @charliermarsh on 2024-11-18 15:34_

---

_Label `help wanted` added by @charliermarsh on 2024-11-18 15:34_

---

_Label `error messages` added by @charliermarsh on 2024-11-18 15:34_

---

_Comment by @charliermarsh on 2024-11-18 15:34_

The action item here is: give a better error message when `git` isn't installed.

---

_Referenced in [astral-sh/uv#9206](../../astral-sh/uv/pulls/9206.md) on 2024-11-18 16:47_

---

_Closed by @charliermarsh on 2024-11-18 17:41_

---
