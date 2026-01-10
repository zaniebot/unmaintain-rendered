```yaml
number: 10733
title: Add tests for function call arguments
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/call-arguments
created_at: 2024-04-02T06:35:35Z
updated_at: 2024-04-03T10:07:08Z
url: https://github.com/astral-sh/ruff/pull/10733
synced_at: 2026-01-10T22:47:02Z
```

# Add tests for function call arguments

---

_Pull request opened by @dhruvmanila on 2024-04-02 06:35_

## Summary

This PR adds tests for call arguments.


---

_Label `parser` added by @dhruvmanila on 2024-04-02 06:35_

---

_Label `testing` added by @dhruvmanila on 2024-04-02 06:35_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-02 06:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/helpers.rs`:148 on 2024-04-02 06:36_

This is a soft syntax error so remove it from the parser.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:658 on 2024-04-02 06:36_

I forgot to remove this from earlier PR, sorry!

---

_@dhruvmanila reviewed on 2024-04-02 06:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/helpers.rs`:148 on 2024-04-02 11:03_

What's a soft syntax error? Is it one that is only thrown at runtime (but still considered a `SyntaxError`)?

I think we should still add the error (at least until we have a checker where we can handle soft errors), but we should continue parsing. This won't give us as good results as it could today because we abort linting after the first syntax error, but this should be less of a concern when downstream parse support error recovery.

Changing this could also break downstream rules that incorrectly assume that keyword arguments are guaranteed to be unique (for a valid AST)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:675 on 2024-04-02 11:05_


```suggestion
            if parser.eat(TokenKind::DoubleStar) {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:711 on 2024-04-02 11:07_

Nit: Should we add a helper method on `Parser` that validates if `parsed_expr` is a yield expression and not parenthesized and in that case adds the error (to avoid repeating the error)?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:770 on 2024-04-02 11:10_

Nit: I think a helper for this test would be valuable to avoid repeating the error message and checks

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/valid/expressions/call.py`:2 on 2024-04-02 12:53_

Is this file intentionally empty or am I misreading something?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__arguments__invalid_expression.py.snap`:46 on 2024-04-02 12:58_

Is my understanding correct that the TODO that you added in the expression parsing is about the nodes being dropped in this specific case? 

---

_@MichaReiser approved on 2024-04-02 13:02_

---

_@dhruvmanila reviewed on 2024-04-03 09:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/helpers.rs`:148 on 2024-04-03 09:19_

> What's a soft syntax error? Is it one that is only thrown at runtime (but still considered a `SyntaxError`)?

Yes, that's correct.

> Changing this could also break downstream rules that incorrectly assume that keyword arguments are guaranteed to be unique (for a valid AST)

Oh right, thanks for noticing this.

---

_Comment by @codspeed-hq[bot] on 2024-04-03 09:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/call-arguments)

### Merging #10733 will **degrade performances by 5.46%**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>dhruv/call-arguments</code> (4652b0b) with <code>dhruv/parser</code> (065e90b)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/call-arguments)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/call-arguments` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.7 ms | 26.3 ms | +9.11% |
| ⚡ | `parser[pydantic/types.py]` | 11.7 ms | 10.7 ms | +8.78% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.46% |


---

_@dhruvmanila reviewed on 2024-04-03 09:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/valid/expressions/call.py`:2 on 2024-04-03 09:25_

Yes, it is filled in #10734. So, two files where `call.py` is for testing call expression `<this>()` and `arguments.py` is for call arguments (`call(<this>)`).

---

_@dhruvmanila reviewed on 2024-04-03 09:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__arguments__invalid_expression.py.snap`:46 on 2024-04-03 09:26_

Yes, in case the parameter name is not a `ExprName`.

---

_@dhruvmanila reviewed on 2024-04-03 09:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/helpers.rs`:148 on 2024-04-03 09:29_

Ok, it turns out that the validation function short circuits on the first error. I'll expand it to return a vector of `ParseError` instead.

---

_Merged by @dhruvmanila on 2024-04-03 09:47_

---

_Closed by @dhruvmanila on 2024-04-03 09:47_

---

_Branch deleted on 2024-04-03 09:47_

---

_Comment by @github-actions[bot] on 2024-04-03 10:07_

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
