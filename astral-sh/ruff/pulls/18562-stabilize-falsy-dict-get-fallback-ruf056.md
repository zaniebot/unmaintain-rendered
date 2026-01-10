```yaml
number: 18562
title: "Stabilize `falsy-dict-get-fallback` (`RUF056`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-ruf056
created_at: 2025-06-08T19:13:41Z
updated_at: 2025-06-09T19:09:41Z
url: https://github.com/astral-sh/ruff/pull/18562
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `falsy-dict-get-fallback` (`RUF056`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:13_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:13_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:13_

---

_Comment by @github-actions[bot] on 2025-06-08 19:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+48 -0 violations, +0 -0 fixes in 10 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1546'>rasa/shared/core/domain.py:1546:43:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/rasa_layers.py#L397'>rasa/utils/tensorflow/rasa_layers.py:397:47:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/rasa_layers.py#L398'>rasa/utils/tensorflow/rasa_layers.py:398:50:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/rasa_layers.py#L758'>rasa/utils/tensorflow/rasa_layers.py:758:77:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/airflow-core/src/airflow/serialization/serialized_objects.py#L1485'>airflow-core/src/airflow/serialization/serialized_objects.py:1485:67:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/airflow-core/src/airflow/serialization/serialized_objects.py#L1488'>airflow-core/src/airflow/serialization/serialized_objects.py:1488:89:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/airflow-core/src/airflow/serialization/serialized_objects.py#L1496'>airflow-core/src/airflow/serialization/serialized_objects.py:1496:85:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/airflow-core/src/airflow/serialization/serialized_objects.py#L1526'>airflow-core/src/airflow/serialization/serialized_objects.py:1526:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/providers/apache/hdfs/src/airflow/providers/apache/hdfs/hooks/webhdfs.py#L122'>providers/apache/hdfs/src/airflow/providers/apache/hdfs/hooks/webhdfs.py:122:90:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/providers/apache/hdfs/src/airflow/providers/apache/hdfs/hooks/webhdfs.py#L141'>providers/apache/hdfs/src/airflow/providers/apache/hdfs/hooks/webhdfs.py:141:40:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/providers/elasticsearch/src/airflow/providers/elasticsearch/hooks/elasticsearch.py#L191'>providers/elasticsearch/src/airflow/providers/elasticsearch/hooks/elasticsearch.py:191:43:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/a227f670edb23b2255c62448b8cb20807194122a/scripts/in_container/in_container_utils.py#L61'>scripts/in_container/in_container_utils.py:61:74:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/6513445000d0bbf437766d47a7485a8e4456e018/superset/db_engine_specs/databricks.py#L290'>superset/db_engine_specs/databricks.py:290:69:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/superset/blob/6513445000d0bbf437766d47a7485a8e4456e018/superset/migrations/shared/migrate_viz/processors.py#L503'>superset/migrations/shared/migrate_viz/processors.py:503:65:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/superset/blob/6513445000d0bbf437766d47a7485a8e4456e018/superset/migrations/shared/migrate_viz/query_functions.py#L477'>superset/migrations/shared/migrate_viz/query_functions.py:477:49:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L1320'>pymilvus/client/grpc_handler.py:1320:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L1401'>pymilvus/client/grpc_handler.py:1401:65:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L1643'>pymilvus/client/grpc_handler.py:1643:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L2285'>pymilvus/client/grpc_handler.py:2285:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L311'>pymilvus/client/grpc_handler.py:311:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L696'>pymilvus/client/grpc_handler.py:696:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L707'>pymilvus/client/grpc_handler.py:707:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L734'>pymilvus/client/grpc_handler.py:734:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L745'>pymilvus/client/grpc_handler.py:745:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L807'>pymilvus/client/grpc_handler.py:807:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L881'>pymilvus/client/grpc_handler.py:881:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L898'>pymilvus/client/grpc_handler.py:898:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L906'>pymilvus/client/grpc_handler.py:906:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/grpc_handler.py#L921'>pymilvus/client/grpc_handler.py:921:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/prepare.py#L431'>pymilvus/client/prepare.py:431:42:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/client/prepare.py#L432'>pymilvus/client/prepare.py:432:35:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/collection.py#L583'>pymilvus/orm/collection.py:583:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/collection.py#L656'>pymilvus/orm/collection.py:656:60:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/collection.py#L800'>pymilvus/orm/collection.py:800:63:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/collection.py#L819'>pymilvus/orm/collection.py:819:59:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/collection.py#L947'>pymilvus/orm/collection.py:947:63:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/collection.py#L964'>pymilvus/orm/collection.py:964:59:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/index.py#L77'>pymilvus/orm/index.py:77:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/orm/partition.py#L51'>pymilvus/orm/partition.py:51:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/plotting/_core.py#L1728'>pandas/plotting/_core.py:1728:44:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/8f5a80e7b741226e525bd90e45496b8f68dc69b6/scripts/stubsabot.py#L663'>scripts/stubsabot.py:663:59:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/pypi_repository.py#L220'>src/poetry/repositories/pypi_repository.py:220:36:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b9ab93068a4f844346960f30b464fffd87d8af5d/zerver/actions/streams.py#L360'>zerver/actions/streams.py:360:66:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/zulip/zulip/blob/b9ab93068a4f844346960f30b464fffd87d8af5d/zerver/lib/import_realm.py#L937'>zerver/lib/import_realm.py:937:54:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/zulip/zulip/blob/b9ab93068a4f844346960f30b464fffd87d8af5d/zerver/lib/outgoing_webhook.py#L88'>zerver/lib/outgoing_webhook.py:88:55:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/zulip/zulip/blob/b9ab93068a4f844346960f30b464fffd87d8af5d/zerver/views/documentation.py#L269'>zerver/views/documentation.py:269:53:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9e9633de9da7a9fab03b4bba3a326bf85b412050/src/_pytest/_py/path.py#L953'>src/_pytest/_py/path.py:953:30:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/utils/masked/tests/test_function_helpers.py#L1044'>astropy/utils/masked/tests/test_function_helpers.py:1044:53:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF056 | 48 | 48 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:28_

---

_@ntBre approved on 2025-06-09 18:33_

---

_Merged by @dylwil3 on 2025-06-09 19:03_

---

_Closed by @dylwil3 on 2025-06-09 19:03_

---

_Branch deleted on 2025-06-09 19:03_

---
