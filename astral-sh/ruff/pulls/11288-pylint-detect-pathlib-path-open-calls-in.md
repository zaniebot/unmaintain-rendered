```yaml
number: 11288
title: "[`pylint`] Detect `pathlib.Path.open` calls in `unspecified-encoding` (`PLW1514`) "
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: plw1514
created_at: 2024-05-05T01:27:24Z
updated_at: 2024-05-09T17:29:04Z
url: https://github.com/astral-sh/ruff/pull/11288
synced_at: 2026-01-10T22:05:26Z
```

# [`pylint`] Detect `pathlib.Path.open` calls in `unspecified-encoding` (`PLW1514`) 

---

_Pull request opened by @augustelalande on 2024-05-05 01:27_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #11263

Detect `pathlib.Path.open` calls which do not specify a file encoding.

## Test Plan

Test cases added to fixture.


---

_Comment by @augustelalande on 2024-05-05 01:28_

I was not able to detect violations of the form
```
x = Path("foo.txt")
x.open()
```
I don't know if this is currently doable.

---

_Comment by @github-actions[bot] on 2024-05-05 01:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+76 -0 violations, +0 -0 fixes in 7 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/79b1c194d616618a9a1095a95db8266f49e3867a/.github/scripts/authors_in_cff.py#L35'>.github/scripts/authors_in_cff.py:35:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/providers/alibaba/cloud/log/oss_task_handler.py#L98'>airflow/providers/alibaba/cloud/log/oss_task_handler.py:98:19:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/providers/amazon/aws/log/s3_task_handler.py#L106'>airflow/providers/amazon/aws/log/s3_task_handler.py:106:19:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/providers/smtp/notifications/smtp.py#L105'>airflow/providers/smtp/notifications/smtp.py:105:24:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/providers/smtp/notifications/smtp.py#L112'>airflow/providers/smtp/notifications/smtp.py:112:33:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/providers/smtp/notifications/smtp.py#L119'>airflow/providers/smtp/notifications/smtp.py:119:33:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/providers/trino/hooks/trino.py#L118'>airflow/providers/trino/hooks/trino.py:118:25:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/airflow/utils/code_utils.py#L65'>airflow/utils/code_utils.py:65:18:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L404'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:404:13:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L414'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:414:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/dev/breeze/src/airflow_breeze/global_constants.py#L386'>dev/breeze/src/airflow_breeze/global_constants.py:386:6:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/dev/breeze/src/airflow_breeze/utils/md5_build_check.py#L50'>dev/breeze/src/airflow_breeze/utils/md5_build_check.py:50:36:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/docker_tests/test_examples_of_prod_image_building.py#L78'>docker_tests/test_examples_of_prod_image_building.py:78:15:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/docs/exts/operators_and_hooks_ref.py#L213'>docs/exts/operators_and_hooks_ref.py:213:52:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/docs/exts/operators_and_hooks_ref.py#L318'>docs/exts/operators_and_hooks_ref.py:318:52:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/docs/exts/providers_extensions.py#L111'>docs/exts/providers_extensions.py:111:48:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/scripts/ci/pre_commit/check_aiobotocore_optional.py#L35'>scripts/ci/pre_commit/check_aiobotocore_optional.py:35:48:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/tests/providers/amazon/aws/hooks/test_eks.py#L1251'>tests/providers/amazon/aws/hooks/test_eks.py:1251:37:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/tests/providers/google/common/hooks/test_base_google.py#L226'>tests/providers/google/common/hooks/test_base_google.py:226:20:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/tests/system/providers/google/cloud/gcs/resources/transform_timespan.py#L34'>tests/system/providers/google/cloud/gcs/resources/transform_timespan.py:34:1:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/c7c680ee3b5a6f61012c222f092cd8c19f7e1b3d/tests/system/providers/weaviate/example_weaviate_cohere.py#L59'>tests/system/providers/weaviate/example_weaviate_cohere.py:59:26:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/integration/buildcmd/test_build_samconfig.py#L129'>tests/integration/buildcmd/test_build_samconfig.py:129:42:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/integration/buildcmd/test_build_samconfig.py#L131'>tests/integration/buildcmd/test_build_samconfig.py:131:31:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/integration/buildcmd/test_build_samconfig.py#L88'>tests/integration/buildcmd/test_build_samconfig.py:88:42:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/integration/buildcmd/test_build_samconfig.py#L89'>tests/integration/buildcmd/test_build_samconfig.py:89:27:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/integration/init/test_init_command.py#L822'>tests/integration/init/test_init_command.py:822:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/integration/init/test_init_command.py#L837'>tests/integration/init/test_init_command.py:837:26:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/pipeline/init/test_initeractive_init_flow.py#L606'>tests/unit/commands/pipeline/init/test_initeractive_init_flow.py:606:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/pipeline/init/test_initeractive_init_flow.py#L617'>tests/unit/commands/pipeline/init/test_initeractive_init_flow.py:617:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/pipeline/init/test_initeractive_init_flow.py#L629'>tests/unit/commands/pipeline/init/test_initeractive_init_flow.py:629:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L1032'>tests/unit/commands/samconfig/test_samconfig.py:1032:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L1033'>tests/unit/commands/samconfig/test_samconfig.py:1033:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L1070'>tests/unit/commands/samconfig/test_samconfig.py:1070:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L1175'>tests/unit/commands/samconfig/test_samconfig.py:1175:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L1253'>tests/unit/commands/samconfig/test_samconfig.py:1253:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L130'>tests/unit/commands/samconfig/test_samconfig.py:130:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L190'>tests/unit/commands/samconfig/test_samconfig.py:190:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L247'>tests/unit/commands/samconfig/test_samconfig.py:247:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L303'>tests/unit/commands/samconfig/test_samconfig.py:303:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L30'>tests/unit/commands/samconfig/test_samconfig.py:30:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L31'>tests/unit/commands/samconfig/test_samconfig.py:31:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L365'>tests/unit/commands/samconfig/test_samconfig.py:365:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L429'>tests/unit/commands/samconfig/test_samconfig.py:429:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/c066963e149f72a34cb5b039438a76a2d3c81e22/tests/unit/commands/samconfig/test_samconfig.py#L496'>tests/unit/commands/samconfig/test_samconfig.py:496:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/3a50273be99c78c78ea169854b2264f1edef3dab/securedrop/source_user.py#L234'>securedrop/source_user.py:234:21:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/3a50273be99c78c78ea169854b2264f1edef3dab/securedrop/source_user.py#L235'>securedrop/source_user.py:235:26:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/bcdf0bf266812c0ff253907cac4a2065f5f62f48/ibis/backends/duckdb/tests/test_register.py#L280'>ibis/backends/duckdb/tests/test_register.py:280:13:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/30a0decb47aff80ee8909c918eb89b75ff422643/bin/update_how_it_works_image.py#L26'>bin/update_how_it_works_image.py:26:16:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/30a0decb47aff80ee8909c918eb89b75ff422643/bin/update_how_it_works_image.py#L28'>bin/update_how_it_works_image.py:28:17:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/30a0decb47aff80ee8909c918eb89b75ff422643/bin/update_how_it_works_image.py#L29'>bin/update_how_it_works_image.py:29:17:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/30a0decb47aff80ee8909c918eb89b75ff422643/bin/update_how_it_works_image.py#L30'>bin/update_how_it_works_image.py:30:17:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/2994db5d993f0cb252dbeebfac7d66677b670ed8/tests/test_custom_modules.py#L23'>tests/test_custom_modules.py:23:16:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/2994db5d993f0cb252dbeebfac7d66677b670ed8/tests/test_dynamic_metadata.py#L267'>tests/test_dynamic_metadata.py:267:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1514 | 76 | 76 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:80 on 2024-05-06 07:29_

I think it would be useful to utilize the `is_violation` function for the same. The qualified name would be `&["pathlib", "Path"]` if I'm not mistaken. This will be useful for cases which involves starred and double starred arguments because currently it gets flagged.

```py
from pathlib import Path

with Path("foo.txt").open(*args) as f:
    pass

with Path("foo.txt").open(**kwargs) as f:
    pass
```

This is a rough sketch of what the APIs could look like:

`is_violation` would be the entry point which would take in a `ExprCall` and a new `Segments` (or a better name) enum. This new enum would be either the segments of the current call expression or an attribute value of the `Path` call expression. The `Segments` enum will be created from the call expression along with the help of the semantic model and it would return `None` if it couldn't in which case the rule function would return.

```rs
// TODO: better name?
enum Segments<'a> {
    Regular(&'a [&'a str]),
    Pathlib(&'a str),
}

impl Segments {
    fn try_from_call(call: &ast::ExprCall, semantic: &SemanticModel) -> Self {}
}

fn is_violation(call: &ast::ExprCall, segments: Segments<'_>) -> bool {
    // If we have something like `*args`, which might contain the encoding argument, abort.
    if call.arguments.args.iter().any(Expr::is_starred_expr) {
        return false;
    }
    // If we have something like `**kwargs`, which might contain the encoding argument, abort.
    if call
        .arguments
        .keywords
        .iter()
        .any(|keyword| keyword.arg.is_none())
    {
        return false;
    }

    match segments {
        Segments::Regular(segments) => match segments {
            ["" | "codecs" | "_io", "open"] => {
                if let Some(mode_arg) = call.arguments.find_argument("mode", 1) {
                    if is_binary_mode(mode_arg).unwrap_or(true) {
                        // binary mode or unknown mode is no violation
                        return false;
                    }
                }
                // else mode not specified, defaults to text mode
                call.arguments.find_argument("encoding", 3).is_none()
            }
            ["tempfile", "TemporaryFile" | "NamedTemporaryFile" | "SpooledTemporaryFile"] => {
                let mode_pos = usize::from(segments[1] == "SpooledTemporaryFile");
                if let Some(mode_arg) = call.arguments.find_argument("mode", mode_pos) {
                    if is_binary_mode(mode_arg).unwrap_or(true) {
                        // binary mode or unknown mode is no violation
                        return false;
                    }
                } else {
                    // defaults to binary mode
                    return false;
                }
                call.arguments
                    .find_argument("encoding", mode_pos + 2)
                    .is_none()
            }
            ["io" | "_io", "TextIOWrapper"] => {
                call.arguments.find_argument("encoding", 1).is_none()
            }
            _ => false,
        },
        Segments::Pathlib(attr) => match attr {
            "open" => {
                if let Some(mode_arg) = call.arguments.find_argument("mode", 0) {
                    if is_binary_mode(mode_arg).unwrap_or(true) {
                        // binary mode or unknown mode is no violation
                        return false;
                    }
                }
                // else mode not specified, defaults to text mode
                call.arguments.find_argument("encoding", 2).is_none()
            }
            "read_text" => call.arguments.find_argument("encoding", 0).is_none(),
            "write_text" => call.arguments.find_argument("encoding", 1).is_none(),
            _ => false,
        },
    }
}
```

---

_@dhruvmanila requested changes on 2024-05-06 07:30_

---

_Label `rule` added by @dhruvmanila on 2024-05-06 07:30_

---

_Comment by @augustelalande on 2024-05-08 20:44_

@dhruvmanila Done as requested.

> I was not able to detect violations of the form
> 
> ```
> x = Path("foo.txt")
> x.open()
> ```
> 
> I don't know if this is currently doable.

Any comment on this


---

_Comment by @dhruvmanila on 2024-05-09 11:04_

> @dhruvmanila Done as requested.
> 
> > I was not able to detect violations of the form
> > ```
> > x = Path("foo.txt")
> > x.open()
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > I don't know if this is currently doable.
> 
> Any comment on this

I think it's fine to not detect this now as this would fall under the bucket of type-inference.

---

_@dhruvmanila approved on 2024-05-09 11:11_

Thank you for working on this!

I've added some additional test cases and actually used `QualifiedName` for the `Regular` variant. This is to re-use the `Display` implementation on `QualifiedName`.

---

_Comment by @dhruvmanila on 2024-05-09 11:13_

The ecosystem changes looks correct.

---

_Label `preview` added by @dhruvmanila on 2024-05-09 11:13_

---

_Comment by @dhruvmanila on 2024-05-09 12:29_

After a good amount of discussion, I've decided to name it `Callee` ;)

Feel free to disagree with any of the changes made by me.

---

_Merged by @dhruvmanila on 2024-05-09 12:36_

---

_Closed by @dhruvmanila on 2024-05-09 12:36_

---

_Branch deleted on 2024-05-09 17:29_

---
