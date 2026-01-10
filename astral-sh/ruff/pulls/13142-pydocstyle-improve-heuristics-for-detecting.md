```yaml
number: 13142
title: "[`pydocstyle`] Improve heuristics for detecting Google-style docstrings"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: alex/pydocstyle-convention
created_at: 2024-08-28T18:00:29Z
updated_at: 2024-08-29T15:33:20Z
url: https://github.com/astral-sh/ruff/pull/13142
synced_at: 2026-01-10T21:38:32Z
```

# [`pydocstyle`] Improve heuristics for detecting Google-style docstrings

---

_Pull request opened by @AlexWaygood on 2024-08-28 18:00_

## Summary

This PR improves our heuristics for detecting which docstring convention users have in their docstrings. Specifically, we used to infer the following docstring as being a numpy-style docstring, when it's clearly a Google-style docstring (because the section title ends with a colon, and does not have an underline on the following line):

```py
def foo(a: str) -> str:
    """Foo bar.

    Examples:
        Some explanation here.
        >>> bla bla bla

    """
    return a
```

While fixing the bug, I noticed that we were also checking D407 on docstrings we infer as being Google-style, even though our docs say we don't. I fixed that as well in this PR, since otherwise the fixture I added would have led to an incorrect snapshot being added (D407 would have been emitted on the snippet, when it shouldn't have been). This D407 change resulted in a several changes to the existing snapshots, but I think they're all for the better. It looks like we no longer emit D407 on several docstrings in our fixtures that seem pretty clearly to be Google-style docstrings to me, and this rule definitely shouldn't be enforced for Google-style docstrings in my opinion.

Fixes #13139

## Test Plan

- `cargo test -p ruff_linter --lib`
- Ecosystem report


---

_Label `bug` added by @AlexWaygood on 2024-08-28 18:00_

---

_Label `docstring` added by @AlexWaygood on 2024-08-28 18:00_

---

_Comment by @github-actions[bot] on 2024-08-28 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1147 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -34 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/decorators/base.py#L137'>airflow/decorators/base.py:137:5:</a> D407 [*] Missing dashed underline after section ("Example")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D407 [*] Missing dashed underline after section ("example")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/models/dag.py#L3909'>airflow/models/dag.py:3909:5:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/hooks/s3.py#L1401'>airflow/providers/amazon/aws/hooks/s3.py:1401:9:</a> D407 [*] Missing dashed underline after section ("Note")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/operators/base_aws.py#L40'>airflow/providers/amazon/aws/operators/base_aws.py:40:5:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/operators/base_aws.py#L40'>airflow/providers/amazon/aws/operators/base_aws.py:40:5:</a> D407 [*] Missing dashed underline after section ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/sensors/base_aws.py#L40'>airflow/providers/amazon/aws/sensors/base_aws.py:40:5:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/sensors/base_aws.py#L40'>airflow/providers/amazon/aws/sensors/base_aws.py:40:5:</a> D407 [*] Missing dashed underline after section ("Examples")
... 15 additional changes omitted for rule D407
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/utils/mixins.py#L69'>airflow/providers/amazon/aws/utils/mixins.py:69:9:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/apache/hive/hooks/hive.py#L840'>airflow/providers/apache/hive/hooks/hive.py:840:5:</a> D406 [*] Section name should end with a newline ("Notes")
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/scripts/cancel_github_workflows.py#L21'>scripts/cancel_github_workflows.py:21:1:</a> D407 [*] Missing dashed underline after section ("Example")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/scripts/cancel_github_workflows.py#L68'>scripts/cancel_github_workflows.py:68:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/scripts/cancel_github_workflows.py#L68'>scripts/cancel_github_workflows.py:68:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/cli/test_db.py#L154'>superset/cli/test_db.py:154:5:</a> D407 [*] Missing dashed underline after section ("TODO")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/commands/chart/importers/v1/utils.py#L35'>superset/commands/chart/importers/v1/utils.py:35:5:</a> D407 [*] Missing dashed underline after section ("TODO")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/commands/sql_lab/query.py#L39'>superset/commands/sql_lab/query.py:39:5:</a> D406 [*] Section name should end with a newline ("Attributes")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/commands/sql_lab/query.py#L39'>superset/commands/sql_lab/query.py:39:5:</a> D407 [*] Missing dashed underline after section ("Attributes")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/daos/tag.py#L312'>superset/daos/tag.py:312:9:</a> D407 [*] Missing dashed underline after section ("Args")
... 7 additional changes omitted for rule D407
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/db_engine_specs/base.py#L194'>superset/db_engine_specs/base.py:194:5:</a> D406 [*] Section name should end with a newline ("Attributes")
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1081 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L157'>src/bokeh/application/application.py:157:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L253'>src/bokeh/application/application.py:253:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L256'>src/bokeh/application/application.py:256:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L340'>src/bokeh/application/application.py:340:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L344'>src/bokeh/application/application.py:344:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L91'>src/bokeh/application/application.py:91:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L95'>src/bokeh/application/application.py:95:9:</a> D407 [*] Missing dashed underline after section ("Keyword Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L139'>src/bokeh/application/handlers/code.py:139:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L85'>src/bokeh/application/handlers/code.py:85:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L171'>src/bokeh/application/handlers/code_runner.py:171:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L171'>src/bokeh/application/handlers/code_runner.py:171:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L207'>src/bokeh/application/handlers/code_runner.py:207:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L75'>src/bokeh/application/handlers/code_runner.py:75:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L89'>src/bokeh/application/handlers/code_runner.py:89:9:</a> D407 [*] Missing dashed underline after section ("Raises")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L247'>src/bokeh/application/handlers/directory.py:247:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L261'>src/bokeh/application/handlers/directory.py:261:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L276'>src/bokeh/application/handlers/directory.py:276:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L286'>src/bokeh/application/handlers/directory.py:286:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L296'>src/bokeh/application/handlers/directory.py:296:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L299'>src/bokeh/application/handlers/directory.py:299:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/function.py#L94'>src/bokeh/application/handlers/function.py:94:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L134'>src/bokeh/application/handlers/handler.py:134:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L137'>src/bokeh/application/handlers/handler.py:137:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L150'>src/bokeh/application/handlers/handler.py:150:9:</a> D407 [*] Missing dashed underline after section ("Args")
... 918 additional changes omitted for rule D407
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L174'>src/bokeh/client/connection.py:174:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L242'>src/bokeh/client/connection.py:242:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L395'>src/bokeh/client/session.py:395:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L407'>src/bokeh/client/session.py:407:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L473'>src/bokeh/client/session.py:473:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L175'>src/bokeh/colors/color.py:175:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L187'>src/bokeh/colors/color.py:187:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L199'>src/bokeh/colors/color.py:199:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L238'>src/bokeh/colors/color.py:238:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L317'>src/bokeh/colors/color.py:317:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L332'>src/bokeh/colors/color.py:332:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L345'>src/bokeh/colors/color.py:345:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L355'>src/bokeh/colors/color.py:355:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L406'>src/bokeh/colors/color.py:406:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L456'>src/bokeh/colors/color.py:456:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L468'>src/bokeh/colors/color.py:468:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L478'>src/bokeh/colors/color.py:478:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L222'>src/bokeh/command/subcommands/file_output.py:222:9:</a> D406 [*] Section name should end with a newline ("Raises")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L459'>src/bokeh/core/has_props.py:459:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L462'>src/bokeh/core/has_props.py:462:9:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L529'>src/bokeh/core/has_props.py:529:9:</a> D406 [*] Section name should end with a newline ("Returns")
... 1034 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -17 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L158'>zerver/data_import/slack.py:158:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L158'>zerver/data_import/slack.py:158:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L253'>zerver/data_import/slack.py:253:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L253'>zerver/data_import/slack.py:253:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L510'>zerver/data_import/slack.py:510:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L510'>zerver/data_import/slack.py:510:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L733'>zerver/data_import/slack.py:733:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L733'>zerver/data_import/slack.py:733:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L876'>zerver/data_import/slack.py:876:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L876'>zerver/data_import/slack.py:876:5:</a> D407 [*] Missing dashed underline after section ("Returns")
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D407 | 983 | 0 | 983 | 0 | 0 |
| D406 | 164 | 0 | 164 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1147 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -34 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/decorators/base.py#L137'>airflow/decorators/base.py:137:5:</a> D407 [*] Missing dashed underline after section ("Example")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/hooks/filesystem.py#L32'>airflow/hooks/filesystem.py:32:5:</a> D407 [*] Missing dashed underline after section ("example")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/models/dag.py#L3909'>airflow/models/dag.py:3909:5:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/hooks/s3.py#L1401'>airflow/providers/amazon/aws/hooks/s3.py:1401:9:</a> D407 [*] Missing dashed underline after section ("Note")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/operators/base_aws.py#L40'>airflow/providers/amazon/aws/operators/base_aws.py:40:5:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/operators/base_aws.py#L40'>airflow/providers/amazon/aws/operators/base_aws.py:40:5:</a> D407 [*] Missing dashed underline after section ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/sensors/base_aws.py#L40'>airflow/providers/amazon/aws/sensors/base_aws.py:40:5:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/sensors/base_aws.py#L40'>airflow/providers/amazon/aws/sensors/base_aws.py:40:5:</a> D407 [*] Missing dashed underline after section ("Examples")
... 15 additional changes omitted for rule D407
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/amazon/aws/utils/mixins.py#L69'>airflow/providers/amazon/aws/utils/mixins.py:69:9:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/apache/airflow/blob/e55ecd58705f19e48c3fbf680c71e158b647a221/airflow/providers/apache/hive/hooks/hive.py#L840'>airflow/providers/apache/hive/hooks/hive.py:840:5:</a> D406 [*] Section name should end with a newline ("Notes")
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/scripts/cancel_github_workflows.py#L21'>scripts/cancel_github_workflows.py:21:1:</a> D407 [*] Missing dashed underline after section ("Example")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/scripts/cancel_github_workflows.py#L68'>scripts/cancel_github_workflows.py:68:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/scripts/cancel_github_workflows.py#L68'>scripts/cancel_github_workflows.py:68:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/cli/test_db.py#L154'>superset/cli/test_db.py:154:5:</a> D407 [*] Missing dashed underline after section ("TODO")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/commands/chart/importers/v1/utils.py#L35'>superset/commands/chart/importers/v1/utils.py:35:5:</a> D407 [*] Missing dashed underline after section ("TODO")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/commands/sql_lab/query.py#L39'>superset/commands/sql_lab/query.py:39:5:</a> D406 [*] Section name should end with a newline ("Attributes")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/commands/sql_lab/query.py#L39'>superset/commands/sql_lab/query.py:39:5:</a> D407 [*] Missing dashed underline after section ("Attributes")
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/daos/tag.py#L312'>superset/daos/tag.py:312:9:</a> D407 [*] Missing dashed underline after section ("Args")
... 7 additional changes omitted for rule D407
- <a href='https://github.com/apache/superset/blob/75c500c9a53ce503b8636761f17b5b63eb8ee8e2/superset/db_engine_specs/base.py#L194'>superset/db_engine_specs/base.py:194:5:</a> D406 [*] Section name should end with a newline ("Attributes")
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1081 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L157'>src/bokeh/application/application.py:157:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L253'>src/bokeh/application/application.py:253:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L256'>src/bokeh/application/application.py:256:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L340'>src/bokeh/application/application.py:340:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L344'>src/bokeh/application/application.py:344:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L91'>src/bokeh/application/application.py:91:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L95'>src/bokeh/application/application.py:95:9:</a> D407 [*] Missing dashed underline after section ("Keyword Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L139'>src/bokeh/application/handlers/code.py:139:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L85'>src/bokeh/application/handlers/code.py:85:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L171'>src/bokeh/application/handlers/code_runner.py:171:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L171'>src/bokeh/application/handlers/code_runner.py:171:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L207'>src/bokeh/application/handlers/code_runner.py:207:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L75'>src/bokeh/application/handlers/code_runner.py:75:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L89'>src/bokeh/application/handlers/code_runner.py:89:9:</a> D407 [*] Missing dashed underline after section ("Raises")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L247'>src/bokeh/application/handlers/directory.py:247:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L261'>src/bokeh/application/handlers/directory.py:261:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L276'>src/bokeh/application/handlers/directory.py:276:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L286'>src/bokeh/application/handlers/directory.py:286:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L296'>src/bokeh/application/handlers/directory.py:296:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L299'>src/bokeh/application/handlers/directory.py:299:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/function.py#L94'>src/bokeh/application/handlers/function.py:94:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L134'>src/bokeh/application/handlers/handler.py:134:9:</a> D407 [*] Missing dashed underline after section ("Args")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L137'>src/bokeh/application/handlers/handler.py:137:9:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L150'>src/bokeh/application/handlers/handler.py:150:9:</a> D407 [*] Missing dashed underline after section ("Args")
... 918 additional changes omitted for rule D407
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L174'>src/bokeh/client/connection.py:174:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L242'>src/bokeh/client/connection.py:242:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L395'>src/bokeh/client/session.py:395:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L407'>src/bokeh/client/session.py:407:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L473'>src/bokeh/client/session.py:473:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L175'>src/bokeh/colors/color.py:175:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L187'>src/bokeh/colors/color.py:187:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L199'>src/bokeh/colors/color.py:199:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L238'>src/bokeh/colors/color.py:238:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L317'>src/bokeh/colors/color.py:317:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L332'>src/bokeh/colors/color.py:332:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L345'>src/bokeh/colors/color.py:345:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L355'>src/bokeh/colors/color.py:355:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L406'>src/bokeh/colors/color.py:406:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L456'>src/bokeh/colors/color.py:456:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L468'>src/bokeh/colors/color.py:468:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L478'>src/bokeh/colors/color.py:478:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L222'>src/bokeh/command/subcommands/file_output.py:222:9:</a> D406 [*] Section name should end with a newline ("Raises")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L459'>src/bokeh/core/has_props.py:459:9:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L462'>src/bokeh/core/has_props.py:462:9:</a> D406 [*] Section name should end with a newline ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L529'>src/bokeh/core/has_props.py:529:9:</a> D406 [*] Section name should end with a newline ("Returns")
... 1034 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -17 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L158'>zerver/data_import/slack.py:158:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L158'>zerver/data_import/slack.py:158:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L253'>zerver/data_import/slack.py:253:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L253'>zerver/data_import/slack.py:253:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L510'>zerver/data_import/slack.py:510:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L510'>zerver/data_import/slack.py:510:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L733'>zerver/data_import/slack.py:733:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L733'>zerver/data_import/slack.py:733:5:</a> D407 [*] Missing dashed underline after section ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L876'>zerver/data_import/slack.py:876:5:</a> D406 [*] Section name should end with a newline ("Returns")
- <a href='https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/zerver/data_import/slack.py#L876'>zerver/data_import/slack.py:876:5:</a> D407 [*] Missing dashed underline after section ("Returns")
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D407 | 983 | 0 | 983 | 0 | 0 |
| D406 | 164 | 0 | 164 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-08-28 18:16_

The ecosystem hits look good to me, but it's a big change...

---

_Marked ready for review by @AlexWaygood on 2024-08-28 18:16_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-08-28 18:16_

---

_Comment by @charliermarsh on 2024-08-28 18:24_

Can we quickly check which of those projects actually have `D` enabled, and which are being tested with `ALL` in the ecosystem CI?

---

_Comment by @AlexWaygood on 2024-08-28 18:45_

> Can we quickly check which of those projects actually have `D` enabled, and which are being tested with `ALL` in the ecosystem CI?

- apache/airflow enables a bunch of pydocstyle rules, but [not D406 or D407](https://github.com/apache/airflow/blob/032ac87b1d93abc53d3281313c957708017e21d4/pyproject.toml#L260-L310)
- apache/superset [does not enable any `pydocstyle` rules](https://github.com/apache/superset/blob/07985e2f5aa165f6868abbf88594e6d75300caae/pyproject.toml#L442-L452)
- bokeh [also does not enable any pydocstyle rules](https://github.com/bokeh/bokeh/blob/a28073d0792e28d96416c35b9a41de39b226e658/pyproject.toml#L148-L174)
- And [nor does zulip](https://github.com/zulip/zulip/blob/3d6dcf8fe5f8441d72864b83eba94842acdef1aa/pyproject.toml#L104-L141)

---

_@charliermarsh approved on 2024-08-29 15:31_

---

_Merged by @AlexWaygood on 2024-08-29 15:33_

---

_Closed by @AlexWaygood on 2024-08-29 15:33_

---

_Branch deleted on 2024-08-29 15:33_

---
