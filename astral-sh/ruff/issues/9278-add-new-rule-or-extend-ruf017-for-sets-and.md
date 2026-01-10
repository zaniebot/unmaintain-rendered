---
number: 9278
title: "Add new rule or extend `RUF017` for sets and dictionaries ops as well."
type: issue
state: open
author: Skylion007
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-12-25T16:16:02Z
updated_at: 2023-12-26T22:04:19Z
url: https://github.com/astral-sh/ruff/issues/9278
synced_at: 2026-01-10T01:22:49Z
---

# Add new rule or extend `RUF017` for sets and dictionaries ops as well.

---

_Issue opened by @Skylion007 on 2023-12-25 16:16_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
`RUF017` is great at preventing you from shooting yourself in the foot by excessively and unnecessarily concatting elements in a list. However, one other very common pattern that would benefit from a similar treatment are the following code snippits.

## set union
```python
set_union = functools.reduce(operator.or, seq_of_sets, set())
```
should probably become
```python
set_union = functools.reduce(operator.ior, seq_of_sets, set())
```
## dict extension
This also hold in python `>= 3.9` with dictionaries
```python
dict_extended = functools.reduce(operator.or, seq_of_dicts, {})
```
should become
```python
dict_extended = functools.reduce(operator.ior, seq_of_dicts, {})
```

# set and
One final useful case that should be covered is intersection operators for sets as well.
```python
set_intersect = functools.reduce(operator.and, seq_of_sets, set())
```
should become
```python
set_intersect = functools.reduce(operator.iand, seq_of_sets, set())
```

# set difference
```python
set_difference = functools.reduce(operator.sub, seq_of_sets, set())
```
should become
```python
set_difference = functools.reduce(operator.isub, seq_of_sets, set())
```

# symmetric difference
```python
set_symmetric_difference = functools.reduce(operator.xor, seq_of_sets, set())
```
should become
```python
set_symmetric_difference = functools.reduce(operator.ixor, seq_of_sets, set())
```


it's unclear if you want to keep the functools reduce syntax in general. Other alternatives are:
```python
# update versions are faster, but not really necessary when doing it on an empty set.
set().union(*seq_sets) 
set().update(*seq_sets) # faster than above
set().intersection(*seq_sets) 
set().intersection_update(*seq_sets) # faster than above
set().difference(*seq_sets) 
set().difference_update(*seq_sets) # faster than above
# set().symmetric_difference_update(*seq_sets) # does not exist surprisingly according to python docs...
```

Regardless, operator.or, operator.and etc is wasteful as it copies the set quite a bit during its reduction. It's not quite as bad as the quadratic, but it is leaving performance on the table.

---

_Label `rule` added by @charliermarsh on 2023-12-26 22:04_

---

_Label `needs-decision` added by @charliermarsh on 2023-12-26 22:04_

---
