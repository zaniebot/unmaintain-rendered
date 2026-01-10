---
number: 8948
title: "UV not found; Docker install fails with latest install and `v0.5.0`"
type: issue
state: closed
author: dwillhelm
labels: []
assignees: []
created_at: 2024-11-08T16:00:43Z
updated_at: 2025-04-01T08:34:24Z
url: https://github.com/astral-sh/uv/issues/8948
synced_at: 2026-01-10T01:24:34Z
---

# UV not found; Docker install fails with latest install and `v0.5.0`

---

_Issue opened by @dwillhelm on 2024-11-08 16:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi, love your work. 

My docker builds are failing this morning due to a `uv` installation issue. I'm following [this example](https://docs.astral.sh/uv/guides/integration/docker/#installing-uv:~:text=uvx%20/bin/-,Or%2C%20with%20the%20installer%3A,-Dockerfile) from the docs. I was able to fix it by pinning to a older version, but figured it was worthwhile to create the issue. 

In my dockerfile, using
```dockerfile
ADD https://astral.sh/uv/install.sh /uv-installer.sh
```
or 
```dockerfile 
ADD https://astral.sh/uv/0.5.0/install.sh /uv-installer.sh
```

Results in a 
```console 
RUN uv sync --frozen --no-cache

/bin/sh: 1: uv: not found 
```

Running with an older version, however, does not fail. For example 
```dockerfile 
ADD https://astral.sh/uv/0.4.17/install.sh /uv-installer.sh
```

Here's part of my `Dockerfile` just in case
```dockerfile
# this is my dockerfile
FROM python:3.11-slim

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/0.4.17/install.sh /uv-installer.sh # <-- pinning version fixed it for me! 

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.cargo/bin/:$PATH"

WORKDIR /backend
...
...

```


I resolved my issue by pinning the version, but figured others might come across the issue. Thanks

---

_Comment by @my1e5 on 2024-11-08 16:03_

Duplicate of https://github.com/astral-sh/uv/issues/8925

---

_Comment by @charliermarsh on 2024-11-08 16:03_

In v0.5, we moved the default install location to `~/.local/bin`. You can view the release notes here: https://github.com/astral-sh/uv/releases/tag/0.5.0. I think the issue is you need to change this line:
```
# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.cargo/bin/:$PATH"
```

---

_Comment by @dwillhelm on 2024-11-08 16:07_

That works! Thanks! 

---

_Closed by @zanieb on 2024-11-08 16:15_

---

_Referenced in [commaai/openpilot#33973](../../commaai/openpilot/pulls/33973.md) on 2024-11-09 04:41_

---

_Comment by @juan11iguel on 2025-01-28 13:19_

This should be updated in the [documentation](https://docs.astral.sh/uv/guides/integration/docker/#installing-uv)

---

_Comment by @charliermarsh on 2025-01-28 13:59_

Sorry, where? We don't reference `/root/.cargo/bin` on that page.

---

_Comment by @juan11iguel on 2025-01-28 16:55_

> Sorry, where? We don't reference `/root/.cargo/bin` on that page.

Sorry! Just checked again and you are right, I must have misread it before

---

_Comment by @MyNameIsNeXTSTEP on 2025-04-01 08:34_

@charliermarsh @juan11iguel If not referrencd in the docs, but the issue is here and resolved, why not add to docs too 🤔 ?

---
