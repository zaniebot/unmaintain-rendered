---
number: 2067
title: Support remote url constraints?
type: issue
state: closed
author: notatallshaw
labels: []
assignees: []
created_at: 2024-02-29T00:33:58Z
updated_at: 2024-02-29T00:47:27Z
url: https://github.com/astral-sh/uv/issues/2067
synced_at: 2026-01-10T01:23:11Z
---

# Support remote url constraints?

---

_Issue opened by @notatallshaw on 2024-02-29 00:33_

One feature pip has for constraints file is specifying a URL, it is used to effect in the Airflow installation guide: https://airflow.apache.org/docs/apache-airflow/stable/installation/installing-from-pypi.html

> pip install "apache-airflow[celery]==2.8.2" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.8.2/constraints-3.8.txt"

However this does not work for uv:

```
$ echo "apache-airflow==2.3.4" | uv pip compile - --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.3.4/constraints-3.10.txt"
error: failed to open file `https://raw.githubusercontent.com/apache/airflow/constraints-2.3.4/constraints-3.10.txt`
  Caused by: No such file or directory (os error 2)
```

I don't think it's a very important feature, but Airflow uses are used to it from pip.

---

_Comment by @charliermarsh on 2024-02-29 00:45_

I think this is the same as https://github.com/astral-sh/uv/issues/1332.

---

_Comment by @notatallshaw on 2024-02-29 00:47_

Oh yeah.

---

_Closed by @notatallshaw on 2024-02-29 00:47_

---
