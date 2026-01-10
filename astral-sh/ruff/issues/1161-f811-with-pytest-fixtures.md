```yaml
number: 1161
title: F811 with pytest fixtures
type: issue
state: closed
author: jiwidi
labels: []
assignees: []
created_at: 2022-12-09T10:45:14Z
updated_at: 2022-12-09T11:17:37Z
url: https://github.com/astral-sh/ruff/issues/1161
synced_at: 2026-01-10T12:06:18Z
```

# F811 with pytest fixtures

---

_Issue opened by @jiwidi on 2022-12-09 10:45_

Hi!

With the latest pip release 0.0.171 I noticed a new change in the linting rules.

```python
from fixtures import fixtureA

def random_unit_test(fixtureA):
	assert 1==1
```

This code will import a fixture `fixtureA` that would be used by `random_unit_test`. Not directly used in any variable of the function but lets assume is required by the test because it prepares a needed context.

Ruff will raise a new error:
```
test.py:4:22: F811 Redefinition of unused `fixtureA` from line 1
```

I can work around it with the addition of NOQA on each test that takes that fixture, but feel is not a nice solution, neither is it ignoring F811 across all the codebase.

How would you recommend treating this pytest fixture case with Ruff?

---

_Comment by @jiwidi on 2022-12-09 11:17_

mm actually i can just add the fixture requirement with 

`@pytest.mark.usefixtures("fixtureA")` and it will work, closing the issue!

---

_Closed by @jiwidi on 2022-12-09 11:17_

---
