```yaml
number: 10155
title: Fix the ecosystem check
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: AlexWaygood-patch-1
created_at: 2024-02-28T17:35:13Z
updated_at: 2024-02-28T17:53:48Z
url: https://github.com/astral-sh/ruff/pull/10155
synced_at: 2026-01-10T22:57:10Z
```

# Fix the ecosystem check

---

_Pull request opened by @AlexWaygood on 2024-02-28 17:35_

We need to bump the number of files we expect to have formatting violations following #10093


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-02-28 17:35_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-02-28 17:35_

---

_@MichaReiser approved on 2024-02-28 17:36_

Thanks

---

_Label `ci` added by @MichaReiser on 2024-02-28 17:38_

---

_Merged by @AlexWaygood on 2024-02-28 17:42_

---

_Closed by @AlexWaygood on 2024-02-28 17:42_

---

_Branch deleted on 2024-02-28 17:42_

---

_Comment by @github-actions[bot] on 2024-02-28 17:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+26 -0 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

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
ℹ️ ecosystem check **detected linter changes**. (+45 -0 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

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
<details><summary>Changes by rule (11 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 12 | 12 | 0 | 0 | 0 |
| I001 | 9 | 9 | 0 | 0 | 0 |
| F401 | 8 | 8 | 0 | 0 | 0 |
| PLR2004 | 4 | 4 | 0 | 0 | 0 |
| E741 | 3 | 3 | 0 | 0 | 0 |
| PLW1514 | 2 | 2 | 0 | 0 | 0 |
| PLW1641 | 2 | 2 | 0 | 0 | 0 |
| UP032 | 2 | 2 | 0 | 0 | 0 |
| PLR0904 | 1 | 1 | 0 | 0 | 0 |
| PLC0415 | 1 | 1 | 0 | 0 | 0 |
| PLR6201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+3 -1 lines in 1 file in 1 projects; 1 project error; 41 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+3 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L644'>samcli/lib/build/app_builder.py~L644</a>
```diff
             # pylint: disable=fixme
             # FIXME: _build_lambda_image assumes metadata is not None, we need to throw an exception here
             return self._build_lambda_image(
-                function_name=function_name, metadata=metadata, architecture=architecture  # type: ignore
+                function_name=function_name,
+                metadata=metadata,
+                architecture=architecture,  # type: ignore
             )
         if packagetype == ZIP:
             if runtime in self._deprecated_runtimes:
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+48 -64 lines in 7 files in 2 projects; 1 project error; 40 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+45 -58 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/commands/build/build_context.py#L597'>samcli/commands/build/build_context.py~L597</a>
```diff
         """
         result = ResourcesToBuildCollector()
         excludes: Tuple[str, ...] = self._exclude if self._exclude is not None else ()
-        result.add_functions(
-            [
-                f
-                for f in self.function_provider.get_all()
-                if (f.name not in excludes) and f.function_build_info.is_buildable()
-            ]
-        )
-        result.add_layers(
-            [
-                l
-                for l in self.layer_provider.get_all()
-                if (l.name not in excludes) and BuildContext.is_layer_buildable(l)
-            ]
-        )
+        result.add_functions([
+            f
+            for f in self.function_provider.get_all()
+            if (f.name not in excludes) and f.function_build_info.is_buildable()
+        ])
+        result.add_layers([
+            l for l in self.layer_provider.get_all() if (l.name not in excludes) and BuildContext.is_layer_buildable(l)
+        ])
         return result
 
     @property
```
<a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/app_builder.py#L644'>samcli/lib/build/app_builder.py~L644</a>
```diff
             # pylint: disable=fixme
             # FIXME: _build_lambda_image assumes metadata is not None, we need to throw an exception here
             return self._build_lambda_image(
-                function_name=function_name, metadata=metadata, architecture=architecture  # type: ignore
+                function_name=function_name,
+                metadata=metadata,
+                architecture=architecture,  # type: ignore
             )
         if packagetype == ZIP:
             if runtime in self._deprecated_runtimes:
```
<a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/bundler.py#L130'>samcli/lib/build/bundler.py~L130</a>
```diff
                 if not isinstance(existing_options, str):
                     invalid_node_option = True
                 else:
-                    template_resource["Properties"]["Environment"]["Variables"]["NODE_OPTIONS"] = " ".join(
-                        [existing_options, "--enable-source-maps"]
-                    )
+                    template_resource["Properties"]["Environment"]["Variables"]["NODE_OPTIONS"] = " ".join([
+                        existing_options,
+                        "--enable-source-maps",
+                    ])
 
                 using_source_maps = True
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/cd539c5f8aa94f9020d2188b6d09a2348cd93190/samcli/lib/build/workflow_config.py#L166'>samcli/lib/build/workflow_config.py~L166</a>
```diff
         "go1.x": BasicWorkflowSelector(GO_MOD_CONFIG),
         # When Maven builder exists, add to this list so we can automatically choose a builder based on the supported
         # manifest
-        "java8": ManifestWorkflowSelector(
-            [
-                # Gradle builder needs custom executable paths to find `gradlew` binary
-                JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_MAVEN_CONFIG,
-            ]
-        ),
-        "java11": ManifestWorkflowSelector(
-            [
-                # Gradle builder needs custom executable paths to find `gradlew` binary
-                JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_MAVEN_CONFIG,
-            ]
-        ),
-        "java8.al2": ManifestWorkflowSelector(
-            [
-                # Gradle builder needs custom executable paths to find `gradlew` binary
-                JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_MAVEN_CONFIG,
-            ]
-        ),
-        "java17": ManifestWorkflowSelector(
-            [
-                # Gradle builder needs custom executable paths to find `gradlew` binary
-                JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_MAVEN_CONFIG,
-            ]
-        ),
-        "java21": ManifestWorkflowSelector(
-            [
-                # Gradle builder needs custom executable paths to find `gradlew` binary
-                JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
-                JAVA_MAVEN_CONFIG,
-            ]
-        ),
+        "java8": ManifestWorkflowSelector([
+            # Gradle builder needs custom executable paths to find `gradlew` binary
+            JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_MAVEN_CONFIG,
+        ]),
+        "java11": ManifestWorkflowSelector([
+            # Gradle builder needs custom executable paths to find `gradlew` binary
+            JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_MAVEN_CONFIG,
+        ]),
+        "java8.al2": ManifestWorkflowSelector([
+            # Gradle builder needs custom executable paths to find `gradlew` binary
+            JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_MAVEN_CONFIG,
+        ]),
+        "java17": ManifestWorkflowSelector([
+            # Gradle builder needs custom executable paths to find `gradlew` binary
+            JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_MAVEN_CONFIG,
+        ]),
+        "java21": ManifestWorkflowSelector([
+            # Gradle builder needs custom executable paths to find `gradlew` binary
+            JAVA_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_KOTLIN_GRADLE_CONFIG._replace(executable_search_paths=[code_dir, project_dir]),
+            JAVA_MAVEN_CONFIG,
+        ]),
         "provided": BasicWorkflowSelector(PROVIDED_MAKE_CONFIG),
         "provided.al2": BasicWorkflowSelector(PROVIDED_MAKE_CONFIG),
         "provided.al2023": BasicWorkflowSelector(PROVIDED_MAKE_CONFIG),
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+3 -6 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/metadata.py#L1'>src/pip/_internal/operations/build/metadata.py~L1</a>
```diff
-"""Metadata generation logic for source distributions.
-"""
+"""Metadata generation logic for source distributions."""
 
 import os
 
```
<a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/metadata_editable.py#L1'>src/pip/_internal/operations/build/metadata_editable.py~L1</a>
```diff
-"""Metadata generation logic for source distributions.
-"""
+"""Metadata generation logic for source distributions."""
 
 import os
 
```
<a href='https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/operations/build/metadata_legacy.py#L1'>src/pip/_internal/operations/build/metadata_legacy.py~L1</a>
```diff
-"""Metadata generation logic for legacy source distributions.
-"""
+"""Metadata generation logic for legacy source distributions."""
 
 import logging
 import os
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---
