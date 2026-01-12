```yaml
number: 1032
title: "Propagate cancellation errors in `OnceMap`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cancellation
created_at: 2024-01-22T01:13:44Z
updated_at: 2024-01-22T14:00:23Z
url: https://github.com/astral-sh/uv/pull/1032
synced_at: 2026-01-12T16:04:22Z
```

# Propagate cancellation errors in `OnceMap`

---

_@charliermarsh_

## Summary

Ensures that if an operation is cancelled in one thread, we propagate it to others rather than panicking.

Related to https://github.com/astral-sh/puffin/issues/1005.


---

_Review requested from @konstin by @charliermarsh on 2024-01-22 01:13_

---

_Label `bug` added by @charliermarsh on 2024-01-22 01:13_

---

_Comment by @konstin on 2024-01-22 08:15_

This does fix the panic, but it means that all ecosystem checks after pywin32 fail.

```shell
cargo run --bin puffin-dev --features vendored-openssl -- resolve-many --cache-dir cache-docker-no-build --no-build scripts/popular_packages/pypi_10k_most_dependents.tx
```

```
2024-01-22T08:13:42.624265Z  INFO resolve many: Success (351/9936, 98 ms): python-binance
2024-01-22T08:13:42.627480Z  INFO resolve many: Error for huey (352/9936, 56 ms): Building source distributions is disabled
2024-01-22T08:13:42.631608Z  INFO resolve many: Success (353/9936, 0 ms): pycparser
2024-01-22T08:13:42.632048Z  INFO resolve many: Success (354/9936, 101 ms): pytesseract
2024-01-22T08:13:42.632686Z  INFO resolve many: Error for pywin32 (355/9936, 53 ms): No solution found when resolving: pywin32
  Caused by: Because there are no versions of pywin32 and you require pywin32, we can conclude that the requirements are unsatisfiable.
2024-01-22T08:13:42.632783Z  INFO resolve many: Error for types-requests (356/9936, 16 ms): No solution found when resolving: types-requests
  Caused by: The operation was canceled
2024-01-22T08:13:42.632899Z  INFO resolve many: Error for pymc (357/9936, 125 ms): No solution found when resolving: pymc
  Caused by: The operation was canceled
2024-01-22T08:13:42.632981Z  INFO resolve many: Error for torchtext (358/9936, 2 ms): No solution found when resolving: torchtext
  Caused by: The operation was canceled
2024-01-22T08:13:42.633047Z  INFO resolve many: Error for types-setuptools (359/9936, 14 ms): No solution found when resolving: types-setuptools
  Caused by: The operation was canceled
2024-01-22T08:13:42.633105Z  INFO resolve many: Error for pyqtgraph (360/9936, 2 ms): No solution found when resolving: pyqtgraph
  Caused by: The operation was canceled
2024-01-22T08:13:42.633161Z  INFO resolve many: Error for phonenumbers (361/9936, 2 ms): No solution found when resolving: phonenumbers
  Caused by: The operation was canceled
2024-01-22T08:13:42.633217Z  INFO resolve many: Error for elasticsearch-dsl (362/9936, 13 ms): No solution found when resolving: elasticsearch-dsl
  Caused by: The operation was canceled
2024-01-22T08:13:42.633665Z  INFO resolve many: Error for google-cloud-storage (363/9936, 199 ms): No solution found when resolving: google-cloud-storage
  Caused by: The operation was canceled
2024-01-22T08:13:42.634033Z  INFO resolve many: Error for shap (364/9936, 156 ms): No solution found when resolving: shap
  Caused by: The operation was canceled
[...]
2024-01-22T08:13:44.354055Z  INFO resolve many: Success: 600, Error: 9336
```

---

_Comment by @charliermarsh on 2024-01-22 13:36_

I guess this is specific to the ecosystem script, which uses a single index and continues even if a resolution fails? I’ll need to change the script, probably.

---

_Comment by @charliermarsh on 2024-01-22 14:00_

Closing this (but not the issue).

---

_Merged by @charliermarsh on 2024-01-22 14:00_

---

_Closed by @charliermarsh on 2024-01-22 14:00_

---

_Branch deleted on 2024-01-22 14:00_

---
