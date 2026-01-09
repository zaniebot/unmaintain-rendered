---
number: 10223
title: "PGH005: False positive in case of `requests-mock` fixture `called_once` property/attribute"
type: issue
state: open
author: Taragolis
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-03-04T08:40:33Z
updated_at: 2024-03-13T17:31:57Z
url: https://github.com/astral-sh/ruff/issues/10223
synced_at: 2026-01-07T13:12:15-06:00
---

# PGH005: False positive in case of `requests-mock` fixture `called_once` property/attribute

---

_Issue opened by @Taragolis on 2024-03-04 08:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The [invalid-mock-access `PGH005`](https://docs.astral.sh/ruff/rules/invalid-mock-access/#invalid-mock-access-pgh005) works well until some object provide arguments which looks like invalid `unittest.mock.Mock` arguments/properties

Playground: https://play.ruff.rs/327e8861-39fc-44ca-8a60-918bfb97c25c

```python
import unittest
from unittest import mock

import requests

# False possitive
def test_requests_mock(requests_mock):
    # Use fixture from requests-mock package
    mocked_request = requests_mock.get("https://httpbin.org/anything", status_code=200)
    requests.get("https://httpbin.org/anything")
    # https://requests-mock.readthedocs.io/en/latest/release-notes.html?highlight=called_once#relnotes-1-1-0-new-features
    assert mocked_request.called_once
    # For avoid false positive PGH005
    # assert mocked_request.call_count == 1

# This is fine
def test_pytest_mocker(mocker):
    # Use fixture from pytest-mocker, provide almost same interface as unittest.mock
    mocked_request = mocker.patch("requests.get", return_value=b"{}")
    requests.get("https://httpbin.org/anything")
    assert mocked_request.called_once()
    # One of the options which should use instead
    # mocked_request.assert_called_once("https://httpbin.org/anything")

# This is fine
@unittest.mock.patch("requests.get", return_value=b"{}")
def test_unittest_decorator(mocked_request):
    requests.get("https://httpbin.org/anything")
    assert mocked_request.called_once()
    # One of the options which should use instead
    # mocked_request.assert_called_once("https://httpbin.org/anything")

# This is fine
def test_unittest_context_manager():
    with unittest.mock.patch("requests.get", return_value=b"{}") as mocked_request:
        requests.get("https://httpbin.org/anything")
        assert mocked_request.called_once()
        # One of the options which should use instead
        # mocked_request.assert_called_once("https://httpbin.org/anything")
```

It would be nice if it possible to add some exclusion list for objects, but I don't have much hope since this might be only the grep rule without analysing original object types.

---

_Referenced in [apache/airflow#37874](../../apache/airflow/pulls/37874.md) on 2024-03-04 08:53_

---

_Label `rule` added by @MichaReiser on 2024-03-04 11:50_

---

_Comment by @MichaReiser on 2024-03-04 11:54_

Seems reasonable. But it probably requires type awareness to rule out false-positives fully. 

Is there a naming convention for calling mock objects / request_mocks that could be used to reduce the false positive rate?

---

_Comment by @Taragolis on 2024-03-05 10:21_

I guess there is not specific conversions for the third party fixtures/mockers it could be anything depend on the autor/maintainer decision. Particular for `requests_mock` there are could be two solution:
1. Do not use `called_once and use instead `assert <mocked_request_object>.call_count == 1` or `assert <mocked_request_object>.call_count > 1` or `assert <mocked_request_object>.call_count`
2. Explicit ignore rule by `noqa: PGH005`

The rule itself works well for the `unittest.mock.Mock`, subclasses and [`pytest-mock`](https://pytest-mock.readthedocs.io/en/latest/) mocker fixture because it is quite popular mistake to use
`assert <mocked_object>.called_once` which should be always evaluated as True instead of  `<mocked_object>.assert_called_once()`

---

_Comment by @zanieb on 2024-03-11 17:29_

We could look at type annotations for the fixture and use that to determine if it should be excluded. I don't see a clear way to do it otherwise? You'd need to type-annotate the fixture though.

---

_Comment by @Taragolis on 2024-03-12 02:08_

Is annotate it into the `conftest.py` would be enough? Some example how it done into the Apache Airflow repo https://github.com/apache/airflow/blob/7213fc5b7a5cb9b69fc9f42126f44d214a24b575/tests/conftest.py#L1297-L1318, but it is more helper (TBH hack as workaround for pluggy) for autosuggestion in IDEs which have integration with pytest

If this wouldn’t work, then I think unlikely that someone would put same type checking into the each individual test module. In this case better just mention into the documentation that there is no type checking and in some circumstances could be false positive behavior.

---

_Comment by @zanieb on 2024-03-12 05:33_

> If this wouldn’t work, then I think unlikely that someone would put same type checking into the each individual test module

Idk at my last company it was standard practice to type annotate fixtures otherwise they're just magic values.

Unfortunately, I don't think a `conftest.py` annotation would be enough today. We can do better once we have type checking or multifile analysis but we don't have those yet.

Improving the documentation to note the false positive seems reasonable.


---

_Label `type-inference` added by @zanieb on 2024-03-12 05:33_

---

_Comment by @Taragolis on 2024-03-13 08:07_

> Idk at my last company it was standard practice to type annotate fixtures otherwise they're just magic values.

The problem here could be defined into the different levels: 
1. same module
2. root `conftest.py`
3. any other `conftest.py` in the same package/subpackage
4. fixtures from the third party packages.

With annotation for the first case there is no problem, for other it required additional steps from the developer side, e.g.

```python
if TYPE_CHECKING:
    @pytest.fixture
    def awesome_outer_fixture() -> AwesomeType:
        ...
```

Which I believe most of the devs would skip

---

_Comment by @zanieb on 2024-03-13 15:00_

I'm talking about

```python
def test_foo(awesome_fixture: AwesomeType): 
    ...
```

which give type hints around your fixture type out of the box.

My point is not that this is an ideal solution (the ideal solution is type inference), but it allows a developer to annotate the type to pass linting instead of adding a `# noqa` to each call site.

---

_Comment by @Taragolis on 2024-03-13 17:31_

Yep, it should work in the cases when return of fixture is known. For the case when it is unknown, might changed (third-paties) or just return some factory class which build into the fixture itself it might not work.

 So my point here just it might be nice to have this ability but it might required a big effort (especially into the big projects.) to change existed code if compare to `# noqa` or use other methods if applicable. 

---
