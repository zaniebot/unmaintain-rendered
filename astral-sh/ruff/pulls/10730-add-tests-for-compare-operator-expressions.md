```yaml
number: 10730
title: Add tests for compare operator expressions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/compare-op-expr
created_at: 2024-04-02T03:48:52Z
updated_at: 2024-04-03T09:30:40Z
url: https://github.com/astral-sh/ruff/pull/10730
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for compare operator expressions

---

_@dhruvmanila_

## Summary

This PR adds tests for compare operator expressions.

It also updates the parsing logic in a way to avoid using the `next_token` function and instead rely on `bump` wherever possible.


---

_Label `parser` added by @dhruvmanila on 2024-04-02 03:48_

---

_Label `testing` added by @dhruvmanila on 2024-04-02 03:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-02 03:48_

---

_Comment by @github-actions[bot] on 2024-04-02 07:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2802 -2802 violations, +0 -0 fixes in 6 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_tracker_stores.py#L812'>tests/core/test_tracker_stores.py:812:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_tracker_stores.py#L818'>tests/core/test_tracker_stores.py:818:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/nlu/training_data/test_features.py#L179'>tests/shared/nlu/training_data/test_features.py:179:5:</a> D411 [*] Missing blank line before section ("Args")
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/nlu/training_data/test_features.py#L184'>tests/shared/nlu/training_data/test_features.py:184:5:</a> D411 [*] Missing blank line before section ("Args")
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+45 -45 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L130'>airflow/decorators/base.py:130:5:</a> D407 [*] Missing dashed underline after section ("Example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L130'>airflow/decorators/base.py:130:5:</a> D413 [*] Missing blank line after last section ("Example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L136'>airflow/decorators/base.py:136:5:</a> D407 [*] Missing dashed underline after section ("Example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L136'>airflow/decorators/base.py:136:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D405 [*] Section name should be properly capitalized ("example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D407 [*] Missing dashed underline after section ("example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D413 [*] Missing blank line after last section ("example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D405 [*] Section name should be properly capitalized ("example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D407 [*] Missing dashed underline after section ("example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D413 [*] Missing blank line after last section ("example")
... 80 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1262 -1262 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D407 [*] Missing dashed underline after section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L155'>src/bokeh/application/application.py:155:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L157'>src/bokeh/application/application.py:157:9:</a> D407 [*] Missing dashed underline after section ("Args")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D407 [*] Missing dashed underline after section ("Args")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D407 [*] Missing dashed underline after section ("Returns")
... 1879 additional changes omitted for rule D407
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L256'>src/bokeh/application/application.py:256:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L169'>src/bokeh/application/handlers/code_runner.py:169:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L171'>src/bokeh/application/handlers/code_runner.py:171:9:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L293'>src/bokeh/application/handlers/directory.py:293:9:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L299'>src/bokeh/application/handlers/directory.py:299:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L199'>src/bokeh/application/handlers/handler.py:199:9:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L205'>src/bokeh/application/handlers/handler.py:205:9:</a> D413 [*] Missing blank line after last section ("Returns")
... 53 additional changes omitted for rule D413
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L168'>src/bokeh/client/connection.py:168:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L174'>src/bokeh/client/connection.py:174:9:</a> D406 [*] Section name should end with a newline ("Returns")
... 275 additional changes omitted for rule D406
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommand.py#L101'>src/bokeh/command/subcommand.py:101:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommand.py#L79'>src/bokeh/command/subcommand.py:79:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L100'>src/bokeh/command/subcommands/file_output.py:100:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L65'>src/bokeh/command/subcommands/file_output.py:65:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
... 2502 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/resources/tasks.py#L317'>latch/resources/tasks.py:317:5:</a> D411 [*] Missing blank line before section ("Args")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/resources/tasks.py#L322'>latch/resources/tasks.py:322:5:</a> D411 [*] Missing blank line before section ("Args")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L100'>latch/types/metadata.py:100:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L461'>latch/types/metadata.py:461:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L463'>latch/types/metadata.py:463:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L98'>latch/types/metadata.py:98:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L157'>latch_cli/auth/oauth2.py:157:9:</a> D410 [*] Missing blank line after section ("Args")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L157'>latch_cli/auth/oauth2.py:157:9:</a> D411 [*] Missing blank line before section ("Returns")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L159'>latch_cli/auth/oauth2.py:159:9:</a> D410 [*] Missing blank line after section ("Args")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L161'>latch_cli/auth/oauth2.py:161:9:</a> D411 [*] Missing blank line before section ("Returns")
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1445 -1445 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/conftest.py#L12'>benchmarks/conftest.py:12:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/conftest.py#L17'>benchmarks/conftest.py:17:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L152'>benchmarks/test_benchmark_compile_components.py:152:5:</a> D413 [*] Missing blank line after last section ("Yields")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L157'>benchmarks/test_benchmark_compile_components.py:157:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L174'>benchmarks/test_benchmark_compile_components.py:174:5:</a> D413 [*] Missing blank line after last section ("Yields")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L179'>benchmarks/test_benchmark_compile_components.py:179:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L196'>benchmarks/test_benchmark_compile_components.py:196:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L19'>benchmarks/test_benchmark_compile_components.py:19:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L201'>benchmarks/test_benchmark_compile_components.py:201:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L222'>benchmarks/test_benchmark_compile_components.py:222:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L224'>benchmarks/test_benchmark_compile_components.py:224:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L24'>benchmarks/test_benchmark_compile_components.py:24:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L250'>benchmarks/test_benchmark_compile_components.py:250:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L252'>benchmarks/test_benchmark_compile_components.py:252:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L275'>benchmarks/test_benchmark_compile_components.py:275:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L277'>benchmarks/test_benchmark_compile_components.py:277:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L303'>benchmarks/test_benchmark_compile_components.py:303:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L305'>benchmarks/test_benchmark_compile_components.py:305:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L328'>benchmarks/test_benchmark_compile_components.py:328:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L330'>benchmarks/test_benchmark_compile_components.py:330:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L356'>benchmarks/test_benchmark_compile_components.py:356:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L358'>benchmarks/test_benchmark_compile_components.py:358:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_pages.py#L202'>benchmarks/test_benchmark_compile_pages.py:202:5:</a> D413 [*] Missing blank line after last section ("Yields")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_pages.py#L207'>benchmarks/test_benchmark_compile_pages.py:207:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_pages.py#L219'>benchmarks/test_benchmark_compile_pages.py:219:5:</a> D413 [*] Missing blank line after last section ("Yields")
... 2865 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+40 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D407 [*] Missing dashed underline after section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L142'>zerver/data_import/gitter.py:142:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L142'>zerver/data_import/gitter.py:142:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L142'>zerver/data_import/gitter.py:142:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D407 [*] Missing dashed underline after section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L42'>zerver/data_import/gitter.py:42:5:</a> D406 [*] Section name should end with a newline ("Returns")
... 70 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (9 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 3006 | 1503 | 1503 | 0 | 0 |
| D407 | 1940 | 970 | 970 | 0 | 0 |
| D406 | 318 | 159 | 159 | 0 | 0 |
| D412 | 290 | 145 | 145 | 0 | 0 |
| D416 | 16 | 8 | 8 | 0 | 0 |
| D411 | 12 | 6 | 6 | 0 | 0 |
| D405 | 12 | 6 | 6 | 0 | 0 |
| D214 | 6 | 3 | 3 | 0 | 0 |
| D410 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2802 -2802 violations, +0 -0 fixes in 6 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_tracker_stores.py#L812'>tests/core/test_tracker_stores.py:812:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_tracker_stores.py#L818'>tests/core/test_tracker_stores.py:818:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/nlu/training_data/test_features.py#L179'>tests/shared/nlu/training_data/test_features.py:179:5:</a> D411 [*] Missing blank line before section ("Args")
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/nlu/training_data/test_features.py#L184'>tests/shared/nlu/training_data/test_features.py:184:5:</a> D411 [*] Missing blank line before section ("Args")
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+45 -45 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L130'>airflow/decorators/base.py:130:5:</a> D407 [*] Missing dashed underline after section ("Example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L130'>airflow/decorators/base.py:130:5:</a> D413 [*] Missing blank line after last section ("Example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L136'>airflow/decorators/base.py:136:5:</a> D407 [*] Missing dashed underline after section ("Example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/decorators/base.py#L136'>airflow/decorators/base.py:136:5:</a> D413 [*] Missing blank line after last section ("Example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D405 [*] Section name should be properly capitalized ("example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D407 [*] Missing dashed underline after section ("example")
+ <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L27'>airflow/hooks/filesystem.py:27:5:</a> D413 [*] Missing blank line after last section ("example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D405 [*] Section name should be properly capitalized ("example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D407 [*] Missing dashed underline after section ("example")
- <a href='https://github.com/apache/airflow/blob/c65b08399d11410795b470640eb4fe6f42748c36/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D413 [*] Missing blank line after last section ("example")
... 80 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1262 -1262 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D407 [*] Missing dashed underline after section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L155'>src/bokeh/application/application.py:155:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L157'>src/bokeh/application/application.py:157:9:</a> D407 [*] Missing dashed underline after section ("Args")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D407 [*] Missing dashed underline after section ("Args")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D407 [*] Missing dashed underline after section ("Returns")
... 1879 additional changes omitted for rule D407
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L250'>src/bokeh/application/application.py:250:9:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L256'>src/bokeh/application/application.py:256:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L169'>src/bokeh/application/handlers/code_runner.py:169:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L171'>src/bokeh/application/handlers/code_runner.py:171:9:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L293'>src/bokeh/application/handlers/directory.py:293:9:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L299'>src/bokeh/application/handlers/directory.py:299:9:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L199'>src/bokeh/application/handlers/handler.py:199:9:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L205'>src/bokeh/application/handlers/handler.py:205:9:</a> D413 [*] Missing blank line after last section ("Returns")
... 53 additional changes omitted for rule D413
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L168'>src/bokeh/client/connection.py:168:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L174'>src/bokeh/client/connection.py:174:9:</a> D406 [*] Section name should end with a newline ("Returns")
... 275 additional changes omitted for rule D406
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommand.py#L101'>src/bokeh/command/subcommand.py:101:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommand.py#L79'>src/bokeh/command/subcommand.py:79:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L100'>src/bokeh/command/subcommands/file_output.py:100:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L65'>src/bokeh/command/subcommands/file_output.py:65:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
... 2502 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/resources/tasks.py#L317'>latch/resources/tasks.py:317:5:</a> D411 [*] Missing blank line before section ("Args")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/resources/tasks.py#L322'>latch/resources/tasks.py:322:5:</a> D411 [*] Missing blank line before section ("Args")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L100'>latch/types/metadata.py:100:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L461'>latch/types/metadata.py:461:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L463'>latch/types/metadata.py:463:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch/types/metadata.py#L98'>latch/types/metadata.py:98:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L157'>latch_cli/auth/oauth2.py:157:9:</a> D410 [*] Missing blank line after section ("Args")
+ <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L157'>latch_cli/auth/oauth2.py:157:9:</a> D411 [*] Missing blank line before section ("Returns")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L159'>latch_cli/auth/oauth2.py:159:9:</a> D410 [*] Missing blank line after section ("Args")
- <a href='https://github.com/latchbio/latch/blob/c94d277c3bad133c41be49fb60dbf0d8032a79d4/latch_cli/auth/oauth2.py#L161'>latch_cli/auth/oauth2.py:161:9:</a> D411 [*] Missing blank line before section ("Returns")
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1445 -1445 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/conftest.py#L12'>benchmarks/conftest.py:12:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/conftest.py#L17'>benchmarks/conftest.py:17:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L152'>benchmarks/test_benchmark_compile_components.py:152:5:</a> D413 [*] Missing blank line after last section ("Yields")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L157'>benchmarks/test_benchmark_compile_components.py:157:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L174'>benchmarks/test_benchmark_compile_components.py:174:5:</a> D413 [*] Missing blank line after last section ("Yields")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L179'>benchmarks/test_benchmark_compile_components.py:179:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L196'>benchmarks/test_benchmark_compile_components.py:196:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L19'>benchmarks/test_benchmark_compile_components.py:19:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L201'>benchmarks/test_benchmark_compile_components.py:201:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L222'>benchmarks/test_benchmark_compile_components.py:222:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L224'>benchmarks/test_benchmark_compile_components.py:224:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L24'>benchmarks/test_benchmark_compile_components.py:24:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L250'>benchmarks/test_benchmark_compile_components.py:250:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L252'>benchmarks/test_benchmark_compile_components.py:252:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L275'>benchmarks/test_benchmark_compile_components.py:275:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L277'>benchmarks/test_benchmark_compile_components.py:277:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L303'>benchmarks/test_benchmark_compile_components.py:303:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L305'>benchmarks/test_benchmark_compile_components.py:305:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L328'>benchmarks/test_benchmark_compile_components.py:328:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L330'>benchmarks/test_benchmark_compile_components.py:330:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L356'>benchmarks/test_benchmark_compile_components.py:356:5:</a> D413 [*] Missing blank line after last section ("Args")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_components.py#L358'>benchmarks/test_benchmark_compile_components.py:358:5:</a> D413 [*] Missing blank line after last section ("Args")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_pages.py#L202'>benchmarks/test_benchmark_compile_pages.py:202:5:</a> D413 [*] Missing blank line after last section ("Yields")
- <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_pages.py#L207'>benchmarks/test_benchmark_compile_pages.py:207:5:</a> D413 [*] Missing blank line after last section ("Yields")
+ <a href='https://github.com/reflex-dev/reflex/blob/00b1e193ad1ad5e3471626c9e9c933aafdd2f46a/benchmarks/test_benchmark_compile_pages.py#L219'>benchmarks/test_benchmark_compile_pages.py:219:5:</a> D413 [*] Missing blank line after last section ("Yields")
... 2865 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+40 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D407 [*] Missing dashed underline after section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L141'>zerver/data_import/gitter.py:141:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L142'>zerver/data_import/gitter.py:142:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L142'>zerver/data_import/gitter.py:142:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L142'>zerver/data_import/gitter.py:142:5:</a> D413 [*] Missing blank line after last section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D406 [*] Section name should end with a newline ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D407 [*] Missing dashed underline after section ("Returns")
+ <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L41'>zerver/data_import/gitter.py:41:5:</a> D413 [*] Missing blank line after last section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/8f00ed4178d15b026b94eb380edfed6b6b533d9f/zerver/data_import/gitter.py#L42'>zerver/data_import/gitter.py:42:5:</a> D406 [*] Section name should end with a newline ("Returns")
... 70 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (9 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 3006 | 1503 | 1503 | 0 | 0 |
| D407 | 1940 | 970 | 970 | 0 | 0 |
| D406 | 318 | 159 | 159 | 0 | 0 |
| D412 | 290 | 145 | 145 | 0 | 0 |
| D416 | 16 | 8 | 8 | 0 | 0 |
| D411 | 12 | 6 | 6 | 0 | 0 |
| D405 | 12 | 6 | 6 | 0 | 0 |
| D214 | 6 | 3 | 3 | 0 | 0 |
| D410 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1009 on 2024-04-02 10:36_

Nit: I think I would change the signature and call site to avoid having to pass both `operator`, `operator_binding_power` AND convert `operator` to `CmpOp`

1. Add a new `as_compare_operator` method to `TokenKind` that returns `Option<CompareOp>`
1. Refactor `TokenKind::is_compare_operator` to `self.as_compare_operator().is_some()`
1. `parse_expression_with_precedence`: Replace `op.is_compare_operator` with `if let Some(compare_operator) = op.as_compare_operator() {` 
1. Implement `From<CmpOp> for Precedence` (can never fail)
1. Change `parse_compare_expression` to only accept a `CmpOp`
  * You can get the token by calling the (inlineable) `TokenKind::from(cmp_op)` 
  * You can get the precedence by calling the (inlineable) `Precedence::from(cmp_op)


Edit: Okay, `as_compare_operator` may not work because it requires lookahead. I think we can either keep `as_compare_operator` and match on the comparator in the parser to see if we may need to change it in case it's a two kind operator (we need to bump the extra token anyway)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1030 on 2024-04-02 10:36_

Nit: I would use the word precedence here, considering that the type is called `Precedence`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1040 on 2024-04-02 10:37_

Nit: The code here could use `kind.as_compare_operator()` here to simplify some of the logic (see my other comment above)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1051 on 2024-04-02 10:39_

Nit: You could add a `bump_compare_operator` method to avoid the repetition OR you move the handling of the first comparator into the loop.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__compare__missing_lhs.py.snap`:24 on 2024-04-02 10:45_

The parse tree here looks incorrect. What happened with the `>` token? I think this should parse as a comparator expression with a missing left hand side. 

Edit: Okay, I think what's happening here is that we drop the `>` as part of the statement parsing because we aren't at the start of an expression. That's fine. We can do the other improvement later.

---

_@MichaReiser approved on 2024-04-02 10:49_

---

_@dhruvmanila reviewed on 2024-04-03 09:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__compare__missing_lhs.py.snap`:24 on 2024-04-03 09:13_

Yes, that's correct. `>` isn't a start of any statement nor expression which is why it gets dropped and the RHS is parsed as a standalone expression. I've noted it down in https://github.com/astral-sh/ruff/issues/10653

---

_Merged by @dhruvmanila on 2024-04-03 09:13_

---

_Closed by @dhruvmanila on 2024-04-03 09:13_

---

_Branch deleted on 2024-04-03 09:13_

---
