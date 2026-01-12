```yaml
number: 5818
title: "Include function name in `undocumented-param` message"
type: pull_request
state: merged
author: charliermarsh
labels:
  - docstring
assignees: []
merged: true
base: main
head: charlie/D417
created_at: 2023-07-17T02:24:54Z
updated_at: 2023-07-17T02:55:55Z
url: https://github.com/astral-sh/ruff/pull/5818
synced_at: 2026-01-12T03:30:21Z
```

# Include function name in `undocumented-param` message

---

_Pull request opened by @charliermarsh on 2023-07-17 02:24_

Closes #5814.

---

_Renamed from "Include function name in undocumented-param message" to "Include function name in `undocumented-param` message" by @charliermarsh on 2023-07-17 02:24_

---

_Label `docstring` added by @charliermarsh on 2023-07-17 02:25_

---

_Comment by @github-actions[bot] on 2023-07-17 02:39_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+117, -117, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/airflow/models/dag.py#L3874'>airflow/models/dag.py:3874:5:</a> D417 Missing argument description in the docstring for `_run_task`: `session`
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/airflow/models/dag.py#L3874'>airflow/models/dag.py:3874:5:</a> D417 Missing argument description in the docstring: `session`
</pre>

</p>
</details>
<details><summary>bokeh (+115, -115)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/code.py#L82'>src/bokeh/application/handlers/code.py:82:9:</a> D417 Missing argument description in the docstring for `__init__`: `package`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/code.py#L82'>src/bokeh/application/handlers/code.py:82:9:</a> D417 Missing argument description in the docstring: `package`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L243'>src/bokeh/application/handlers/directory.py:243:9:</a> D417 Missing argument description in the docstring for `on_server_loaded`: `server_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L243'>src/bokeh/application/handlers/directory.py:243:9:</a> D417 Missing argument description in the docstring: `server_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L256'>src/bokeh/application/handlers/directory.py:256:9:</a> D417 Missing argument description in the docstring for `on_server_unloaded`: `server_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L256'>src/bokeh/application/handlers/directory.py:256:9:</a> D417 Missing argument description in the docstring: `server_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L272'>src/bokeh/application/handlers/directory.py:272:9:</a> D417 Missing argument description in the docstring for `on_session_created`: `session_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L272'>src/bokeh/application/handlers/directory.py:272:9:</a> D417 Missing argument description in the docstring: `session_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L282'>src/bokeh/application/handlers/directory.py:282:9:</a> D417 Missing argument description in the docstring for `on_session_destroyed`: `session_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L282'>src/bokeh/application/handlers/directory.py:282:9:</a> D417 Missing argument description in the docstring: `session_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L143'>src/bokeh/application/handlers/handler.py:143:9:</a> D417 Missing argument description in the docstring for `on_server_loaded`: `server_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L143'>src/bokeh/application/handlers/handler.py:143:9:</a> D417 Missing argument description in the docstring: `server_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L156'>src/bokeh/application/handlers/handler.py:156:9:</a> D417 Missing argument description in the docstring for `on_server_unloaded`: `server_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L156'>src/bokeh/application/handlers/handler.py:156:9:</a> D417 Missing argument description in the docstring: `server_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L173'>src/bokeh/application/handlers/handler.py:173:15:</a> D417 Missing argument description in the docstring for `on_session_created`: `session_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L173'>src/bokeh/application/handlers/handler.py:173:15:</a> D417 Missing argument description in the docstring: `session_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L186'>src/bokeh/application/handlers/handler.py:186:15:</a> D417 Missing argument description in the docstring for `on_session_destroyed`: `session_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/handler.py#L186'>src/bokeh/application/handlers/handler.py:186:15:</a> D417 Missing argument description in the docstring: `session_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L107'>src/bokeh/application/handlers/lifecycle.py:107:15:</a> D417 Missing argument description in the docstring for `on_session_created`: `session_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L107'>src/bokeh/application/handlers/lifecycle.py:107:15:</a> D417 Missing argument description in the docstring: `session_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L117'>src/bokeh/application/handlers/lifecycle.py:117:15:</a> D417 Missing argument description in the docstring for `on_session_destroyed`: `session_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L117'>src/bokeh/application/handlers/lifecycle.py:117:15:</a> D417 Missing argument description in the docstring: `session_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L82'>src/bokeh/application/handlers/lifecycle.py:82:9:</a> D417 Missing argument description in the docstring for `on_server_loaded`: `server_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L82'>src/bokeh/application/handlers/lifecycle.py:82:9:</a> D417 Missing argument description in the docstring: `server_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L92'>src/bokeh/application/handlers/lifecycle.py:92:9:</a> D417 Missing argument description in the docstring for `on_server_unloaded`: `server_context`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/lifecycle.py#L92'>src/bokeh/application/handlers/lifecycle.py:92:9:</a> D417 Missing argument description in the docstring: `server_context`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_lifecycle.py#L55'>src/bokeh/application/handlers/server_lifecycle.py:55:9:</a> D417 Missing argument description in the docstring for `__init__`: `package`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_lifecycle.py#L55'>src/bokeh/application/handlers/server_lifecycle.py:55:9:</a> D417 Missing argument description in the docstring: `package`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_request_handler.py#L57'>src/bokeh/application/handlers/server_request_handler.py:57:9:</a> D417 Missing argument description in the docstring for `__init__`: `package`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_request_handler.py#L57'>src/bokeh/application/handlers/server_request_handler.py:57:9:</a> D417 Missing argument description in the docstring: `package`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/client/session.py#L194'>src/bokeh/client/session.py:194:5:</a> D417 Missing argument description in the docstring for `show_session`: `controller`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/client/session.py#L194'>src/bokeh/client/session.py:194:5:</a> D417 Missing argument description in the docstring: `controller`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/command/util.py#L184'>src/bokeh/command/util.py:184:5:</a> D417 Missing argument description in the docstring for `report_server_init_errors`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/command/util.py#L184'>src/bokeh/command/util.py:184:5:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L414'>src/bokeh/core/has_props.py:414:9:</a> D417 Missing argument description in the docstring for `set_from_json`: `value`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L414'>src/bokeh/core/has_props.py:414:9:</a> D417 Missing argument description in the docstring: `value`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L571'>src/bokeh/core/has_props.py:571:9:</a> D417 Missing argument description in the docstring for `properties_with_values`: `include_undefined`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L571'>src/bokeh/core/has_props.py:571:9:</a> D417 Missing argument description in the docstring: `include_undefined`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L608'>src/bokeh/core/has_props.py:608:9:</a> D417 Missing argument description in the docstring for `query_properties_with_values`: `include_undefined`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/has_props.py#L608'>src/bokeh/core/has_props.py:608:9:</a> D417 Missing argument description in the docstring: `include_undefined`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/dataspec.py#L445'>src/bokeh/core/property/dataspec.py:445:9:</a> D417 Missing argument description in the docstring for `make_descriptors`: `base_name`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/dataspec.py#L445'>src/bokeh/core/property/dataspec.py:445:9:</a> D417 Missing argument description in the docstring: `base_name`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L397'>src/bokeh/core/property/descriptors.py:397:9:</a> D417 Missing argument description in the docstring for `set_from_json`: `value`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L397'>src/bokeh/core/property/descriptors.py:397:9:</a> D417 Missing argument description in the docstring: `value`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L430'>src/bokeh/core/property/descriptors.py:430:9:</a> D417 Missing argument description in the docstring for `trigger_if_changed`: `obj`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L430'>src/bokeh/core/property/descriptors.py:430:9:</a> D417 Missing argument description in the docstring: `obj`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L563'>src/bokeh/core/property/descriptors.py:563:9:</a> D417 Missing argument descriptions in the docstring for `_set`: `hint`, `obj`, `value`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L563'>src/bokeh/core/property/descriptors.py:563:9:</a> D417 Missing argument descriptions in the docstring: `hint`, `obj`, `value`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L626'>src/bokeh/core/property/descriptors.py:626:9:</a> D417 Missing argument description in the docstring for `_notify_mutated`: `hint`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L626'>src/bokeh/core/property/descriptors.py:626:9:</a> D417 Missing argument description in the docstring: `hint`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L661'>src/bokeh/core/property/descriptors.py:661:9:</a> D417 Missing argument descriptions in the docstring for `_trigger`: `hint`, `obj`, `value`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L661'>src/bokeh/core/property/descriptors.py:661:9:</a> D417 Missing argument descriptions in the docstring: `hint`, `obj`, `value`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L783'>src/bokeh/core/property/descriptors.py:783:9:</a> D417 Missing argument descriptions in the docstring for `set_from_json`: `obj`, `value`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/descriptors.py#L783'>src/bokeh/core/property/descriptors.py:783:9:</a> D417 Missing argument descriptions in the docstring: `obj`, `value`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/query.py#L64'>src/bokeh/core/query.py:64:5:</a> D417 Missing argument description in the docstring for `find`: `objs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/query.py#L64'>src/bokeh/core/query.py:64:5:</a> D417 Missing argument description in the docstring: `objs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/validation/decorators.py#L55'>src/bokeh/core/validation/decorators.py:55:5:</a> D417 Missing argument description in the docstring for `_validator`: `code_or_name`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/validation/decorators.py#L55'>src/bokeh/core/validation/decorators.py:55:5:</a> D417 Missing argument description in the docstring: `code_or_name`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/callbacks.py#L450'>src/bokeh/document/callbacks.py:450:5:</a> D417 Missing argument description in the docstring for `_combine_document_events`: `old_events`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/callbacks.py#L450'>src/bokeh/document/callbacks.py:450:5:</a> D417 Missing argument description in the docstring: `old_events`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/document.py#L349'>src/bokeh/document/document.py:349:9:</a> D417 Missing argument description in the docstring for `apply_json_patch`: `patch_json`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/document.py#L349'>src/bokeh/document/document.py:349:9:</a> D417 Missing argument description in the docstring: `patch_json`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/document.py#L491'>src/bokeh/document/document.py:491:9:</a> D417 Missing argument description in the docstring for `hold`: `policy`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/document.py#L491'>src/bokeh/document/document.py:491:9:</a> D417 Missing argument description in the docstring: `policy`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/document.py#L712'>src/bokeh/document/document.py:712:9:</a> D417 Missing argument description in the docstring for `set_select`: `updates`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/document.py#L712'>src/bokeh/document/document.py:712:9:</a> D417 Missing argument description in the docstring: `updates`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L221'>src/bokeh/document/events.py:221:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L221'>src/bokeh/document/events.py:221:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L357'>src/bokeh/document/events.py:357:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L357'>src/bokeh/document/events.py:357:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L387'>src/bokeh/document/events.py:387:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `attr`, `data`, `model`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L387'>src/bokeh/document/events.py:387:9:</a> D417 Missing argument descriptions in the docstring: `attr`, `data`, `model`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L431'>src/bokeh/document/events.py:431:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L431'>src/bokeh/document/events.py:431:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L479'>src/bokeh/document/events.py:479:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `attr`, `model`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L479'>src/bokeh/document/events.py:479:9:</a> D417 Missing argument descriptions in the docstring: `attr`, `model`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L535'>src/bokeh/document/events.py:535:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L535'>src/bokeh/document/events.py:535:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L577'>src/bokeh/document/events.py:577:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `attr`, `model`, `patches`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L577'>src/bokeh/document/events.py:577:9:</a> D417 Missing argument descriptions in the docstring: `attr`, `model`, `patches`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L618'>src/bokeh/document/events.py:618:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L618'>src/bokeh/document/events.py:618:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L703'>src/bokeh/document/events.py:703:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L703'>src/bokeh/document/events.py:703:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L761'>src/bokeh/document/events.py:761:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L761'>src/bokeh/document/events.py:761:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L821'>src/bokeh/document/events.py:821:9:</a> D417 Missing argument description in the docstring for `to_serializable`: `serializer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/events.py#L821'>src/bokeh/document/events.py:821:9:</a> D417 Missing argument description in the docstring: `serializer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/locking.py#L130'>src/bokeh/document/locking.py:130:9:</a> D417 Missing argument description in the docstring for `add_next_tick_callback`: `callback`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/locking.py#L130'>src/bokeh/document/locking.py:130:9:</a> D417 Missing argument description in the docstring: `callback`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/locking.py#L139'>src/bokeh/document/locking.py:139:9:</a> D417 Missing argument description in the docstring for `remove_next_tick_callback`: `callback`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/document/locking.py#L139'>src/bokeh/document/locking.py:139:9:</a> D417 Missing argument description in the docstring: `callback`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/driving.py#L124'>src/bokeh/driving.py:124:5:</a> D417 Missing argument description in the docstring for `force`: `f`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/driving.py#L124'>src/bokeh/driving.py:124:5:</a> D417 Missing argument description in the docstring: `f`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/driving.py#L140'>src/bokeh/driving.py:140:5:</a> D417 Missing argument description in the docstring for `linear`: `b`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/driving.py#L140'>src/bokeh/driving.py:140:5:</a> D417 Missing argument description in the docstring: `b`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L150'>src/bokeh/embed/bundle.py:150:5:</a> D417 Missing argument descriptions in the docstring for `bundle_for_objs_and_resources`: `objs`, `resources`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L150'>src/bokeh/embed/bundle.py:150:5:</a> D417 Missing argument descriptions in the docstring: `objs`, `resources`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L348'>src/bokeh/embed/bundle.py:348:5:</a> D417 Missing argument descriptions in the docstring for `_any`: `objs`, `query`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L348'>src/bokeh/embed/bundle.py:348:5:</a> D417 Missing argument descriptions in the docstring: `objs`, `query`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L363'>src/bokeh/embed/bundle.py:363:5:</a> D417 Missing argument description in the docstring for `_use_tables`: `all_objs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L363'>src/bokeh/embed/bundle.py:363:5:</a> D417 Missing argument description in the docstring: `all_objs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L376'>src/bokeh/embed/bundle.py:376:5:</a> D417 Missing argument description in the docstring for `_use_widgets`: `all_objs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L376'>src/bokeh/embed/bundle.py:376:5:</a> D417 Missing argument description in the docstring: `all_objs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L438'>src/bokeh/embed/bundle.py:438:5:</a> D417 Missing argument description in the docstring for `_use_gl`: `all_objs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L438'>src/bokeh/embed/bundle.py:438:5:</a> D417 Missing argument description in the docstring: `all_objs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/elements.py#L148'>src/bokeh/embed/elements.py:148:5:</a> D417 Missing argument descriptions in the docstring for `script_for_render_items`: `absolute_url`, `app_path`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/elements.py#L148'>src/bokeh/embed/elements.py:148:5:</a> D417 Missing argument descriptions in the docstring: `absolute_url`, `app_path`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/elements.py#L82'>src/bokeh/embed/elements.py:82:5:</a> D417 Missing argument descriptions in the docstring for `html_page_for_render_items`: `render_items`, `title`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/elements.py#L82'>src/bokeh/embed/elements.py:82:5:</a> D417 Missing argument descriptions in the docstring: `render_items`, `title`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L129'>src/bokeh/embed/server.py:129:5:</a> D417 Missing argument description in the docstring for `server_session`: `headers`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L129'>src/bokeh/embed/server.py:129:5:</a> D417 Missing argument description in the docstring: `headers`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L225'>src/bokeh/embed/server.py:225:5:</a> D417 Missing argument descriptions in the docstring for `server_html_page_for_session`: `resources`, `session`, `template`, `template_variables`, `title`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L225'>src/bokeh/embed/server.py:225:5:</a> D417 Missing argument descriptions in the docstring: `resources`, `session`, `template`, `template_variables`, `title`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L262'>src/bokeh/embed/server.py:262:5:</a> D417 Missing argument description in the docstring for `_clean_url`: `url`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L262'>src/bokeh/embed/server.py:262:5:</a> D417 Missing argument description in the docstring: `url`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L282'>src/bokeh/embed/server.py:282:5:</a> D417 Missing argument description in the docstring for `_get_app_path`: `url`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L282'>src/bokeh/embed/server.py:282:5:</a> D417 Missing argument description in the docstring: `url`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L60'>src/bokeh/embed/server.py:60:5:</a> D417 Missing argument descriptions in the docstring for `server_document`: `arguments`, `headers`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L60'>src/bokeh/embed/server.py:60:5:</a> D417 Missing argument descriptions in the docstring: `arguments`, `headers`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/standalone.py#L371'>src/bokeh/embed/standalone.py:371:5:</a> D417 Missing argument description in the docstring for `json_item`: `target`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/standalone.py#L371'>src/bokeh/embed/standalone.py:371:5:</a> D417 Missing argument description in the docstring: `target`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/standalone.py#L86'>src/bokeh/embed/standalone.py:86:5:</a> D417 Missing argument descriptions in the docstring for `autoload_static`: `model`, `resources`, `script_path`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/standalone.py#L86'>src/bokeh/embed/standalone.py:86:5:</a> D417 Missing argument descriptions in the docstring: `model`, `resources`, `script_path`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/export.py#L218'>src/bokeh/io/export.py:218:5:</a> D417 Missing argument descriptions in the docstring for `get_screenshot_as_png`: `height`, `resources`, `width`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/export.py#L218'>src/bokeh/io/export.py:218:5:</a> D417 Missing argument descriptions in the docstring: `height`, `resources`, `width`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/notebook.py#L252'>src/bokeh/io/notebook.py:252:5:</a> D417 Missing argument description in the docstring for `push_notebook`: `handle`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/notebook.py#L252'>src/bokeh/io/notebook.py:252:5:</a> D417 Missing argument description in the docstring: `handle`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/notebook.py#L341'>src/bokeh/io/notebook.py:341:5:</a> D417 Missing argument descriptions in the docstring for `run_notebook_hook`: `**kwargs`, `*args`, `action`, `notebook_type`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/notebook.py#L341'>src/bokeh/io/notebook.py:341:5:</a> D417 Missing argument descriptions in the docstring: `**kwargs`, `*args`, `action`, `notebook_type`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/notebook.py#L501'>src/bokeh/io/notebook.py:501:5:</a> D417 Missing argument description in the docstring for `show_app`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/notebook.py#L501'>src/bokeh/io/notebook.py:501:5:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/showing.py#L54'>src/bokeh/io/showing.py:54:5:</a> D417 Missing argument description in the docstring for `show`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/io/showing.py#L54'>src/bokeh/io/showing.py:54:5:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L120'>src/bokeh/layouts.py:120:5:</a> D417 Missing argument description in the docstring for `column`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L120'>src/bokeh/layouts.py:120:5:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L151'>src/bokeh/layouts.py:151:5:</a> D417 Missing argument descriptions in the docstring for `layout`: `**kwargs`, `*args`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L151'>src/bokeh/layouts.py:151:5:</a> D417 Missing argument descriptions in the docstring: `**kwargs`, `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L85'>src/bokeh/layouts.py:85:5:</a> D417 Missing argument description in the docstring for `row`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L85'>src/bokeh/layouts.py:85:5:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/model.py#L319'>src/bokeh/model/model.py:319:9:</a> D417 Missing argument descriptions in the docstring for `js_link`: `attr_selector`, `other`, `other_attr`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/model.py#L319'>src/bokeh/model/model.py:319:9:</a> D417 Missing argument descriptions in the docstring: `attr_selector`, `other`, `other_attr`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/model.py#L473'>src/bokeh/model/model.py:473:9:</a> D417 Missing argument description in the docstring for `select`: `selector`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/model.py#L473'>src/bokeh/model/model.py:473:9:</a> D417 Missing argument description in the docstring: `selector`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/model.py#L504'>src/bokeh/model/model.py:504:9:</a> D417 Missing argument descriptions in the docstring for `set_select`: `selector`, `updates`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/model.py#L504'>src/bokeh/model/model.py:504:9:</a> D417 Missing argument descriptions in the docstring: `selector`, `updates`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/util.py#L123'>src/bokeh/model/util.py:123:5:</a> D417 Missing argument description in the docstring for `collect_models`: `*input_values`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/util.py#L123'>src/bokeh/model/util.py:123:5:</a> D417 Missing argument description in the docstring: `*input_values`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/util.py#L80'>src/bokeh/model/util.py:80:5:</a> D417 Missing argument descriptions in the docstring for `collect_filtered_models`: `*input_values`, `discard`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/model/util.py#L80'>src/bokeh/model/util.py:80:5:</a> D417 Missing argument descriptions in the docstring: `*input_values`, `discard`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/plots.py#L124'>src/bokeh/models/plots.py:124:9:</a> D417 Missing argument description in the docstring for `select`: `*args`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/plots.py#L124'>src/bokeh/models/plots.py:124:9:</a> D417 Missing argument description in the docstring: `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/plots.py#L346'>src/bokeh/models/plots.py:346:9:</a> D417 Missing argument descriptions in the docstring for `add_glyph`: `**kwargs`, `source_or_glyph`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/plots.py#L346'>src/bokeh/models/plots.py:346:9:</a> D417 Missing argument descriptions in the docstring: `**kwargs`, `source_or_glyph`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/plots.py#L380'>src/bokeh/models/plots.py:380:9:</a> D417 Missing argument description in the docstring for `add_tile`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/plots.py#L380'>src/bokeh/models/plots.py:380:9:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/sources.py#L568'>src/bokeh/models/sources.py:568:9:</a> D417 Missing argument description in the docstring for `patch`: `setter`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/sources.py#L568'>src/bokeh/models/sources.py:568:9:</a> D417 Missing argument description in the docstring: `setter`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/util/structure.py#L150'>src/bokeh/models/util/structure.py:150:9:</a> D417 Missing argument description in the docstring for `_make_graph`: `M`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/util/structure.py#L150'>src/bokeh/models/util/structure.py:150:9:</a> D417 Missing argument description in the docstring: `M`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L235'>src/bokeh/plotting/_figure.py:235:9:</a> D417 Missing argument description in the docstring for `hexbin`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L235'>src/bokeh/plotting/_figure.py:235:9:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L346'>src/bokeh/plotting/_figure.py:346:9:</a> D417 Missing argument description in the docstring for `harea_stack`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L346'>src/bokeh/plotting/_figure.py:346:9:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L388'>src/bokeh/plotting/_figure.py:388:9:</a> D417 Missing argument description in the docstring for `hbar_stack`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L388'>src/bokeh/plotting/_figure.py:388:9:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L429'>src/bokeh/plotting/_figure.py:429:9:</a> D417 Missing argument descriptions in the docstring for `_line_stack`: `**kw`, `x`, `y`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L429'>src/bokeh/plotting/_figure.py:429:9:</a> D417 Missing argument descriptions in the docstring: `**kw`, `x`, `y`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L487'>src/bokeh/plotting/_figure.py:487:9:</a> D417 Missing argument description in the docstring for `hline_stack`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L487'>src/bokeh/plotting/_figure.py:487:9:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L526'>src/bokeh/plotting/_figure.py:526:9:</a> D417 Missing argument description in the docstring for `varea_stack`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L526'>src/bokeh/plotting/_figure.py:526:9:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L568'>src/bokeh/plotting/_figure.py:568:9:</a> D417 Missing argument description in the docstring for `vbar_stack`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L568'>src/bokeh/plotting/_figure.py:568:9:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L610'>src/bokeh/plotting/_figure.py:610:9:</a> D417 Missing argument description in the docstring for `vline_stack`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_figure.py#L610'>src/bokeh/plotting/_figure.py:610:9:</a> D417 Missing argument description in the docstring: `**kw`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_tools.py#L79'>src/bokeh/plotting/_tools.py:79:5:</a> D417 Missing argument description in the docstring for `process_active_tools`: `tool_map`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_tools.py#L79'>src/bokeh/plotting/_tools.py:79:5:</a> D417 Missing argument description in the docstring: `tool_map`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/glyph_api.py#L1024'>src/bokeh/plotting/glyph_api.py:1024:9:</a> D417 Missing argument description in the docstring for `scatter`: `*args`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/glyph_api.py#L1024'>src/bokeh/plotting/glyph_api.py:1024:9:</a> D417 Missing argument description in the docstring: `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/gmap.py#L107'>src/bokeh/plotting/gmap.py:107:5:</a> D417 Missing argument description in the docstring for `gmap`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/gmap.py#L107'>src/bokeh/plotting/gmap.py:107:5:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/graph.py#L37'>src/bokeh/plotting/graph.py:37:5:</a> D417 Missing argument description in the docstring for `from_networkx`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/graph.py#L37'>src/bokeh/plotting/graph.py:37:5:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/__init__.py#L122'>src/bokeh/protocol/__init__.py:122:9:</a> D417 Missing argument descriptions in the docstring for `create`: `**kwargs`, `*args`, `msgtype`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/__init__.py#L122'>src/bokeh/protocol/__init__.py:122:9:</a> D417 Missing argument descriptions in the docstring: `**kwargs`, `*args`, `msgtype`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/__init__.py#L133'>src/bokeh/protocol/__init__.py:133:9:</a> D417 Missing argument descriptions in the docstring for `assemble`: `content_json`, `header_json`, `metadata_json`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/__init__.py#L133'>src/bokeh/protocol/__init__.py:133:9:</a> D417 Missing argument descriptions in the docstring: `content_json`, `header_json`, `metadata_json`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L138'>src/bokeh/protocol/message.py:138:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `content`, `header`, `metadata`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L138'>src/bokeh/protocol/message.py:138:9:</a> D417 Missing argument descriptions in the docstring: `content`, `header`, `metadata`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L165'>src/bokeh/protocol/message.py:165:9:</a> D417 Missing argument descriptions in the docstring for `assemble`: `content_json`, `header_json`, `metadata_json`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L165'>src/bokeh/protocol/message.py:165:9:</a> D417 Missing argument descriptions in the docstring: `content_json`, `header_json`, `metadata_json`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L206'>src/bokeh/protocol/message.py:206:9:</a> D417 Missing argument description in the docstring for `add_buffer`: `buffer`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L206'>src/bokeh/protocol/message.py:206:9:</a> D417 Missing argument description in the docstring: `buffer`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L249'>src/bokeh/protocol/message.py:249:15:</a> D417 Missing argument description in the docstring for `write_buffers`: `locked`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/message.py#L249'>src/bokeh/protocol/message.py:249:15:</a> D417 Missing argument description in the docstring: `locked`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/error.py#L77'>src/bokeh/protocol/messages/error.py:77:9:</a> D417 Missing argument description in the docstring for `create`: `**metadata`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/error.py#L77'>src/bokeh/protocol/messages/error.py:77:9:</a> D417 Missing argument description in the docstring: `**metadata`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/ok.py#L54'>src/bokeh/protocol/messages/ok.py:54:9:</a> D417 Missing argument description in the docstring for `create`: `**metadata`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/ok.py#L54'>src/bokeh/protocol/messages/ok.py:54:9:</a> D417 Missing argument description in the docstring: `**metadata`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/patch_doc.py#L68'>src/bokeh/protocol/messages/patch_doc.py:68:9:</a> D417 Missing argument description in the docstring for `create`: `**metadata`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/patch_doc.py#L68'>src/bokeh/protocol/messages/patch_doc.py:68:9:</a> D417 Missing argument description in the docstring: `**metadata`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/pull_doc_reply.py#L67'>src/bokeh/protocol/messages/pull_doc_reply.py:67:9:</a> D417 Missing argument description in the docstring for `create`: `**metadata`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/pull_doc_reply.py#L67'>src/bokeh/protocol/messages/pull_doc_reply.py:67:9:</a> D417 Missing argument description in the docstring: `**metadata`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/server_info_reply.py#L73'>src/bokeh/protocol/messages/server_info_reply.py:73:9:</a> D417 Missing argument description in the docstring for `create`: `**metadata`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/protocol/messages/server_info_reply.py#L73'>src/bokeh/protocol/messages/server_info_reply.py:73:9:</a> D417 Missing argument description in the docstring: `**metadata`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L114'>src/bokeh/server/callbacks.py:114:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `callback`, `callback_id`, `period`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L114'>src/bokeh/server/callbacks.py:114:9:</a> D417 Missing argument descriptions in the docstring: `callback`, `callback_id`, `period`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L144'>src/bokeh/server/callbacks.py:144:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `callback`, `callback_id`, `timeout`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L144'>src/bokeh/server/callbacks.py:144:9:</a> D417 Missing argument descriptions in the docstring: `callback`, `callback_id`, `timeout`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L59'>src/bokeh/server/callbacks.py:59:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `callback`, `callback_id`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L59'>src/bokeh/server/callbacks.py:59:9:</a> D417 Missing argument descriptions in the docstring: `callback`, `callback_id`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L95'>src/bokeh/server/callbacks.py:95:9:</a> D417 Missing argument descriptions in the docstring for `__init__`: `callback`, `callback_id`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/callbacks.py#L95'>src/bokeh/server/callbacks.py:95:9:</a> D417 Missing argument descriptions in the docstring: `callback`, `callback_id`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/server.py#L153'>src/bokeh/server/server.py:153:9:</a> D417 Missing argument description in the docstring for `stop`: `wait`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/server.py#L153'>src/bokeh/server/server.py:153:9:</a> D417 Missing argument description in the docstring: `wait`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/server.py#L351'>src/bokeh/server/server.py:351:9:</a> D417 Missing argument description in the docstring for `__init__`: `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/server.py#L351'>src/bokeh/server/server.py:351:9:</a> D417 Missing argument description in the docstring: `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/util.py#L166'>src/bokeh/server/util.py:166:5:</a> D417 Missing argument descriptions in the docstring for `match_host`: `host`, `pattern`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/util.py#L166'>src/bokeh/server/util.py:166:5:</a> D417 Missing argument descriptions in the docstring: `host`, `pattern`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/views/ws.py#L181'>src/bokeh/server/views/ws.py:181:15:</a> D417 Missing argument description in the docstring for `_async_open`: `token`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/server/views/ws.py#L181'>src/bokeh/server/views/ws.py:181:15:</a> D417 Missing argument description in the docstring: `token`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/callback_manager.py#L138'>src/bokeh/util/callback_manager.py:138:9:</a> D417 Missing argument description in the docstring for `on_change`: `*callbacks`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/callback_manager.py#L138'>src/bokeh/util/callback_manager.py:138:9:</a> D417 Missing argument description in the docstring: `*callbacks`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/callback_manager.py#L168'>src/bokeh/util/callback_manager.py:168:9:</a> D417 Missing argument descriptions in the docstring for `trigger`: `attr`, `hint`, `new`, `old`, `setter`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/callback_manager.py#L168'>src/bokeh/util/callback_manager.py:168:9:</a> D417 Missing argument descriptions in the docstring: `attr`, `hint`, `new`, `old`, `setter`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/strings.py#L63'>src/bokeh/util/strings.py:63:5:</a> D417 Missing argument description in the docstring for `nice_join`: `conjuction`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/strings.py#L63'>src/bokeh/util/strings.py:63:5:</a> D417 Missing argument description in the docstring: `conjuction`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/support/util/env.py#L41'>tests/support/util/env.py:41:5:</a> D417 Missing argument description in the docstring for `envset`: `**kw`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/support/util/env.py#L41'>tests/support/util/env.py:41:5:</a> D417 Missing argument description in the docstring: `**kw`
</pre>

</p>
</details>
<details><summary>zulip (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/tex.py#L11'>zerver/lib/tex.py:11:5:</a> D417 Missing argument descriptions in the docstring for `render_tex`: `is_inline`, `tex`
- <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/tex.py#L11'>zerver/lib/tex.py:11:5:</a> D417 Missing argument descriptions in the docstring: `is_inline`, `tex`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| D417 | 234 | 117 | 117 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.03ms     4.3 MB/sec    1.00      9.4±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   1884.1±1.92µs     8.8 MB/sec    1.00   1865.0±2.81µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01    210.9±0.29µs    14.0 MB/sec    1.00    209.5±0.35µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.07ms     3.1 MB/sec    1.00     13.2±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.4±0.96µs     6.8 MB/sec    1.00    438.1±0.76µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1389.7±2.05µs    12.0 MB/sec    1.01   1405.1±4.56µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.1±0.32µs    19.1 MB/sec    1.01    155.7±0.55µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.01      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.05ms     3.7 MB/sec    1.00     11.1±0.10ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.9 MB/sec    1.01      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   231.9±20.01µs    12.7 MB/sec    1.01    233.4±8.60µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.4 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.31ms     2.6 MB/sec    1.00     15.3±0.14ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.02      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    400.3±4.61µs     7.4 MB/sec    1.04    417.7±7.28µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.01      7.0±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.04ms     5.1 MB/sec    1.02      8.1±0.13ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1580.0±46.16µs    10.5 MB/sec    1.05  1653.5±24.20µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.8±0.70µs    18.2 MB/sec    1.05    169.1±2.02µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.3 MB/sec    1.02      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-17 02:51_

---

_Closed by @charliermarsh on 2023-07-17 02:51_

---

_Branch deleted on 2023-07-17 02:51_

---
