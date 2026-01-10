```yaml
number: 18989
title: "[`Airflow`] Make `AIR312` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-06-27T17:44:27Z
updated_at: 2025-06-28T15:20:39Z
url: https://github.com/astral-sh/ruff/pull/18989
synced_at: 2026-01-10T18:39:09Z
```

# [`Airflow`] Make `AIR312` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-27 17:44_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [airflow3-suggested-to-move-to-provider (AIR312)](https://docs.astral.sh/ruff/rules/airflow3-suggested-to-move-to-provider/#airflow3-suggested-to-move-to-provider-air312)'s example error out-of-the-box

[Old example](https://play.ruff.rs/1be0d654-1ed5-4a0b-8791-cc5db73333d5)
```py
from airflow.operators.python import PythonOperator
```

[New example](https://play.ruff.rs/b6260206-fa19-4ab2-8d45-ddd43c46a759)
```py
from airflow.operators.python import PythonOperator


def print_context(ds=None, **kwargs):
    print(kwargs)
    print(ds)


print_the_context = PythonOperator(
    task_id="print_the_context", python_callable=print_context
)
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 17:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-28 15:16_

---

_Label `documentation` added by @dylwil3 on 2025-06-28 15:17_

---

_Merged by @dylwil3 on 2025-06-28 15:17_

---

_Closed by @dylwil3 on 2025-06-28 15:17_

---

_Branch deleted on 2025-06-28 15:20_

---
