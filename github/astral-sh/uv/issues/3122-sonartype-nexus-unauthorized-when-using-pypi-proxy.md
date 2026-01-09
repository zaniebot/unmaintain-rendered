---
number: 3122
title: Sonartype Nexus - Unauthorized when using PyPi Proxy
type: issue
state: closed
author: alelavml3
labels:
  - bug
  - registry
assignees: []
created_at: 2024-04-18T14:02:50Z
updated_at: 2024-04-19T15:12:01Z
url: https://github.com/astral-sh/uv/issues/3122
synced_at: 2026-01-07T13:12:17-06:00
---

# Sonartype Nexus - Unauthorized when using PyPi Proxy

---

_Issue opened by @alelavml3 on 2024-04-18 14:02_

Hi, with `v0.1.33`  release we are facing an authentication problems. We use a private `Sonatype Nexus` repository server which uses a private PyPi Repo to store our company private Python packages.
The command for compiling and install are the following:
```bash
export PYPI_INDEX_URL=https://$(NEXUS_USER):$(NEXUS_PWD)@$(NEXUS_SERVER_URL)
uv pip compile pyproject.toml -o requirements.txt --index-url $(PYPI_INDEX_URL)
uv pip sync requirements.txt --index-url $(PYPI_INDEX_URL)
```

The error we get is during `uv pip sync`:
```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: google-cloud-pubsub==2.21.0
  Caused by: HTTP status client error (401 Unauthorized) for url (<NEXUS_SERVER_URL>/packages/google-cloud-pubsub/2.21.0/google_cloud_pubsub-2.21.0-py2.py3-none-any.whl#sha256=fabd19e08faa1f70081b0e5ea003a3c031a1d2c1b798cf1be5ded2dbf1d1dbef)
```
here the package is `google-cloud-pubsub` but it happens with any random package.

How can we fix this issue with this new version? For now, we are using v0.1.32 and everything works.

Thank you in advance for your answers!

---

_Renamed from "Sonar" to "Sonartype Nexus - Unauthorized when using PyPi Proxy" by @alelavml3 on 2024-04-18 14:03_

---

_Comment by @zanieb on 2024-04-18 15:16_

Hi! Sorry about that, this looks like a bug. 

Can you provide logs with `RUST_LOG=trace uv pip install -v ...`?

---

_Label `bug` added by @zanieb on 2024-04-18 15:16_

---

_Label `registry` added by @zanieb on 2024-04-18 15:16_

---

_Assigned to @zanieb by @zanieb on 2024-04-18 15:16_

---

_Comment by @alelavml3 on 2024-04-18 15:33_

pyproject.toml
```toml
[project]
name = "my-project"
version = "0.1.0"
description = "my description"
authors = [ {name = "My Name", email = "myemail@example.com"}, ]
requires-python = ">= 3.8"
dependencies = [
    "matplotlib==3.7.4",
    "ruff"
]

[project.optional-dependencies]
build = ["twine", "build"]

[build-system]
requires = ["setuptools >= 61.0"]
build-backend = "setuptools.build_meta"
```

The output of command `RUST_LOG=trace uv pip sync requirements.txt --index-url https://nexus.internal/repository/pypi-group/simple --cache-dir .uv_cache --verbose` is uploaded here because it is too long:
https://paste.ec/paste/8pAMIGZV#+p-t45+mOpgwb4QaRm9R08tenqqcbrDNzXXUR0NLw9M

---

_Referenced in [astral-sh/uv#3123](../../astral-sh/uv/issues/3123.md) on 2024-04-18 16:09_

---

_Referenced in [astral-sh/uv#3124](../../astral-sh/uv/pulls/3124.md) on 2024-04-18 17:53_

---

_Referenced in [astral-sh/uv#3130](../../astral-sh/uv/pulls/3130.md) on 2024-04-18 22:44_

---

_Comment by @alelavml3 on 2024-04-19 06:07_

I tried the `v0.1.34` and it works. Thank you for the quick response!

---

_Closed by @alelavml3 on 2024-04-19 06:07_

---

_Comment by @zanieb on 2024-04-19 15:12_

Great to hear! Thanks for checking back in.

---
