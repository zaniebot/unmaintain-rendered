```yaml
number: 2270
title: autofix for PT003
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: pt003-autofix
created_at: 2023-01-27T17:50:31Z
updated_at: 2023-01-30T07:52:17Z
url: https://github.com/astral-sh/ruff/pull/2270
synced_at: 2026-01-12T04:52:00Z
```

# autofix for PT003

---

_Pull request opened by @sbrugman on 2023-01-27 17:50_

Closes https://github.com/charliermarsh/ruff/issues/2260

---

_@charliermarsh reviewed on 2023-01-27 17:55_

---

_Review comment by @charliermarsh on `src/rules/flake8_pytest_style/rules/fixture.rs`:327 on 2023-01-27 17:55_

I think we need to be a bit more careful here. E.g. with `@pytest.fixture(scope="function",)`, would this leave the trailing comma and thus lead to a syntax error? What if this is the last argument, and we don't remove the preceding comma?

Trying to find an example of where we handle this... I guess `remove_class_def_base`? But maybe not the best implementation.

---

_Comment by @edgarrmondragon on 2023-01-27 17:55_

[`pytest.fixture`](https://docs.pytest.org/en/7.1.x/reference/reference.html#pytest.fixture) can have more arguments, this seems to preserve them but what happens to the comma?

```python
@pytest_fixture(scope='function', name='my_fixture')
def dummy():
    return 1
```

---

_Comment by @sbrugman on 2023-01-27 20:31_

Thanks for the pointers @edgarrmondragon and @charliermarsh.
I've changed my naive solution to take the `keywords.iter()` and check for a next value, if present it searches for the comma and extended the tests.
(Refactored the related violations with it)

---

_@charliermarsh reviewed on 2023-01-27 23:55_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_pytest_style/PT003.py`:39 on 2023-01-27 23:55_

I think there are still a few cases that aren't quite handled correctly:

```py
@pytest.fixture(
    scope="function",
    name="my_fixture",
)
def error_multiple_args():
    ...


@pytest.fixture(name="my_fixture", scope="function")
def error_multiple_args():
    ...
```

These give:

```py
@pytest.fixture(
    
    name="my_fixture",
)
def error_multiple_args():
    ...


@pytest.fixture(name="my_fixture", )
def error_multiple_args():
    ...
```

Sorry for all the churn, but do you mind tweaking to address those?

---

_@sbrugman reviewed on 2023-01-27 23:58_

---

_Review comment by @sbrugman on `resources/test/fixtures/flake8_pytest_style/PT003.py`:39 on 2023-01-27 23:58_

Will do, should have tested a bit better

---

_@charliermarsh reviewed on 2023-01-27 23:59_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_pytest_style/PT003.py`:39 on 2023-01-27 23:59_

Na don't sweat it. I only know to try these because I've run into them before :joy:

---

_@sbrugman reviewed on 2023-01-28 00:08_

---

_Review comment by @sbrugman on `resources/test/fixtures/flake8_pytest_style/PT003.py`:39 on 2023-01-28 00:08_

Would be nice to turn them into utilities after (e.g. `remove_argument`)

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_pytest_style/PT003.py`:39 on 2023-01-28 00:19_

Strongly agree!

---

_@charliermarsh reviewed on 2023-01-28 00:19_

---

_@charliermarsh requested changes on 2023-01-29 18:46_

(Just requesting changes to help manage my own queue, no rush of course.)

---

_Comment by @sbrugman on 2023-01-29 19:09_

No problem, working on a general fix helper for removing (keyword)args. Will try to complete today.

---

_Comment by @charliermarsh on 2023-01-29 19:12_

Genuinely no rush, take your time :)

---

_Converted to draft by @sbrugman on 2023-01-29 19:18_

---

_Comment by @sbrugman on 2023-01-29 22:08_

@charliermarsh Introduced a helper function in `autofix/helpers.rs`. 
This is now used here, and for pyupgrade's `object` base removal (class definition and function call use the same `arguments` part in the python [grammar](https://docs.python.org/3/reference/grammar.html)).

This implementation does not take trailing commas into account (nor did the pyupgrade version), but that can be added later.

(I also see opportunity to use it to autofix `PD002`, working on that now)

Will rebase this PR and squash it into two commits (helper function and pyupgrade refactor)

---

_Marked ready for review by @sbrugman on 2023-01-30 01:22_

---

_Merged by @charliermarsh on 2023-01-30 03:30_

---

_Closed by @charliermarsh on 2023-01-30 03:30_

---

_Branch deleted on 2023-01-30 07:52_

---
