```yaml
number: 12642
title: "[`pydoclint`] Deduplicate collected exceptions after traversing function bodies"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - docstring
  - preview
assignees: []
merged: true
base: main
head: alex/dedupe-pydoclint
created_at: 2024-08-02T19:02:34Z
updated_at: 2024-08-02T22:17:09Z
url: https://github.com/astral-sh/ruff/pull/12642
synced_at: 2026-01-10T21:47:02Z
```

# [`pydoclint`] Deduplicate collected exceptions after traversing function bodies

---

_Pull request opened by @AlexWaygood on 2024-08-02 19:02_

## Summary

I noticed when looking through the ecosystem report for #12639 that a function like this will be flagged twice for not stating in its docstring that it raises `TypeError`. We probably only need to emit one DOC501 violation for such a function:

```py
def foo():
    """Foo.

    Returns:
        42: int
    """
    raise TypeError
    raise TypeError
    return 42
```

## Test Plan

`cargo test -p ruff_linter`


---

_Label `rule` added by @AlexWaygood on 2024-08-02 19:02_

---

_Label `docstring` added by @AlexWaygood on 2024-08-02 19:02_

---

_Label `preview` added by @AlexWaygood on 2024-08-02 19:02_

---

_Comment by @github-actions[bot] on 2024-08-02 19:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -644 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -437 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/experimental/__init__.py#L43'>airflow/api/common/experimental/__init__.py:43:15:</a> DOC501 Raised exception `DagNotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/experimental/pool.py#L66'>airflow/api/common/experimental/pool.py:66:15:</a> DOC501 Raised exception `AirflowBadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/experimental/pool.py#L71'>airflow/api/common/experimental/pool.py:71:15:</a> DOC501 Raised exception `AirflowBadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/experimental/pool.py#L95'>airflow/api/common/experimental/pool.py:95:15:</a> DOC501 Raised exception `AirflowBadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L127'>airflow/api/common/mark_tasks.py:127:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L131'>airflow/api/common/mark_tasks.py:131:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L134'>airflow/api/common/mark_tasks.py:134:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L139'>airflow/api/common/mark_tasks.py:139:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L410'>airflow/api/common/mark_tasks.py:410:19:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L413'>airflow/api/common/mark_tasks.py:413:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L463'>airflow/api/common/mark_tasks.py:463:19:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L467'>airflow/api/common/mark_tasks.py:467:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L554'>airflow/api/common/mark_tasks.py:554:19:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/mark_tasks.py#L557'>airflow/api/common/mark_tasks.py:557:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api/common/trigger_dag.py#L70'>airflow/api/common/trigger_dag.py:70:19:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/connection_endpoint.py#L140'>airflow/api_connexion/endpoints/connection_endpoint.py:140:15:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/connection_endpoint.py#L169'>airflow/api_connexion/endpoints/connection_endpoint.py:169:15:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dag_endpoint.py#L154'>airflow/api_connexion/endpoints/dag_endpoint.py:154:19:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dag_endpoint.py#L178'>airflow/api_connexion/endpoints/dag_endpoint.py:178:19:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dag_parsing.py#L56'>airflow/api_connexion/endpoints/dag_parsing.py:56:15:</a> DOC501 Raised exception `NotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dag_run_endpoint.py#L318'>airflow/api_connexion/endpoints/dag_run_endpoint.py:318:15:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dag_run_endpoint.py#L361'>airflow/api_connexion/endpoints/dag_run_endpoint.py:361:19:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dag_run_endpoint.py#L371'>airflow/api_connexion/endpoints/dag_run_endpoint.py:371:11:</a> DOC501 Raised exception `AlreadyExists` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/dataset_endpoint.py#L354'>airflow/api_connexion/endpoints/dataset_endpoint.py:354:15:</a> DOC501 Raised exception `NotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/extra_link_endpoint.py#L58'>airflow/api_connexion/endpoints/extra_link_endpoint.py:58:15:</a> DOC501 Raised exception `NotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/extra_link_endpoint.py#L69'>airflow/api_connexion/endpoints/extra_link_endpoint.py:69:15:</a> DOC501 Raised exception `NotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/log_endpoint.py#L78'>airflow/api_connexion/endpoints/log_endpoint.py:78:15:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/pool_endpoint.py#L111'>airflow/api_connexion/endpoints/pool_endpoint.py:111:15:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/pool_endpoint.py#L127'>airflow/api_connexion/endpoints/pool_endpoint.py:127:19:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/pool_endpoint.py#L135'>airflow/api_connexion/endpoints/pool_endpoint.py:135:19:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/pool_endpoint.py#L156'>airflow/api_connexion/endpoints/pool_endpoint.py:156:15:</a> DOC501 Raised exception `BadRequest` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/task_endpoint.py#L44'>airflow/api_connexion/endpoints/task_endpoint.py:44:15:</a> DOC501 Raised exception `NotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/task_instance_endpoint.py#L105'>airflow/api_connexion/endpoints/task_instance_endpoint.py:105:15:</a> DOC501 Raised exception `NotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/a5743ff82675810d1c64a8f428e854b9a0a7751c/airflow/api_connexion/endpoints/task_instance_endpoint.py#L107'>airflow/api_connexion/endpoints/task_instance_endpoint.py:107:15:</a> DOC501 Raised exception `NotFound` missing from docstring
... 406 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -70 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/database/uploaders/base.py#L129'>superset/commands/database/uploaders/base.py:129:19:</a> DOC501 Raised exception `DatabaseUploadFailed` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/database/uploaders/columnar_reader.py#L102'>superset/commands/database/uploaders/columnar_reader.py:102:23:</a> DOC501 Raised exception `DatabaseUploadFailed` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/database/uploaders/columnar_reader.py#L89'>superset/commands/database/uploaders/columnar_reader.py:89:23:</a> DOC501 Raised exception `DatabaseUploadFailed` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/database/uploaders/columnar_reader.py#L95'>superset/commands/database/uploaders/columnar_reader.py:95:31:</a> DOC501 Raised exception `DatabaseUploadFailed` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/database/uploaders/excel_reader.py#L94'>superset/commands/database/uploaders/excel_reader.py:94:19:</a> DOC501 Raised exception `DatabaseUploadFailed` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/importers/v1/utils.py#L74'>superset/commands/importers/v1/utils.py:74:19:</a> DOC501 Raised exception `IncorrectVersionError` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/commands/report/execute.py#L272'>superset/commands/report/execute.py:272:19:</a> DOC501 Raised exception `ReportScheduleScreenshotFailedError` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/common/query_object.py#L438'>superset/common/query_object.py:438:23:</a> DOC501 Raised exception `InvalidPostProcessingError` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/connectors/sqla/utils.py#L119'>superset/connectors/sqla/utils.py:119:15:</a> DOC501 Raised exception `SupersetSecurityException` missing from docstring
- <a href='https://github.com/apache/superset/blob/c87a4fd642415ffa60d967a76ea73990c6264fd1/superset/daos/tag.py#L95'>superset/daos/tag.py:95:19:</a> DOC501 Raised exception `NoResultFound` missing from docstring
... 60 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -65 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L153'>src/bokeh/application/handlers/directory.py:153:19:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L215'>src/bokeh/client/connection.py:215:19:</a> DOC501 Raised exception `RuntimeError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L235'>src/bokeh/client/connection.py:235:19:</a> DOC501 Raised exception `RuntimeError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L415'>src/bokeh/client/session.py:415:23:</a> DOC501 Raised exception `OSError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L416'>src/bokeh/client/session.py:416:19:</a> DOC501 Raised exception `OSError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L135'>src/bokeh/command/util.py:135:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/enums.py#L239'>src/bokeh/core/enums.py:239:15:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L329'>src/bokeh/core/property/descriptors.py:329:19:</a> DOC501 Raised exception `RuntimeError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L758'>src/bokeh/core/property/descriptors.py:758:19:</a> DOC501 Raised exception `RuntimeError` missing from docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/check.py#L217'>src/bokeh/core/validation/check.py:217:19:</a> DOC501 Raised exception `RuntimeError` missing from docstring
... 55 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -72 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/analytics/lib/fixtures.py#L58'>analytics/lib/fixtures.py:58:15:</a> DOC501 Raised exception `AssertionError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/confirmation/models.py#L103'>confirmation/models.py:103:15:</a> DOC501 Raised exception `ConfirmationKeyError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/confirmation/models.py#L106'>confirmation/models.py:106:15:</a> DOC501 Raised exception `ConfirmationKeyError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/confirmation/models.py#L116'>confirmation/models.py:116:15:</a> DOC501 Raised exception `ConfirmationKeyError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/confirmation/models.py#L307'>confirmation/models.py:307:15:</a> DOC501 Raised exception `InvalidError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/corporate/views/remote_billing_page.py#L217'>corporate/views/remote_billing_page.py:217:15:</a> DOC501 Raised exception `JsonableError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/corporate/views/remote_billing_page.py#L299'>corporate/views/remote_billing_page.py:299:15:</a> DOC501 Raised exception `JsonableError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/corporate/views/remote_billing_page.py#L304'>corporate/views/remote_billing_page.py:304:15:</a> DOC501 Raised exception `JsonableError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/zerver/actions/invites.py#L165'>zerver/actions/invites.py:165:19:</a> DOC501 Raised exception `InvitationError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/2011e0df760cea52c31914e7b77d9b4e38e9ee74/zerver/actions/message_edit.py#L108'>zerver/actions/message_edit.py:108:19:</a> DOC501 Raised exception `JsonableError` missing from docstring
... 62 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC501 | 647 | 3 | 644 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-08-02 21:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:545 on 2024-08-02 21:34_

Could we use a `BTreeSet` instead, to get this for free?

---

_@AlexWaygood reviewed on 2024-08-02 21:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:545 on 2024-08-02 21:46_

I don't think so, because we're storing `ExceptionEntry` objects in the `Vec` rather than `QualifiedName`s. `ExceptionEntry`s have two fields — one is a `QualifiedName` and the other is a `TextRange`. We want to make sure that no two `ExceptionEntry`s in the Vec have the same `QualifiedName`, regardless of whether they have the same `TextRange` or not 

---

_@charliermarsh approved on 2024-08-02 22:14_

---

_@charliermarsh reviewed on 2024-08-02 22:15_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:545 on 2024-08-02 22:15_

It _is_ possible to do this by implementing `Hash` for `ExceptionEntry` manually, and omitting `TextRange`. But probably not worth it, can create confusion.

---

_Merged by @AlexWaygood on 2024-08-02 22:17_

---

_Closed by @AlexWaygood on 2024-08-02 22:17_

---

_Branch deleted on 2024-08-02 22:17_

---
