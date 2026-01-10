```yaml
number: 1802
title: "pyspark: No argument provided for required parameter 1"
type: issue
state: closed
author: cgahr
labels:
  - needs-info
assignees: []
created_at: 2025-12-08T08:47:31Z
updated_at: 2025-12-08T09:43:17Z
url: https://github.com/astral-sh/ty/issues/1802
synced_at: 2026-01-10T01:56:41Z
```

# pyspark: No argument provided for required parameter 1

---

_Issue opened by @cgahr on 2025-12-08 08:47_

### Summary

The following snippet
```python
from pyspark.sql.functions import col

col("name").isNull()
```
emits the error
```
No argument provided for required parameter 1
```
I don't quite understand why, `col` has the signature
```
def col(col: str) -> Column:
    ...
```
as defined [here](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/sql/functions/builtin.html#col) while `isNull` is a method of `Column`  with signature 
```python
class Column:
    ...
    isNull = _unary_op("isNull", _isNull_doc)

def _unary_op(
    name: str,
    doc: str = "unary operator",
) -> Callable[["Column"], "Column"]:
    """Create a method for given unary operator"""
    ...

```
defined [here](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/sql/column.html#Column.isNull)

This might be related to https://github.com/astral-sh/ty/issues/310 https://github.com/astral-sh/ty/issues/312 https://github.com/astral-sh/ty/issues/1129 

### Version

ty 0.0.1-alpha.31 (51c73d687 2025-12-04)
ty 0.0.1-alpha.32 (84a188116 2025-12-05)
python 3.12.2
pyspark 3.5.7

---

_Comment by @dhruvmanila on 2025-12-08 09:27_

Thanks for the report although I'm unable to reproduce this locally. Can you provide the `pyspark` version and the Python version? Can you also try it on the latest version of ty (`0.0.1-alpha.32`) and see if the issue persists?

---

_Label `needs-info` added by @MichaReiser on 2025-12-08 09:29_

---

_Comment by @cgahr on 2025-12-08 09:34_

I added pyspark and python versions. The error persists for the latest ty version. I'm currently upgrading to pyspark 4.0.1 to see if the error persists.

---

_Comment by @cgahr on 2025-12-08 09:40_

This error seems to have been fixed with `pyspark 4.0.1`. Thank you and sorry for the noise.

---

_Closed by @cgahr on 2025-12-08 09:40_

---

_Comment by @sharkdp on 2025-12-08 09:43_

I think this is related to #491 for older versions of pyspark. See also https://github.com/astral-sh/ty/issues/1209. Glad it's resolved with newer pyspark versions.

---
