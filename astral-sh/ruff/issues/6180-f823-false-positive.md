```yaml
number: 6180
title: "`F823` false positive"
type: issue
state: closed
author: bastimeyer
labels:
  - bug
assignees: []
created_at: 2023-07-30T14:00:26Z
updated_at: 2023-08-01T15:05:56Z
url: https://github.com/astral-sh/ruff/issues/6180
synced_at: 2026-01-10T11:09:48Z
```

# `F823` false positive

---

_Issue opened by @bastimeyer on 2023-07-30 14:00_

```
$ ruff -V
ruff 0.0.280
```

Consider the following code snippet which overrides the [default `requests_mock` pytest fixture](https://github.com/jamielennox/requests-mock/blob/1.11.0/requests_mock/contrib/_pytest_plugin.py#L70-L86) with `InvalidRequest` being raised on unknown requests, so it needs to import the `requests_mock` package with the same name first in order to access certain exports like [`Mocker`](https://github.com/jamielennox/requests-mock/blob/1.11.0/requests_mock/__init__.py#L15), [`ANY`](https://github.com/jamielennox/requests-mock/blob/1.11.0/requests_mock/__init__.py#L13), or the [`exceptions` submodule](https://github.com/jamielennox/requests-mock/blob/1.11.0/requests_mock/exceptions.py#L29-L30):

```py
import pytest
import requests_mock as rm

@pytest.fixture()
def requests_mock(requests_mock: rm.Mocker) -> rm.Mocker:
    requests_mock.register_uri(rm.ANY, rm.ANY, exc=rm.exceptions.InvalidRequest)
    return requests_mock
```

```
$ ruff check --select F823 ./F823.py
F823.py:
  6:32 F823 Local variable `requests_mock` referenced before assignment
    |
  4 | @pytest.fixture()
  5 | def requests_mock(requests_mock: rm.Mocker) -> rm.Mocker:
  6 |     requests_mock.register_uri(rm.ANY, rm.ANY, exc=rm.exceptions.InvalidRequest)
    |                                ^^ F823
  7 |     return requests_mock
    |
  
Found 1 error.
```

Apparently, ruff doesn't handle the renamed import correctly. When renaming the pytest fixture function or the fixture argument, which both can't be done because it's a global fixture override in our test code, then ruff doesn't raise an error.

`F823` wasn't an issue in `0.0.272` but it now is after upgrading to `0.0.280`. The last modification of that rule was in #6036.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-30 16:19_

---

_Comment by @charliermarsh on 2023-07-30 16:19_

I think this is my bad, I will fix this in time for Monday’s release.

---

_Label `bug` added by @charliermarsh on 2023-07-30 16:19_

---

_Closed by @charliermarsh on 2023-07-30 22:16_

---

_Comment by @charliermarsh on 2023-08-01 15:05_

This should be fixed in v0.0.282, which is out now.

---
