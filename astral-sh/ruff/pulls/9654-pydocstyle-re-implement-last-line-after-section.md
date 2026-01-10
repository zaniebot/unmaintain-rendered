```yaml
number: 9654
title: "[`pydocstyle`] Re-implement `last-line-after-section` (`D413`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/D413
created_at: 2024-01-26T19:22:07Z
updated_at: 2024-01-26T20:08:54Z
url: https://github.com/astral-sh/ruff/pull/9654
synced_at: 2026-01-10T22:57:09Z
```

# [`pydocstyle`] Re-implement `last-line-after-section` (`D413`)

---

_Pull request opened by @charliermarsh on 2024-01-26 19:22_

## Summary

This rule was just incorrect, it didn't match the examples in the docs. (It's a very rarely-used rule since it's not included in any of the conventions.)

Closes https://github.com/astral-sh/ruff/issues/9452.


---

_Label `bug` added by @charliermarsh on 2024-01-26 19:22_

---

_Label `docstring` added by @charliermarsh on 2024-01-26 19:22_

---

_Merged by @charliermarsh on 2024-01-26 19:30_

---

_Closed by @charliermarsh on 2024-01-26 19:30_

---

_Branch deleted on 2024-01-26 19:31_

---

_Comment by @github-actions[bot] on 2024-01-26 19:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2706 -0 violations, +0 -0 fixes in 7 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+847 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L100'>disnake/abc.py:100:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1073'>disnake/abc.py:1073:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1153'>disnake/abc.py:1153:9:</a> D413 [*] Missing blank line after last section ("Raises")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L120'>disnake/abc.py:120:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1288'>disnake/abc.py:1288:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1377'>disnake/abc.py:1377:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1528'>disnake/abc.py:1528:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1803'>disnake/abc.py:1803:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L180'>disnake/abc.py:180:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1831'>disnake/abc.py:1831:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1865'>disnake/abc.py:1865:9:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1929'>disnake/abc.py:1929:5:</a> D413 [*] Missing blank line after last section ("Note")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1961'>disnake/abc.py:1961:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L228'>disnake/abc.py:228:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L530'>disnake/abc.py:530:9:</a> D413 [*] Missing blank line after last section ("Returns")
... 832 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/decorators/base.py#L131'>airflow/decorators/base.py:131:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D413 [*] Missing blank line after last section ("example")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/io/path.py#L254'>airflow/io/path.py:254:9:</a> D413 [*] Missing blank line after last section ("See Also")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/models/dag.py#L4055'>airflow/models/dag.py:4055:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/amazon/aws/operators/base_aws.py#L32'>airflow/providers/amazon/aws/operators/base_aws.py:32:5:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/amazon/aws/sensors/base_aws.py#L32'>airflow/providers/amazon/aws/sensors/base_aws.py:32:5:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/amazon/aws/utils/mixins.py#L65'>airflow/providers/amazon/aws/utils/mixins.py:65:9:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/apache/hive/hooks/hive.py#L809'>airflow/providers/apache/hive/hooks/hive.py:809:5:</a> D413 [*] Missing blank line after last section ("Notes")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/fab/auth_manager/security_manager/override.py#L1408'>airflow/providers/fab/auth_manager/security_manager/override.py:1408:9:</a> D413 [*] Missing blank line after last section ("NOTE")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/slack/hooks/slack.py#L61'>airflow/providers/slack/hooks/slack.py:61:5:</a> D413 [*] Missing blank line after last section ("Examples")
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+92 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L293'>src/bokeh/application/handlers/directory.py:293:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L199'>src/bokeh/application/handlers/handler.py:199:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/request_handler.py#L63'>src/bokeh/application/handlers/request_handler.py:63:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L160'>src/bokeh/core/property/bases.py:160:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/container.py#L247'>src/bokeh/core/property/container.py:247:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L215'>src/bokeh/core/property/dataspec.py:215:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L442'>src/bokeh/core/property/dataspec.py:442:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L7'>src/bokeh/driving.py:7:1:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/elements.py#L150'>src/bokeh/embed/elements.py:150:5:</a> D413 [*] Missing blank line after last section ("Returns")
... 82 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+494 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/__init__.py#L39'>ibis/__init__.py:39:5:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L100'>ibis/backends/base/__init__.py:100:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1063'>ibis/backends/base/__init__.py:1063:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1098'>ibis/backends/base/__init__.py:1098:9:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L110'>ibis/backends/base/__init__.py:110:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1114'>ibis/backends/base/__init__.py:1114:9:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1136'>ibis/backends/base/__init__.py:1136:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1160'>ibis/backends/base/__init__.py:1160:9:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1174'>ibis/backends/base/__init__.py:1174:9:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1222'>ibis/backends/base/__init__.py:1222:9:</a> D413 [*] Missing blank line after last section ("Parameters")
... 484 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1241 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/benchmarks.py#L12'>integration/benchmarks/benchmarks.py:12:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/benchmarks.py#L48'>integration/benchmarks/benchmarks.py:48:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/benchmarks.py#L75'>integration/benchmarks/benchmarks.py:75:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/helpers.py#L16'>integration/benchmarks/helpers.py:16:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L104'>integration/benchmarks/test_compile_benchmark.py:104:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L114'>integration/benchmarks/test_compile_benchmark.py:114:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L30'>integration/benchmarks/test_compile_benchmark.py:30:9:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L39'>integration/benchmarks/test_compile_benchmark.py:39:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L51'>integration/benchmarks/test_compile_benchmark.py:51:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L70'>integration/benchmarks/test_compile_benchmark.py:70:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L80'>integration/benchmarks/test_compile_benchmark.py:80:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L90'>integration/benchmarks/test_compile_benchmark.py:90:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/conftest.py#L16'>integration/conftest.py:16:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/conftest.py#L37'>integration/conftest.py:37:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/conftest.py#L68'>integration/conftest.py:68:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L104'>integration/test_background_task.py:104:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L121'>integration/test_background_task.py:121:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L139'>integration/test_background_task.py:139:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L164'>integration/test_background_task.py:164:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_call_script.py#L233'>integration/test_call_script.py:233:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_call_script.py#L250'>integration/test_call_script.py:250:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_call_script.py#L267'>integration/test_call_script.py:267:5:</a> D413 [*] Missing blank line after last section ("Returns")
... 1219 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/sphinx/config.py#L69'>sphinx/config.py:69:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/sphinx/ext/autodoc/__init__.py#L2448'>sphinx/ext/autodoc/__init__.py:2448:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/sphinx/ext/autodoc/__init__.py#L2527'>sphinx/ext/autodoc/__init__.py:2527:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/tests/test_extensions/ext_napoleon_pep526_data_google.py#L8'>tests/test_extensions/ext_napoleon_pep526_data_google.py:8:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/tests/test_extensions/ext_napoleon_pep526_data_numpy.py#L8'>tests/test_extensions/ext_napoleon_pep526_data_numpy.py:8:5:</a> D413 [*] Missing blank line after last section ("Attributes")
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/gitter.py#L84'>zerver/data_import/gitter.py:84:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L154'>zerver/data_import/slack.py:154:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L247'>zerver/data_import/slack.py:247:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L504'>zerver/data_import/slack.py:504:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L727'>zerver/data_import/slack.py:727:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L880'>zerver/data_import/slack.py:880:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack_message_conversion.py#L140'>zerver/data_import/slack_message_conversion.py:140:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/lib/compatibility.py#L55'>zerver/lib/compatibility.py:55:5:</a> D413 [*] Missing blank line after last section ("Returns")
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 2706 | 2706 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2706 -0 violations, +0 -0 fixes in 7 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+847 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L100'>disnake/abc.py:100:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1073'>disnake/abc.py:1073:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1153'>disnake/abc.py:1153:9:</a> D413 [*] Missing blank line after last section ("Raises")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L120'>disnake/abc.py:120:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1288'>disnake/abc.py:1288:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1377'>disnake/abc.py:1377:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1528'>disnake/abc.py:1528:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1803'>disnake/abc.py:1803:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L180'>disnake/abc.py:180:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1831'>disnake/abc.py:1831:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1865'>disnake/abc.py:1865:9:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1929'>disnake/abc.py:1929:5:</a> D413 [*] Missing blank line after last section ("Note")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L1961'>disnake/abc.py:1961:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L228'>disnake/abc.py:228:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L530'>disnake/abc.py:530:9:</a> D413 [*] Missing blank line after last section ("Returns")
... 832 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/decorators/base.py#L131'>airflow/decorators/base.py:131:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D413 [*] Missing blank line after last section ("example")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/io/path.py#L254'>airflow/io/path.py:254:9:</a> D413 [*] Missing blank line after last section ("See Also")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/models/dag.py#L4055'>airflow/models/dag.py:4055:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/amazon/aws/operators/base_aws.py#L32'>airflow/providers/amazon/aws/operators/base_aws.py:32:5:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/amazon/aws/sensors/base_aws.py#L32'>airflow/providers/amazon/aws/sensors/base_aws.py:32:5:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/amazon/aws/utils/mixins.py#L65'>airflow/providers/amazon/aws/utils/mixins.py:65:9:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/apache/hive/hooks/hive.py#L809'>airflow/providers/apache/hive/hooks/hive.py:809:5:</a> D413 [*] Missing blank line after last section ("Notes")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/fab/auth_manager/security_manager/override.py#L1408'>airflow/providers/fab/auth_manager/security_manager/override.py:1408:9:</a> D413 [*] Missing blank line after last section ("NOTE")
+ <a href='https://github.com/apache/airflow/blob/714a9a79a59c317f58d6bd621acba9dd4e2a4622/airflow/providers/slack/hooks/slack.py#L61'>airflow/providers/slack/hooks/slack.py:61:5:</a> D413 [*] Missing blank line after last section ("Examples")
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+92 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L293'>src/bokeh/application/handlers/directory.py:293:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L199'>src/bokeh/application/handlers/handler.py:199:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/request_handler.py#L63'>src/bokeh/application/handlers/request_handler.py:63:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L160'>src/bokeh/core/property/bases.py:160:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/container.py#L247'>src/bokeh/core/property/container.py:247:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L215'>src/bokeh/core/property/dataspec.py:215:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L442'>src/bokeh/core/property/dataspec.py:442:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L7'>src/bokeh/driving.py:7:1:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/elements.py#L150'>src/bokeh/embed/elements.py:150:5:</a> D413 [*] Missing blank line after last section ("Returns")
... 82 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+494 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/__init__.py#L39'>ibis/__init__.py:39:5:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L100'>ibis/backends/base/__init__.py:100:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1063'>ibis/backends/base/__init__.py:1063:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1098'>ibis/backends/base/__init__.py:1098:9:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L110'>ibis/backends/base/__init__.py:110:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1114'>ibis/backends/base/__init__.py:1114:9:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1136'>ibis/backends/base/__init__.py:1136:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1160'>ibis/backends/base/__init__.py:1160:9:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1174'>ibis/backends/base/__init__.py:1174:9:</a> D413 [*] Missing blank line after last section ("Examples")
+ <a href='https://github.com/ibis-project/ibis/blob/69560c898d5cac3c14b2788f81a9b5245f660bfe/ibis/backends/base/__init__.py#L1222'>ibis/backends/base/__init__.py:1222:9:</a> D413 [*] Missing blank line after last section ("Parameters")
... 484 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1241 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/benchmarks.py#L12'>integration/benchmarks/benchmarks.py:12:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/benchmarks.py#L48'>integration/benchmarks/benchmarks.py:48:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/benchmarks.py#L75'>integration/benchmarks/benchmarks.py:75:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/helpers.py#L16'>integration/benchmarks/helpers.py:16:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L104'>integration/benchmarks/test_compile_benchmark.py:104:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L114'>integration/benchmarks/test_compile_benchmark.py:114:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L30'>integration/benchmarks/test_compile_benchmark.py:30:9:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L39'>integration/benchmarks/test_compile_benchmark.py:39:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L51'>integration/benchmarks/test_compile_benchmark.py:51:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L70'>integration/benchmarks/test_compile_benchmark.py:70:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L80'>integration/benchmarks/test_compile_benchmark.py:80:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/benchmarks/test_compile_benchmark.py#L90'>integration/benchmarks/test_compile_benchmark.py:90:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/conftest.py#L16'>integration/conftest.py:16:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/conftest.py#L37'>integration/conftest.py:37:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/conftest.py#L68'>integration/conftest.py:68:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L104'>integration/test_background_task.py:104:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L121'>integration/test_background_task.py:121:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L139'>integration/test_background_task.py:139:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_background_task.py#L164'>integration/test_background_task.py:164:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_call_script.py#L233'>integration/test_call_script.py:233:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_call_script.py#L250'>integration/test_call_script.py:250:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/4219a78e7c404a223ad791f3a9a4130b416a8418/integration/test_call_script.py#L267'>integration/test_call_script.py:267:5:</a> D413 [*] Missing blank line after last section ("Returns")
... 1219 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/sphinx/config.py#L69'>sphinx/config.py:69:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/sphinx/ext/autodoc/__init__.py#L2448'>sphinx/ext/autodoc/__init__.py:2448:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/sphinx/ext/autodoc/__init__.py#L2527'>sphinx/ext/autodoc/__init__.py:2527:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/tests/test_extensions/ext_napoleon_pep526_data_google.py#L8'>tests/test_extensions/ext_napoleon_pep526_data_google.py:8:5:</a> D413 [*] Missing blank line after last section ("Attributes")
+ <a href='https://github.com/sphinx-doc/sphinx/blob/fa290049515c38e68edda7e8c17be69b8793bb84/tests/test_extensions/ext_napoleon_pep526_data_numpy.py#L8'>tests/test_extensions/ext_napoleon_pep526_data_numpy.py:8:5:</a> D413 [*] Missing blank line after last section ("Attributes")
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/gitter.py#L84'>zerver/data_import/gitter.py:84:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L154'>zerver/data_import/slack.py:154:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L247'>zerver/data_import/slack.py:247:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L504'>zerver/data_import/slack.py:504:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L727'>zerver/data_import/slack.py:727:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack.py#L880'>zerver/data_import/slack.py:880:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/data_import/slack_message_conversion.py#L140'>zerver/data_import/slack_message_conversion.py:140:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/7b4afb35e7093f09e5075813805ed8878cb0913c/zerver/lib/compatibility.py#L55'>zerver/lib/compatibility.py:55:5:</a> D413 [*] Missing blank line after last section ("Returns")
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 2706 | 2706 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-01-26 20:08_

I'm surprised so many projects have this rule enabled, I suspect it's an oversight.

---
