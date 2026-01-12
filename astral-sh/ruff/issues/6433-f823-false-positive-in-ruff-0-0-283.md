```yaml
number: 6433
title: "`F823` false positive in ruff 0.0.283"
type: issue
state: closed
author: harupy
labels:
  - bug
assignees: []
created_at: 2023-08-09T01:02:59Z
updated_at: 2023-08-09T02:36:42Z
url: https://github.com/astral-sh/ruff/issues/6433
synced_at: 2026-01-12T15:54:46Z
```

# `F823` false positive in ruff 0.0.283

---

_@harupy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Command to reproduce

```
docker run --rm python:3.8 bash -c "git clone --depth=1 https://github.com/mlflow/mlflow.git && cd mlflow && pip install ruff && ruff --show-source --select F823 ."
```

## Result

```
tests/sklearn/test_sklearn_autolog.py:1464:5: F823 Local variable `sklearn` referenced before assignment
     |
1462 |     import sklearn.linear_model
1463 | 
1464 |     mlflow.sklearn.autolog()
     |     ^^^^^^ F823
1465 |     from sklearn.metrics import roc_auc_score
     |

tests/sklearn/test_sklearn_autolog.py:1777:5: F823 Local variable `sklearn` referenced before assignment
     |
1776 | def test_autolog_pos_label_used_for_training_metric():
1777 |     mlflow.sklearn.autolog(pos_label=1)
     |     ^^^^^^ F823
1778 | 
1779 |     import sklearn.ensemble
     |

Found 2 errors.
```

---

_Comment by @harupy on 2023-08-09 01:04_

The first error is raised on this line:

https://github.com/mlflow/mlflow/blob/e4192790f0a0df882a0e5d59a9be50b0468badea/tests/sklearn/test_sklearn_autolog.py#L1464

---

_Comment by @harupy on 2023-08-09 01:07_

The error disappears if I replace `import mlflow.sklearn` with `import mlflow`:

```
diff --git a/tests/sklearn/test_sklearn_autolog.py b/tests/sklearn/test_sklearn_autolog.py
index 43b77b7a2..354679571 100644
--- a/tests/sklearn/test_sklearn_autolog.py
+++ b/tests/sklearn/test_sklearn_autolog.py
@@ -25,7 +25,9 @@ from scipy.sparse import csr_matrix, csc_matrix
 from mlflow.exceptions import MlflowException
 from mlflow.models import Model, infer_signature
 from mlflow.models.utils import _read_example
-import mlflow.sklearn
+
+# import mlflow.sklearn
+import mlflow
 from mlflow.entities import RunStatus
 from mlflow.sklearn.utils import (
     _is_metric_supported,


> ruff --select F823 .
```

---

_Comment by @harupy on 2023-08-09 01:10_

A minimum reproduction:

```python
import sklearn.base
import mlflow.sklearn


def f():
    import sklearn

    mlflow
```

---

_Comment by @charliermarsh on 2023-08-09 01:45_

Thank you, I will make sure this is fixed for the next release.

---

_Label `bug` added by @charliermarsh on 2023-08-09 01:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-09 02:11_

---

_Closed by @charliermarsh on 2023-08-09 02:36_

---
