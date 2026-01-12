```yaml
number: 16565
title: "[`isort`] Check full module path against project root(s) when categorizing first-party"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
  - isort
  - preview
assignees: []
merged: true
base: main
head: namespaces
created_at: 2025-03-08T09:43:44Z
updated_at: 2025-05-05T16:40:01Z
url: https://github.com/astral-sh/ruff/pull/16565
synced_at: 2026-01-12T15:55:55Z
```

# [`isort`] Check full module path against project root(s) when categorizing first-party

---

_@dylwil3_

When attempting to determine whether `import foo.bar.baz` is a known first-party import relative to [user-provided source paths](https://docs.astral.sh/ruff/settings/#src), when `preview` is enabled we now check that `SRC/foo/bar/baz` is a directory or `SRC/foo/bar/baz.py` or `SRC/foo/bar/baz.pyi` exist.

Previously, we just checked the analogous thing for `SRC/foo`, but this can be misleading in situations with disjoint namespace packages that share a common base name (e.g. we may be working inside the namespace package `foo.buzz` and importing `foo.bar` from elsewhere).

Supersedes #12987 
Closes #12984

# Testing

We've added some unit tests displaying the different behavior in the function `match_sources` which implements this logic. In addition, here are the results of an ad-hoc, local test and comparison with `isort`. Notice that the preview behavior now coincides with `isort` - including the inconsistent classification of `from` imports.

(I've manually edited out some of the irrelevant logs and such in the outputs)

```console
❯ tree
.
├── pyproject.toml
└── src
    └── mynamespace
        └── subpackage_a
            └── __init__.py

❯ ../target/debug/ruff check src/mynamespace/subpackage_a/__init__.py --no-cache --select I -v
[2025-04-13][12:33:24][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'mynamespace.subpackage_b' as Known(FirstParty) (SourceMatch("/Users/dmbp/Documents/dev/contrib/ruff/exampleproj/src"))
[2025-04-13][12:33:24][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'mynamespace' as Known(FirstParty) (SourceMatch("/Users/dmbp/Documents/dev/contrib/ruff/exampleproj/src"))
All checks passed!

❯ ../target/debug/ruff check src/mynamespace/subpackage_a/__init__.py --no-cache --select I -v --preview
[2025-04-13][12:33:30][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'mynamespace.subpackage_b' as Known(ThirdParty) (NoMatch)
[2025-04-13][12:33:30][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'mynamespace' as Known(FirstParty) (SourceMatch("/Users/dmbp/Documents/dev/contrib/ruff/exampleproj/src"))
src/mynamespace/subpackage_a/__init__.py:1:1: I001 [*] Import block is un-sorted or un-formatted
  |
1 | / import mynamespace.subpackage_b
2 | | from mynamespace import subpackage_b
  | |____________________________________^ I001
  |
  = help: Organize imports

Found 1 error.
[*] 1 fixable with the `--fix` option.

❯ uvx isort . --check --diff -v --src src

else-type place_module for mynamespace.subpackage_b returned THIRDPARTY
from-type place_module for mynamespace returned FIRSTPARTY
ERROR: /Users/dmbp/Documents/dev/contrib/ruff/exampleproj/src/mynamespace/subpackage_a/__init__.py Imports are incorrectly sorted and/or formatted.
else-type place_module for mynamespace.subpackage_b returned THIRDPARTY
from-type place_module for mynamespace returned FIRSTPARTY
--- /Users/dmbp/Documents/dev/contrib/ruff/exampleproj/src/mynamespace/subpackage_a/__init__.py:before	2025-04-13 12:29:49.050993
+++ /Users/dmbp/Documents/dev/contrib/ruff/exampleproj/src/mynamespace/subpackage_a/__init__.py:after	2025-04-13 12:36:28.299245
@@ -1,2 +1,3 @@
 import mynamespace.subpackage_b
+
 from mynamespace import subpackage_b
```

# Summary of Ecosystem Checks

## `snowcli`
These are all true positives! The package in this repo is `snowflake.cli`, and the ecosystem reports all have to do with importing third-party packages like `snowflake.snowpark` and `snowflake.connector`. 

## `airflow`
Also true positives: the dependency `docs.utils.conf_constants` appears in various places and is correctly identified as third-party, whereas before it was considered first-party because a `docs` folder.

## `freedomofpress`
This is a false positive because this repo contains a Python file that does not have a `.py` suffix, but instead uses the shebang specification: https://github.com/freedomofpress/securedrop/blob/develop/journalist_gui/SecureDropUpdater

## `mlflow`

The _change_ is a true positive, but there is a leftover false positive. The change concerns two imports:

```python
import docker
from docker.errors import APIError, BuildError
```

On `main`, these are _both_ incorrectly identified as first-party due to a local folder named `docker`. In this PR with preview enabled, `docker.errors` is correctly identified as third-party, which causes a lint error to be emitted.

## `pytest`
The problematic import here is `from py.path import error` which is, unfortunately, incorrectly labeled as third-party in `preview`. This is because it is not possible to know this import is first party from the file system alone! Indeed, `path` is actually a submodule of a module called `_py`, and then we have:

```python
# src/py.py
import _pytest._py.path as path
```

If that's not bad enough, if you go to `src/_pytest/_py` you will not find `path/error` instead you will find `path.py` which has this line:

```python
# path.py
from . import error
```

I don't think it's in scope to support something like that here! Maybe one day `ty` will power our import sorting!

---

_Label `bug` added by @dylwil3 on 2025-03-08 09:43_

---

_Label `rule` added by @dylwil3 on 2025-03-08 09:43_

---

_Label `isort` added by @dylwil3 on 2025-03-08 09:43_

---

_Comment by @github-actions[bot] on 2025-03-08 09:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+22 -13 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+15 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/scripts/cleanup.py#L14'>scripts/cleanup.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/connection/util.py#L15'>src/snowflake/cli/_plugins/connection/util.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/logs/manager.py#L1'>src/snowflake/cli/_plugins/logs/manager.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/logs/utils.py#L1'>src/snowflake/cli/_plugins/logs/utils.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/nativeapp/sf_sql_facade.py#L14'>src/snowflake/cli/_plugins/nativeapp/sf_sql_facade.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/project/manager.py#L14'>src/snowflake/cli/_plugins/project/manager.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/spcs/compute_pool/manager.py#L15'>src/snowflake/cli/_plugins/spcs/compute_pool/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/spcs/image_repository/manager.py#L15'>src/snowflake/cli/_plugins/spcs/image_repository/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/sql/manager.py#L15'>src/snowflake/cli/_plugins/sql/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/sql/snowsql_commands.py#L1'>src/snowflake/cli/_plugins/sql/snowsql_commands.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/_plugins/stage/diff.py#L15'>src/snowflake/cli/_plugins/stage/diff.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/api/cli_global_context.py#L15'>src/snowflake/cli/api/cli_global_context.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/api/connections.py#L15'>src/snowflake/cli/api/connections.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/api/entities/common.py#L1'>src/snowflake/cli/api/entities/common.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/src/snowflake/cli/api/sql_execution.py#L15'>src/snowflake/cli/api/sql_execution.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py#L14'>test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests/nativeapp/utils.py#L15'>tests/nativeapp/utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests/test_data/projects/glob_patterns/main.py#L1'>tests/test_data/projects/glob_patterns/main.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests/test_data/projects/glob_patterns/src/app.py#L1'>tests/test_data/projects/glob_patterns/src/app.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests/test_data/projects/glob_patterns_zip/main.py#L1'>tests/test_data/projects/glob_patterns_zip/main.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests/test_data/projects/glob_patterns_zip/src/app.py#L1'>tests/test_data/projects/glob_patterns_zip/src/app.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests/test_main.py#L17'>tests/test_main.py:17:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests_integration/nativeapp/test_debug_mode.py#L15'>tests_integration/nativeapp/test_debug_mode.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests_integration/nativeapp/test_telemetry_errors.py#L14'>tests_integration/nativeapp/test_telemetry_errors.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests_integration/notebook/test_notebooks.py#L15'>tests_integration/notebook/test_notebooks.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests_integration/spcs/testing_utils/image_repository_utils.py#L1'>tests_integration/spcs/testing_utils/image_repository_utils.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests_integration/test_config.py#L15'>tests_integration/test_config.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/7db8e8eed4bceb3e7aafd427e0294b18dfb24543/tests_integration/testing_utils/sql_utils.py#L15'>tests_integration/testing_utils/sql_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3b80419e28a6ea95246b546460ffba78d09f3255/airflow-core/docs/conf.py#L21'>airflow-core/docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/apache/airflow/blob/3b80419e28a6ea95246b546460ffba78d09f3255/chart/docs/conf.py#L21'>chart/docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/apache/airflow/blob/3b80419e28a6ea95246b546460ffba78d09f3255/docker-stack-docs/conf.py#L21'>docker-stack-docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/apache/airflow/blob/3b80419e28a6ea95246b546460ffba78d09f3255/providers-summary-docs/conf.py#L21'>providers-summary-docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/216a5ca20d9075ec8a0a53f3d6b017522961a039/journalist_gui/test_gui.py#L1'>journalist_gui/test_gui.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/cd5ad85d6ed18c7298135a2ead3534267ca18bb2/tests/projects/utils.py#L50'>tests/projects/utils.py:50:5:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/testing/_py/test_local.py#L2'>testing/_py/test_local.py:2:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 35 | 22 | 13 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Renamed from "[`isort`] Only infer subpackages of namespace packages as first-party" to "[`isort`] Check full module path against project root(s) when categorizing first-party" by @dylwil3 on 2025-03-27 21:40_

---

_Marked ready for review by @dylwil3 on 2025-03-27 21:46_

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-03-27 21:46_

---

_Comment by @MichaReiser on 2025-04-01 12:18_

I think that PR slipped our attention last week. At least I didn't notice it :)

---

_Comment by @MichaReiser on 2025-04-01 12:19_

Did you click through the ecosystem checks?

I'm torn but I think this should be gated behind preview. The ecosystem fallouts are significant enough.

---

_Comment by @AlexWaygood on 2025-04-01 12:20_

I haven't forgotten about this. I'm still waging war against my notification queue after the on-site 😢

---

_Comment by @dylwil3 on 2025-04-02 10:34_

> I think that PR slipped our attention last week. At least I didn't notice it :)
> I haven't forgotten about this. I'm still waging war against my notification queue after the on-site 😢

No worries!

> Did you click through the ecosystem checks?

I did quickly before, but going through them again I noticed two examples that need more investigation.

- `freedomofpress` has a strange directory layout leading to a false positive, but it's possible that this is expected and they just need to specify a different `tool.ruff.src`
- `mlflow` has a directory named `docker` with a Dockerfile in it... and imports a Python package named `docker`. This caused a _silent_ error before where both imports `docker` and `docker.errors` were labeled as first party, but, since they were the only imports present, this had no effect on import sorting. But now we see that there is not `docker/errors` and label the second as third party.

> I'm torn but I think this should be gated behind preview. The ecosystem fallouts are significant enough.

Good point - I'll try to sort out the least ugly way to do that...

-------

Finally, I think I should try to fix the following limitation of the current approach. When we see 

```python
from foo import bar
```

the former approach would look for a `foo` directory or a `foo.py` file and call it a day. The present approach does the same - we don't look for a `bar` subdirectory because we don't know whether `bar` is supposed to be an imported _object_ or _module_. In fact, one source of false positives in the ecosystem check, and the reason for introducing `.module_source`, was that `categorize` was being passed the fully qualified name `foo.bar`, failing to find `foo/bar.py` or `foo/bar/`, and then declaring the import third party. So I think I need to add some logic around this, otherwise we could get different categorizations for importing a submodule depending on whether you did

```python
from foo import bar
```

or 

```python
import foo.bar as bar
```


I'm going to revert back to draft while I sort out all of the above. Sorry for the needless ping!


---

_Converted to draft by @dylwil3 on 2025-04-02 10:34_

---

_Label `preview` added by @dylwil3 on 2025-04-13 17:22_

---

_Marked ready for review by @dylwil3 on 2025-04-13 17:39_

---

_Comment by @MichaReiser on 2025-04-23 07:32_

I haven't looked at the code yet but could you update the PR summary or add a new comment that replies to your ecosystem findings and explains why we think they're correct. This will help us to remember the reasoning when writing the changelog in the next minor release

---

_Review comment by @MichaReiser on `crates/ruff_linter/Cargo.toml`:79 on 2025-04-23 07:33_

```suggestion
tempfile = { workspace = true }
```

I don't know why we use the long form, I think @charliermarsh migrated it some time ago but I prefer to keep using a consistent format.

---

_@MichaReiser reviewed on 2025-04-23 07:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:317 on 2025-04-23 07:37_

I'll use this PR to establish a new practice around preview checks that, hopefully, will make promoting preview features easier (and finding them!)

Let's create a `preview.rs` file similar to the one we have in the [formatter](https://github.com/astral-sh/ruff/blob/1aad180aae84cd5d12d5a47c879183f559c871ed/crates/ruff_python_formatter/src/preview.rs#L1-L0) where we create a dedicated function for each preview functionality of the form:

```
def fn is_xy_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}
```

This has the advantage that:

1. It's easy to find all preview behavior and it gives us a place to document it (including instructions on what needs to be changed when promoting this behavior, e.g. update the documentations for x, y, z)
2. It's easy to promote the preview behavior. Simply delete the function and Rust will guide you to change all call sites





---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:315 on 2025-04-23 07:39_

Nit: Maybe add a `QualifiedName.to_dotted_name()` function

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/mod.rs`:175 on 2025-04-23 07:40_

Would it make sense to move this check into `categorize_imports` instead of repeating it on every call site?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/categorize.rs`:203 on 2025-04-23 07:45_

I find the early return here hard to understand. Why is it necessary? What I understand is that this can only be true if `name` is empty (has no components). Should we instead have a test if name is empty and early return in `match_sources`? Is it even possible that `name` is empty?

---

_Review comment by @MichaReiser on `docs/faq.md`:315 on 2025-04-23 07:47_

Can we add some documentation explaining how to work around the case you found in the ecosystem check where a project has a `docker` folder but also uses the `docker` third party module?

---

_@MichaReiser approved on 2025-04-23 07:49_

Thanks to work on this. This looks good to me. I've a few smaller nit comments and I think we need to extend the documentation and the motivation for the change a bit before landing.

---

_@dylwil3 reviewed on 2025-04-26 19:39_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:317 on 2025-04-26 19:39_

Is this the sort of thing you mean? https://github.com/astral-sh/ruff/pull/17646

---

_@dylwil3 reviewed on 2025-04-26 20:16_

---

_Review comment by @dylwil3 on `docs/faq.md`:315 on 2025-04-26 20:16_

I've added some... I'm tempted to say something like "this is much less likely to happen if you use the `src` layout" but I was worried it would come across a bit harsh.

---

_@dylwil3 reviewed on 2025-04-26 20:16_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/isort/categorize.rs`:203 on 2025-04-26 20:16_

I think it's written that way because `components()` doesn't have an `is_empty` method. in theory if `name` was, say "." or "...." then this would trigger. I've turned it into an explicit check.

I think that won't ever actually happen by the time we get to `match_sources` because `categorize` has already handled the relative import case by then - but I didn't want this function to depend on that assumption.

---

_@dylwil3 reviewed on 2025-04-26 20:17_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:315 on 2025-04-26 20:17_

`import.source_name()` is just a slice not a `QualifiedName`, and `import` is something implementing the `Import` trait - so it seems like it could be somewhat disruptive to put that method everywhere? But I'm happy to do ti if you still think I should!

---

_@dylwil3 reviewed on 2025-04-26 20:18_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/isort/categorize.rs`:203 on 2025-04-26 20:18_

Haha never mind it's clippy's fault:

```
error: this block may be rewritten with the `?` operator
   --> crates/ruff_linter/src/rules/isort/categorize.rs:204:13
    |
204 | /             if relative_path.components().next().is_none() {
205 | |                 return None;
206 | |             }
    | |_____________^ help: replace it with: `relative_path.components().next()?;`
```

---

_Comment by @dylwil3 on 2025-04-26 20:24_

Note: There have been some other issues raised in #10519 but I think they are orthogonal to this fix (the issues are present both before and after the current change) so it's worth exploring solutions in a followup.

---

_@MichaReiser reviewed on 2025-04-27 09:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:317 on 2025-04-27 09:56_

Yes! I didn't expect that you would change all existing preview usages too. Thank you

---

_@dylwil3 reviewed on 2025-04-28 21:56_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:317 on 2025-04-28 21:56_

Ok this is now gated using the new `preview` module!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-05 13:04_

---

_Comment by @AlexWaygood on 2025-05-05 13:05_

I basically lost all context on this because the work I did on it was such a long time ago :-(

if @MichaReiser is happy with this, I'm happy with it! I trust both of you to get it right 😃

---

_Merged by @dylwil3 on 2025-05-05 16:40_

---

_Closed by @dylwil3 on 2025-05-05 16:40_

---
