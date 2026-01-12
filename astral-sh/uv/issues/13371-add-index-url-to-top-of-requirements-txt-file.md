```yaml
number: 13371
title: "Add Index URL to Top of `requirements.txt` File Created with `uv export`"
type: issue
state: closed
author: krishsai7
labels:
  - enhancement
assignees: []
created_at: 2025-05-10T00:06:30Z
updated_at: 2025-05-10T12:24:20Z
url: https://github.com/astral-sh/uv/issues/13371
synced_at: 2026-01-12T16:01:26Z
```

# Add Index URL to Top of `requirements.txt` File Created with `uv export`

---

_@krishsai7_

### Summary

When I export a requirements.txt file using the following command, can either the default behavior or opt-in via flag be to list my index at the top of my file? We use a privately hosted Artifactory that all requests for package installs must go through and parsing the requirements.txt can support indexes listed at the top of the file.
Current example:
`pyproject.toml`
```
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "boto3[secretsmanager]>=1.36.2",
]
```

Command used: `uv export --no-hashes --format requirements.txt -o requirements.txt --no-dev --index https://privatepypiartifactory.com --no-header` Given that we also have a single Artifactory, I have this the actual index set within the `pyproject.toml` under the `[[tool.uv.lock]]` section but at home I don't have this URL set. In addition, this URL is set as my `UV_DEFAULT_INDEX` environment variable.

Current output:
```
boto3==1.38.13
    # via test-uv
botocore==1.38.13
    # via
    #   boto3
    #   s3transfer
jmespath==1.0.1
    # via
    #   boto3
    #   botocore
python-dateutil==2.9.0.post0
    # via botocore
s3transfer==0.12.0
    # via boto3
six==1.17.0
    # via python-dateutil
urllib3==2.4.0
    # via botocore
```

Current `uv` version: `uv 0.7.3 (3c413f74b 2025-05-07)`
This current behavior is consistent on both Windows 10 and MacOS.

### Example

AWS Glue 5.0 will accept a requirements.txt file with an index at the top so I would like to see an output such as:
```
-i https://privatepypiartifactory.com
boto3==1.38.13
    # via test-uv
botocore==1.38.13
    # via
    #   boto3
    #   s3transfer
jmespath==1.0.1
    # via
    #   boto3
    #   botocore
python-dateutil==2.9.0.post0
    # via botocore
s3transfer==0.12.0
    # via boto3
six==1.17.0
    # via python-dateutil
urllib3==2.4.0
    # via botocore
```

---

_Label `enhancement` added by @krishsai7 on 2025-05-10 00:06_

---

_Renamed from "Add Index URL to Top of `requirements.txt` File" to "Add Index URL to Top of `requirements.txt` File Created with `uv export`" by @krishsai7 on 2025-05-10 00:07_

---

_Comment by @zanieb on 2025-05-10 12:24_

Duplicate of https://github.com/astral-sh/uv/issues/10008

> We can't support this because our --index features are not representable in requirements.txt files. For example, there is no way to represent index pinning or uv's index strategies. It's possible there's something we could do here, but it's an open problem.

---

_Closed by @zanieb on 2025-05-10 12:24_

---
