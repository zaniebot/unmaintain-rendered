```yaml
number: 10154
title: "[`flake8-bandit`] Remove `suspicious-lxml-import` (`S410`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-02-28T17:25:56Z
updated_at: 2024-03-13T20:57:00Z
url: https://github.com/astral-sh/ruff/pull/10154
synced_at: 2026-01-10T22:47:01Z
```

# [`flake8-bandit`] Remove `suspicious-lxml-import` (`S410`)

---

_Pull request opened by @charliermarsh on 2024-02-28 17:25_

## Summary

The `lxml` library has been modified to address known vulnerabilities and unsafe defaults. As such, the `defusedxml`
library is no longer necessary, `defusedxml` has deprecated its `lxml` module.

Closes https://github.com/astral-sh/ruff/issues/10030.


---

_Label `breaking` added by @charliermarsh on 2024-02-28 17:26_

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-02-28 17:26_

---

_Label `rule` added by @charliermarsh on 2024-02-28 17:27_

---

_Comment by @github-actions[bot] on 2024-02-28 17:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+26 -0 violations, +0 -0 fixes in 2 projects; 1 project error; 40 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+22 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L581'>samcli/commands/build/build_context.py:581:46:</a> E741 Ambiguous variable name: `l`
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L5'>samcli/commands/build/build_context.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L610'>samcli/commands/build/build_context.py:610:21:</a> E741 Ambiguous variable name: `l`
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L642'>samcli/commands/build/build_context.py:642:46:</a> E741 Ambiguous variable name: `l`
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L25'>samcli/commands/build/command.py:25:5:</a> F401 [*] `samcli.commands._utils.options.terraform_plan_file_option` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L5'>samcli/commands/build/command.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/core/options.py#L5'>samcli/commands/build/core/options.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L1003'>samcli/lib/build/app_builder.py:1003:28:</a> PLR2004 Magic value used in comparison, consider replacing `-32601` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L45'>samcli/lib/build/app_builder.py:45:44:</a> F401 [*] `samcli.local.docker.exceptions.ContainerNotStartableException` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L5'>samcli/lib/build/app_builder.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L991'>samcli/lib/build/app_builder.py:991:16:</a> PLR2004 Magic value used in comparison, consider replacing `400` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L991'>samcli/lib/build/app_builder.py:991:34:</a> PLR2004 Magic value used in comparison, consider replacing `500` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L995'>samcli/lib/build/app_builder.py:995:28:</a> PLR2004 Magic value used in comparison, consider replacing `505` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_graph.py#L5'>samcli/lib/build/build_graph.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_strategy.py#L30'>samcli/lib/build/build_strategy.py:30:51:</a> F401 [*] `samcli.lib.utils.architecture.ARM64` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_strategy.py#L5'>samcli/lib/build/build_strategy.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/bundler.py#L10'>samcli/lib/build/bundler.py:10:50:</a> F401 [*] `samcli.commands.local.lib.exceptions.InvalidHandlerPathError` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/bundler.py#L7'>samcli/lib/build/bundler.py:7:27:</a> F401 [*] `pathlib.PosixPath` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/dependency_hash_generator.py#L3'>samcli/lib/build/dependency_hash_generator.py:3:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/utils.py#L5'>samcli/lib/build/utils.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/workflow_config.py#L5'>samcli/lib/build/workflow_config.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/workflow_config.py#L7'>samcli/lib/build/workflow_config.py:7:42:</a> F401 [*] `typing.Tuple` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/build_tracker.py#L6'>src/pip/_internal/operations/build/build_tracker.py:6:47:</a> F401 [*] `typing.Set` imported but unused
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/build_tracker.py#L8'>src/pip/_internal/operations/build/build_tracker.py:8:39:</a> F401 [*] `pip._internal.models.link.Link` imported but unused
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/wheel_legacy.py#L43'>src/pip/_internal/operations/build/wheel_legacy.py:43:15:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/wheel_legacy.py#L49'>src/pip/_internal/operations/build/wheel_legacy.py:49:15:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 9 | 9 | 0 | 0 | 0 |
| F401 | 8 | 8 | 0 | 0 | 0 |
| PLR2004 | 4 | 4 | 0 | 0 | 0 |
| E741 | 3 | 3 | 0 | 0 | 0 |
| UP032 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+45 -3 violations, +0 -0 fixes in 4 projects; 1 project error; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/dc08e6acc2def50c88c7a2425c300a61338f0b0a/airflow/providers/amazon/aws/hooks/base_aws.py#L338'>airflow/providers/amazon/aws/hooks/base_aws.py:338:14:</a> S410 `lxml` is vulnerable to XML attacks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+41 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L53'>samcli/commands/build/build_context.py:53:1:</a> PLR0904 Too many public methods (26 > 20)
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L54'>samcli/commands/build/build_context.py:54:9:</a> PLR0917 Too many positional arguments (26/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L581'>samcli/commands/build/build_context.py:581:46:</a> E741 Ambiguous variable name: `l`
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L5'>samcli/commands/build/build_context.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L610'>samcli/commands/build/build_context.py:610:21:</a> E741 Ambiguous variable name: `l`
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L642'>samcli/commands/build/build_context.py:642:46:</a> E741 Ambiguous variable name: `l`
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L139'>samcli/commands/build/command.py:139:5:</a> PLR0917 Too many positional arguments (25/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L200'>samcli/commands/build/command.py:200:5:</a> PLR0917 Too many positional arguments (22/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L228'>samcli/commands/build/command.py:228:5:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L25'>samcli/commands/build/command.py:25:5:</a> F401 [*] `samcli.commands._utils.options.terraform_plan_file_option` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/command.py#L5'>samcli/commands/build/command.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/core/options.py#L5'>samcli/commands/build/core/options.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L1003'>samcli/lib/build/app_builder.py:1003:28:</a> PLR2004 Magic value used in comparison, consider replacing `-32601` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L458'>samcli/lib/build/app_builder.py:458:9:</a> PLR0917 Too many positional arguments (10/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L45'>samcli/lib/build/app_builder.py:45:44:</a> F401 [*] `samcli.local.docker.exceptions.ContainerNotStartableException` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L593'>samcli/lib/build/app_builder.py:593:9:</a> PLR0917 Too many positional arguments (11/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L5'>samcli/lib/build/app_builder.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L732'>samcli/lib/build/app_builder.py:732:9:</a> PLR0917 Too many positional arguments (8/5)
... 7 additional changes omitted for rule PLR0917
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L991'>samcli/lib/build/app_builder.py:991:16:</a> PLR2004 Magic value used in comparison, consider replacing `400` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L991'>samcli/lib/build/app_builder.py:991:34:</a> PLR2004 Magic value used in comparison, consider replacing `500` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L995'>samcli/lib/build/app_builder.py:995:28:</a> PLR2004 Magic value used in comparison, consider replacing `505` with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_graph.py#L379'>samcli/lib/build/build_graph.py:379:13:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_graph.py#L471'>samcli/lib/build/build_graph.py:471:13:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_graph.py#L515'>samcli/lib/build/build_graph.py:515:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_graph.py#L579'>samcli/lib/build/build_graph.py:579:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_graph.py#L5'>samcli/lib/build/build_graph.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_strategy.py#L30'>samcli/lib/build/build_strategy.py:30:51:</a> F401 [*] `samcli.lib.utils.architecture.ARM64` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/build_strategy.py#L5'>samcli/lib/build/build_strategy.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
... 4 additional changes omitted for rule I001
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/bundler.py#L10'>samcli/lib/build/bundler.py:10:50:</a> F401 [*] `samcli.commands.local.lib.exceptions.InvalidHandlerPathError` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/bundler.py#L7'>samcli/lib/build/bundler.py:7:27:</a> F401 [*] `pathlib.PosixPath` imported but unused
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/utils.py#L17'>samcli/lib/build/utils.py:17:28:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/workflow_config.py#L7'>samcli/lib/build/workflow_config.py:7:42:</a> F401 [*] `typing.Tuple` imported but unused
... 1 additional changes omitted for rule F401
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/build_tracker.py#L6'>src/pip/_internal/operations/build/build_tracker.py:6:47:</a> F401 [*] `typing.Set` imported but unused
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/build_tracker.py#L8'>src/pip/_internal/operations/build/build_tracker.py:8:39:</a> F401 [*] `pip._internal.models.link.Link` imported but unused
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/wheel_legacy.py#L43'>src/pip/_internal/operations/build/wheel_legacy.py:43:15:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/wheel_legacy.py#L49'>src/pip/_internal/operations/build/wheel_legacy.py:49:15:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/5d604a44299b352fd702d85b5b1d49b3f3f0d8d9/zerver/migrations/0257_fix_has_link_attribute.py#L6'>zerver/migrations/0257_fix_has_link_attribute.py:6:8:</a> S410 `lxml` is vulnerable to XML attacks
- <a href='https://github.com/zulip/zulip/blob/5d604a44299b352fd702d85b5b1d49b3f3f0d8d9/zerver/views/documentation.py#L12'>zerver/views/documentation.py:12:6:</a> S410 `lxml` is vulnerable to XML attacks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>
<details><summary>Changes by rule (12 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 12 | 12 | 0 | 0 | 0 |
| I001 | 9 | 9 | 0 | 0 | 0 |
| F401 | 8 | 8 | 0 | 0 | 0 |
| PLR2004 | 4 | 4 | 0 | 0 | 0 |
| E741 | 3 | 3 | 0 | 0 | 0 |
| S410 | 3 | 0 | 3 | 0 | 0 |
| PLW1514 | 2 | 2 | 0 | 0 | 0 |
| PLW1641 | 2 | 2 | 0 | 0 | 0 |
| UP032 | 2 | 2 | 0 | 0 | 0 |
| PLR0904 | 1 | 1 | 0 | 0 | 0 |
| PLC0415 | 1 | 1 | 0 | 0 | 0 |
| PLR6201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-02-28 17:38_

---

_Closed by @charliermarsh on 2024-02-28 17:38_

---

_Branch deleted on 2024-02-28 17:38_

---

_Comment by @T-256 on 2024-02-28 20:58_

> [pypa/pip](https://github.com/pypa/pip) (+4 -0 violations, +0 -0 fixes)
> + [src/pip/_internal/operations/build/build_tracker.py:6:47:](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/build_tracker.py#L6) F401 [*] `typing.Set` imported but unused
> + [src/pip/_internal/operations/build/build_tracker.py:8:39:](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/build_tracker.py#L8) F401 [*] `pip._internal.models.link.Link` imported but unused
> + [src/pip/_internal/operations/build/wheel_legacy.py:43:15:](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/wheel_legacy.py#L43) UP032 [*] Use f-string instead of `format` call
> + [src/pip/_internal/operations/build/wheel_legacy.py:49:15:](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/wheel_legacy.py#L49) UP032 [*] Use f-string instead of `format` call


These are unrelated ecosystem change, I think for preventing them at here, possible solution could be using tag or static revision of git projects instead of dynamic main/master branch.
https://github.com/astral-sh/ruff/blob/0293908b71eeaf610d8ce77d2950f0602fd88dc5/python/ruff-ecosystem/ruff_ecosystem/defaults.py#L16-L21

Over time you can update the tags manually or by a script runs every month.

---

_Label `breaking` removed by @MichaReiser on 2024-02-29 09:22_

---

_Comment by @MichaReiser on 2024-02-29 09:24_

I think we shouldn't consider this a breaking change because it is a preview rule. Unfortunately, our versioning policy isn't explicit about if it is or isn't.

---

_Comment by @diinngg on 2024-03-13 20:55_

Should [S320](https://docs.astral.sh/ruff/rules/suspicious-xmle-tree-usage/) also be removed? It looks to be specific to lxml

---
