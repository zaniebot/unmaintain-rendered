```yaml
number: 8998
title: "Default `max-positional-args` to `max-args`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/args
created_at: 2023-12-04T18:50:29Z
updated_at: 2023-12-04T19:08:41Z
url: https://github.com/astral-sh/ruff/pull/8998
synced_at: 2026-01-12T15:55:27Z
```

# Default `max-positional-args` to `max-args`

---

_@charliermarsh_

_No description provided._

---

_Label `configuration` added by @charliermarsh on 2023-12-04 18:50_

---

_Renamed from "Default max-positional-args to max-args" to "Default `max-positional-args` to `max-args`" by @charliermarsh on 2023-12-04 18:51_

---

_Merged by @charliermarsh on 2023-12-04 19:02_

---

_Closed by @charliermarsh on 2023-12-04 19:02_

---

_Branch deleted on 2023-12-04 19:02_

---

_Comment by @github-actions[bot] on 2023-12-04 19:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -97 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/bulk_writer/bulk_import.py#L79'>pymilvus/bulk_writer/bulk_import.py:79:5:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/bulk_writer/remote_bulk_writer.py#L36'>pymilvus/bulk_writer/remote_bulk_writer.py:36:13:</a> PLR0917 Too many positional arguments: (10/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/bulk_writer/remote_bulk_writer.py#L58'>pymilvus/bulk_writer/remote_bulk_writer.py:58:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/abstract.py#L389'>pymilvus/client/abstract.py:389:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1281'>pymilvus/client/grpc_handler.py:1281:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1295'>pymilvus/client/grpc_handler.py:1295:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1338'>pymilvus/client/grpc_handler.py:1338:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1595'>pymilvus/client/grpc_handler.py:1595:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1617'>pymilvus/client/grpc_handler.py:1617:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1648'>pymilvus/client/grpc_handler.py:1648:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1706'>pymilvus/client/grpc_handler.py:1706:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L590'>pymilvus/client/grpc_handler.py:590:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L712'>pymilvus/client/grpc_handler.py:712:9:</a> PLR0917 Too many positional arguments: (11/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/prepare.py#L1009'>pymilvus/client/prepare.py:1009:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/prepare.py#L406'>pymilvus/client/prepare.py:406:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/prepare.py#L426'>pymilvus/client/prepare.py:426:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/prepare.py#L557'>pymilvus/client/prepare.py:557:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/prepare.py#L568'>pymilvus/client/prepare.py:568:9:</a> PLR0917 Too many positional arguments: (10/5)
- <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/prepare.py#L707'>pymilvus/client/prepare.py:707:9:</a> PLR0917 Too many positional arguments: (6/5)
... 19 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -59 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/cli/req_command.py#L271'>src/pip/_internal/cli/req_command.py:271:9:</a> PLR0917 Too many positional arguments: (9/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/cli/req_command.py#L326'>src/pip/_internal/cli/req_command.py:326:9:</a> PLR0917 Too many positional arguments: (12/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/index/collector.py#L260'>src/pip/_internal/index/collector.py:260:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/index/package_finder.py#L120'>src/pip/_internal/index/package_finder.py:120:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/index/package_finder.py#L393'>src/pip/_internal/index/package_finder.py:393:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/index/package_finder.py#L428'>src/pip/_internal/index/package_finder.py:428:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/index/package_finder.py#L597'>src/pip/_internal/index/package_finder.py:597:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/locations/__init__.py#L230'>src/pip/_internal/locations/__init__.py:230:5:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/locations/_distutils.py#L115'>src/pip/_internal/locations/_distutils.py:115:5:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/locations/_distutils.py#L35'>src/pip/_internal/locations/_distutils.py:35:5:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/locations/_sysconfig.py#L124'>src/pip/_internal/locations/_sysconfig.py:124:5:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/metadata/base.py#L645'>src/pip/_internal/metadata/base.py:645:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/models/link.py#L197'>src/pip/_internal/models/link.py:197:9:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/models/scheme.py#L19'>src/pip/_internal/models/scheme.py:19:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/models/selection_prefs.py#L24'>src/pip/_internal/models/selection_prefs.py:24:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/network/session.py#L211'>src/pip/_internal/network/session.py:211:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/operations/freeze.py#L26'>src/pip/_internal/operations/freeze.py:26:5:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/operations/install/wheel.py#L426'>src/pip/_internal/operations/install/wheel.py:426:5:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/operations/install/wheel.py#L713'>src/pip/_internal/operations/install/wheel.py:713:5:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/operations/prepare.py#L140'>src/pip/_internal/operations/prepare.py:140:5:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/operations/prepare.py#L216'>src/pip/_internal/operations/prepare.py:216:9:</a> PLR0917 Too many positional arguments: (15/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/req/__init__.py#L37'>src/pip/_internal/req/__init__.py:37:5:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/req/req_file.py#L103'>src/pip/_internal/req/req_file.py:103:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/req/req_file.py#L208'>src/pip/_internal/req/req_file.py:208:5:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/req/req_file.py#L85'>src/pip/_internal/req/req_file.py:85:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/req/req_install.py#L72'>src/pip/_internal/req/req_install.py:72:9:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/req/req_install.py#L809'>src/pip/_internal/req/req_install.py:809:9:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/resolution/legacy/resolver.py#L120'>src/pip/_internal/resolution/legacy/resolver.py:120:9:</a> PLR0917 Too many positional arguments: (12/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/resolution/resolvelib/candidates.py#L141'>src/pip/_internal/resolution/resolvelib/candidates.py:141:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/resolution/resolvelib/candidates.py#L250'>src/pip/_internal/resolution/resolvelib/candidates.py:250:9:</a> PLR0917 Too many positional arguments: (6/5)
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 97 | 0 | 97 | 0 | 0 |

</p>
</details>




---
