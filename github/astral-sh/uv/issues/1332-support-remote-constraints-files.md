---
number: 1332
title: Support remote constraints files
type: issue
state: closed
author: manojkarthick
labels:
  - enhancement
assignees: []
created_at: 2024-02-15T20:31:17Z
updated_at: 2024-03-06T04:18:13Z
url: https://github.com/astral-sh/uv/issues/1332
synced_at: 2026-01-07T13:12:16-06:00
---

# Support remote constraints files

---

_Issue opened by @manojkarthick on 2024-02-15 20:31_

Big fan of `ruff` and just checking out `uv` from the twitter announcement. Thanks a lot for creating this and improving the python ecosystem.

Looks like constraints using `--constraint` flag inside a `requirements.txt` file is not supported?

I'm receiving the following error:

```
❯ uv pip install -r requirements.txt
error: Error parsing included file in `requirements.txt` at position 0
  Caused by: failed to open file `/Users/manojkarthick/code/sandbox/uvtest/"https://raw.githubusercontent.com/apache/airflow/constraints-2.6.3/constraints-3.10.txt"`
  Caused by: No such file or directory (os error 2)
```

<details>
<summary>Requirements file with constraints that fails</summary>

```
--constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.6.3/constraints-3.10.txt"
virtualenv==20.23.1
boto3==1.26.161
cachetools==5.3.1
pandas==1.5.3
numpy==1.24.4
pendulum==2.1.2
psycopg2-binary==2.9.6
protobuf==4.23.4
PyGithub==1.59.0
SQLAlchemy==1.4.49
apache-airflow-providers-amazon==8.2.0
apache-airflow-providers-http==4.4.2
apache-airflow-providers-common-sql==1.5.2
apache-airflow-providers-jdbc==4.0.0
apache-airflow-providers-postgres==5.5.1
apache-airflow-providers-docker==3.7.1
apache-airflow-providers-ssh==3.7.1
apache-airflow-providers-apache-spark==4.1.1
apache-airflow-providers-snowflake==4.2.0
apache-airflow-providers-slack==7.3.1
apache-airflow-providers-elasticsearch==4.5.1
apache-airflow-providers-databricks==4.3.0
apache-airflow-providers-github==2.3.1
snowflake-connector-python==3.0.4
snowflake-sqlalchemy==1.4.7
protobuf==4.23.4
fsspec==2023.6.0
attrs==23.1.0
requests==2.31.0
uszipcode==1.0.1
clickhouse-connect==0.7.0
```
</details>


I do see https://github.com/astral-sh/uv/issues/172, so please feel free to close this issue if it's redundant. 

---

_Assigned to @zanieb by @zanieb on 2024-02-15 20:33_

---

_Comment by @zanieb on 2024-02-15 20:36_

Hi! Thanks for the kind words.

It looks like we support this during `pip compile` but not `pip install` which seems like an oversight.

---

_Label `enhancement` added by @zanieb on 2024-02-15 20:36_

---

_Renamed from "Support pip constraints in requirements.txt files" to "Support pip constraints in requirements.txt files in `uv pip install`" by @zanieb on 2024-02-15 20:36_

---

_Comment by @charliermarsh on 2024-02-15 20:37_

Could the issue here be that it's a remote URL? I'm not sure we support that yet.

---

_Comment by @zanieb on 2024-02-15 20:41_

Yeah actually that does look like the case, adding test coverage for `pip install` with constraints right now...

---

_Referenced in [astral-sh/uv#1334](../../astral-sh/uv/pulls/1334.md) on 2024-02-15 20:43_

---

_Referenced in [astral-sh/uv#1461](../../astral-sh/uv/issues/1461.md) on 2024-02-16 09:32_

---

_Comment by @recrsn on 2024-02-16 13:12_

Looks like we don't have support for remote constraint URLs yet, not even on the `--constraint` flag

---

_Comment by @zanieb on 2024-02-16 17:38_

Yeah this is the problem not the presence of `--constraint` (https://github.com/astral-sh/uv/pull/1334)

---

_Renamed from "Support pip constraints in requirements.txt files in `uv pip install`" to "Support remote constraints files" by @zanieb on 2024-02-16 17:38_

---

_Comment by @jannisko on 2024-02-17 20:24_

So I guess this could be solved by replacing the `PathBuf` here
https://github.com/astral-sh/uv/blob/8675f66e74dc2c7d45b65e9aef21479c97dd0015/crates/uv/src/main.rs#L174
with something like a `DynObjectStore`: https://docs.rs/object_store/latest/object_store/#configuration-system to automatically dispatch to a local file or url depending on what is given. I'd be down to implement this if you think the additional dependency is worth it.
If you know another crate that could help us do this let me know. 🙂 

---

_Unassigned @zanieb by @zanieb on 2024-02-17 20:34_

---

_Comment by @jensens on 2024-02-28 22:28_

I think my problem is related:
```shell
(venv) ~/ws/sandbox/cplone$ uv pip install -r requirements.txt 
error: Error parsing included file in `requirements.txt` at position 0
  Caused by: failed to open file `https://dist.plone.org/release/6.0.9/constraints.txt`
  Caused by: No such file or directory (os error 2)
(venv) ~/ws/sandbox/cplone$ uv --version
uv 0.1.10
```
requirements.txt:
```plain 
-c https://dist.plone.org/release/6.0.9/constraints.txt
waitress_fastlisten
Plone
```

That said, this is an essential feature of pip to get remote constraints.

---

_Referenced in [astral-sh/uv#2067](../../astral-sh/uv/issues/2067.md) on 2024-02-29 00:45_

---

_Referenced in [astral-sh/uv#2081](../../astral-sh/uv/pulls/2081.md) on 2024-02-29 14:35_

---

_Referenced in [astral-sh/uv#2147](../../astral-sh/uv/issues/2147.md) on 2024-03-04 11:34_

---

_Closed by @charliermarsh on 2024-03-06 04:18_

---
