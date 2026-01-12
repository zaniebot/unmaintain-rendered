```yaml
number: 6777
title: Allow up to two empty lines after top-level imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/import-lines
created_at: 2023-08-22T15:55:06Z
updated_at: 2023-08-22T16:29:29Z
url: https://github.com/astral-sh/ruff/pull/6777
synced_at: 2026-01-12T15:55:22Z
```

# Allow up to two empty lines after top-level imports

---

_@charliermarsh_

## Summary

For imports, we enforce that there's _at least_ one empty line after an import (assuming the next statement is _not_ an import), but allow up to two at the module level.

Closes https://github.com/astral-sh/ruff/issues/6760.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-22 15:55_

---

_Review requested from @konstin by @charliermarsh on 2023-08-22 15:55_

---

_Label `formatter` added by @charliermarsh on 2023-08-22 15:55_

---

_@MichaReiser approved on 2023-08-22 16:18_

---

_Comment by @MichaReiser on 2023-08-22 16:19_

Can you verify that this PR fixes all differences for the django project?

---

_Comment by @charliermarsh on 2023-08-22 16:27_

Confirmed, here's the diff of diffs:

```diff
diff --git a/django/core/checks/__init__.py b/django/core/checks/__init__.py
index 98b1286eaf..998ab9dee2 100644
--- a/django/core/checks/__init__.py
+++ b/django/core/checks/__init__.py
@@ -27,6 +27,7 @@ import django.core.checks.templates  # NOQA isort:skip
 import django.core.checks.translation  # NOQA isort:skip
 import django.core.checks.urls  # NOQA isort:skip
 
+
 __all__ = [
     "CheckMessage",
     "Debug",
diff --git a/django/db/models/__init__.py b/django/db/models/__init__.py
index aa4285ca4d..ffca81de91 100644
--- a/django/db/models/__init__.py
+++ b/django/db/models/__init__.py
@@ -60,6 +60,7 @@ from django.db.models.fields.related import (  # isort:skip
     OneToOneRel,
 )
 
+
 __all__ = aggregates_all + constraints_all + enums_all + fields_all + indexes_all
 __all__ += [
     "ObjectDoesNotExist",
diff --git a/django/template/__init__.py b/django/template/__init__.py
index c2c1763582..adb431c00d 100644
--- a/django/template/__init__.py
+++ b/django/template/__init__.py
@@ -71,4 +71,5 @@ from .library import Library  # NOQA isort:skip
 # Import the .autoreload module to trigger the registrations of signals.
 from . import autoreload  # NOQA isort:skip
 
+
 __all__ += ("Template", "Context", "RequestContext")
```

---

_Merged by @charliermarsh on 2023-08-22 16:27_

---

_Closed by @charliermarsh on 2023-08-22 16:27_

---

_Branch deleted on 2023-08-22 16:27_

---

_Comment by @github-actions[bot] on 2023-08-22 16:29_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.0±0.11ms    10.2 MB/sec    1.00      4.0±0.14ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   827.4±44.10µs    20.1 MB/sec    1.01   832.3±18.41µs    20.0 MB/sec
formatter/numpy/globals.py                 1.00     84.6±2.64µs    34.9 MB/sec    1.01     85.4±2.76µs    34.5 MB/sec
formatter/pydantic/types.py                1.00  1644.1±37.73µs    15.5 MB/sec    1.00  1647.4±76.95µs    15.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.46ms     3.0 MB/sec    1.02     13.7±0.44ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.07ms     4.8 MB/sec    1.01      3.5±0.09ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   507.5±18.12µs     5.8 MB/sec    1.00   496.2±22.98µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.21ms     3.6 MB/sec    1.00      7.0±0.20ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.26ms     6.0 MB/sec    1.04      7.1±0.17ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1512.8±39.14µs    11.0 MB/sec    1.01  1520.5±37.06µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.9±7.43µs    15.8 MB/sec    1.02   190.0±10.93µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.06ms     8.2 MB/sec    1.03      3.2±0.08ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    10.9 MB/sec    1.00      3.7±0.05ms    11.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   766.1±21.48µs    21.7 MB/sec    1.00   765.9±17.03µs    21.7 MB/sec
formatter/numpy/globals.py                 1.00     78.4±1.67µs    37.7 MB/sec    1.01     79.3±2.33µs    37.2 MB/sec
formatter/pydantic/types.py                1.00  1527.7±32.86µs    16.7 MB/sec    1.01  1536.0±29.23µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.01     12.7±0.14ms     3.2 MB/sec    1.00     12.5±0.12ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.05ms     4.8 MB/sec    1.00      3.5±0.06ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.0±6.49µs     6.8 MB/sec    1.00    434.5±6.02µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.02      6.6±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.07ms     5.7 MB/sec    1.00      7.0±0.09ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1498.3±26.77µs    11.1 MB/sec    1.00  1491.1±18.43µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    175.5±3.46µs    16.8 MB/sec    1.00    171.9±2.56µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.0 MB/sec    1.00      3.2±0.05ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
