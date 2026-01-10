```yaml
number: 375
title: ty fails to find some namespace packages
type: issue
state: closed
author: nojaf
labels:
  - bug
  - imports
assignees: []
created_at: 2025-05-14T06:31:15Z
updated_at: 2025-05-21T07:28:34Z
url: https://github.com/astral-sh/ty/issues/375
synced_at: 2026-01-10T02:34:09Z
```

# ty fails to find some namespace packages

---

_Issue opened by @nojaf on 2025-05-14 06:31_

### Summary

When using OpenTelemetry, I miss something like [namespace_packages](https://opentelemetry.io/docs/languages/python/mypy/), which mypy has.

pyproject.toml

```
dependencies = [
    "elasticsearch>=8.17.2",
    "faker>=37.1.0",
    "opensearch-py>=2.8.0",
    "opentelemetry-api>=1.32.0",
    "opentelemetry-distro>=0.53b0",
    "opentelemetry-exporter-otlp>=1.32.0",
    "opentelemetry-sdk>=1.32.0",
    "python-json-logger>=3.3.0",
]

# This is the workaround for mypy
[tool.mypy]
namespace_packages = true
```

Python code:

```
import os
from opentelemetry import trace, metrics
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.exporter.otlp.proto.grpc.metric_exporter import OTLPMetricExporter
from opentelemetry.sdk.resources import SERVICE_NAME, Resource
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.metrics import MeterProvider
from opentelemetry.sdk.metrics.export import PeriodicExportingMetricReader
import time
import logging
```

`uvx ty check`:

```
error[unresolved-import]: Cannot resolve imported module `opentelemetry`
 --> jaeger.py:1:6
  |
1 | from opentelemetry import trace
  |      ^^^^^^^^^^^^^
2 | from opentelemetry.sdk.trace import TracerProvider
3 | from opentelemetry.sdk.trace.export import BatchSpanProcessor
  |
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `opentelemetry`
 --> main.py:2:6
  |
1 | from time import time
2 | from opentelemetry import trace
  |      ^^^^^^^^^^^^^
3 | from opentelemetry.sdk.trace import TracerProvider
4 | from opentelemetry.sdk.trace.export import BatchSpanProcessor
  |
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `opentelemetry`
 --> test_exporter.py:2:6
  |
1 | import os
2 | from opentelemetry import trace, metrics
  |      ^^^^^^^^^^^^^
3 | from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
4 | from opentelemetry.exporter.otlp.proto.grpc.metric_exporter import OTLPMetricExporter
  |
info: rule `unresolved-import` is enabled by default

Found 3 diagnostics
```



### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Comment by @MichaReiser on 2025-05-14 06:33_

This sounds related to https://github.com/astral-sh/ty/issues/363

ty should support namespace packages without any additional configuration but it seems there's something off in the module resolver that makes it skip over some of the opentelemetry imports

---

_Label `bug` added by @MichaReiser on 2025-05-14 06:33_

---

_Label `imports` added by @MichaReiser on 2025-05-14 06:33_

---

_Comment by @MichaReiser on 2025-05-14 06:43_

Logs when running a debug build:

```
3      └─┐ty_python_semantic::module_resolver::resolver::resolve_module{name=opentelemetry}
3        ├─   0.020084s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry-stubs/__init__.pyi'
3        ├─   0.020140s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry-stubs/__init__.py'
3        ├─   0.020163s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry-stubs.pyi'
3        ├─   0.020182s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry-stubs.py'
3        ├─   0.020198s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open' contains no stub package named `opentelemetry-stubs`.
3        ├─   0.020212s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry/__init__.pyi'
3        ├─   0.020230s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry/__init__.py'
3        ├─   0.020335s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry.pyi'
3        ├─   0.020353s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/opentelemetry.py'
3        ├─   0.020368s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open' contains no package named `opentelemetry`.
3        ├─   0.020400s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path 'vendored://stdlib' contains no package named `opentelemetry`.
3        ├─   0.020487s   0ms TRACE ty_project::db Salsa event: Event { thread_id: ThreadId(3), kind: DidInternValue { key: dynamic_resolution_paths::interned_arguments(Id(2000)), revision: R1 } }
3        ├─   0.020529s   0ms TRACE ty_project::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: dynamic_resolution_paths(Id(2000)) } }
3        ├─   0.020542s   0ms DEBUG ty_python_semantic::module_resolver::resolver Resolving dynamic module resolution paths
3        ├─   0.020780s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry-stubs/__init__.pyi'
3        ├─   0.020932s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry-stubs/__init__.py'
3        ├─   0.020973s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry-stubs.pyi'
3        ├─   0.021000s   0ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry-stubs.py'
3        ├─   0.021020s   0ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages' contains no stub package named `opentelemetry-stubs`.
3        ├─   0.021038s   1ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry/__init__.pyi'
3        ├─   0.021061s   1ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry/__init__.py'
3        ├─   0.021082s   1ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry.pyi'
3        ├─   0.021103s   1ms TRACE ruff_db::files Adding file '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages/opentelemetry.py'
3        ├─   0.021120s   1ms TRACE ty_python_semantic::module_resolver::resolver Search path '/Users/micha/astral/test/open/.venv/lib/python3.13/site-packages' contains no package named `opentelemetry`.
3        ├─   0.021134s   1ms DEBUG ty_python_semantic::module_resolver::resolver Module `opentelemetry` not found in search paths
3      ┌─┘
```

You can see that the issue is that we only look for python files in `opentelemetry` but aren't testing if the folder itself exists (making it a valid namespace package, which will cause fun problems with wanadb)

---

_Renamed from "Add support for namespace_packages" to "ty fails to find some namespace packages" by @carljm on 2025-05-14 19:57_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-05-16 13:22_

---

_Closed by @MichaReiser on 2025-05-21 07:28_

---
