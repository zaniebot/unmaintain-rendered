---
number: 9749
title: "S113 only considers `timeout=` probable, unpacking is not recognised."
type: issue
state: open
author: rogier-stegeman
labels:
  - question
assignees: []
created_at: 2024-02-01T07:49:36Z
updated_at: 2024-07-24T20:25:02Z
url: https://github.com/astral-sh/ruff/issues/9749
synced_at: 2026-01-07T13:12:15-06:00
---

# S113 only considers `timeout=` probable, unpacking is not recognised.

---

_Issue opened by @rogier-stegeman on 2024-02-01 07:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Rule [S113](https://docs.astral.sh/ruff/rules/request-without-timeout/#request-without-timeout-s113) does not consider anything except a hard-coded `request=` to be "probable". This can be frustrating when one has a lot of configuration which is shared between many `requests` calls.

```python
from typing import TypedDict
import requests

requests.post('test', **{"timeout": 1000})

class Options(TypedDict):
    timeout: int

options: Options = {"timeout": 1000}

requests.post('test', **options)
```

`ruff --select S113 --isolated test.py`

> test.py:4:1: S113 Probable use of requests call without timeout
> test.py:11:1: S113 Probable use of requests call without timeout
> Found 2 errors.

Version: ruff 0.1.15

---

_Comment by @charliermarsh on 2024-02-01 14:13_

We could probably get this right in limited cases (e.g., defined in the same file, variable is _not_ overloaded or assigned to multiple times). What's the "real" setup that you have in your project? Mostly want to check if those "limited cases" would be useful to you or not.

---

_Label `question` added by @charliermarsh on 2024-02-01 14:13_

---

_Comment by @rogier-stegeman on 2024-02-01 14:38_

I can't share the actual code, but basically something like this.

```python
class RequestOptions(TypedDict):
    headers: dict[str, str]
    timeout: int

class MyClass:
    def __init__(self) -> None:
        """Initialises settings."""
        self.options: RequestOptions = { 
          'headers': {'Authorization': f'Bearer {env.TIMECHIMP_TOKEN}'},
          'timeout': 20000
        }

    def get(self, endpoint: str) -> requests.Response:
        # More function parameters and request customisation here, irrelevant.
        return requests.get(endpoint, **self.options)
    
    # A lot more similar functions here, that all use self.options.
```

---

_Comment by @camerondavison on 2024-07-24 20:25_

This may not be what you need/want but I would say that what I am seen most people do is create a _internal_request or such method in the MyClass and then call that instead. this way you have all the requests going through 1 method with just different params. basically mimicing what requests does internally with get just proxying to requests('get', ...)

---
