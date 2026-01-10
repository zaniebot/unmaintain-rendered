---
number: 3899
title: E712 Error message should be improved. Suggestion is not valid in pyspark code
type: issue
state: closed
author: jnlaia
labels:
  - bug
assignees: []
created_at: 2023-04-06T11:41:11Z
updated_at: 2023-04-13T19:02:25Z
url: https://github.com/astral-sh/ruff/issues/3899
synced_at: 2026-01-10T01:22:42Z
---

# E712 Error message should be improved. Suggestion is not valid in pyspark code

---

_Issue opened by @jnlaia on 2023-04-06 11:41_



When a E712 violation is found by ruff, the message looks like
```
E712 Comparison to `True` should be `cond is True`
```
This message is not valid if we are talking about pyspark code (and if autofix is enabled, it will fix it wrongly). For instance, the following code snippet is not valid pyspark code
```python
spark_dataframe.where(sf.col("a_boolean_column") is True)
```
but the lines below are both valid
```python
spark_dataframe.where(sf.col("a_boolean_column"))
spark_dataframe.where(sf.col("a_boolean_column") == True)
```

In flake8, the error message was 
```
E712 comparison to True should be 'if cond is True:' or 'if cond:'
```
which would cover the pyspark scenario


---

_Label `bug` added by @charliermarsh on 2023-04-06 21:26_

---

_Label `type-inference` added by @charliermarsh on 2023-04-06 21:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-13 18:48_

---

_Label `type-inference` removed by @charliermarsh on 2023-04-13 18:48_

---

_Referenced in [astral-sh/ruff#3962](../../astral-sh/ruff/pulls/3962.md) on 2023-04-13 18:56_

---

_Closed by @charliermarsh on 2023-04-13 19:02_

---
