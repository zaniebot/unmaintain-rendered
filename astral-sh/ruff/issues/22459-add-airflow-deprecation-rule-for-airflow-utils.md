```yaml
number: 22459
title: "Add Airflow deprecation rule for `airflow.utils` module's deprecated classes"
type: issue
state: open
author: zach-overflow
labels:
  - rule
assignees: []
created_at: 2026-01-08T14:49:13Z
updated_at: 2026-01-09T08:17:48Z
url: https://github.com/astral-sh/ruff/issues/22459
synced_at: 2026-01-10T11:10:00Z
```

# Add Airflow deprecation rule for `airflow.utils` module's deprecated classes

---

_Issue opened by @zach-overflow on 2026-01-08 14:49_

### Summary

Currently, the Airflow ruff rules do not fail on certain deprecated imports from `airflow.utils` for Airflow version `3.1.5`. Ideally, the airflow deprecations ruff rules should trigger a failure for [these deprecated class imports](https://github.com/apache/airflow/blob/7c4520ba62ec4bb5a35e07fb451dcda276a2a841/airflow-core/src/airflow/utils/__init__.py).

Worth noting: There are other similar `__deprecated_classes` dictionaries defined in the airflow-core submodules, which may also be worth filing issues for as needed. A good bit of them are already covered by the ruff rules, but I imagine many of these deprecations came about from Airflow `3.1`, and those newer deprecations may need to be reflected in `ruff` accordingly.

---

_Renamed from "Add Airflow deprecation rule for `airflow.utils` module" to "Add Airflow deprecation rule for `airflow.utils` module's deprecated classes" by @zach-overflow on 2026-01-08 14:49_

---

_Label `rule` added by @amyreese on 2026-01-08 19:46_

---

_Comment by @MichaReiser on 2026-01-09 08:13_

CC: @Lee-W 

---

_Comment by @Lee-W on 2026-01-09 08:17_

This is tracked by https://github.com/apache/airflow/issues/54714

@sjyangkevin is working on it. @amoghrajesh and I will finalize a list. Instead of fixing everything, I guess we're just to recommend our users not to use some of them.

---
