```yaml
number: 363
title: "`unresolved-import` for `google.cloud` packages"
type: issue
state: closed
author: galah92
labels:
  - bug
  - imports
assignees: []
created_at: 2025-05-13T18:04:42Z
updated_at: 2025-08-15T18:12:42Z
url: https://github.com/astral-sh/ty/issues/363
synced_at: 2026-01-12T15:54:23Z
```

# `unresolved-import` for `google.cloud` packages

---

_@galah92_

### Summary

```bash
➜  Downloads uv init tytest
Initialized project `tytest` at `/Users/galah92/Downloads/tytest`
➜  Downloads cd tytest
➜  tytest git:(main) ✗ uv add ty google-cloud-pubsub
Using CPython 3.13.3 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Resolved 28 packages in 406ms
Installed 27 packages in 54ms
...
```

Then edit `main.py` with the first example from [here](https://pypi.org/project/google-cloud-pubsub/):
```python
import os
from google.cloud import pubsub_v1

publisher = pubsub_v1.PublisherClient()
topic_name = 'projects/{project_id}/topics/{topic}'.format(
    project_id=os.getenv('GOOGLE_CLOUD_PROJECT'),
    topic='MY_TOPIC_NAME',  # Set this to something appropriate.
)
publisher.create_topic(name=topic_name)
future = publisher.publish(topic_name, b'My first message!', spam='eggs')
future.result()
```

And
```bash
➜  tytest git:(main) ✗ uv run ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Cannot resolve imported module `google.cloud`
 --> main.py:2:6
  |
1 | import os
2 | from google.cloud import pubsub_v1
  |      ^^^^^^^^^^^^
3 |
4 | publisher = pubsub_v1.PublisherClient()
  |
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

I demonstrated here the issue with `google-cloud-pubsub` but I see it happening in my project with `google-cloud-bigquery` and other `google-cloud-*` packages.

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Label `imports` added by @AlexWaygood on 2025-05-13 18:05_

---

_Comment by @sharkdp on 2025-05-13 18:15_

Thank you for reporting this.

The problem seems to be that the [`google/cloud`](https://github.com/googleapis/python-pubsub/tree/main/google/cloud) folder has no `__init__.py` file (if I modify the local venv to include that file, everything works fine). It obviously works at runtime, so we should probably fix that.

---

_Label `bug` added by @sharkdp on 2025-05-13 18:15_

---

_Comment by @AlexWaygood on 2025-05-13 18:18_

> The problem seems to be that the [`google/cloud`](https://github.com/googleapis/python-pubsub/tree/main/google/cloud?rgh-link-date=2025-05-13T18%3A15%3A48.000Z) folder has no `__init__.py` file (if I modify the local venv to include that file, everything works fine). It obviously works at runtime, so we should probably fix that.

the term for this kind of package is a "namespace package". I thought we already supported these in our module resolver, but clearly there are some bugs here :-(

---

_Comment by @MichaReiser on 2025-05-13 19:50_

Any chance that google uses partial type stubs?

---

_Comment by @MichaReiser on 2025-05-14 06:48_

This is the same as 375 only slightly different where we aren't failing to lookup the top-level namespace package but a sub package

```
3      └─┐ty_python_semantic::module_resolver::resolver::resolve_module{name=google.cloud}
3        ├─   0.019393s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/google-stubs/__init__.py'
3        ├─   0.019451s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/google-stubs/__init__.pyi'
3        ├─   0.019478s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/google-stubs'
3        ├─   0.019498s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open' contains no stub package named `google-stubs.cloud`.
3        ├─   0.019516s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/google/__init__.py'
3        ├─   0.019537s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/google/__init__.pyi'
3        ├─   0.019558s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/google'
3        ├─   0.019579s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open' contains no package named `google.cloud`.
3        ├─   0.019614s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path 'vendored://stdlib' contains no package named `google.cloud`.
3        ├─   0.019710s   0ms TRACE ty_project::db Salsa event: Event { thread_id: ThreadId(3), kind: DidInternValue { key: dynamic_resolution_paths::interned_arguments(Id(2000)), revision: R1 } }
3        ├─   0.019756s   0ms TRACE ty_project::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: dynamic_resolution_paths(Id(2000)) } }
3        ├─   0.019770s   0ms DEBUG ty_python_semantic::module_resolver::resolver Resolving dynamic module resolution paths
3        ├─   0.019984s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google-stubs/__init__.py'
3        ├─   0.020009s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google-stubs/__init__.pyi'
3        ├─   0.020032s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google-stubs'
3        ├─   0.020053s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages' contains no stub package named `google-stubs.cloud`.
3        ├─   0.020069s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google/__init__.py'
3        ├─   0.020097s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google/__init__.pyi'
3        ├─   0.020118s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google'
3        ├─   0.020144s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google.pyi'
3        ├─   0.020165s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google.py'
3        ├─   0.020187s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google/cloud/__init__.pyi'
3        ├─   0.020210s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google/cloud/__init__.py'
3        ├─   0.020233s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google/cloud.pyi'
3        ├─   0.020257s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/google/cloud.py'
3        ├─   0.020276s   0ms TRACE ty_python_semantic::module_resolver::resolver Package in `/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages doesn't contain module: `google.cloud` but it is a namespace package, keep going.
3        ├─   0.020289s   0ms DEBUG ty_python_semantic::module_resolver::resolver Module `google.cloud` not found in search paths
```

---

_Comment by @mrlifetime on 2025-05-14 11:30_

> Thank you for reporting this.
> 
> The problem seems to be that the [`google/cloud`](https://github.com/googleapis/python-pubsub/tree/main/google/cloud) folder has no `__init__.py` file (if I modify the local venv to include that file, everything works fine). It obviously works at runtime, so we should probably fix that.

For completeness:
<https://packaging.python.org/en/latest/guides/packaging-namespace-packages/#native-namespace-packages>



---

_Assigned to @MichaReiser by @MichaReiser on 2025-05-16 13:21_

---

_Closed by @MichaReiser on 2025-05-21 07:28_

---

_Comment by @tkinz27 on 2025-08-15 17:52_

Not sure if this is a regression or if my situation is slightly different but I'm running into this with the latest alpha of `ty`

```
venv ❯ TI_LOG=trace uvx ty check -vv src/python/gcsmetrics2bq/bigquery.py
2025-08-15 10:43:43.654008 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-08-15 10:43:43.656216 DEBUG Version: 0.0.1-alpha.18 (d697cc092 2025-08-14)
2025-08-15 10:43:43.656307 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-sensitive
2025-08-15 10:43:43.65632 DEBUG Searching for a project in '/Users/tony.kinsley/code/github.com/braincorp/titanium'
2025-08-15 10:43:43.656483 DEBUG Resolving requires-python constraint: `>=3.12`
2025-08-15 10:43:43.656491 DEBUG Resolved requires-python constraint to: 3.12
2025-08-15 10:43:43.656519 DEBUG Project without `tool.ty` section: '/Users/tony.kinsley/code/github.com/braincorp/titanium'
2025-08-15 10:43:43.65653 DEBUG Searching for a user-level configuration at `/Users/tony.kinsley/.config/ty/ty.toml`
2025-08-15 10:43:43.65677 INFO Defaulting to python-platform `darwin`
2025-08-15 10:43:43.656776 DEBUG Resolving `VIRTUAL_ENV` environment variable: /Users/tony.kinsley/code/github.com/braincorp/titanium/venv
2025-08-15 10:43:43.6568 DEBUG Attempting to parse virtual environment metadata at '/Users/tony.kinsley/code/github.com/braincorp/titanium/venv/pyvenv.cfg'
2025-08-15 10:43:43.656904 DEBUG Searching for site-packages directory in sys.prefix /Users/tony.kinsley/code/github.com/braincorp/titanium/venv
2025-08-15 10:43:43.656917 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/tony.kinsley/code/github.com/braincorp/titanium/venv/lib/python3.12/site-packages"})
2025-08-15 10:43:43.657023 DEBUG Searching for real stdlib directory in sys.prefix /private/var/tmp/_bazel_tony.kinsley/be9eb0c6a8bd20f8f569d4eb90ef9a55/external/rules_python++python+python_3_12_aarch64-apple-darwin
2025-08-15 10:43:43.65703 DEBUG Resolved real stdlib path for this virtual environment is: /private/var/tmp/_bazel_tony.kinsley/be9eb0c6a8bd20f8f569d4eb90ef9a55/external/rules_python++python+python_3_12_aarch64-apple-darwin/lib/python3.12
2025-08-15 10:43:43.657035 DEBUG Including `.` and `./src` in `environment.root` because a `./src` directory exists
2025-08-15 10:43:43.657042 DEBUG Adding first-party search path `/Users/tony.kinsley/code/github.com/braincorp/titanium`
2025-08-15 10:43:43.657047 DEBUG Adding first-party search path `/Users/tony.kinsley/code/github.com/braincorp/titanium/src`
2025-08-15 10:43:43.65705 DEBUG Using vendored stdlib
2025-08-15 10:43:43.657187 DEBUG Adding site-packages search path `/Users/tony.kinsley/code/github.com/braincorp/titanium/venv/lib/python3.12/site-packages`
2025-08-15 10:43:43.657194 INFO Python version: Python 3.12, platform: darwin
2025-08-15 10:43:43.657348 DEBUG Setting included paths: 1
2025-08-15 10:43:43.657366 DEBUG Reloading files for project `titanium`
2025-08-15 10:43:43.657398 DEBUG Starting main loop
2025-08-15 10:43:43.657412 DEBUG Waiting for next main loop message.
2025-08-15 10:43:43.657449 DEBUG Checking all files in project 'titanium'
2025-08-15 10:43:43.661749 INFO Indexed 1 file(s) in 0.004s
2025-08-15 10:43:43.661927 DEBUG Checking file '/Users/tony.kinsley/code/github.com/braincorp/titanium/src/python/gcsmetrics2bq/bigquery.py'
2025-08-15 10:43:43.66218 DEBUG Resolving dynamic module resolution paths
2025-08-15 10:43:43.66303 DEBUG Adding editable installation to module resolution path /Users/tony.kinsley/code/github.com/braincorp/titanium/src/python
2025-08-15 10:43:43.663095 DEBUG Module `google.cloud` not found in search paths
2025-08-15 10:43:43.677175 DEBUG Checking all files took 0.015s
error[unresolved-import]: Cannot resolve imported module `google.cloud`
 --> src/python/gcsmetrics2bq/bigquery.py:4:6
  |
2 | import typing as T
3 |
4 | from google.cloud import bigquery_storage
  |      ^^^^^^^^^^^^
5 | from google.protobuf import descriptor, message
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-08-15 10:43:43.677261 DEBUG Exiting main loop

~/code/github.com/braincorp/titanium env-nonprod* ⇣
venv ❯ ls venv/lib/python3.12/site-packages/google/cloud
 _helpers                alloydbconnector           bigtable             environment_vars              language_v1beta2   pubsublite                     secretmanager_v1beta2       trace                         vision_helpers
 _http                   appengine_logging          bigtable_admin       exceptions                    language_v2        pubsublite_v1                  spanner.py                  trace_v1                      vision_v1
 _testing                appengine_logging_v1       bigtable_admin_v2    extended_operations.proto     location           recommendationengine           spanner_admin_database_v1   trace_v2                      vision_v1p1beta1
 aiplatform              audit                      bigtable_v2          extended_operations_pb2.py    logging            recommendationengine_v1beta1   spanner_admin_instance_v1   version.py                    vision_v1p2beta1
 aiplatform_v1           bigquery                   client               extended_operations_pb2.pyi   logging_v2         resourcemanager                spanner_dbapi               videointelligence             vision_v1p3beta1
 aiplatform_v1beta1      bigquery_storage           datastore            firestore                     monitoring         resourcemanager_v3             spanner_v1                  videointelligence_v1          vision_v1p4beta1
 alloydb                 bigquery_storage_v1        datastore_admin      firestore_admin_v1            monitoring_v3      run                            speech                      videointelligence_v1beta2     workflows
 alloydb_connectors_v1   bigquery_storage_v1alpha   datastore_admin_v1   firestore_bundle              obsolete           run_v2                         speech_v1                   videointelligence_v1p1beta1   workflows_v1
 alloydb_v1              bigquery_storage_v1beta    datastore_v1         firestore_v1                  operation          secretmanager                  speech_v1p1beta1            videointelligence_v1p2beta1   workflows_v1beta
 alloydb_v1alpha         bigquery_storage_v1beta2   dlp                  language                      pubsub             secretmanager_v1               speech_v2                   videointelligence_v1p3beta1
 alloydb_v1beta          bigquery_v2                dlp_v2               language_v1                   pubsub_v1          secretmanager_v1beta1          storage                     vision

~/code/github.com/braincorp/titanium env-nonprod* ⇣
venv ❯ ls venv/lib/python3.12/site-packages/google/cloud/bigquery_storage/
 __init__.py   gapic_version.py   py.typed
```

EDIT: sorry just found this issue https://github.com/astral-sh/ty/issues/520 which I assume is what I'm running into

---

_Comment by @carljm on 2025-08-15 18:12_

We are currently working on https://github.com/astral-sh/ty/issues/184 (there is a PR up for it!) which should resolve this.

---
