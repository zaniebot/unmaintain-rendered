---
number: 8024
title: TCH004 false positive with boto3 stubs - bedrock-runtime
type: issue
state: closed
author: sansmoraxz
labels: []
assignees: []
created_at: 2023-10-17T20:42:44Z
updated_at: 2023-10-18T04:32:23Z
url: https://github.com/astral-sh/ruff/issues/8024
synced_at: 2026-01-07T13:12:15-06:00
---

# TCH004 false positive with boto3 stubs - bedrock-runtime

---

_Issue opened by @sansmoraxz on 2023-10-17 20:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Using the following python code

```py
import typing

import boto3

from .const import REGION

if typing.TYPE_CHECKING:
    from mypy_boto3_bedrock_runtime import BedrockRuntimeClient   # TCH004 flags here for some reason
    from mypy_boto3_s3 import S3Client
else:
    BedrockClient = boto3.client
    S3Client = boto3.client

def get_bedrock_client() -> BedrockRuntimeClient:
    """Return bedrock client."""
    from botocore.config import Config
    config = Config(
    retries = {
            'max_attempts': 100,
            'mode': 'adaptive',
        },
    )
    return boto3.client(
        service_name='bedrock-runtime',
        region_name=REGION,
        config=config,
        endpoint_url=f'https://bedrock.{REGION}.amazonaws.com')

def get_s3_client() -> S3Client: # doesn't flag here
    """Return S3 client."""
    return boto3.client(
        service_name='s3',
        region_name=REGION)
```

Dependencies:-

```toml
[tool.poetry.group.dev.dependencies]
boto3 = "^1.28.64"
botocore = "^1.31.64"
boto3-stubs = {extras = ["bedrock", "bedrock-runtime", "s3"], version = "^1.28.65"}
ruff = "^0.1.0"
```
Wierdly enough it only flags for `bedrock-runtime`. Not `s3` and `bedrock` client.

---

_Comment by @charliermarsh on 2023-10-17 23:43_

I believe `BedrockRuntimeClient` _does_ need to be imported here. If you add `from __future__ import annotations`, it doesn't, but without `from __future__ import annotations`, Python _does_ need runtime access to return type annotations on functions. I would expect this code to fail at runtime -- does it not?

(Should `BedrockClient = boto3.client` be `BedrockRuntimeClient = boto3.client`?)

---

_Comment by @sansmoraxz on 2023-10-18 04:10_

All the mypy  imports here are for generating stubs.  https://pypi.org/project/boto3-stubs/ Specifically `boto3.client` builds and retuns a different type of client for each instance.

---

_Comment by @sansmoraxz on 2023-10-18 04:10_

Anyway I figured it out. it should've been `BedrockRuntimeClient` in the else part. Forgot to change that when migrating to newer boto3.

---

_Closed by @sansmoraxz on 2023-10-18 04:10_

---

_Comment by @charliermarsh on 2023-10-18 04:32_

Thanks, that was my second question, in parentheses!

---
