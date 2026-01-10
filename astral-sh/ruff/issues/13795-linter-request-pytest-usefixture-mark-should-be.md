```yaml
number: 13795
title: "Linter request: pytest \"usefixture\" mark should be flagged"
type: issue
state: open
author: lengau
labels:
  - rule
assignees: []
created_at: 2024-10-17T16:01:47Z
updated_at: 2024-12-13T08:07:34Z
url: https://github.com/astral-sh/ruff/issues/13795
synced_at: 2026-01-10T11:09:55Z
```

# Linter request: pytest "usefixture" mark should be flagged

---

_Issue opened by @lengau on 2024-10-17 16:01_

I've seen multiple cases where a test used `pytest.mark.usefixture` rather than `pytest.mark.usefixtures` in ways that caused the tests to only sometimes fail (e.g. only on certain platforms). It would be handy to have a linter that changed this.

E.g. the following code:

```python
@pytest.mark.usefixture("side_effect_fixture")
def test_my_function():
    assert my_function() == True
```

could be (probably with an `unsafe` marker?) converted to:

```python
@pytest.mark.usefixtures("side_effect_fixture")
def test_my_function():
    assert my_function() == True
```

---

_Comment by @MichaReiser on 2024-10-18 06:43_

Do you have a reference to what `pytest.mark.usefixture`? I was only able to find the documentation for [`pytest.mark.usefixtures`](https://docs.pytest.org/en/7.1.x/reference/reference.html#pytest-mark-usefixtures) and it's not clear to me what the difference between the two is (I never used pytest)

---

_Comment by @autinerd on 2024-10-19 15:43_

`@pytest.mark.usefixture` is almost always a typo and should emit a pytest warning. (but technically, a plugin could register the `usefixture` mark and do something with it)

Could you run `pytest <path_to_file> --markers` to check if `usefixture` is really a registered marker in your context?

---

_Label `needs-info` added by @MichaReiser on 2024-10-19 16:57_

---

_Comment by @sbrugman on 2024-10-30 20:13_

Real-world examples:

- https://github.com/apache/superset/blob/master/tests/unit_tests/commands/dataset/test_update.py#L29
- https://github.com/bigchaindb/bigchaindb/blob/master/tests/tendermint/test_lib.py#L348

Reference issue at the pytest repository: https://github.com/pytest-dev/pytest/issues/3972

---

_Comment by @lengau on 2024-12-13 00:44_

@MichaReiser as @autinerd said, it's almost always a typo. At runtime `pytest` will by default make an unknown mark warning. However, this popped up for me in a project where (unfortunately) the pytest runs are full of warnings, and it's faster/easier to fix stuff with ruff before tackling all of those warnings.

---

_Label `needs-info` removed by @MichaReiser on 2024-12-13 08:06_

---

_Label `rule` added by @MichaReiser on 2024-12-13 08:07_

---
