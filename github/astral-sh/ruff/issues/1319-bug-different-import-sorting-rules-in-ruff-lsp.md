---
number: 1319
title: "[bug] Different import sorting rules in `ruff-lsp` and command line"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2022-12-22T00:56:59Z
updated_at: 2022-12-22T01:30:21Z
url: https://github.com/astral-sh/ruff/issues/1319
synced_at: 2026-01-07T13:12:14-06:00
---

# [bug] Different import sorting rules in `ruff-lsp` and command line

---

_Issue opened by @smackesey on 2022-12-22 00:56_

Sample case: file `python_modules/libraries/dagster-aws/dagster_aws/emr.py` in [Dagster](https://github.com/dagster-io/dagster). The language server does not recognize `dagster_aws` as first party, so it wants to do this:

```
# notice `dagster_aws` grouped with third-party imports
import boto3
import dagster
import dagster._check as check
from botocore.exceptions import WaiterError
from dagster_aws.utils.mrjob.utils import _boto3_now, _wrap_aws_client, strip_microseconds
```

Same result doing this:

```
$ cat python_modules/libraries/dagster-aws/dagster_aws/emr/emr.py | ruff --select=I001 --stdin-filename python_modules/libraries/dagster-aws/dagster_aws/emr/emr.py -
```

But command line `$ ruff .` from the repo root gives correct behavior of auto-detect of first-party package:

```
# `dagster_aws` correctly given first-party status
import boto3
import dagster
import dagster._check as check
from botocore.exceptions import WaiterError

from dagster_aws.utils.mrjob.utils import _boto3_now, _wrap_aws_client, strip_microseconds
```

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-22 01:05_

---

_Comment by @charliermarsh on 2022-12-22 01:06_

What's the right branch to test from?

---

_Label `bug` added by @charliermarsh on 2022-12-22 01:08_

---

_Comment by @charliermarsh on 2022-12-22 01:09_

Ah, yeah, this is an oversight. Will fix.

---

_Comment by @smackesey on 2022-12-22 01:11_

`sean/ruff` if you still need a branch

---

_Comment by @charliermarsh on 2022-12-22 01:13_

(Just forgot to thread through the `__init__.py` in the stdin case.)

---

_Referenced in [astral-sh/ruff#1321](../../astral-sh/ruff/pulls/1321.md) on 2022-12-22 01:27_

---

_Closed by @charliermarsh on 2022-12-22 01:30_

---

_Comment by @charliermarsh on 2022-12-22 01:30_

Will cut another release tonight to include this.

---
