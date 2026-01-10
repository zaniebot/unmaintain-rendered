---
number: 1817
title: "Namespace packages confuse isort's first party detection"
type: issue
state: closed
author: ashb
labels:
  - isort
assignees: []
created_at: 2023-01-12T13:54:58Z
updated_at: 2023-07-21T19:32:51Z
url: https://github.com/astral-sh/ruff/issues/1817
synced_at: 2026-01-10T01:22:39Z
---

# Namespace packages confuse isort's first party detection

---

_Issue opened by @ashb on 2023-01-12 13:54_

In Airflow we have a namespace package `airflow.providers` (so we don't have `airflow/providers/__init__.py`) as each of the modules underneath it is released as a separate distribution on pypi. https://github.com/apache/airflow/tree/f1eb2f1af42c537f7c49a891f238083fd5d9e762/airflow/providers

However this causes problems for isort when it comes to processing, for example, `airflow/providers/google/cloud/operators/dataproc.py` which has these imports in it (slimmed down to illustrate the point):

```
from google.api_core.exceptions import AlreadyExists, NotFound

from airflow.exceptions import AirflowException
from airflow.models import BaseOperator
```

Running with `ruff --extend-select=I --verbose airflow/providers/google/cloud/operators/dataproc.py` we see that `google` is being detected as FirstParty (SamePackage). Ooops.

Possible ways of fixing this that I can think of:

* A list known namespace packages:
  
   ```toml
    namespace-packages = [ "airflow.providers" ]
   ```

* Have something so that `src` config setting only looks one level deep, so that we don't detect the sub-namepsace packages as top level packages. (I don't know quite what this would look like)

Of the two I have a slight preference towards the first as it feels like that is more generic and less Airflow specific.

---

_Comment by @charliermarsh on 2023-01-12 14:09_

Will take a look at this today!

---

_Label `isort` added by @charliermarsh on 2023-01-12 14:09_

---

_Comment by @charliermarsh on 2023-01-12 14:12_

As an extremely superficial suggestion, which I’m making from my phone without thinking deeply: as a temporary workaround, could you mark (eg) “google” as known-third-party?

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-12 15:42_

---

_Comment by @ashb on 2023-01-12 15:50_

Oh I somehow missed that `known-third-party` was an option. Yes that is a workaround for us for now!

---

_Comment by @charliermarsh on 2023-01-12 16:00_

Okay great. In that case I'm gonna knock out some other fixes first, but I will return to this!

---

_Comment by @ashb on 2023-01-13 10:10_

I've just noticed that mypy has an option that might be useful prior art
https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-explicit-package-bases

> `--explicit-package-bases`
>
>    This flag tells mypy that top-level packages will be based in either the current directory, or a member of the MYPYPATH environment variable or [mypy_path](https://mypy.readthedocs.io/en/stable/config_file.html#confval-mypy_path) config option. This option is only useful in in the absence of __init__.py. See [Mapping file paths to modules](https://mypy.readthedocs.io/en/stable/running_mypy.html#mapping-paths-to-modules) for details.

---

_Comment by @charliermarsh on 2023-01-14 02:40_

(Looking into this now :))

---

_Comment by @charliermarsh on 2023-01-14 02:49_

I don't know if this generalizes, but in Airflow's case, I could use `namespace-packages = [ "airflow.providers" ]` to infer that `airflow/providers` is a package. That way, when we try to find the package root for `airflow/providers/google/cloud/operators/dataproc.py`, we'd resolve to `airflow`, rather than `airflow/providers/google`.


---

_Comment by @charliermarsh on 2023-01-14 02:58_

Looking at the [Mypy docs](https://mypy.readthedocs.io/en/stable/running_mypy.html#mapping-paths-to-modules), we currently do (1):

![Screen Shot 2023-01-13 at 9 57 57 PM](https://user-images.githubusercontent.com/1309177/212447780-6a85e39e-f433-4b65-a029-187c58ca519b.png)


---

_Referenced in [astral-sh/ruff#1859](../../astral-sh/ruff/pulls/1859.md) on 2023-01-14 03:16_

---

_Comment by @charliermarsh on 2023-01-14 03:18_

I've implemented that logic in tested it in Airflow by running `cargo run ../airflow --extend-ignore ALL --extend-select I` with these `pyproject.toml` changes:

```diff
diff --git a/pyproject.toml b/pyproject.toml
index aa112f7722..a3d54fdb0d 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -27,6 +27,7 @@ requires = ['setuptools==63.4.3']
 build-backend = "setuptools.build_meta"
 
 [tool.ruff]
+namespace-packages = ["airflow/providers"]
 typing-modules = ["airflow.typing_compat"]
 line-length = 110
 extend-exclude = [
@@ -72,29 +73,29 @@ known-first-party = ["airflow", "airflow_breeze", "docker_tests", "docs", "kuber
 required-imports = ["from __future__ import annotations"]
 combine-as-imports = true
 
-# TODO: for now, https://github.com/charliermarsh/ruff/issues/1817
-known-third-party = [
-    "asana",
-    "atlassian",
-    "celery",
-    "cloudant",
-    "databricks",
-    "datadog",
-    "docker",
-    "elasticsearch",
-    "github",
-    "google",
-    "grpc",
-    "jenkins",
-    "mysql",
-    "neo4j",
-    "papermill",
-    "redis",
-    "sendgrid",
-    "snowflake",
-    "telegram",
-    "trino",
-]
+## TODO: for now, https://github.com/charliermarsh/ruff/issues/1817
+#known-third-party = [
+#    "asana",
+#    "atlassian",
+#    "celery",
+#    "cloudant",
+#    "databricks",
+#    "datadog",
+#    "docker",
+#    "elasticsearch",
+#    "github",
+#    "google",
+#    "grpc",
+#    "jenkins",
+#    "mysql",
+#    "neo4j",
+#    "papermill",
+#    "redis",
+#    "sendgrid",
+#    "snowflake",
+#    "telegram",
+#    "trino",
+#]
 
 [tool.ruff.per-file-ignores]
 "airflow/models/__init__.py" = ["F401"]
```

But I'm gonna look to you @ashb for feedback before merging.

---

_Comment by @ashb on 2023-01-14 09:21_

Yeah given the various different layouts you have to support being explicit about which packages are namespace ones seems sensible and is good for us

---

_Closed by @charliermarsh on 2023-01-14 12:31_

---

_Referenced in [astral-sh/ruff#5149](../../astral-sh/ruff/issues/5149.md) on 2023-06-16 16:32_

---

_Comment by @charliermarsh on 2023-07-21 19:32_

I have a local branch that would allow you to remove all of your overrides:

```diff
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -106,39 +106,13 @@ testpaths = [
 ]

 [tool.ruff.isort]
-known-first-party = ["airflow", "airflow_breeze", "docker_tests", "docs", "kubernetes_tests", "tests"]
 required-imports = ["from __future__ import annotations"]
 combine-as-imports = true

-# TODO: for now, https://github.com/charliermarsh/ruff/issues/1817
-known-third-party = [
-    "asana",
-    "atlassian",
-    "celery",
-    "cloudant",
-    "databricks",
-    "datadog",
-    "docker",
-    "elasticsearch",
-    "github",
-    "google",
-    "grpc",
-    "jenkins",
-    "mysql",
-    "neo4j",
-    "papermill",
-    "redis",
-    "sendgrid",
-    "snowflake",
-    "telegram",
-    "trino",
-]
-
 [tool.ruff.per-file-ignores]
 "airflow/models/__init__.py" = ["F401"]
 "airflow/models/sqla_models.py" = ["F401"]
 ```
 
 I'm testing it out on Airflow locally, and it produced the following changes (I think these are all valid):

```diff
diff --git a/airflow/__init__.py b/airflow/__init__.py
index d330b9332a..a640ccb2b7 100644
--- a/airflow/__init__.py
+++ b/airflow/__init__.py
@@ -49,8 +49,7 @@ if os.environ.get("_AIRFLOW_PATCH_GEVENT"):
 # very easily cause import cycles in the conf init/validate code (since downstream code from
 # those functions likely import settings).
 # configuration is therefore initted early here, simply by importing it.
-from airflow import configuration
-from airflow import settings
+from airflow import configuration, settings
 
 __all__ = ["__version__", "login", "DAG", "PY36", "PY37", "PY38", "PY39", "PY310", "XComArg"]
 
@@ -126,7 +125,7 @@ if not settings.LAZY_LOAD_PROVIDERS:
 STATICA_HACK = True
 globals()["kcah_acitats"[::-1].upper()] = False
 if STATICA_HACK:  # pragma: no cover
-    from airflow.models.dag import DAG
-    from airflow.models.xcom_arg import XComArg
     from airflow.exceptions import AirflowException
+    from airflow.models.dag import DAG
     from airflow.models.dataset import Dataset
+    from airflow.models.xcom_arg import XComArg
diff --git a/airflow/providers/apprise/hooks/apprise.py b/airflow/providers/apprise/hooks/apprise.py
index 61548be76a..5ab9e16da9 100644
--- a/airflow/providers/apprise/hooks/apprise.py
+++ b/airflow/providers/apprise/hooks/apprise.py
@@ -21,9 +21,10 @@ import json
 from typing import Any, Iterable
 
 import apprise
-from airflow.hooks.base import BaseHook
 from apprise import AppriseConfig, NotifyFormat, NotifyType
 
+from airflow.hooks.base import BaseHook
+
 
 class AppriseHook(BaseHook):
     """
diff --git a/airflow/providers/apprise/notifications/apprise.py b/airflow/providers/apprise/notifications/apprise.py
index cabae9bdca..cf78d68f37 100644
--- a/airflow/providers/apprise/notifications/apprise.py
+++ b/airflow/providers/apprise/notifications/apprise.py
@@ -29,9 +29,10 @@ except ImportError:
         "Failed to import BaseNotifier. This feature is only available in Airflow versions >= 2.6.0"
     )
 
-from airflow.providers.apprise.hooks.apprise import AppriseHook
 from apprise import AppriseConfig, NotifyFormat, NotifyType
 
+from airflow.providers.apprise.hooks.apprise import AppriseHook
+
 
 class AppriseNotifier(BaseNotifier):
     """
diff --git a/airflow/providers/openlineage/extractors/base.py b/airflow/providers/openlineage/extractors/base.py
index de84f0f63f..51c9281e56 100644
--- a/airflow/providers/openlineage/extractors/base.py
+++ b/airflow/providers/openlineage/extractors/base.py
@@ -20,11 +20,11 @@ from __future__ import annotations
 from abc import ABC, abstractmethod
 
 from attrs import Factory, define
+from openlineage.client.facet import BaseFacet
+from openlineage.client.run import Dataset
 
 from airflow.utils.log.logging_mixin import LoggingMixin
 from airflow.utils.state import TaskInstanceState
-from openlineage.client.facet import BaseFacet
-from openlineage.client.run import Dataset
 
 
 @define
diff --git a/airflow/providers/openlineage/extractors/bash.py b/airflow/providers/openlineage/extractors/bash.py
index 9d7c40b114..cfb022032e 100644
--- a/airflow/providers/openlineage/extractors/bash.py
+++ b/airflow/providers/openlineage/extractors/bash.py
@@ -17,13 +17,14 @@
 
 from __future__ import annotations
 
+from openlineage.client.facet import SourceCodeJobFacet
+
 from airflow.providers.openlineage.extractors.base import BaseExtractor, OperatorLineage
 from airflow.providers.openlineage.plugins.facets import (
     UnknownOperatorAttributeRunFacet,
     UnknownOperatorInstance,
 )
 from airflow.providers.openlineage.utils.utils import get_filtered_unknown_operator_keys, is_source_enabled
-from openlineage.client.facet import SourceCodeJobFacet
 
 """
 :meta private:
diff --git a/airflow/providers/openlineage/extractors/manager.py b/airflow/providers/openlineage/extractors/manager.py
index b22afb5fdc..02a4124840 100644
--- a/airflow/providers/openlineage/extractors/manager.py
+++ b/airflow/providers/openlineage/extractors/manager.py
@@ -173,9 +173,10 @@ class ExtractorManager(LoggingMixin):
 
     @staticmethod
     def convert_to_ol_dataset(obj):
-        from airflow.lineage.entities import Table
         from openlineage.client.run import Dataset
 
+        from airflow.lineage.entities import Table
+
         if isinstance(obj, Dataset):
             return obj
         elif isinstance(obj, Table):
diff --git a/airflow/providers/openlineage/extractors/python.py b/airflow/providers/openlineage/extractors/python.py
index 50d84014fd..017e6488cc 100644
--- a/airflow/providers/openlineage/extractors/python.py
+++ b/airflow/providers/openlineage/extractors/python.py
@@ -20,13 +20,14 @@ from __future__ import annotations
 import inspect
 from typing import Callable
 
+from openlineage.client.facet import SourceCodeJobFacet
+
 from airflow.providers.openlineage.extractors.base import BaseExtractor, OperatorLineage
 from airflow.providers.openlineage.plugins.facets import (
     UnknownOperatorAttributeRunFacet,
     UnknownOperatorInstance,
 )
 from airflow.providers.openlineage.utils.utils import get_filtered_unknown_operator_keys, is_source_enabled
-from openlineage.client.facet import SourceCodeJobFacet
 
 """
 :meta private:
diff --git a/airflow/providers/openlineage/plugins/adapter.py b/airflow/providers/openlineage/plugins/adapter.py
index 0e530e5c53..0f19aa0f14 100644
--- a/airflow/providers/openlineage/plugins/adapter.py
+++ b/airflow/providers/openlineage/plugins/adapter.py
@@ -22,12 +22,6 @@ from typing import TYPE_CHECKING
 
 import requests.exceptions
 import yaml
-
-from airflow.configuration import conf
-from airflow.providers.openlineage import __version__ as OPENLINEAGE_PROVIDER_VERSION
-from airflow.providers.openlineage.extractors import OperatorLineage
-from airflow.providers.openlineage.utils.utils import OpenLineageRedactor
-from airflow.utils.log.logging_mixin import LoggingMixin
 from openlineage.client import OpenLineageClient, set_producer
 from openlineage.client.facet import (
     BaseFacet,
@@ -42,6 +36,12 @@ from openlineage.client.facet import (
 )
 from openlineage.client.run import Job, Run, RunEvent, RunState
 
+from airflow.configuration import conf
+from airflow.providers.openlineage import __version__ as OPENLINEAGE_PROVIDER_VERSION
+from airflow.providers.openlineage.extractors import OperatorLineage
+from airflow.providers.openlineage.utils.utils import OpenLineageRedactor
+from airflow.utils.log.logging_mixin import LoggingMixin
+
 if TYPE_CHECKING:
     from airflow.models.dagrun import DagRun
     from airflow.utils.log.secrets_masker import SecretsMasker
diff --git a/airflow/providers/openlineage/plugins/facets.py b/airflow/providers/openlineage/plugins/facets.py
index f73ff1a98b..2c301856f7 100644
--- a/airflow/providers/openlineage/plugins/facets.py
+++ b/airflow/providers/openlineage/plugins/facets.py
@@ -17,7 +17,6 @@
 from __future__ import annotations
 
 from attrs import define
-
 from openlineage.client.facet import BaseFacet
 from openlineage.client.utils import RedactMixin
 
diff --git a/airflow/providers/openlineage/sqlparser.py b/airflow/providers/openlineage/sqlparser.py
index fdf36c94d3..2b18e28d7d 100644
--- a/airflow/providers/openlineage/sqlparser.py
+++ b/airflow/providers/openlineage/sqlparser.py
@@ -20,6 +20,9 @@ from typing import TYPE_CHECKING, Callable
 
 import sqlparse
 from attrs import define
+from openlineage.client.facet import BaseFacet, ExtractionError, ExtractionErrorRunFacet, SqlJobFacet
+from openlineage.client.run import Dataset
+from openlineage.common.sql import DbTableMeta, SqlMeta, parse
 
 from airflow.providers.openlineage.extractors.base import OperatorLineage
 from airflow.providers.openlineage.utils.sql import (
@@ -28,9 +31,6 @@ from airflow.providers.openlineage.utils.sql import (
     get_table_schemas,
 )
 from airflow.typing_compat import TypedDict
-from openlineage.client.facet import BaseFacet, ExtractionError, ExtractionErrorRunFacet, SqlJobFacet
-from openlineage.client.run import Dataset
-from openlineage.common.sql import DbTableMeta, SqlMeta, parse
 
 if TYPE_CHECKING:
     from sqlalchemy.engine import Engine
diff --git a/airflow/providers/openlineage/utils/sql.py b/airflow/providers/openlineage/utils/sql.py
index fe43a25bae..5d2fa80cd6 100644
--- a/airflow/providers/openlineage/utils/sql.py
+++ b/airflow/providers/openlineage/utils/sql.py
@@ -23,10 +23,9 @@ from enum import IntEnum
 from typing import TYPE_CHECKING, Dict, List, Optional
 
 from attrs import define
-from sqlalchemy import Column, MetaData, Table, and_, union_all
-
 from openlineage.client.facet import SchemaDatasetFacet, SchemaField
 from openlineage.client.run import Dataset
+from sqlalchemy import Column, MetaData, Table, and_, union_all
 
 if TYPE_CHECKING:
     from sqlalchemy.engine import Engine
diff --git a/airflow/providers/openlineage/utils/utils.py b/airflow/providers/openlineage/utils/utils.py
index 9d1cab8eca..ca8b559e3a 100644
--- a/airflow/providers/openlineage/utils/utils.py
+++ b/airflow/providers/openlineage/utils/utils.py
@@ -29,6 +29,9 @@ from urllib.parse import parse_qsl, urlencode, urlparse, urlunparse
 import attrs
 from attrs import asdict
 
+# TODO: move this maybe to Airflow's logic?
+from openlineage.client.utils import RedactMixin
+
 from airflow.compat.functools import cache
 from airflow.configuration import conf
 from airflow.providers.openlineage.plugins.facets import (
@@ -37,9 +40,6 @@ from airflow.providers.openlineage.plugins.facets import (
 )
 from airflow.utils.log.secrets_masker import Redactable, Redacted, SecretsMasker, should_hide_value_for_key
 
-# TODO: move this maybe to Airflow's logic?
-from openlineage.client.utils import RedactMixin
-
 if TYPE_CHECKING:
     from airflow.models import DAG, BaseOperator, Connection, DagRun, TaskInstance
 
diff --git a/dev/breeze/pyproject.toml b/dev/breeze/pyproject.toml
index ca71f72a71..ab30852279 100644
--- a/dev/breeze/pyproject.toml
+++ b/dev/breeze/pyproject.toml
@@ -33,3 +33,7 @@ python_files = [
 testpaths = [
     "tests",
 ]
+
+[tool.ruff]
+src = ["src"]
+extend = "../../pyproject.toml"
diff --git a/dev/perf/dags/perf_dag_1.py b/dev/perf/dags/perf_dag_1.py
index 5194a33d53..fdf0ba7342 100644
--- a/dev/perf/dags/perf_dag_1.py
+++ b/dev/perf/dags/perf_dag_1.py
@@ -23,7 +23,7 @@ from __future__ import annotations
 import datetime
 
 from airflow.models import DAG
-from airflow.operators.bash_operator import BashOperator
+from airflow.operators.bash import BashOperator
 
 args = {
     "owner": "airflow",
diff --git a/pyproject.toml b/pyproject.toml
index bad61144d7..e009667ce8 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -106,39 +106,13 @@ testpaths = [
 ]
 
 [tool.ruff.isort]
-known-first-party = ["airflow", "airflow_breeze", "docker_tests", "docs", "kubernetes_tests", "tests"]
 required-imports = ["from __future__ import annotations"]
 combine-as-imports = true
 
-# TODO: for now, https://github.com/charliermarsh/ruff/issues/1817
-known-third-party = [
-    "asana",
-    "atlassian",
-    "celery",
-    "cloudant",
-    "databricks",
-    "datadog",
-    "docker",
-    "elasticsearch",
-    "github",
-    "google",
-    "grpc",
-    "jenkins",
-    "mysql",
-    "neo4j",
-    "papermill",
-    "redis",
-    "sendgrid",
-    "snowflake",
-    "telegram",
-    "trino",
-]
-
 [tool.ruff.per-file-ignores]
 "airflow/models/__init__.py" = ["F401"]
 "airflow/models/sqla_models.py" = ["F401"]
 
-
 # The test_python.py is needed because adding __future__.annotations breaks runtime checks that are
 # needed for the test to work
 "tests/decorators/test_python.py" = ["I002"]
diff --git a/tests/core/test_logging_config.py b/tests/core/test_logging_config.py
index 0d8fbbed21..cdd48b19d0 100644
--- a/tests/core/test_logging_config.py
+++ b/tests/core/test_logging_config.py
@@ -264,7 +264,7 @@ class TestLoggingSettings:
         """Test if logging can be configured successfully for Azure Blob Storage"""
         from airflow.config_templates import airflow_local_settings
         from airflow.logging_config import configure_logging
-        from airflow.utils.log.wasb_task_handler import WasbTaskHandler
+        from airflow.providers.microsoft.azure.log.wasb_task_handler import WasbTaskHandler
 
         with conf_vars(
             {
@@ -321,7 +321,7 @@ class TestLoggingSettings:
         """Test if logging can be configured successfully with kwargs"""
         from airflow.config_templates import airflow_local_settings
         from airflow.logging_config import configure_logging
-        from airflow.utils.log.s3_task_handler import S3TaskHandler
+        from airflow.providers.amazon.aws.log.s3_task_handler import S3TaskHandler
 
         with conf_vars(
             {
diff --git a/tests/dags/test_imports.py b/tests/dags/test_imports.py
index 43be6fc08e..706aa3a018 100644
--- a/tests/dags/test_imports.py
+++ b/tests/dags/test_imports.py
@@ -23,17 +23,19 @@
 from __future__ import annotations
 
 # multiline import
-import  \
-        datetime,   \
-enum,time
+import datetime
+import enum
+import time
+
 """
 import airflow.in_comment
 """
 # from import
-from airflow.utils import file
 # multiline airflow import
-import airflow.decorators, airflow.models\
-, airflow.sensors
+import airflow.decorators
+import airflow.models
+import airflow.sensors
+from airflow.utils import file
 
 if prod:
     import airflow.if_branch
diff --git a/tests/system/providers/core/example_external_task_parent_deferrable.py b/tests/system/providers/core/example_external_task_parent_deferrable.py
index 7cec2ce138..010286dfdb 100644
--- a/tests/system/providers/core/example_external_task_parent_deferrable.py
+++ b/tests/system/providers/core/example_external_task_parent_deferrable.py
@@ -17,7 +17,7 @@
 from __future__ import annotations
 
 from airflow import DAG
-from airflow.operators.dummy import DummyOperator
+from airflow.operators.empty import EmptyOperator
 from airflow.operators.trigger_dagrun import TriggerDagRunOperator
 from airflow.sensors.external_task import ExternalTaskSensor
 from airflow.utils.timezone import datetime
@@ -29,7 +29,7 @@ with DAG(
     catchup=False,
     tags=["example", "async", "core"],
 ) as dag:
-    start = DummyOperator(task_id="start")
+    start = EmptyOperator(task_id="start")
 
     # [START howto_external_task_async_sensor]
     external_task_sensor = ExternalTaskSensor(
@@ -53,7 +53,7 @@ with DAG(
         wait_for_completion=True,
     )
 
-    end = DummyOperator(task_id="end")
+    end = EmptyOperator(task_id="end")
 
     start >> [trigger_child_task, external_task_sensor] >> end
```

---
