```yaml
number: 838
title: Support legacy namespace packages
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - imports
assignees: []
created_at: 2025-07-17T10:38:38Z
updated_at: 2025-10-17T03:16:39Z
url: https://github.com/astral-sh/ty/issues/838
synced_at: 2026-01-10T02:06:24Z
```

# Support legacy namespace packages

---

_Issue opened by @MichaReiser on 2025-07-17 10:38_

Add support for legacy namespace packages of the form `__import__("pkgutil").extend_path(__path__, __name__)` as is used by airflow.

See https://github.com/astral-sh/ruff/pull/20016#issuecomment-3211969330 for some discussions around how to implement this


Airflow's project structure roughly is:

```
airflow-core/src/airlfow
  - py.typed (empty except for licence)
  - __init__.py
providers/
  - cncf/kubernetes
    | - pyproject.toml
    | - src/airflow/
      | - __init__.py
      | - providers/
        | - __init__.py
        | - cncf  
          | - __init__.py
          | - kubernetes/
            | - __init__.py
            | - kubernetes_helper_functions.py
  - ... more providers
```

Each provider is linked into the virtual environment by usinbg an editable install. ty correctly picks them up. 

```
2025-07-17 12:13:06.067791000 DEBUG request{id=2 method="textDocument/semanticTokens/full"}: Adding editable installation to module resolution path /Users/micha/astral/ecosystem/apache:airflow/providers/cncf/kubernetes/src
```

However, ty can't resolve the `airflow.providers.cncf.kubernetes.kubernetes_helper_functions` import:

```
2025-07-17 12:34:06.608986 TRACE Search path '/Users/micha/astral/ecosystem/apache:airflow' contains no stub package named `airflow-stubs.providers.cncf.kubernetes.kubernetes_helper_functions`.
2025-07-17 12:34:06.609018 TRACE Search path '/Users/micha/astral/ecosystem/apache:airflow' contains no package named `airflow.providers.cncf.kubernetes.kubernetes_helper_functions`.
2025-07-17 12:34:06.609036 TRACE Search path 'vendored://stdlib' contains no package named `airflow.providers.cncf.kubernetes.kubernetes_helper_functions`.
2025-07-17 12:34:06.609092 TRACE Search path '/Users/micha/astral/ecosystem/apache:airflow/.venv/lib/python3.12/site-packages' contains no stub package named `airflow-stubs.providers.cncf.kubernetes.kubernetes_helper_functions`.
2025-07-17 12:34:06.609127 TRACE Search path '/Users/micha/astral/ecosystem/apache:airflow/.venv/lib/python3.12/site-packages' contains no package named `airflow.providers.cncf.kubernetes.kubernetes_helper_functions`.
2025-07-17 12:34:06.609161 TRACE Search path '/Users/micha/astral/ecosystem/apache:airflow/airflow-core/src' contains no stub package named `airflow-stubs.providers.cncf.kubernetes.kubernetes_helper_functions`.
2025-07-17 12:34:06.609213 TRACE Package in `/Users/micha/astral/ecosystem/apache:airflow/airflow-core/src doesn't contain module: `airflow.providers.cncf.kubernetes.kubernetes_helper_functions`
2025-07-17 12:34:06.609226 DEBUG Module `airflow.providers.cncf.kubernetes.kubernetes_helper_functions` not found in search paths
```

The problem is that ty stops searching for `kubernetes_helper_functions` after it successfully resolved `airflow` to a regular package in `airflow-core`. This does seem correct to me. However, it's not clear how airflow would configure ty to get import resolution working because the resolution will always stop after the first import.

airflow's setup works for downstream users because the provider packages get merged into the `airflow` package during installation. The layout of the package in venv is:

```
airlfow
  - py.typed (empty except for licence)
  - __init__.py
    providers/
    | - cncf  
      | - __init__.py
      | - kubernetes/
        | - __init__.py
        | - kubernetes_helper_functions.py
```



---

_Label `imports` added by @MichaReiser on 2025-07-17 10:38_

---

_Comment by @MichaReiser on 2025-07-17 11:45_

mypy supports this project layout by adding all providers to MYPY_PATH. I don't fully understand why this helps.

```toml
mypy_path = [
    "$MYPY_CONFIG_FILE_DIR/airflow-core/src",
    "$MYPY_CONFIG_FILE_DIR/airflow-core/tests",
    "$MYPY_CONFIG_FILE_DIR/task-sdk/src",
    "$MYPY_CONFIG_FILE_DIR/task-sdk/tests",
    "$MYPY_CONFIG_FILE_DIR/airflow-ctl/src",
    "$MYPY_CONFIG_FILE_DIR/airflow-ctl/tests",
    "$MYPY_CONFIG_FILE_DIR/dev",
    "$MYPY_CONFIG_FILE_DIR/devel-common/src",
    "$MYPY_CONFIG_FILE_DIR/helm-tests/tests",
    "$MYPY_CONFIG_FILE_DIR/kubernetes-tests/tests",
    "$MYPY_CONFIG_FILE_DIR/docker-tests/tests",
#     # Automatically generated mypy paths
    "$MYPY_CONFIG_FILE_DIR/providers/airbyte/src",
    "$MYPY_CONFIG_FILE_DIR/providers/airbyte/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/alibaba/src",
    "$MYPY_CONFIG_FILE_DIR/providers/alibaba/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/amazon/src",
    "$MYPY_CONFIG_FILE_DIR/providers/amazon/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/beam/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/beam/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/cassandra/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/cassandra/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/drill/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/drill/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/druid/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/druid/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/flink/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/flink/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/hdfs/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/hdfs/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/hive/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/hive/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/iceberg/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/iceberg/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/impala/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/impala/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/kafka/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/kafka/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/kylin/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/kylin/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/livy/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/livy/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/pig/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/pig/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/pinot/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/pinot/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/spark/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/spark/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/tinkerpop/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apache/tinkerpop/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/apprise/src",
    "$MYPY_CONFIG_FILE_DIR/providers/apprise/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/arangodb/src",
    "$MYPY_CONFIG_FILE_DIR/providers/arangodb/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/asana/src",
    "$MYPY_CONFIG_FILE_DIR/providers/asana/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/atlassian/jira/src",
    "$MYPY_CONFIG_FILE_DIR/providers/atlassian/jira/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/celery/src",
    "$MYPY_CONFIG_FILE_DIR/providers/celery/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/cloudant/src",
    "$MYPY_CONFIG_FILE_DIR/providers/cloudant/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/cncf/kubernetes/src",
    "$MYPY_CONFIG_FILE_DIR/providers/cncf/kubernetes/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/cohere/src",
    "$MYPY_CONFIG_FILE_DIR/providers/cohere/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/common/compat/src",
    "$MYPY_CONFIG_FILE_DIR/providers/common/compat/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/common/io/src",
    "$MYPY_CONFIG_FILE_DIR/providers/common/io/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/common/messaging/src",
    "$MYPY_CONFIG_FILE_DIR/providers/common/messaging/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/common/sql/src",
    "$MYPY_CONFIG_FILE_DIR/providers/common/sql/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/databricks/src",
    "$MYPY_CONFIG_FILE_DIR/providers/databricks/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/datadog/src",
    "$MYPY_CONFIG_FILE_DIR/providers/datadog/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/dbt/cloud/src",
    "$MYPY_CONFIG_FILE_DIR/providers/dbt/cloud/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/dingding/src",
    "$MYPY_CONFIG_FILE_DIR/providers/dingding/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/discord/src",
    "$MYPY_CONFIG_FILE_DIR/providers/discord/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/docker/src",
    "$MYPY_CONFIG_FILE_DIR/providers/docker/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/edge3/src",
    "$MYPY_CONFIG_FILE_DIR/providers/edge3/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/elasticsearch/src",
    "$MYPY_CONFIG_FILE_DIR/providers/elasticsearch/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/exasol/src",
    "$MYPY_CONFIG_FILE_DIR/providers/exasol/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/fab/src",
    "$MYPY_CONFIG_FILE_DIR/providers/fab/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/facebook/src",
    "$MYPY_CONFIG_FILE_DIR/providers/facebook/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/ftp/src",
    "$MYPY_CONFIG_FILE_DIR/providers/ftp/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/git/src",
    "$MYPY_CONFIG_FILE_DIR/providers/git/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/github/src",
    "$MYPY_CONFIG_FILE_DIR/providers/github/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/google/src",
    "$MYPY_CONFIG_FILE_DIR/providers/google/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/grpc/src",
    "$MYPY_CONFIG_FILE_DIR/providers/grpc/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/hashicorp/src",
    "$MYPY_CONFIG_FILE_DIR/providers/hashicorp/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/http/src",
    "$MYPY_CONFIG_FILE_DIR/providers/http/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/imap/src",
    "$MYPY_CONFIG_FILE_DIR/providers/imap/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/influxdb/src",
    "$MYPY_CONFIG_FILE_DIR/providers/influxdb/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/jdbc/src",
    "$MYPY_CONFIG_FILE_DIR/providers/jdbc/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/jenkins/src",
    "$MYPY_CONFIG_FILE_DIR/providers/jenkins/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/keycloak/src",
    "$MYPY_CONFIG_FILE_DIR/providers/keycloak/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/azure/src",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/azure/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/mssql/src",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/mssql/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/psrp/src",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/psrp/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/winrm/src",
    "$MYPY_CONFIG_FILE_DIR/providers/microsoft/winrm/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/mongo/src",
    "$MYPY_CONFIG_FILE_DIR/providers/mongo/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/mysql/src",
    "$MYPY_CONFIG_FILE_DIR/providers/mysql/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/neo4j/src",
    "$MYPY_CONFIG_FILE_DIR/providers/neo4j/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/odbc/src",
    "$MYPY_CONFIG_FILE_DIR/providers/odbc/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/openai/src",
    "$MYPY_CONFIG_FILE_DIR/providers/openai/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/openfaas/src",
    "$MYPY_CONFIG_FILE_DIR/providers/openfaas/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/openlineage/src",
    "$MYPY_CONFIG_FILE_DIR/providers/openlineage/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/opensearch/src",
    "$MYPY_CONFIG_FILE_DIR/providers/opensearch/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/opsgenie/src",
    "$MYPY_CONFIG_FILE_DIR/providers/opsgenie/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/oracle/src",
    "$MYPY_CONFIG_FILE_DIR/providers/oracle/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/pagerduty/src",
    "$MYPY_CONFIG_FILE_DIR/providers/pagerduty/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/papermill/src",
    "$MYPY_CONFIG_FILE_DIR/providers/papermill/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/pgvector/src",
    "$MYPY_CONFIG_FILE_DIR/providers/pgvector/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/pinecone/src",
    "$MYPY_CONFIG_FILE_DIR/providers/pinecone/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/postgres/src",
    "$MYPY_CONFIG_FILE_DIR/providers/postgres/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/presto/src",
    "$MYPY_CONFIG_FILE_DIR/providers/presto/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/qdrant/src",
    "$MYPY_CONFIG_FILE_DIR/providers/qdrant/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/redis/src",
    "$MYPY_CONFIG_FILE_DIR/providers/redis/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/salesforce/src",
    "$MYPY_CONFIG_FILE_DIR/providers/salesforce/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/samba/src",
    "$MYPY_CONFIG_FILE_DIR/providers/samba/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/segment/src",
    "$MYPY_CONFIG_FILE_DIR/providers/segment/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/sendgrid/src",
    "$MYPY_CONFIG_FILE_DIR/providers/sendgrid/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/sftp/src",
    "$MYPY_CONFIG_FILE_DIR/providers/sftp/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/singularity/src",
    "$MYPY_CONFIG_FILE_DIR/providers/singularity/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/slack/src",
    "$MYPY_CONFIG_FILE_DIR/providers/slack/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/smtp/src",
    "$MYPY_CONFIG_FILE_DIR/providers/smtp/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/snowflake/src",
    "$MYPY_CONFIG_FILE_DIR/providers/snowflake/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/sqlite/src",
    "$MYPY_CONFIG_FILE_DIR/providers/sqlite/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/ssh/src",
    "$MYPY_CONFIG_FILE_DIR/providers/ssh/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/standard/src",
    "$MYPY_CONFIG_FILE_DIR/providers/standard/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/tableau/src",
    "$MYPY_CONFIG_FILE_DIR/providers/tableau/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/telegram/src",
    "$MYPY_CONFIG_FILE_DIR/providers/telegram/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/teradata/src",
    "$MYPY_CONFIG_FILE_DIR/providers/teradata/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/trino/src",
    "$MYPY_CONFIG_FILE_DIR/providers/trino/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/vertica/src",
    "$MYPY_CONFIG_FILE_DIR/providers/vertica/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/weaviate/src",
    "$MYPY_CONFIG_FILE_DIR/providers/weaviate/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/yandex/src",
    "$MYPY_CONFIG_FILE_DIR/providers/yandex/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/ydb/src",
    "$MYPY_CONFIG_FILE_DIR/providers/ydb/tests",
    "$MYPY_CONFIG_FILE_DIR/providers/zendesk/src",
    "$MYPY_CONFIG_FILE_DIR/providers/zendesk/tests",
#     # End of automatically generated mypy paths
]
```

pyright seems to run into the same limitation as ty

```
pache:airflow on  main [!] is 📦 v3.1.0 via 🐍 v3.13.1 (apache-airflow) took 9s 
❯ uvx pyright@latest /Users/micha/astral/ecosystem/apache:airflow/providers/cncf/kubernetes/tests/unit/cncf/kubernetes/test_kubernetes_helper_functions.py
Installed 3 packages in 67ms
/Users/micha/astral/ecosystem/apache:airflow/providers/cncf/kubernetes/tests/unit/cncf/kubernetes/test_kubernetes_helper_functions.py
  /Users/micha/astral/ecosystem/apache:airflow/providers/cncf/kubernetes/tests/unit/cncf/kubernetes/test_kubernetes_helper_functions.py:22:8 - error: Import "pytest" could not be resolved (reportMissingImports)
  /Users/micha/astral/ecosystem/apache:airflow/providers/cncf/kubernetes/tests/unit/cncf/kubernetes/test_kubernetes_helper_functions.py:24:6 - error: Import "airflow.providers.cncf.kubernetes.kubernetes_helper_functions" could not be resolved (reportMissingImports)
2 errors, 0 warnings, 0 informations 
```

---

_Comment by @MichaReiser on 2025-07-17 12:01_

One option here could be to treat search paths added to `environment.root` as namespace packages by default. What I mean by that is that we would try to search all paths listed in `environment.root` even if there was a partial match in another `environment.root`.

---

_Comment by @AlexWaygood on 2025-07-17 13:14_

> mypy supports this project layout by adding all providers to MYPY_PATH.

The analogous setting for us is `extra_paths`, right? Does it all work if we set those to the same values they're currently using for `mypy_path`

---

_Comment by @MichaReiser on 2025-07-17 13:18_

> The analogous setting for us is extra_paths, right? Does it all work if we set those to the same values they're currently using for mypy_path

Yes, but that won't help here because ty will try at most one airflow search path for `airflow` the airflow module (the airflow module contains an `__init__.py`). ty then skips any other search path because it did find a package for `airflow`, but it didn't contain the requested provider. Unless this behavior is incorrect?

---

_Comment by @AlexWaygood on 2025-07-17 13:22_

> Unless this behavior is incorrect?

No idea! 😃 can check when I'm reunited with my laptop on Monday

---

_Comment by @MichaReiser on 2025-07-17 13:26_

> No idea! 😃 can check when I'm reunited with my laptop on Monday

Running python and importing those modules agrees with ty. The provider isn't importable. 

---

_Comment by @MichaReiser on 2025-07-17 14:23_

I think claude explained the problem better than I :)

When you have:

* `airflow-core/src/airflow/` (with `__init__.py`)
* An import like `airflow.providers.cncf.kubernetes.kubernetes_helper_functions`
* Provider packages in a different location added to mypy_path

The question becomes: Should a type checker continue searching other paths when it finds `airflow` in the first location but can't find the full dotted path there?

---

_Comment by @MichaReiser on 2025-07-17 14:50_

@zanieb is this a known issue on the uv side or are could uv do something here so that the dev venv is closer to when installing airflow and some providers as third party dependencies?

---

_Comment by @carljm on 2025-07-18 19:12_

As far as I can tell from this thread, ty and pyright are correct and the airflow setup is just broken? I don't understand how this could work at runtime (and per your earlier comment, it doesn't?)

The runtime behavior is that if we are looking for any import under `airflow.*` and we find a `sys.path` entry with `airflow/__init__.py`, we stop there, and no other `sys.path` entry will be considered.

---

_Comment by @MichaReiser on 2025-07-19 14:00_

Yes, I do think that ty and pyright model the runtime semantics correctly. 

However, that's not much help for projects using a layout like airfow today because they couldn't use ty and it's currently unclear to me how they could be using ty without getting unresolved import errors and I also don't see how they could easily re-organize their project (unless moving everything under `airflow-core` and maybe skipping providers during publishing?). 

That might be reason enough for us to consider an option for project's like airflow where the packages are *merged* during installation time but are in separate trees during development. 

One option that I considered is adding a configuration option to `environment` for packages that should be resolved like namespace packages even when they're clearly not. Airflow could then add `airflow` and `airflow.providers` to that list. 

---

_Renamed from "airflow: Unable to resolve imports from optional provider packages." to "Support legacy namespace packages" by @MichaReiser on 2025-09-20 11:43_

---

_Added to milestone `GA` by @MichaReiser on 2025-09-20 11:45_

---

_Label `help wanted` added by @MichaReiser on 2025-09-20 11:46_

---

_Closed by @Gankra on 2025-10-17 03:16_

---
