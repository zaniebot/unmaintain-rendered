---
number: 7128
title: Caching issue using a self-hosted runner in a GitHub action
type: issue
state: closed
author: mobidyc
labels: []
assignees: []
created_at: 2024-09-06T16:04:57Z
updated_at: 2024-09-06T17:57:14Z
url: https://github.com/astral-sh/uv/issues/7128
synced_at: 2026-01-07T13:12:17-06:00
---

# Caching issue using a self-hosted runner in a GitHub action

---

_Issue opened by @mobidyc on 2024-09-06 16:04_

Very strange behavior,
resolved using the `--no-cache` flag for both `uv pip compile` and `sync`.

The problem is that I do not understand why I do need the `--no-cache` flag
It happens only in my GitHub self-hosted runner using a homemade docker image _mobidyc/github-runner-custom:latest_

Note: Manually running the docker image does not trigger the issue.
Note 2: The requirements.txt file triggers no issue using the standard `pip install -r` command

uv version: `0.4.4`
platform: `linux x86`

env:
`UV_SYSTEM_PYTHON=1`

both the following commands trigger the same error:
```shell
uv pip compile --python 3.10 /requirements.txt > /tmp/pip_reqs.txt
uv pip sync --python 3.10 /tmp/pip_reqs.txt
```

error:
```shell
error: Failed to download and build `sentence-transformers==2.2.2`
  Caused by: /github/home/.cache/uv/built-wheels-v3/pypi/sentence-transformers/2.2.2/p4CnbxHIutHC6oUx_C88w/sentence-transformers-2.2.2.tar.gz does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` is present in the directory
Error: Process completed with exit code 2.
```

Please find attached a debug log
[uv-debug.log](https://github.com/user-attachments/files/16910388/uv-debug.log)


---

_Comment by @mobidyc on 2024-09-06 17:57_

I found that /github/home/ was a mount binding with certainly a corrupted  /github/home/.cache/uv/built-wheels-v3/pypi/sentence-transformers/2.2.2/p4CnbxHIutHC6oUx_C88w/sentence-transformers-2.2.2.tar.gz file.

I was sure that the Github container was a brand new instance at each run ....
Sorry for the noise.

---

_Closed by @mobidyc on 2024-09-06 17:57_

---
