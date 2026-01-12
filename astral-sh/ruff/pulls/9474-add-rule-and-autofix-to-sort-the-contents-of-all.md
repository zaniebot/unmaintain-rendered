```yaml
number: 9474
title: "Add rule and autofix to sort the contents of `__all__`"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: sort-dunder-all-2
created_at: 2024-01-11T17:31:08Z
updated_at: 2024-10-08T16:39:37Z
url: https://github.com/astral-sh/ruff/pull/9474
synced_at: 2026-01-12T15:55:29Z
```

# Add rule and autofix to sort the contents of `__all__`

---

_@AlexWaygood_

## Summary

This implements the rule proposed in #1198 (though it doesn't close the issue, as there are some open questions about configuration that might merit some further discussion). This ended up being a bigger PR than I expected! I've tried to make sure it's well commented, so hopefully it's not too hard to figure out what's going on.

## Test Plan

`cargo test` / `cargo insta review`. I also ran this rule on the CPython codebase, and it produces this diff, which looks pretty good to me: [cpython_diff.txt](https://github.com/astral-sh/ruff/files/13907385/cpython_diff.txt)



---

_Comment by @AlexWaygood on 2024-01-11 18:02_

Some miscellaneous notes:
- We don't try to sort `__all__` if there's any implicitly concatenated strings inside `__all__`.
- We don't try to sort `__all__` if there are any parenthesized items inside `__all__`, e.g.

  ```py
  __all__ = (
      "foo",
      (
          "why_would_you_do_this?"
      ),
      "bar"
  )
  ```

- There's no attempt made to deduplicate elements in `__all__`: elements are first sorted alphabetically, then by the order they originally appeared in.
- Some people on the issue wanted to also be able to sort elements by definition order as well as by alphabetical order. That could possibly be implemented as a followup, but I'm not sure it's worth it. It feels like it would add significant complexity, and I don't know how many people would use it.
- The autofix makes a complete hash of `__all__` definitions like [this](https://github.com/python/cpython/blob/0d8fec79ca30e67870752c6ad4e299f591271e69/Lib/_pydecimal.py#L115-L148) or [this](https://github.com/python/cpython/blob/0d8fec79ca30e67870752c6ad4e299f591271e69/Lib/typing.py#L43-L157), which use comments to create subsections in `__all__`. This is, unfortunately, reasonably common. Ideally, we'd have an option where users could tell ruff to treat all comments on their own line as section delimiters -- if that option was selected, ruff would sort `__all__` within sections, but wouldn't ever move an element out of the section it's already in. I don't attempt to do that here, however, and there are some open design questions about how we would let users opt into that alternative behaviour (see discussion in https://github.com/astral-sh/ruff/issues/1198).

---

_Comment by @github-actions[bot] on 2024-01-11 18:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+156 -0 violations, +0 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+48 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/abc.py#L50'>disnake/abc.py:50:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/activity.py#L13'>disnake/activity.py:13:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/app_commands.py#L49'>disnake/app_commands.py:49:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/appinfo.py#L23'>disnake/appinfo.py:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/audit_logs.py#L34'>disnake/audit_logs.py:34:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/automod.py#L51'>disnake/automod.py:51:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/channel.py#L52'>disnake/channel.py:52:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/client.py#L100'>disnake/client.py:100:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/colour.py#L13'>disnake/colour.py:13:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/components.py#L44'>disnake/components.py:44:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/custom_warnings.py#L5'>disnake/custom_warnings.py:5:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/enums.py#L23'>disnake/enums.py:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/errors.py#L16'>disnake/errors.py:16:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/bot.py#L32'>disnake/ext/commands/bot.py:32:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/bot_base.py#L32'>disnake/ext/commands/bot_base.py:32:11:</a> RUF022 [*] `__all__` is not sorted
... 33 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/aio_pika/__init__.pyi#L23'>stubs/aio_pika/__init__.pyi:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L11'>stubs/sanic.pyi:11:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/__init__.py#L54'>airflow/__init__.py:54:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/decorators/__init__.py#L37'>airflow/decorators/__init__.py:37:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/decorators/__init__.pyi#L44'>airflow/decorators/__init__.pyi:44:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/models/__init__.py#L22'>airflow/models/__init__.py:22:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/providers/amazon/aws/operators/rds.py#L927'>airflow/providers/amazon/aws/operators/rds.py:927:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/providers/amazon/aws/sensors/rds.py#L189'>airflow/providers/amazon/aws/sensors/rds.py:189:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/providers/cncf/kubernetes/utils/__init__.py#L19'>airflow/providers/cncf/kubernetes/utils/__init__.py:19:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/providers/openlineage/extractors/__init__.py#L27'>airflow/providers/openlineage/extractors/__init__.py:27:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/secrets/__init__.py#L29'>airflow/secrets/__init__.py:29:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/57c921193b86569f42bb521a356d28d1c4c619f3/airflow/typing_compat.py#L21'>airflow/typing_compat.py:21:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+53 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/release/checks.py#L24'>release/checks.py:24:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/__init__.py#L46'>src/bokeh/client/__init__.py:46:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/states.py#L40'>src/bokeh/client/states.py:40:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/colors/__init__.py#L37'>src/bokeh/colors/__init__.py:37:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/enums.py#L91'>src/bokeh/core/enums.py:91:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/has_props.py#L81'>src/bokeh/core/has_props.py:81:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/properties.py#L201'>src/bokeh/core/properties.py:201:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/color.py#L42'>src/bokeh/core/property/color.py:42:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/primitive.py#L40'>src/bokeh/core/property/primitive.py:40:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/string.py#L38'>src/bokeh/core/property/string.py:38:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/visual.py#L50'>src/bokeh/core/property/visual.py:50:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property_mixins.py#L126'>src/bokeh/core/property_mixins.py:126:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/query.py#L43'>src/bokeh/core/query.py:43:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/templates.py#L51'>src/bokeh/core/templates.py:51:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/callbacks.py#L63'>src/bokeh/document/callbacks.py:63:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/events.py#L98'>src/bokeh/document/events.py:98:11:</a> RUF022 [*] `__all__` is not sorted
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/backends/base/sql/alchemy/__init__.py#L51'>ibis/backends/base/sql/alchemy/__init__.py:51:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/backends/base/sql/compiler/__init__.py#L15'>ibis/backends/base/sql/compiler/__init__.py:15:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/backends/base/sql/ddl.py#L448'>ibis/backends/base/sql/ddl.py:448:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/backends/base/sql/registry/__init__.py#L21'>ibis/backends/base/sql/registry/__init__.py:21:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/backends/impala/compat.py#L20'>ibis/backends/impala/compat.py:20:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/backends/impala/udf.py#L28'>ibis/backends/impala/udf.py:28:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/b3f0ee8f29b5ff2d31fcad38fbc4551558ac2c5c/ibis/expr/api.py#L52'>ibis/expr/api.py:52:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/d7a402b8cfdd24d3e5626da817de828ca14d8acb/pymilvus/__init__.py#L88'>pymilvus/__init__.py:88:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/ee4a8803fe3f632b8937d80dd327469dac96fdc9/src/build/__init__.py#L380'>src/build/__init__.py:380:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/pypa/build/blob/ee4a8803fe3f632b8937d80dd327469dac96fdc9/src/build/env.py#L287'>src/build/env.py:287:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/cibuildwheel/_compat/typing.py#L10'>cibuildwheel/_compat/typing.py:10:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/cibuildwheel/typing.py#L8'>cibuildwheel/typing.py:8:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/cibuildwheel/util.py#L37'>cibuildwheel/util.py:37:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/7280d6cf5dc4ebc3028dc78df9c580444fee588b/rotkehlchen/chain/ethereum/interfaces/ammswap/__init__.py#L3'>rotkehlchen/chain/ethereum/interfaces/ammswap/__init__.py:3:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/rotki/rotki/blob/7280d6cf5dc4ebc3028dc78df9c580444fee588b/rotkehlchen/chain/ethereum/modules/__init__.py#L1'>rotkehlchen/chain/ethereum/modules/__init__.py:1:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/rotki/rotki/blob/7280d6cf5dc4ebc3028dc78df9c580444fee588b/rotkehlchen/chain/ethereum/modules/balancer/__init__.py#L1'>rotkehlchen/chain/ethereum/modules/balancer/__init__.py:1:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/030e6c2abb685a74b176dad50dbdc3c612b9f73f/skbuild/__init__.py#L16'>skbuild/__init__.py:16:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build/blob/030e6c2abb685a74b176dad50dbdc3c612b9f73f/skbuild/_compat/typing.py#L10'>skbuild/_compat/typing.py:10:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build/blob/030e6c2abb685a74b176dad50dbdc3c612b9f73f/tests/samples/issue-707-nested-packages/hello_nested/__init__.py#L6'>tests/samples/issue-707-nested-packages/hello_nested/__init__.py:6:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+23 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/_compat/typing.py#L29'>src/scikit_build_core/_compat/typing.py:29:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/_logging.py#L10'>src/scikit_build_core/_logging.py:10:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/build/__init__.py#L9'>src/scikit_build_core/build/__init__.py:9:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/build/_pathutil.py#L12'>src/scikit_build_core/build/_pathutil.py:12:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/build/_wheelfile.py#L41'>src/scikit_build_core/build/_wheelfile.py:41:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/builder/builder.py#L31'>src/scikit_build_core/builder/builder.py:31:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/builder/macos.py#L9'>src/scikit_build_core/builder/macos.py:9:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/builder/sysconfig.py#L15'>src/scikit_build_core/builder/sysconfig.py:15:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/errors.py#L9'>src/scikit_build_core/errors.py:9:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/src/scikit_build_core/file_api/_cattrs_converter.py#L18'>src/scikit_build_core/file_api/_cattrs_converter.py:18:11:</a> RUF022 [*] `__all__` is not sorted
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/906559c2b56bc95700aeaa0062b53b774df8daf2/zerver/lib/url_preview/parsers/__init__.py#L4'>zerver/lib/url_preview/parsers/__init__.py:4:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF022 | 156 | 156 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-01-11 18:11_

Oh, one other note: I decided to implement this as an RUF rule, since:

1. Although isort has this functionality, it's undocumented, and has always been undocumented
2. It doesn't really feel like it has much to do with imports

Since starting working on the rule, however, I realised that the https://github.com/cpendery/asort tool _does_ also exist, so possibly we could call this rule ASORT001 instead of RUF022. I don't have a strong opinion either way!

---

_@zanieb reviewed on 2024-01-11 18:35_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 18:35_

This could be an `enum` e.g. (not necessarily the best names)

```rust
enum SortedDunderAll {
    AlreadySorted,
    Sorted(String)
}
```

---

_@AlexWaygood reviewed on 2024-01-11 18:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 18:46_

Oh nice! Will make that change

---

_Comment by @codspeed-hq[bot] on 2024-01-11 18:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:sort-dunder-all-2)

### Merging #9474 will **degrade performances by 4.36%**

<sub>Comparing <code>AlexWaygood:sort-dunder-all-2</code> (c89556c) with <code>main</code> (f9191b0)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:sort-dunder-all-2)._

### Benchmarks breakdown

|     | Benchmark | `main` | `AlexWaygood:sort-dunder-all-2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.36% |


---

_@charliermarsh reviewed on 2024-01-11 18:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 18:54_

I'd recommend adding separate methods in `sort_dunder_all` for `Assign` and `AugAssign` (and probably also `AnnAssign`?), and then have those call into a single method internally that takes the statement and value. That way, we ensure we're only calling this rule on the correct AST nodes.

---

_@charliermarsh reviewed on 2024-01-11 18:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 18:57_

Alternatively, `construct_sorted_all` could return `Option<SortedDunderAll>`, where `None` indicates "already sorted". It's the same idea. `enum SortedDunderAll` is arguably more explicit.

---

_@AlexWaygood reviewed on 2024-01-11 19:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 19:00_

There's three states we need to represent here:
- `__all__` was already sorted; don't emit the violation
- `__all__` was not sorted, so we should emit the violation, but we don't know how to safely fix it here for whatever reason; something failed along the way
- `__all__` was not sorted, and here's the fix

For now I've gone with the enum, which I think is definitely better than what I originally had, but the naming still doesn't feel _ideal_. Open to suggestions :)

---

_@AlexWaygood reviewed on 2024-01-11 19:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 19:06_

I wondered what you might say about this bit :)

Just to double-check I understand: I should have three separate functions in `sort_dunder_all.rs`, with these signatures?

```rs
pub(crate) fn sort_dunder_all_assign(checker: &mut Checker, stmt: &ast::StmtAssign) {}
pub(crate) fn sort_dunder_all_annassign(checker: &mut Checker, stmt: &ast::StmtAnnAssign) {}
pub(crate) fn sort_dunder_all_assign(checker: &mut Checker, stmt: &ast::StmtAugAssign) {}
```

And then call out to those from different places in `checkers/ast/analyze/statement.rs`?

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-11 19:21_

I think `value` is a `Box<Expr>` here right? If so, [using `as_ref()` outside a context where the target is specifically constrained](https://github.com/rust-lang/rust/issues/62586) opens one up to inference regressions. It's a very common way that changes in std actually break Rust code in the wild. (It happens when a new `AsRef` impl is added in std that in turn causes inference to become ambiguous in some cases.)

I think you're just looking for a `&Expr` here right? If so, `&value` should work. (You can also match `value` by reference via `ast::StmtAssign { ref value, targets, .. }`.)

(Similarly for other uses of `as_ref()` below.)

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-11 19:25_

(You might actually be okay here because the surrounding context makes it look like, to me, that inference knows the target of `AsRef` must be `Expr`. But it's not something that is easy to confirm or reject on inspection.)

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:219 on 2024-01-11 19:29_

I'm fond of doing things like `self.items.len().checked_sub(1).expect("non-empty items")` to make the assumption explicit. Or even better in this case, `self.items.last().expect("non-empty items")`.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:208 on 2024-01-11 19:32_

I was slightly confused by the "returned by now" phrasing here, since I assumed that applied to this method. But looking at the code, I think the more precise phrasing is, "construction of `DunderAllValue` guarantees at least two dunder items."

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:209 on 2024-01-11 19:38_

There is also `self.items.first().expect("non-empty items")`.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:139 on 2024-01-11 19:43_

Some docs on this type might help. In particular I'm thinking about the invariant that `items` has length at least 2.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:368 on 2024-01-11 19:51_

I would remove this line since it's redundant with the macro.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:331 on 2024-01-11 19:55_

It might help to add a short sentence saying what this routine does, and in particular, document its preconditions. (My _goal_ is for docs for each function to include all conditions in which it panics. It is a hard goal to achieve perfect on though.) The standard way to write it is like this:

```rust
/// Builds sequence representing an `__all__` value, where each
/// element in the sequence is Python code corresponding to
/// an element in `__all__`.
///
/// # Panics
///
/// When the given slice contains a line that is neither a comment nor an item.
```


---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:331 on 2024-01-11 19:56_

I'd do the same for `DunderAllLine::new` above too.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 19:58_

There is an interesting invariant lurking here I think. Namely, I believe the code calling this function depends on this always returning `None` or a `Vec` with greater than 2 items. And I think this invariant is somewhat complicated to explain? Namely, that there is a guaranteed correspondence between the number of items in a particular AST node and the number items extracted from the tokens at the same location. That seems _probably_ true to me, but I'm having trouble convincing myself it is 100% correct. If it isn't, the code here might want to be a little more defensive.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 20:00_

I also think some comments describing what this function does would be helpful. It seems like a very crucial detail of how  this lint works. In particular, it is the part that tokenizes a region of the code that we already have an AST for. I think I can make some educated guesses as to why you're doing that here (I'm still somewhat new to this code too), but a comment explaining/motivating the technique would be quite helpful I think!

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:139 on 2024-01-11 20:01_

And I'll say that it isn't obvious to me that this invariant holds, but I think, for example, `construct_sorted_all` depends on it (it will panic, I believe, if `self.items` is empty). I was thinking this invariant was upheld by `if elts.len() < 2 { return None }` below. But `elts` gets passed through `collect_dunder_all_lines` and then `collect_dunder_all_items`. And it's the result of that that is used for `DunderAllValue::items`. But is the length preserved through those operations? I'm not sure. (I mused about this below too.)

---

_@BurntSushi reviewed on 2024-01-11 20:02_

Nice work! I think some extra docs on some of the more complicated helper functions/types would be beneficial here. There are some interesting invariants at play here. :-)

---

_@BurntSushi reviewed on 2024-01-11 20:07_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 20:07_

@charliermarsh Is this how the existing lints work? It is interesting to me because it seems nice for the lint implementation itself to own when it is applicable. Otherwise, you wind up redoing the case analysis. Although I suppose if _all_ lints did this, then every lint would do its own case analysis and maybe that would be bad for perf reasons. I think with your suggested approach, maybe most lints don't need to do any case analysis.

@AlexWaygood Your code snippet looks right to me. Although I do wonder if you could avoid re-doing case analysis on `ast::Stmt`. Basically, have your internal function accept a `target` and `original_value`. Not 100% sure on that though.

---

_@AlexWaygood reviewed on 2024-01-11 20:13_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 20:13_

> Although I do wonder if you could avoid re-doing case analysis on `ast::Stmt`. Basically, have your internal function accept a `target` and `original_value`. Not 100% sure on that though.

Charlie [called me out on that](https://github.com/astral-sh/ruff/pull/9313#discussion_r1438924016) last time I tried to do something like that 😅

---

_Comment by @AlexWaygood on 2024-01-11 20:14_

Thanks for the extremely helpful review @BurntSushi!

---

_@BurntSushi reviewed on 2024-01-11 21:22_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 21:22_

Right. I don't mean that. I'm still proposing that you have the three functions @charliermarsh suggested above as the _public_ API for your new lint module. But internally, instead of re-materializing an `ast::Stmt` and just pattern matching on it again, you extract the state you need and pass it to your main internal routine that implements the lint.

---

_@AlexWaygood reviewed on 2024-01-11 21:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 21:24_

Ah, I think we're on the same page! I just pushed cc42bf9 -- is that what you had in mind?

---

_@BurntSushi reviewed on 2024-01-11 21:36_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:24 on 2024-01-11 21:36_

Precisely. :-)

---

_@AlexWaygood reviewed on 2024-01-11 21:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:208 on 2024-01-11 21:46_

I had a go at revamping the comments in this function in https://github.com/astral-sh/ruff/pull/9474/commits/454e7301a15e4b3c05bb584338e711ec6e8d05dc. The most important thing is that, while I think you're right that we probably don't have this function called at all unless `__all__` has at least two items, there's an even stronger reason to be confident in the invariant that `self.items` must have length >= 2 here: if `__all__` has one item or less, then `self.items` and `sorted_items` _have_ to compare equal, so we short-circuit at this point:

```py
        if sorted_items == self.items {
            return SortedDunderAll::AlreadySorted;
        }
```

...and can safely rely on both `self.items` and `sorted_items` having length >=2 for the rest of the function

---

_@charliermarsh reviewed on 2024-01-11 22:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:170 on 2024-01-11 22:08_

This is a good instinct, but `TextRange` is small and implements `Copy`, so it's preferable to just use `TextRange` here. (You won't even need to `.clone()` it.)

---

_@AlexWaygood reviewed on 2024-01-11 22:10_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:170 on 2024-01-11 22:10_

I keep on thinking today will be the day I get to use lifetimes properly for the first time 🙃

---

_@charliermarsh reviewed on 2024-01-11 22:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:325 on 2024-01-11 22:12_

Can you say more about the bracket handling here? I'm trying to follow. Typically, we do something more like this, where we track all open and closing brackets via a counter, to know if we're at the top level: https://github.com/astral-sh/ruff/blob/eb4ed2471b48d6ccba74064c65e37e32cba41a6c/crates/ruff_python_parser/src/soft_keywords.rs#L67

In this case, we're aborting if we see nested brackets, is that right? If so, should `Lbrace` also be included?


---

_@charliermarsh reviewed on 2024-01-11 22:13_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:385 on 2024-01-11 22:13_

Nit: consider destructuring in the iterator? Like `for DunderAllLine { ... } in lines { ... }`? Personal preference though, not blocking.

---

_@AlexWaygood reviewed on 2024-01-11 22:13_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:385 on 2024-01-11 22:13_

ooh you can do that? did not know

---

_@charliermarsh reviewed on 2024-01-11 22:15_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:404 on 2024-01-11 22:15_

I feel like it should be possible to avoid this clone, since we don't need `lines` beyond this method. We could consider either making `lines` an owned `Vec` to this method, or using lifetimes on `DunderAllItem`, and having it take `&str` that's tied to the lifetime of `lines: &[DunderAllLine]`. (I'd probably do the latter.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:480 on 2024-01-11 22:15_

Nit: this can be `let indent = indentation(locator, parent)?;`

---

_@charliermarsh reviewed on 2024-01-11 22:15_

---

_@charliermarsh reviewed on 2024-01-11 22:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:157 on 2024-01-11 22:16_

I tend to think that using unsafe to protect comments is a bit of a misuse of the concept, which is meant to protect against possible breakages via changes in semantics. But, this question has come up before, and I don't know that we have a unified answer.

\cc @zanieb 

---

_@AlexWaygood reviewed on 2024-01-11 22:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:325 on 2024-01-11 22:16_

> In this case, we're aborting if we see nested brackets, is that right?

Yup, that's correct. If there are any parenthesized items in `__all__`, that would required a lot more complexity to handle, and it seems like a massive edge case, so not worth it.

> should `Lbrace` also be included?

It's currently handled by the catch-all `_` condition at the end of the `match` (which just returns `None` if we encounter any token not explicitly enumerated already), but I can move it here if you like?

---

_@charliermarsh reviewed on 2024-01-11 22:22_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:146 on 2024-01-11 22:22_

Personal style, I'd probably just inline this wherever it's used, I don't know that the binding adds much but it does add more things named `dunder_all_*` to worry about.

---

_@charliermarsh reviewed on 2024-01-11 22:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:217 on 2024-01-11 22:26_

Nit: I think idiomatically in Rust you might call this `as_sorted()` (if it takes `&self`) or `into_sorted()`. In this case, I think `into_sorted` would make sense, then it could take `self` instead of `&self` and consume `self.items` (so no need to `.clone()`).

---

_@AlexWaygood reviewed on 2024-01-11 22:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:404 on 2024-01-11 22:26_

I think I initially tried doing the latter, but the borrow checker was very angry with me, so I ran away. I'll have another go tomorrow.

---

_@charliermarsh reviewed on 2024-01-11 22:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:325 on 2024-01-11 22:27_

In that case, though, why  is `Lsqb` handled here?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:347 on 2024-01-11 22:29_

Can you walk me through how the comment-handling works here? How are own-line comments handled? (It looks like `comment_in_line` is for end-of-line comments, like `"foo" # bar`.) What happens if there are multiple own-lines comments between items?

---

_@charliermarsh reviewed on 2024-01-11 22:29_

---

_@charliermarsh reviewed on 2024-01-11 22:30_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:347 on 2024-01-11 22:30_

Oh, a `DunderAllLine` can be _just_ a comment, with no item.

---

_@charliermarsh reviewed on 2024-01-11 22:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:334 on 2024-01-11 22:31_

Even though we're breaking, it might be worth adding:

```rust
                    items_in_line.clear();
                    comment_in_line = None;
```

...to the end of the `if` block above, as a consistent "reset condition".

---

_@charliermarsh reviewed on 2024-01-11 22:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:368 on 2024-01-11 22:31_

Nit: I have a preference towards avoiding single-letter variables in favor of more explicit names. E.g., `item` and `range` here, maybe?

---

_@charliermarsh reviewed on 2024-01-11 22:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:368 on 2024-01-11 22:34_

I think we should be able to avoid this `.to_owned()`... E.g., could we pass in `&mut Vec`? Or maybe we pass in `items.drain(..)` as the argument?

---

_@charliermarsh reviewed on 2024-01-11 22:35_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:403 on 2024-01-11 22:35_

Could `idx` here just be `all_items.len()`, instead of tracking separately?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1068 on 2024-01-11 22:35_

Nit: `aug_assign` to match the `AugAssign` casing? Same with the method name.

---

_@charliermarsh reviewed on 2024-01-11 22:35_

---

_@charliermarsh reviewed on 2024-01-11 22:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-11 22:36_

Oops, good call!

---

_@charliermarsh reviewed on 2024-01-11 22:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-11 22:36_

I always use `.as_ref()` for this, FWIW. I might be doing it wrong then :)

---

_Comment by @charliermarsh on 2024-01-11 22:36_

@AlexWaygood -- Given:

```python
__all__ = [
    # Two major classes
    'Decimal', 'Context',

    # Named tuple representation
    'DecimalTuple',

    # Contexts
    'DefaultContext', 'BasicContext', 'ExtendedContext',

    # Exceptions
    'DecimalException', 'Clamped', 'InvalidOperation', 'DivisionByZero',
    'Inexact', 'Rounded', 'Subnormal', 'Overflow', 'Underflow',
    'FloatOperation',

    # Exceptional conditions that trigger InvalidOperation
    'DivisionImpossible', 'InvalidContext', 'ConversionSyntax', 'DivisionUndefined',

    # Constants for use in setting up contexts
    'ROUND_DOWN', 'ROUND_HALF_UP', 'ROUND_HALF_EVEN', 'ROUND_CEILING',
    'ROUND_FLOOR', 'ROUND_UP', 'ROUND_HALF_DOWN', 'ROUND_05UP',

    # Functions for manipulating contexts
    'setcontext', 'getcontext', 'localcontext',

    # Limits for the C version for compatibility
    'MAX_PREC',  'MAX_EMAX', 'MIN_EMIN', 'MIN_ETINY',

    # C version: compile time choice that enables the thread local context (deprecated, now always true)
    'HAVE_THREADS',

    # C version: compile time choice that enables the coroutine local context
    'HAVE_CONTEXTVAR'
]
```

How exactly does that get formatted after this fix? (Just following up per one of your comments above.)

---

_@AlexWaygood reviewed on 2024-01-11 22:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:325 on 2024-01-11 22:44_

Because `__all__` could be a list, and we don't want to reject the opening square bracket of `__all__` if it's a list 

---

_Comment by @AlexWaygood on 2024-01-11 22:49_

> @AlexWaygood -- Given:
> 
> ```python
> __all__ = [
>     # Two major classes
>     'Decimal', 'Context',
> 
>     # Named tuple representation
>     'DecimalTuple',
> 
>     # Contexts
>     'DefaultContext', 'BasicContext', 'ExtendedContext',
> 
>     # Exceptions
>     'DecimalException', 'Clamped', 'InvalidOperation', 'DivisionByZero',
>     'Inexact', 'Rounded', 'Subnormal', 'Overflow', 'Underflow',
>     'FloatOperation',
> 
>     # Exceptional conditions that trigger InvalidOperation
>     'DivisionImpossible', 'InvalidContext', 'ConversionSyntax', 'DivisionUndefined',
> 
>     # Constants for use in setting up contexts
>     'ROUND_DOWN', 'ROUND_HALF_UP', 'ROUND_HALF_EVEN', 'ROUND_CEILING',
>     'ROUND_FLOOR', 'ROUND_UP', 'ROUND_HALF_DOWN', 'ROUND_05UP',
> 
>     # Functions for manipulating contexts
>     'setcontext', 'getcontext', 'localcontext',
> 
>     # Limits for the C version for compatibility
>     'MAX_PREC',  'MAX_EMAX', 'MIN_EMIN', 'MIN_ETINY',
> 
>     # C version: compile time choice that enables the thread local context (deprecated, now always true)
>     'HAVE_THREADS',
> 
>     # C version: compile time choice that enables the coroutine local context
>     'HAVE_CONTEXTVAR'
> ]
> ```
> 
> How exactly does that get formatted after this fix? (Just following up per one of your comments above.)

Here's the diff:

<details>

```diff
diff --git a/Lib/_pydecimal.py b/Lib/_pydecimal.py
index 2692f2fcba..382f9c4807 100644
--- a/Lib/_pydecimal.py
+++ b/Lib/_pydecimal.py
@@ -113,38 +113,53 @@
 """
 
 __all__ = [
+    'BasicContext',
+    'Clamped',
+    'Context',
+    'ConversionSyntax',
     # Two major classes
-    'Decimal', 'Context',
-
+    'Decimal',
+    # Exceptions
+    'DecimalException',
     # Named tuple representation
     'DecimalTuple',
-
     # Contexts
-    'DefaultContext', 'BasicContext', 'ExtendedContext',
-
-    # Exceptions
-    'DecimalException', 'Clamped', 'InvalidOperation', 'DivisionByZero',
-    'Inexact', 'Rounded', 'Subnormal', 'Overflow', 'Underflow',
-    'FloatOperation',
-
+    'DefaultContext',
+    'DivisionByZero',
     # Exceptional conditions that trigger InvalidOperation
-    'DivisionImpossible', 'InvalidContext', 'ConversionSyntax', 'DivisionUndefined',
-
-    # Constants for use in setting up contexts
-    'ROUND_DOWN', 'ROUND_HALF_UP', 'ROUND_HALF_EVEN', 'ROUND_CEILING',
-    'ROUND_FLOOR', 'ROUND_UP', 'ROUND_HALF_DOWN', 'ROUND_05UP',
-
-    # Functions for manipulating contexts
-    'setcontext', 'getcontext', 'localcontext',
-
-    # Limits for the C version for compatibility
-    'MAX_PREC',  'MAX_EMAX', 'MIN_EMIN', 'MIN_ETINY',
-
+    'DivisionImpossible',
+    'DivisionUndefined',
+    'ExtendedContext',
+    'FloatOperation',
+    # C version: compile time choice that enables the coroutine local context
+    'HAVE_CONTEXTVAR',
     # C version: compile time choice that enables the thread local context (deprecated, now always true)
     'HAVE_THREADS',
-
-    # C version: compile time choice that enables the coroutine local context
-    'HAVE_CONTEXTVAR'
+    'Inexact',
+    'InvalidContext',
+    'InvalidOperation',
+    'MAX_EMAX',
+    # Limits for the C version for compatibility
+    'MAX_PREC',
+    'MIN_EMIN',
+    'MIN_ETINY',
+    'Overflow',
+    'ROUND_05UP',
+    'ROUND_CEILING',
+    # Constants for use in setting up contexts
+    'ROUND_DOWN',
+    'ROUND_FLOOR',
+    'ROUND_HALF_DOWN',
+    'ROUND_HALF_EVEN',
+    'ROUND_HALF_UP',
+    'ROUND_UP',
+    'Rounded',
+    'Subnormal',
+    'Underflow',
+    'getcontext',
+    'localcontext',
+    # Functions for manipulating contexts
+    'setcontext',
 ]
```

</details>

It's all beautifully alphabetical, and the comments are all preserved, but it's probably not what the original author of that code wanted to happen!

---

_@AlexWaygood reviewed on 2024-01-11 22:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:347 on 2024-01-11 22:56_

Yeah, at this stage we're not trying to decide which elements in `__all__` "own" which comments, we're just trying to determine "on line 0 there's a comment"; "on line 1 there's two elements and a comment"; "on line 2 there's an element and no comment", etc.

Later, when we pass the list of `DunderAllLine`s to `collect_dunder_all_items()`, we try to determine which elements in `__all__` "own" which comments (meaning the comments should move with the element when the element moves as part of being sorted).

---

_Comment by @charliermarsh on 2024-01-11 23:04_

Oh I see, so we're not preserving the "sections" that they've created in their comments -- got it. Agree we could figure out some better solution there (but doesn't need to be in this PR).

---

_@charliermarsh reviewed on 2024-01-11 23:05_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:347 on 2024-01-11 23:05_

For clarity, I'm wondering if we should use an enum to represent "line with items, and optionally an end-of-line comment" vs. "own-line comment"? It took me a bit of time to get there in understanding that the struct was being repurposed in this way.

---

_@BurntSushi reviewed on 2024-01-11 23:45_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-11 23:45_

Yeah that's the evil thing about `AsRef::as_ref()`. It works, and then you upgrade Rust, and maybe it breaks. We haven't gotten one in a while, but there have been lots of reports of breakage in the past because std added a trait impl somewhere.

(This is an instance of a more general problem where adding trait impls can cause inference to fail where it previously succeeded. `AsRef` just happens to be one particular instance of it.)

With all that said, the worst thing that happens is our code will stop compiling when a new version of Rust is released. And a fix is (guaranteed actually) to be simple, so long as you diagnose it correctly. You can never get something wrong. But... these things stand out to me because I've been bitten by it myself and users reporting breakage to me have been bitten by it.

---

_@BurntSushi reviewed on 2024-01-11 23:46_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:208 on 2024-01-11 23:46_

I think if some part of this comment made its way into the source code as a comment, that would be great. :-) Thank you for digging in!

---

_@BurntSushi reviewed on 2024-01-11 23:57_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:385 on 2024-01-11 23:57_

The terminology here is that irrefutable patterns are accepted wherever names are assigned. This even includes function parameters!

---

_Comment by @MichaReiser on 2024-01-12 07:19_

> Oh I see, so we're not preserving the "sections" that they've created in their comments -- got it. Agree we could figure out some better solution there (but doesn't need to be in this PR).

Your example makes me wonder if we could use empty lines to recognize sections (regardless if there's a comment or not). 

Doing so has the added benefit that we preserve empty lines between imports (which is probably something that was intended). 

But we can explore this (and other solutions) as a separate PR

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:56 on 2024-01-12 07:28_

This is an intereseting use of unsafe, but it fits `unsafe`'s definition.

> The fix may be what the user intended, but it is uncertain; the resulting code will have valid syntax.

I would probably keep the fix as safe if we feel like it does the right thing in the majority of cases even if there are comments. But I'm also okay going wiht unsafe (can also be easily changed later). Interested to hear what other people think.

---

_@MichaReiser reviewed on 2024-01-12 07:28_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:69 on 2024-01-12 07:31_

What do you think of using [natural ordering](https://en.wikipedia.org/wiki/Natural_sort_order) instead of strict alphabetic ordering (see [natord](https://docs.rs/natord/latest/natord/))?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:127 on 2024-01-12 07:39_

This is an extreme edge case which may no be worth handling. But I was wondering what happens if `__all__` is e.g an imported member (I also don't know what Python does in that case)

```python
from sub import __all__

__all__ = ["evil"]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:56 on 2024-01-12 07:41_

I saw that @charliermarsh commented the same below. I'm resolving this to keep the conversation in a single place.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:177 on 2024-01-12 07:47_

Let's move this check to the bottom to avoid computing it in case we perform an early return. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:215 on 2024-01-12 07:49_

Nit: Some empty lines between the different steps might help with readability
```suggestion
        };
        
        // An `__all__` definition with <2 elements can't be unsorted;
        // no point in proceeding any further here
        if elts.len() < 2 {
            return None;
        }

        for elt in elts {
            // Only consider sorting it if __all__ only has strings in it
            let string_literal = elt.as_string_literal_expr()?;
            // And if any strings are implicitly concatenated, don't bother
            if string_literal.value.is_implicit_concatenated() {
                return None;
            }
        }
        
        // Step (2): parse the `__all__` definition using the raw tokens.
        //
        // (2a). Start by collecting information on each line individually:
        let lines = collect_dunder_all_lines(*range, locator)?;

        // (2b). Group lines together into sortable "items":
        //   - Any "item" contains a single element of the `__all__` list/tuple
        //   - "Items" are ordered according to the element they contain
        //   - Assume that any comments on their own line are meant to be grouped
        //     with the element immediately below them: if the element moves,
        //     the comments above the element move with it.
        //   - The same goes for any comments on the same line as an element:
        //     if the element moves, the comment moves with it.
        let items = collect_dunder_all_items(&lines);

        Some(DunderAllValue {
            items,
            range: *range,
            multiline: is_multiline,
        })
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:182 on 2024-01-12 07:49_

super nit
```suggestion
        // An `__all__` definition with < 2 elements can't be unsorted;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:317 on 2024-01-12 07:55_

You may want to consider using `lex_starts_at` instead of `lex`. `lex_starts_at` gives you absolute text ranges instead of relative text ranges (It should help to get rid of the `offset + range` in `DunerAllLine::new`). I recommend adding a comment to `collect_duner_all_lines` that explains that the ranges in `DunderAllLines` are relative if there's a reason for them to be relative.

---

_@AlexWaygood reviewed on 2024-01-12 07:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:334 on 2024-01-12 07:59_

I get this warning if I try to do that:

```
value assigned to `comment_in_line` is never read
maybe it is overwritten before being read?
`#[warn(unused_assignments)]` on by default
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:344 on 2024-01-12 08:00_

You might be able to avoid cloning the value inside of `DunderAllLine` by using `std::mem::take`. `std::mem::take` *takes* the value from `items_in_line` and replaces it with the `Default` (an empty list in that case). 
```suggestion
                    lines.push(DunderAllLine::new(
                        std::mem::take(&mut items_in_line),
                        comment_in_line,
                        range.start(),
                    ));
                    comment_in_line = None;
```

---

_@AlexWaygood reviewed on 2024-01-12 08:02_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:347 on 2024-01-12 08:02_

> For clarity, I'm wondering if we should use an enum to represent "line with items, and optionally an end-of-line comment" vs. "own-line comment"? It took me a bit of time to get there in understanding that the struct was being repurposed in this way.

That's a really great suggestion -- tried it out, and it makes the code much clearer. Will push shortly.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:233 on 2024-01-12 08:06_

You can use `self.items.is_sorted()` to test if the items are sorted (and avoids cloning all elements). This has the advantage that checking if the items are sorted is `O(n)` instead of `O(n log(n))` (sorting) + `O(n)` (comparing). The complexity remains the same if the items aren't sorted because you have to call `self.items.sort()` to sort the items but that should be less common for projects actively using the lint rule. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:243 on 2024-01-12 08:08_

Nit: One thing I like to do is to add a python snippet to give readers a concrete example:

```suggestion
        // As well as the "items" in the `__all__` definition,
        // there is also a "prelude" and a "postlude":
        //  - Prelude == the region of source code from the opening parenthesis
        //    (if there was one), up to the start of the first element in `__all__`.
        //  - Postlude == the region of source code from the end of the last
        //    element in `__all__` up to and including the closing parenthesis
        //    (if there was one).
        // 
        // ```python
        // __all__ = [
        //   "first item",
        //   "last item"   
        // ]
        // ```
        // The prelude in the above example is the text from right after the `[` up to `"first item"`. The postlude is the text between `"last item"` and `]`.
        let prelude_end = {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:234 on 2024-01-12 08:10_

Nit: You can avoid some of the `expect` messages below by doing

```rust
let [first, .., last] = self.items.as_slice() else {
    panic!("Expected at least two items")
};
```

---

_@AlexWaygood reviewed on 2024-01-12 08:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:403 on 2024-01-12 08:11_

done!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:257 on 2024-01-12 08:15_

Do I understand this correctly (without having read through all the code) that the fix inserts a trailing comma if the last item is on its own line? 

What about users that prefer not to have trailing commas? Should we instead detect if the source contains a trailing comma and preserve it?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:294 on 2024-01-12 08:22_

We can avoid allocating here because you're constructing a new string below anyway. Sorry, I'm very picky when it comes to allocations because they tend to be expensive (see https://vimeo.com/649009599)


```suggestion
        let mut prelude = Cow::Borrowed(locator.slice(TextRange::new(self.start(), prelude_end)));

        let joined_items = if self.multiline {
            let indentation = stylist.indentation();
            let newline = stylist.line_ending().as_str();
            prelude = Cow::Owned(format!("{}{}", prelude.trim_end(), newline));
            join_multiline_dunder_all_items(
                &sorted_items,
                locator,
                parent,
                indentation,
                newline,
                needs_trailing_comma,
            )
        } else {
            Some(join_singleline_dunder_all_items(&sorted_items, locator))
        };

        let postlude = locator.slice(TextRange::new(postlude_start, self.end()));

        let new_dunder_all = joined_items.map(|items| format!("{prelude}{items}{postlude}"));
        SortedDunderAll::Sorted(new_dunder_all)
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:431 on 2024-01-12 08:32_

We can avoid cloning `first_val` and `value` below by passing the owned `Vec<DunderAllLine>` to `collect_dunder_all_items` (instead of a slice) and use `into_iter` here that gives you owned values:


```suggestion
                let mut iter = items.into_iter();
                let Some((first_val, first_range)) = iter.next() else {
                    unreachable!(
                        "LineWithItems::new() should uphold the invariant that this list is always non-empty"
                    )
                };

                let rest = iter;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:421 on 2024-01-12 08:34_

`this_range` seems to be `None` except if it is a comment. How about renaming it `preceding_comment_range`?


What happens if an item has multiple preceding lines? Should this be changed to `this_range.get_or_insert(comment_range)` to only set the range when `this_range` is `None`?

```python
__all__ = [
    "test"
    # comment
    # another comment only line
    "item"
]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:416 on 2024-01-12 08:38_

I think a good approximation of the number of `items` is to use the number of lines if it's a multiline value and the number of items in the first line otherwise.. Initializing items with a specific capacity can help to reduce the number of times the vector needs to be resized.
```suggestion
    let mut all_items = Vec::with_capacity(match lines.as_slice() {
        [DunderAllLine::OneOrMoreItems(single)] => single.items.len(),
        _ => lines.len(),
    });
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:741 on 2024-01-12 08:40_

What happens with `JustAComment` if it is the last line? 

```python
__all__ = [
    'test',
    # comment
]
```

I'm asking because `this_range` then never gets added to all items. Will it be part of the prelude or is there an invariant that prevents that `LineWithJustAComment` can be the last line?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:434 on 2024-01-12 08:41_

A comment here might be helpful explaining why we're extending the range.

*Include the range of any preceding comment in the range of the item to ensure the comment gets moved with the item*.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:449 on 2024-01-12 08:43_

We can use `Vec::extend` here to let `Vec` know that we intend to push multiple items. It can help `Vec` to regrow the vector only once


```suggestion
                all_items.extend(rest.map(|(value, range)| DunderAllItem {
                    value,
                    original_index: all_items.len(),
                    range,
                    additional_comments: None,
                }));
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:484 on 2024-01-12 08:47_

Can you explain (or add a comment) why `sort_index` is needed. My guess is that it is to preserve the original order, but that's something that Rust's `sort` method should already give us for free.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:523 on 2024-01-12 08:49_

Are `additional_comments` always comments on the same line as the item? If so, can we rename it to `end_of_line_comments` or `trailing_comments` to match the naming convention we use in the formatter. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:524 on 2024-01-12 08:50_

This might be a bit opinionated for some :laughing:. Any chance we can preserve the original spacing?

---

_@MichaReiser reviewed on 2024-01-12 08:50_

---

_@AlexWaygood reviewed on 2024-01-12 11:37_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:449 on 2024-01-12 11:37_

(For posterity: we tried this out in a pair-programming session, but the borrow checker was very angry, so we reverted it)

---

_@AlexWaygood reviewed on 2024-01-12 11:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:257 on 2024-01-12 11:39_

> Do I understand this correctly (without having read through all the code) that the fix inserts a trailing comma if the last item is on its own line?

yes, that's correct

> What about users that prefer not to have trailing commas? Should we instead detect if the source contains a trailing comma and preserve it?

good idea; I'll try to implement that

---

_@AlexWaygood reviewed on 2024-01-12 12:03_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:368 on 2024-01-12 12:03_

@MichaReiser and I vanquished all uses of `.clone()` in a pair-programming session; this region of code no longer exists :)

---

_@AlexWaygood reviewed on 2024-01-12 12:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:127 on 2024-01-12 12:05_

I think the redefinition here overrides the import at the top (or, if it's an import below the assignment, the import would override the assignment). Python just uses the runtime value of `__all__`; it doesn't attempt to read the value statically in any way (though some type checkers _do_ do that). So I don't _think_ this edge case should have any impact on the way this rule works

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:741 on 2024-01-12 12:15_

Any comments after the last item are discarded here, but are added back as part of the postlude in `into_sorted_source_code()` (which used to be called `construct_sorted_all()`; I just renamed it)

---

_@AlexWaygood reviewed on 2024-01-12 12:15_

---

_@AlexWaygood reviewed on 2024-01-12 12:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:484 on 2024-01-12 12:19_

yup, my worry here was about the possibility of an `__all__` that had duplicate items in it. I wanted to ensure that in the eventuality where two items in the list compared equal, the item that was originally first in the list remained first in the list. That invariant will hold even if I just use `self.value` as the key for `.sort()`?

---

_Comment by @AlexWaygood on 2024-01-12 13:25_

I discovered a small bug relating to multiline comments -- working on a fix...

(Edit: fixed in fd1dc54!)

---

_@konstin reviewed on 2024-01-12 13:37_

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:127 on 2024-01-12 13:37_

Looks like cpython does indeed not special case `__all__` at all: https://github.com/search?q=repo%3Apython%2Fcpython+__all__+language%3AC+&type=code

---

_@AlexWaygood reviewed on 2024-01-12 13:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:243 on 2024-01-12 13:54_

I added a variant of this in 5e129ade8e77090a2aa1017611258b2c2f65d942. (By the way: writing that up was what made me spot the bug regarding multiline comments that I fixed in fd1dc54c80004d1b2377ca93ad30718d83e77587!)

---

_@AlexWaygood reviewed on 2024-01-12 13:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:69 on 2024-01-12 13:55_

I like that idea!

---

_@AlexWaygood reviewed on 2024-01-12 14:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:69 on 2024-01-12 14:53_

Implemented in 382aed21239afd40b378600b3f80a01cd33e3178!

---

_@AlexWaygood reviewed on 2024-01-12 16:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:257 on 2024-01-12 16:12_

Tweaked it in 6f5f55843c7bfc3ea3f7a336cacfceb33eeeb9c3: we now do not add a trailing comma if there wasn't one in the original source!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:524 on 2024-01-12 16:53_

Ew @ anybody who doesn't put two spaces before an inline comment, but: see 655bf67ae3459da6b0bec34ab959d56cd4d26bfe :)

---

_@AlexWaygood reviewed on 2024-01-12 16:53_

---

_@AlexWaygood reviewed on 2024-01-12 16:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:523 on 2024-01-12 16:58_

9e2bae25679168aa15178f622366dee2481e960c

---

_@AlexWaygood reviewed on 2024-01-12 17:02_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:434 on 2024-01-12 17:02_

af6ca0556fe8c3906bd5e52b85f0129a3e41fef0

---

_@AlexWaygood reviewed on 2024-01-12 17:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:294 on 2024-01-12 17:45_

Should `postlude` also be a `Cow`, or just `prelude`?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:421 on 2024-01-12 17:46_

> How about renaming it `preceding_comment_range`?

Sounds good



> What happens if an item has multiple preceding lines? Should this be changed to `this_range.get_or_insert(comment_range)` to only set the range when `this_range` is `None`?

Ah, I noticed this and fixed it before seeing this review comment 😄 see fd1dc54c80004d1b2377ca93ad30718d83e77587

---

_@AlexWaygood reviewed on 2024-01-12 17:46_

---

_@AlexWaygood reviewed on 2024-01-12 18:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:265 on 2024-01-12 18:34_

Added some more docs on this in my latest push, so I'll mark this as "resolved" for now, but still open to better naming suggestions!

---

_@AlexWaygood reviewed on 2024-01-12 18:36_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:325 on 2024-01-12 18:36_

Added a big comment clarifying this in 572bb66546b9d0307b0de8e1e576041890410396 :)

---

_@AlexWaygood reviewed on 2024-01-12 18:41_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:741 on 2024-01-12 18:41_

Added some more docs about this; is this clearer now?

---

_@AlexWaygood reviewed on 2024-01-12 18:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-12 18:43_

I had a bit of trouble locally getting it to work without `.as_ref()` :(

@burntsushi, would you mind pasting a diff of what you mean here? Or feel free to push straight to my branch if you prefer, whichever's easiest :)

---

_@AlexWaygood reviewed on 2024-01-12 18:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:177 on 2024-01-12 18:51_

Thanks, addressed in 768a3648636e6abbff2ed3373e686c2678b38dc1!

---

_Comment by @AlexWaygood on 2024-01-12 18:53_

Thanks all for the detailed feedback, really appreciate it! I _think_ I've now either addressed or asked clarification on all comments, but sorry if I missed something :)

---

_Review requested from @BurntSushi by @AlexWaygood on 2024-01-12 18:53_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-01-12 18:53_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-01-12 18:53_

---

_@BurntSushi reviewed on 2024-01-12 19:13_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-12 19:13_

Yeah! So it looks like there are only two `.as_ref()` calls left here. This diff is the simplest one that removes them:

```
[andrew@duff ruff]$ g diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
index d42249f38..227454cea 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
@@ -101,7 +101,7 @@ pub(crate) fn sort_dunder_all_aug_assign(
     else {
         return;
     };
-    let ast::Expr::Name(ast::ExprName { id, .. }) = target.as_ref() else {
+    let ast::Expr::Name(ast::ExprName { id, .. }) = &**target else {
         return;
     };
     sort_dunder_all(checker, id, value, parent);
@@ -122,7 +122,7 @@ pub(crate) fn sort_dunder_all_ann_assign(
     else {
         return;
     };
-    let ast::Expr::Name(ast::ExprName { id, .. }) = target.as_ref() else {
+    let ast::Expr::Name(ast::ExprName { id, .. }) = &**target else {
         return;
     };
     sort_dunder_all(checker, id, val, parent);
```

The key here is noticing that `target` has type `&Box<Expr>`. Because you can't pattern match through a `Box` (a limitation that everyone would like to lift, but is I guess difficult to do so), and since you want to borrow from the AST, you want a `&Expr` here. To go from a `&Box<Expr>`, you can dereference twice to get an `Expr` (temporarily), and then add a `&` to get a `&Expr`.

Now for me personally, this is not how I would write this. The only reason why it works is something called [match ergonomics](https://rust-lang.github.io/rfcs/2005-match-ergonomics.html) that essentially tries to remove the need to explicitly mark whether a pattern match is borrowed or not. I think match ergonomics makes code like this less clear because it's harder to tell at a glance what `target` is just from reading the source code.

This is how I would write it:

```
[andrew@duff ruff]$ g diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
index d42249f38..ed3ee0340 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
@@ -93,15 +93,15 @@ pub(crate) fn sort_dunder_all_aug_assign(
     parent: &ast::Stmt,
 ) {
     let ast::StmtAugAssign {
-        value,
-        target,
+        ref value,
+        ref target,
         op: ast::Operator::Add,
         ..
-    } = node
+    } = *node
     else {
         return;
     };
-    let ast::Expr::Name(ast::ExprName { id, .. }) = target.as_ref() else {
+    let ast::Expr::Name(ast::ExprName { ref id, .. }) = **target else {
         return;
     };
     sort_dunder_all(checker, id, value, parent);
@@ -115,14 +115,14 @@ pub(crate) fn sort_dunder_all_ann_assign(
     parent: &ast::Stmt,
 ) {
     let ast::StmtAnnAssign {
-        target,
-        value: Some(val),
+        ref target,
+        value: Some(ref val),
         ..
     } = node
     else {
         return;
     };
-    let ast::Expr::Name(ast::ExprName { id, .. }) = target.as_ref() else {
+    let ast::Expr::Name(ast::ExprName { ref id, .. }) = **target else {
         return;
     };
     sort_dunder_all(checker, id, val, parent);
```

Now you still need the `**target`, but the fact that you need it is now (maybe) a little clearer. That match on `ref target` tells you that `target` _has_ to be a `&SOMETHING`.

---

_@AlexWaygood reviewed on 2024-01-12 19:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-12 19:34_

Thanks so much! I applied your suggestion in https://github.com/astral-sh/ruff/pull/9474/commits/229fec17edf6aa1b1ed2c2a365b9e81e740908db.

I _think_ I understand the solution, but will try to read up a bit more this weekend to check I fully understand what's going on here :)

---

_@BurntSushi reviewed on 2024-01-12 19:38_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:83 on 2024-01-12 19:38_

Yeah it will take some getting used to. There's a reason why match ergonomics exists even if I'm no fan. :-)

---

_@AlexWaygood reviewed on 2024-01-12 21:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:157 on 2024-01-12 21:35_

I feel like as a _user_ of ruff I'd probably be a little annoyed if ruff did something like https://github.com/astral-sh/ruff/pull/9474#issuecomment-1888091353 to my carefully designed `__all__` definition, and then claimed it was a "safe" fix 😄 but I'll defer to you and Zanie :)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:294 on 2024-01-13 14:08_

Having done a bit more reading, I can answer my own question: no, `postlude` doesn't need to be a `Cow`, since the type of `postlude` currently is already a borrowed type (`&str`), which is what we want, but the type of `prelude` currently is an owned type (`String`), which is what we don't want, and what using `Cow` fixes for us.

Thanks!

---

_@AlexWaygood reviewed on 2024-01-13 14:08_

---

_@AlexWaygood reviewed on 2024-01-13 14:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:294 on 2024-01-13 14:19_

bec4a1b714f53fce67b98acaae7b922934537cc2

---

_Comment by @AlexWaygood on 2024-01-13 18:01_

Recent changes:
- Lists/tuples passed to `__all__.extend()` are now sorted in the same way as the rhs of statements such as `__all__ = ...`, `__all__ += ...` and `__all__: list[str] = ...`
- Fixed a bunch of formatting edge cases I spotted while leafing through the diff this PR branch produced when running ruff with `--fix --unsafe-fixes --preview --select=RUF022` on CPython. Here's the new diff on CPython: [cpython_diff.txt](https://github.com/astral-sh/ruff/files/13929051/cpython_diff.txt)
- Switched to an "isort-style" sort instead of a natural sort: SCREAMING_SNAKE_CASE members come first, then CamelCase members, and then anything else after that. Within each category, items are ordered according to a natural sort.


---

_@MichaReiser reviewed on 2024-01-15 07:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:157 on 2024-01-15 07:10_

That's a fair point. We would likely need to mark all fixes as unsafe following that reasoning because:
* some fixes don't handle comments at all
* Many fixes don't produce nicely formatted code. You could use the same argument that we should use unsafe because ruff could mess up my carefully hand-formatted code. 

Let's see what @zanieb thinks 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:714 on 2024-01-15 07:59_

Can we explain what the index is used for instead of documenting what the code below the comment does (adding an orignal_index to each item)? Explaining the why will be valuable for future readers whereas documenting what the code below means only risks to get outdated.

What I find a very good and inspiring talk about commenets: https://youtu.be/yhF7OmuIILc?si=naBTAvTxKwhQ5aaC

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:765 on 2024-01-15 08:15_

Only comparing the original index can be a nice perf improvement but your `PartialEq` and `Ord` implementation now act inconsistently where for a specific `a` and `b` `a.cmp(b) == Ordering::Eq` is true but `a.eq(b)` might return false.

All tests keep passing when I replace the implementation with 

```
impl PartialEq for DunderAllItem {
    fn eq(&self, other: &Self) -> bool {
        self.cmp(other) == Ordering::Equal
    }
}
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:714 on 2024-01-15 08:15_

I can remove the `origianl_index` field and all tests keep passing (after removing all references). Can you add tests showing why `original_index` is necessary or remove it instead? That would allow you to use `extend` in `for (value, range) in following_items { ..`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:434 on 2024-01-15 08:24_

We managed to remove all allocations except that `lex_starts_at` allocates a new string for each `__all__` item (which predominently are strings) and the `lines` and `items` vectors. We are lucky, that only few files have `__all__` usages and most of them have exactly one, or this method would show up in the benchmarks more often. 

Nevertheless, it made me question if we can avoid calling into `lex_starts_at` in the common case where a fix isn't necessary. Would it be possible to split the rule into two phases:

1. An AST based check phase that determines if the items are sorted soley by inspecting the AST (and, in the future, by maybe calling into the `SimpleTokenizer` to detect the number of empty lines between any two items). 
2. A fix phase where we use the full machinery that calls into the lexer to extract the precise comment and item positions necessary to build a fix. 

This would have the added benefit that the fix detection works for edge cases that we aren't able to fix (e.g. for parenthesized-items).


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:533 on 2024-01-15 08:28_

Nit: I would expect a `receive` function to read or at least return some data because I read `x.receive_` as `x` go and receive some content. But instead, it is more of a `x` take this, which is why I would go with either `push` (if it is ok to call the function multiple times), `extend`, or `set` if it overrides the previous value.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:508 on 2024-01-15 08:36_

i think it would help here to document the different possible states this struct represents or would it even be possible to model this as an enum that enforces (and documents) the states. 

I'm happy to explore possible representations once I fully understnad the possible states. What I don't understand yet:

1. Is `comment_range` always trailing own line comment (coming after the items)? I get the impression it must be because `receive_string_token` always sets the `comment_range_start` to the end of its token.
2. What happens if an item has both leading and trailing own line comments? `# comment\n "item"\n # comment 2`? How is this represented?
3. Does `comment_in_line` represent a comment that's on the same line as an item. If so, I recommend renaming it to `trailing_same_line_comment_range` to make that explicit. 
 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:416 on 2024-01-15 08:37_


```suggestion
/// or if it's an edge case we don't support.
```

That's less judgmental ;) 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:468 on 2024-01-15 08:47_

The open parentheses handling might be easier to understand if we move it out of the loop. 


```
let mut lexer = lexer::lex_starts_at(locator::slice(range), Mode::Expression, range.start()).peekable();

// Skip over the opening parentheses of the `__all__` list or tuple (if any).
let parentheses_open = if matches!(lexer.peek(), Some(Ok((Tok::Lpar | Tok::Lsqb, _))) {
    lexer.next();
    true
} else {
    false
};


for pair in lexer {
    ...
}
```

Although this is still not entirely correct if you have 

```python
__all__ = ("test"), another
```

Because both implementations would assume that the `(` belongs to the `__all__` item. 

Unfortunately, detecting if a tuple is parenthesized isn't straightfoward. See https://github.com/astral-sh/ruff/blob/c7aa816f17e3c6d59f35deb17ea7f253f5c024c0/crates/ruff_python_formatter/src/expression/expr_tuple.rs#L228-L254

How common are unparenthesized tuples for `__all__`? I thought `__all__` is recommended to be a list. Maybe it's not worth supporting fixing tuples? 

That would simply the logic and we could even assert that the first token is a `[` (because anything else is a programming error)


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:741 on 2024-01-15 09:01_

Thanks. That helps

---

_@MichaReiser requested changes on 2024-01-15 09:06_

Overall this looks good to me, but there are a few clarification that are needed for me to understand the code and:

* We should align the `PartialEq` and `Ord` implementation of `AllDunderItems`
* Let's add some tests around unparenthesized tuples that start with a parenthesized first item (see inline comment). I suspect that things might go wrong in that case


It would be interesting to explore if we can implement the check as an AST rule and only use lexing for generating the fix. I'll leave that up to you if you want to tackle this and if so, if you want to do it as part of this PR.

---

_@AlexWaygood reviewed on 2024-01-15 09:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:468 on 2024-01-15 09:17_

> How common are unparenthesized tuples for `__all__`? I thought `__all__` is recommended to be a list. Maybe it's not worth supporting fixing tuples?

Tuple `__all__`s are very common; I'd definitely rather support them here. I don't think there's a recommendation w.r.t. whether they should be tuples or lists -- I'm not actually sure which is more common. Conceptually, I sort-of prefer using a tuple for `__all__`, personally. But I think that's just a vague instinct towards preferring immutable types for global constants, rather than something that's founded on any strong reasoning :)

Using an _unparenthesized_ tuple for `__all__` is less common, but not unheard of:

- https://github.com/python/cpython/blob/f8a79109d0c4f408d34d51861cc0a7c447f46d70/Lib/asyncio/base_events.py#L52
- https://github.com/python/cpython/blob/f8a79109d0c4f408d34d51861cc0a7c447f46d70/Lib/asyncio/coroutines.py#L1
- https://github.com/python/cpython/blob/f8a79109d0c4f408d34d51861cc0a7c447f46d70/Lib/asyncio/windows_utils.py#L17

---

_@MichaReiser reviewed on 2024-01-15 09:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:468 on 2024-01-15 09:26_

What happened to 

> There should be one-- and preferably only one --obvious way to do it.



---

_@AlexWaygood reviewed on 2024-01-15 09:27_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:468 on 2024-01-15 09:27_

> What happened to
> 
> > There should be one-- and preferably only one --obvious way to do it.

many people are saying this

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:533 on 2024-01-15 09:29_

I wondered about using `visit_*` as names for these methods -- thoughts on that?

---

_@AlexWaygood reviewed on 2024-01-15 09:29_

---

_@MichaReiser reviewed on 2024-01-15 09:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:533 on 2024-01-15 09:31_

That would work for me.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:714 on 2024-01-15 10:18_

I think `original_index` can be removed. I added it out of paranoia that duplicate elements in `__all__` might be unnecessarily reordered by this autofix. But `.sort()` is a stable sort, so I don't think there's actually any chance of that happening.

In any case, having duplicate elements in `__all__` is likely not what the user wanted, so this is an edge case I probably shouldn't be worrying _too_ much about.

But I'll add a test, anyway! :D

---

_@AlexWaygood reviewed on 2024-01-15 10:18_

---

_@AlexWaygood reviewed on 2024-01-15 12:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:468 on 2024-01-15 12:50_

Well, I'd quite like to use that `is_tuple_parenthesized` function you've got there in the formatter... what's the best course of action there? Copy-and-paste it into `sort_dunder_all.rs`, or move it to `ruff_python_trivia` and import it from that crate in both the formatter and `sort_dunder_all.rs`?

---

_@MichaReiser reviewed on 2024-01-15 14:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:468 on 2024-01-15 14:15_

You could add it to `ruff_python_ast` and either implement it as a free-standing function or as a method on `ExprTuple`.

---

_Comment by @AlexWaygood on 2024-01-15 17:19_

Okay, just pushed some major updates:
- The _check_ is now implemented solely using the AST (but we still look at the raw tokens for the _fix_)
- We now _barely_ look at the raw tokens at all for fixing single-line `__all__` definitions.
- The machinery for multiline `__all__` definitions is much as it was (processes all the raw tokens), but I've tried to add more clarity in the code structure and comments.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:254 on 2024-01-15 17:22_

This obviously isn't ideal :(

I wanted to make this struct generic over a lifetime, i.e.

```rs
struct AllItemSortKey<'a> {
    category: InferredMemberType,
    value: &'a str,
}
```

But had trouble making it work.

---

_@AlexWaygood reviewed on 2024-01-15 17:23_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:508 on 2024-01-15 17:27_

I pushed a bunch more comments in my latest commits. But the key point here is that at this stage, we're building up state that represents information on a single source line in a multiline `__all__` definition. We're _not_ building up state that represents a single "item" in a multiline `__all__` definition (we do that later).

Across the rule as a whole, a multiline `__all__` definition is parsed in two stages:

1. We analyse each line of source code on its own, accumulating state until we reach the end of the line. This struct is basically a "bag of state" that collects that state until we get to the end of the line. Once we reach the end of the line, we use that state to produce a variant of the `DunderAllLine` enum (via `into_dunder_all_line()`).
2. Having constructed a vector of `DunderAllLine` variants, we then convert that vector into a vector of `DunderAllItem` variants.

---

_@AlexWaygood reviewed on 2024-01-15 17:27_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-01-15 17:29_

---

_Comment by @AlexWaygood on 2024-01-15 18:24_

Here's the latest diff when running this autofix on CPython, FWIW: [cpython_diff.txt](https://github.com/astral-sh/ruff/files/13942003/cpython_diff.txt)


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:159 on 2024-01-16 08:49_

We should initialize `vec` with a length of `elts` because that's how many items we normally add:


```suggestion
    let mut string_items = Vec::with_capacity(elts.len());
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:184 on 2024-01-16 09:36_

It's hard to tell if it's worth it (probably not because `__all__` items are so uncommon). But we could avoid allocating a new `Vec` when the items are already sorted.


```suggestion
    let Some(unsorted) = UnsortedDunderAllItems::from_elements(elts) else {
        return;
    };

    let mut diagnostic = Diagnostic::new(UnsortedDunderAll, range);

    if !unsorted.implicit_concatenated {
        if let Some(fix) = get_fix(range, elts, &unsorted.items, &kind, checker) {
            diagnostic.set_fix(fix);
        }
    }

    checker.diagnostics.push(diagnostic);
}

struct UnsortedDunderAllItems<'a> {
    items: Vec<&'a str>,
    implicit_concatenated: bool,
}

impl<'a> UnsortedDunderAllItems<'a> {
    fn from_elements(elements: &'a [Expr]) -> Option<Self> {
        let Some((first, rest)) = elements.split_first() else {
            return None;
        };

        if rest.is_empty() {
            return None;
        }

        let mut previous = first.as_string_literal_expr()?.value.to_str();

        for element in rest {
            let current = element.as_string_literal_expr()?.value.to_str();

            if AllItemSortKey::from(current) < AllItemSortKey::from(previous) {
                let mut implicit_concatenated = false;

                let mut items = Vec::with_capacity(elements.len());

                for element in elements {
                    let string_literal = element.as_string_literal_expr()?;
                    implicit_concatenated |= string_literal.value.is_implicit_concatenated();
                    items.push(string_literal.value.to_str());
                }

                return Some(Self {
                    items,
                    implicit_concatenated,
                });
            }

            previous = current;
        }

        // All Sorted
        None
    }
}

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:178 on 2024-01-16 09:36_

Nit: `create_fix`: Get is most often used to simply read a value (e.g. a field)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:287 on 2024-01-16 09:40_

Nit
```suggestion
        } else if value.starts_with(char::is_uppercase) {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:437 on 2024-01-16 09:46_

Nit: We could consider to store `AllItemSortyKey` on the `DundarAllItem` if we're concerned about the cost. Although I don't think it's necessary because:

* This code path is only exercised for fixes, it's okay if they're somewhat slower
* We can change `AllItemSortKey` to us a `&str` to avoid cloning. The complexity then becomes `O(n)` but only for all capitalized strings.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:227 on 2024-01-16 09:47_

We can avoid cloning here by using `self.items.sort_by` when sorting (I assume that's where you ran into lifetime issues)

```patch
Index: crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
--- a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs	(revision d78e931ef1c7cf71946a4ce63ea5669aa2b27dac)
+++ b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs	(date 1705398024594)
-struct AllItemSortKey {
+struct AllItemSortKey<'a> {
     category: InferredMemberType,
-    value: String,
+    value: &'a str,
 }
 
-impl Ord for AllItemSortKey {
+impl Ord for AllItemSortKey<'_> {
     fn cmp(&self, other: &Self) -> Ordering {
         self.category
             .cmp(&other.category)
@@ -233,31 +249,31 @@
     }
 }
 
-impl PartialOrd for AllItemSortKey {
+impl PartialOrd for AllItemSortKey<'_> {
     fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
         Some(self.cmp(other))
     }
 }
 
-impl PartialEq for AllItemSortKey {
+impl PartialEq for AllItemSortKey<'_> {
     fn eq(&self, other: &Self) -> bool {
         self.cmp(other) == Ordering::Equal
     }
 }
 
-impl Eq for AllItemSortKey {}
+impl Eq for AllItemSortKey<'_> {}
 
-impl From<&str> for AllItemSortKey {
-    fn from(value: &str) -> Self {
+impl<'a> From<&'a str> for AllItemSortKey<'a> {
+    fn from(value: &'a str) -> Self {
         Self {
             category: InferredMemberType::of(value),
-            value: String::from(value),
+            value,
         }
     }
 }
 
-impl From<&DunderAllItem> for AllItemSortKey {
-    fn from(item: &DunderAllItem) -> Self {
+impl<'a> From<&'a DunderAllItem> for AllItemSortKey<'a> {
+    fn from(item: &'a DunderAllItem) -> Self {
         Self::from(item.value.as_str())
     }
 }
@@ -434,7 +450,7 @@
         );
 
         self.items
-            .sort_by_cached_key(|item| AllItemSortKey::from(item));
+            .sort_by(|a, b| AllItemSortKey::from(a).cmp(&AllItemSortKey::from(b)));
         let joined_items = join_multiline_dunder_all_items(
             &self.items,
             locator,

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:848 on 2024-01-16 09:54_

The `newline` returned by `Stylist` is the newline used for fixing, but it may or may not be the newline used in the `postlude`. For example, `Stylist` detects `\n` for a file with mixed line breaks if the first line uses `\n` but `postlude` could use `\r\n`, in which case this path incorrectly assumes that the postlude doesn't start with a newline. 

I don't understnad the code well enough to understand if that's a problem. If it is, changing it to `!postlude.starts_with(['\n', '\r'])` might be enough.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:867 on 2024-01-16 09:56_

Nit: Using `sort_by` is probably sufficient now that we can avoid allocating in `AllItemSortKey`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:803 on 2024-01-16 09:57_

Nit: Rename to `last_item_index`. It holds slightly more information, because it gives meaning to what item the item with the `max` index points to.

---

_@MichaReiser approved on 2024-01-16 09:58_

Overall this looks good to me, but there are a few clarification that are needed for me to understand the code and:

* We should align the `PartialEq` and `Ord` implementation of `AllDunderItems`
* Let's add some tests around unparenthesized tuples that start with a parenthesized first item (see inline comment). I suspect that things might go wrong in that case


It would be interesting to explore if we can implement the check as an AST rule and only use lexing for generating the fix. I'll leave that up to you if you want to tackle this and if so, if you want to do it as part of this PR.

---

_@MichaReiser approved on 2024-01-16 09:59_

Nice improvements. I've a few suggestions but this looks good to me. 

---

_@AlexWaygood reviewed on 2024-01-16 10:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:287 on 2024-01-16 10:06_

Ah, this logic was purely copied-and-pasted from the `isort` rules 😄

---

_@AlexWaygood reviewed on 2024-01-16 10:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:227 on 2024-01-16 10:15_

> We can avoid cloning here by using `self.items.sort_by` when sorting (I assume that's where you ran into lifetime issues)

That is indeed where I ran into lifetime issues, yup! TYVM

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:848 on 2024-01-16 11:07_

Thanks, good call!

---

_@AlexWaygood reviewed on 2024-01-16 11:07_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:157 on 2024-01-16 11:32_

I've pushed 24616f3a55e18640c92b548177842ba65021bd9c to mark the fix as always being safe, but I've kept a note in the docs stating that it might sometimes move comments to unexpected locations

---

_@AlexWaygood reviewed on 2024-01-16 11:32_

---

_@MichaReiser reviewed on 2024-01-16 13:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:229 on 2024-01-16 13:47_

Nit: Maybe `Unsupported`. It's a valid `__all__` expression, but not one the fix supports

---

_@AlexWaygood reviewed on 2024-01-16 13:57_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:229 on 2024-01-16 13:57_

Hmm, maybe `Unsortable` or `NotAStringLiteralList` would be best. We return this variant for truly invalid things like

```py
__all__ = [1, 2, 3]
```

 But we also return this variant for things that might or might not be valid, like

```py
foo = "whatever"
__all__ = [foo]
```

The thing that these have in common is that they're beyond the scope of this rule (not just the fix), so, while they might or might not be valid `__all__` definitions, we don't emit a violation for them

---

_@AlexWaygood reviewed on 2024-01-16 14:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs`:229 on 2024-01-16 14:20_

I've gone with `NotAListOfStringLiterals`, which is slightly verbose but unambiguous

---

_Comment by @AlexWaygood on 2024-01-16 14:21_

Thanks all for the reviews!

---

_Merged by @AlexWaygood on 2024-01-16 14:42_

---

_Closed by @AlexWaygood on 2024-01-16 14:42_

---

_Branch deleted on 2024-01-16 14:42_

---
