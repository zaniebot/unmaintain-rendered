---
number: 6894
title: UV fails to fetch wheel when package is specified in build-system requires
type: issue
state: closed
author: CosmicallyCosmo
labels:
  - bug
assignees: []
created_at: 2024-08-31T17:28:29Z
updated_at: 2024-09-03T21:21:22Z
url: https://github.com/astral-sh/uv/issues/6894
synced_at: 2026-01-10T01:24:07Z
---

# UV fails to fetch wheel when package is specified in build-system requires

---

_Issue opened by @CosmicallyCosmo on 2024-08-31 17:28_

I've tested this with uv version 4.0.1 with `uv sync --reinstall`. It successfully installs the package when specified under `[dependencies]` but get the error below when it's under `[build-system] requires` (anonymized output):

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: test-project @ file:///home/test/test-project
  Caused by: Failed to install requirements from build-system.requires (install)
  Caused by: Failed to prepare distributions
  Caused by: Failed to fetch wheel: my-package==0.1.6+b11023884
  Caused by: HTTP status client error (404 Not Found) for url (https://a-gitlab-instance.com/api/v4/projects/1234/packages/pypi/files/my-hash/my-package-0.1.6+b11023884-py3-none-any.whl#sha256=my-hash)
```

The index where the package lives is specified as an `extra-index-url` authenticated with a token in the header. My `[build-system]` in my `pyproject.toml` looks like this:

```
[build-system]
requires = [
    "my-package",
    "setuptools",
]

build-backend = "setuptools.build_meta"
```



<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Label `bug` added by @charliermarsh on 2024-08-31 17:55_

---

_Comment by @charliermarsh on 2024-08-31 17:55_

Interesting... This _should_ work so sounds like a bug if it's behaving as described. Thanks.

---

_Comment by @charliermarsh on 2024-09-01 16:16_

How are you providing the index URL?

---

_Comment by @CosmicallyCosmo on 2024-09-02 08:56_

Hi, I'm providing them in the pyproject.toml as follows. The one containing this package is under `extra index-url`

```
[tool.uv]
index-url = "https://an-artifactory-instance.com/artifactory/api/pypi/python/simple"
extra-index-url = ["https://__token__:...@a-gitlab-instance.com/api/v4/projects/5678/packages/pypi/simple", "https://__token__:...@a-gitlab-instance.com/api/v4/projects/1234/packages/pypi/simple"]
```

The package is only present in `1234`, I'm fairly sure it's going to be something to do with the local version identifier.

---

_Comment by @CosmicallyCosmo on 2024-09-02 13:23_

Hi, I think this is potentially to do how I configure GitLab tokens and not an issue with UV. I use a different token for each registry (each shares the same hostname and port) which I've recently learnt shouldn't work. Sorry about this!

---

_Comment by @zanieb on 2024-09-03 14:44_

I think https://github.com/astral-sh/uv/pull/6389 might solve this as you could define your token for each URL prefix?

---

_Comment by @CosmicallyCosmo on 2024-09-03 21:21_

Ah yes probably. I've kind of worked around this by using a ci token which has access to both repos but that'd certainly be useful for local developer rigs.

---

_Closed by @CosmicallyCosmo on 2024-09-03 21:21_

---
