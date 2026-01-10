```yaml
number: 20200
title: "[`pyflakes`] Handle some common submodule import situations for `unused-import` (`F401`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
  - great writeup
assignees: []
merged: true
base: main
head: unused-imports
created_at: 2025-09-01T20:10:41Z
updated_at: 2025-09-26T13:22:32Z
url: https://github.com/astral-sh/ruff/pull/20200
synced_at: 2026-01-10T17:40:28Z
```

# [`pyflakes`] Handle some common submodule import situations for `unused-import` (`F401`)

---

_Pull request opened by @dylwil3 on 2025-09-01 20:10_

# Summary

The PR under review attempts to make progress towards the age-old problem of submodule imports, specifically with regards to their treatment by the rule [`unused-import` (`F401`)](https://docs.astral.sh/ruff/rules/unused-import/).

Some related issues:
- https://github.com/astral-sh/ruff/issues/60
- https://github.com/astral-sh/ruff/issues/4656

Prior art:
- https://github.com/astral-sh/ruff/pull/13965
- https://github.com/astral-sh/ruff/pull/5010
- https://github.com/astral-sh/ruff/pull/5011
- https://github.com/astral-sh/ruff/pull/666

One distinguishing feature of this PR is that we have attempted to be less ambitious in the hopes that this will improve the chances of landing nonzero progress into `main`.

## What the People Want

Users expect to see the following behavior in these common situations:

```python
# Example 1
import a    # ok
import a.b  # F401: unused-import

a.foo()
```

```python
# Example 2
import a    # F401: unused-import
import a.b  # ok

a.b.bar()
```

```python
# Example 3
import a    # ok
import a.b  # ok

a.foo()
a.b.bar()
```

The following situations are unintuitive to users, but are valid Python and we are probably forced to proceed as indicated:

```python
# Example 4
import a.b  # ok

a.foo()
```

```python
# Example 5
import a.b  # F401: unused-import
import a.c  # ok

a.x.baz()
```

The goal of this PR is to modify the implementation of `unused-import` to match this expectation.

## Modeling the Python in Users' Brains

Given the expected behavior above, how might a typical user be modeling Python in their minds? One possibility is as follows. Upon seeing the statement:

```python
import a.b
```

the user thinks:

> \[User\]: We have made available the symbol `a`, which is a module, but we are only allowed to access members of this module that have the form `a.b.*`.

This happens to be incorrect. Rather than _restrict_ what is available to access on `a`, the opposite happens:

> \[Python\]: We have executed the equivalent of the statement `import a`, and, in particular, can access any top-level member of `a` made available by this import. We _then_ import the submodule `b`, making possibly even more members available.

Similarly, when this user sees:

```python
import a
import a.b

a.foo()
a.b.bar()
```

they think:

> [User] The line `a.foo()` is only allowed because of the first import. So that is definitely needed. The second import may or may not be needed depending on whether `a/__init__.py` imports `b`.

when really the opposite is true:

> [Python] The first import is totally redundant and is not used. The call `a.foo()` references the symbol `a` that is being loaded in on the _second_ line.

These views are actually incompatible, so it is not possible to model the user's mind without running afoul of Examples 4 and 5 above.

## The Worst of Both Worlds

To model both the behavior that the user expects _and_ the unintuitive behavior that Python allows, we adopt an approach I'll call "Thanks I Hate It (TIHI)". It is the worst of both worlds, and it alters how we think about accessing members of a module. Again we will consider the example:

```python
import a
import a.b

a.foo()
a.b.bar()
```

> \[TIHI\]: Following Python, the import statements both make available the symbol `a` and all of it's top-level members. The second import statement also makes available the members of the form `a.b.*`. However, the attribute call `a.foo` will access `a` _via the first import statement_ and the call `a.b.bar` will access `a.b` via the second.

In general the TIHI model will resolve an attribute load of the form `a.a1...an` (for a module `a`) by:

1. Finding the maximum prefix match amongst all available
   import statements `import a.b1...bm`,
1. Among these, finding those of minimal path-length, and
1. Adding a reference to the binding created by the nearest
   of these.

## What we actually do

Rather than force Ruff's semantic model to be incorrect by actually implementing TIHI, we hack TIHI into the logic of `unused-import` itself. Doing this is somewhat awkward due to the nature of the implementation of `unused-import`.

Let me briefly review the current implementation and then explain what we do differently.

### Current implementation
After having the semantic model visit the entire module, we look at the current _live_ import bindings that have no references. There are various other filters and logic then applied, but our essential starting bucket for this rule consists of that: bindings to import statements that are live at the end of the file and have no references.

In particular, if the module consists solely of:

```python
import a
import a.b
```

then our starting "bucket" where we might emit a lint does not include `import a` since the symbol `a` is shadowed by the statement `import a.b`.

### This PR

From now on we will refer to the current behavior as the "stable" behavior. In this PR we introduce preview behavior under very specific assumptions designed to limit the scope of this PR to handle the most common cases where users get tripped up. If any of these assumptions are not met then we revert to the stable behavior.

To begin, instead of only considering import bindings that are live at the end of the module, we also consider all bindings shadowed by these. We assume that all of these bindings are also import bindings - specifically simple imports and submodule imports without aliases, not `from` imports. If not - revert to stable behavior. Next, we have to decide which of these statements have been "used" in the sense of the "TIHI" model above. So we iterate through the references to the last binding and decide which import statement is _intuitively_ being referenced, according to the matching logic we described earlier.

As a technical note here, a call like `a.b.foo()` will create a reference to the symbol `a` - so we will not see the full attribute access. We must therefore crawl up the AST and collect the qualified name of the attribute access. We assume that all references arise from a `Name` node or from appearance in a binding to `__all__` - if we find some other kind of reference, we revert to stable behavior.

Having marked each import statement as used or unused, we collect the unused ones and then proceed as in the stable behavior.

## Questions

> What if the submodule import has a side-effect and so is used? For example, if `a/b.py` contains `import a.x` but `a/__init__.py` does not. Then `a.x.baz` really _does_ need `import a.b`.

We already don't model side-effects, even for simple module imports like `import a`. So the user would be required to suppress the lint for such an anti-pattern.

> Shouldn't you do ____ to avoid allocating and performance regressions?

Maybe! The benchmarks did not show a regression, but if we want to pre-emptively do that I'm happy to. Some places where it may make sense are:

- We currently allocate a vector of size 1 in a common situation where we "bail" to the stable behavior when iterating over bindings. This is to make the opaque type of various iterators the same. But we could avoid that.
- We could use something like `SmallVec` when collecting candidate unused import bindings shadowing a given one, since there will often not be very many
- We could use a bitmask to mark used bindings (for a given symbol) as long as we are only dealing with < 64 of them and fallback to `Vec<bool>` otherwise
- ... probably other things I'm not thinking of!


---

_Label `rule` added by @dylwil3 on 2025-09-01 20:10_

---

_Label `preview` added by @dylwil3 on 2025-09-01 20:10_

---

_Comment by @github-actions[bot] on 2025-09-01 20:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+245 -0 violations, +0 -0 fixes in 30 projects; 25 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2da07c7ae1b50209cd2d5c1d52117a4f04007682/disnake/ext/commands/bot_base.py#L5'>disnake/ext/commands/bot_base.py:5:8:</a> F401 [*] `collections` imported but unused
+ <a href='https://github.com/DisnakeDev/disnake/blob/2da07c7ae1b50209cd2d5c1d52117a4f04007682/disnake/ext/commands/cog.py#L21'>disnake/ext/commands/cog.py:21:8:</a> F401 [*] `disnake` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+77 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/data.py#L21'>rasa/cli/data.py:21:8:</a> F401 [*] `rasa.shared.utils.cli` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/data.py#L22'>rasa/cli/data.py:22:8:</a> F401 [*] `rasa.utils.common` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/data.py#L6'>rasa/cli/data.py:6:8:</a> F401 [*] `rasa.shared.core.domain` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/export.py#L11'>rasa/cli/export.py:11:8:</a> F401 [*] `rasa.utils.common` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/interactive.py#L22'>rasa/cli/interactive.py:22:8:</a> F401 [*] `rasa.utils.common` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/telemetry.py#L7'>rasa/cli/telemetry.py:7:8:</a> F401 [*] `rasa.cli.utils` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/test.py#L28'>rasa/cli/test.py:28:8:</a> F401 [*] `rasa.utils.common` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/test.py#L8'>rasa/cli/test.py:8:8:</a> F401 [*] `rasa.shared.data` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/train.py#L11'>rasa/cli/train.py:11:8:</a> F401 [*] `rasa.utils.common` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L84'>rasa/core/actions/action.py:84:8:</a> F401 [*] `rasa.shared.utils.io` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/brokers/broker.py#L9'>rasa/core/brokers/broker.py:9:8:</a> F401 [*] `rasa.shared.utils.io` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/rasa_chat.py#L8'>rasa/core/channels/rasa_chat.py:8:8:</a> F401 [*] `jwt.exceptions` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/evaluation/marker_base.py#L24'>rasa/core/evaluation/marker_base.py:24:8:</a> F401 [*] `rasa.shared.nlu.constants` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/evaluation/marker_base.py#L25'>rasa/core/evaluation/marker_base.py:25:8:</a> F401 [*] `rasa.shared.utils.validation` imported but unused
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/evaluation/marker_base.py#L27'>rasa/core/evaluation/marker_base.py:27:8:</a> F401 [*] `rasa.shared.utils.common` imported but unused
... 62 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/8d3b53d518e58013351fe08377b675ee80182d69/tests/test_utils.py#L22'>tests/test_utils.py:22:8:</a> F401 [*] `snowflake.cli._plugins.snowpark.models` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+23 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/airflow-core/src/airflow/plugins_manager.py#L22'>airflow-core/src/airflow/plugins_manager.py:22:8:</a> F401 [*] `importlib` imported but unused
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/airflow-core/tests/unit/utils/test_log_handlers.py#L24'>airflow-core/tests/unit/utils/test_log_handlers.py:24:8:</a> F401 [*] `logging.config` imported but unused
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/devel-common/src/sphinx_exts/docs_build/fetch_inventories.py#L19'>devel-common/src/sphinx_exts/docs_build/fetch_inventories.py:19:8:</a> F401 [*] `concurrent` imported but unused
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/devel-common/src/tests_common/pytest_plugin.py#L2147'>devel-common/src/tests_common/pytest_plugin.py:2147:16:</a> F401 `airflow.sdk._shared.logging` imported but unused; consider using `importlib.util.find_spec` to test for availability
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/devel-common/src/tests_common/pytest_plugin.py#L2150'>devel-common/src/tests_common/pytest_plugin.py:2150:20:</a> F401 `airflow.sdk._shared.logging` imported but unused; consider using `importlib.util.find_spec` to test for availability
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/providers/amazon/src/airflow/providers/amazon/aws/hooks/batch_waiters.py#L36'>providers/amazon/src/airflow/providers/amazon/aws/hooks/batch_waiters.py:36:8:</a> F401 [*] `botocore.client` imported but unused
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L131'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:131:16:</a> F401 [*] `airflow.jobs.local_task_job_runner` imported but unused
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L132'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:132:16:</a> F401 [*] `airflow.macros` imported but unused
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L135'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:135:16:</a> F401 `airflow.providers.standard.operators.bash` imported but unused; consider using `importlib.util.find_spec` to test for availability
+ <a href='https://github.com/apache/airflow/blob/3f2f18037b49291767707f22d573bf887f2079e9/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L136'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:136:16:</a> F401 `airflow.providers.standard.operators.python` imported but unused; consider using `importlib.util.find_spec` to test for availability
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/77a5969dc1570d8ac3560520e05a4872e0248c37/superset/utils/logging_configurator.py#L21'>superset/utils/logging_configurator.py:21:8:</a> F401 [*] `flask.app` imported but unused
+ <a href='https://github.com/apache/superset/blob/77a5969dc1570d8ac3560520e05a4872e0248c37/tests/integration_tests/cli_tests.py#L29'>tests/integration_tests/cli_tests.py:29:8:</a> F401 [*] `superset.cli.importexport` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/6370b15d317bbe1aec7fc12b2c38c9d0f4cea347/samcli/lib/package/s3_uploader.py#L25'>samcli/lib/package/s3_uploader.py:25:8:</a> F401 [*] `botocore` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Multi_Agent_Legacy.py#L57'>crazy_functions/Multi_Agent_Legacy.py:57:16:</a> F401 [*] `autogen` imported but unused
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/request_llms/bridge_chatglm.py#L22'>request_llms/bridge_chatglm.py:22:16:</a> F401 [*] `os` imported but unused
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/request_llms/bridge_chatglm3.py#L22'>request_llms/bridge_chatglm3.py:22:16:</a> F401 [*] `os` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L18'>release/credentials.py:18:8:</a> F401 [*] `boto` imported but unused
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L20'>release/credentials.py:20:8:</a> F401 [*] `boto.s3.key` imported but unused
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/defaults.py#L87'>tests/support/defaults.py:87:12:</a> F401 [*] `bokeh.models` imported but unused
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_cross.py#L39'>tests/test_cross.py:39:8:</a> F401 [*] `_pytest.mark` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/32ce93c655fffe180470ff0ce6a838a550e38a3d/ibis/backends/conftest.py#L5'>ibis/backends/conftest.py:5:8:</a> F401 `importlib.metadata` imported but unused
+ <a href='https://github.com/ibis-project/ibis/blob/32ce93c655fffe180470ff0ce6a838a550e38a3d/ibis/backends/tests/test_dot_sql.py#L11'>ibis/backends/tests/test_dot_sql.py:11:8:</a> F401 `ibis.backends.sql.dialects` imported but unused
+ <a href='https://github.com/ibis-project/ibis/blob/32ce93c655fffe180470ff0ce6a838a550e38a3d/ibis/expr/types/_rich.py#L12'>ibis/expr/types/_rich.py:12:8:</a> F401 `rich` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/986302322f1077122434ff09914e8ce23e03cbc5/libs/core/tests/unit_tests/tracers/test_langchain.py#L3'>libs/core/tests/unit_tests/tracers/test_langchain.py:3:8:</a> F401 [*] `unittest` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/centromere/ctx.py#L12'>src/latch_cli/centromere/ctx.py:12:8:</a> F401 [*] `paramiko.util` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/snakemake/single_task_snakemake.py#L12'>src/latch_cli/snakemake/single_task_snakemake.py:12:8:</a> F401 [*] `snakemake.workflow` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/snakemake/workflow.py#L27'>src/latch_cli/snakemake/workflow.py:27:8:</a> F401 [*] `snakemake` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/settings.py#L3'>lnbits/settings.py:3:8:</a> F401 [*] `importlib` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+52 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/dev/update_ml_package_versions.py#L16'>dev/update_ml_package_versions.py:16:8:</a> F401 [*] `urllib.error` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/examples/flower_classifier/score_images_spark.py#L19'>examples/flower_classifier/score_images_spark.py:19:8:</a> F401 [*] `mlflow` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/examples/hyperparam/search_random.py#L19'>examples/hyperparam/search_random.py:19:8:</a> F401 [*] `mlflow.sklearn` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/examples/hyperparam/search_random.py#L20'>examples/hyperparam/search_random.py:20:8:</a> F401 [*] `mlflow.tracking` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/mlflow/anthropic/autolog.py#L4'>mlflow/anthropic/autolog.py:4:8:</a> F401 [*] `mlflow` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/mlflow/langchain/utils/logging.py#L282'>mlflow/langchain/utils/logging.py:282:12:</a> F401 [*] `langchain.agents.agent` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/mlflow/langchain/utils/logging.py#L283'>mlflow/langchain/utils/logging.py:283:12:</a> F401 [*] `langchain.chains.base` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/mlflow/langchain/utils/logging.py#L285'>mlflow/langchain/utils/logging.py:285:12:</a> F401 [*] `langchain.llms.huggingface_hub` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/mlflow/langchain/utils/logging.py#L286'>mlflow/langchain/utils/logging.py:286:12:</a> F401 [*] `langchain.llms.openai` imported but unused
+ <a href='https://github.com/mlflow/mlflow/blob/5f1428d4f36e33450f4caa3ad89b972cd6fb18a1/mlflow/langchain/utils/logging.py#L77'>mlflow/langchain/utils/logging.py:77:12:</a> F401 [*] `langchain.agents` imported but unused
... 42 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e4ca40511c13cf845058066ce174d4e233840d92/pandas/_libs/__init__.py#L16'>pandas/_libs/__init__.py:16:8:</a> F401 [*] `pandas._libs.pandas_parser` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/pandas-dev/pandas/blob/e4ca40511c13cf845058066ce174d4e233840d92/pandas/tests/indexes/datetimes/methods/test_to_pydatetime.py#L7'>pandas/tests/indexes/datetimes/methods/test_to_pydatetime.py:7:8:</a> F401 [*] `dateutil.tz` imported but unused
+ <a href='https://github.com/pandas-dev/pandas/blob/e4ca40511c13cf845058066ce174d4e233840d92/pandas/tests/indexes/datetimes/test_constructors.py#L12'>pandas/tests/indexes/datetimes/test_constructors.py:12:8:</a> F401 [*] `dateutil` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/integration-tests/test_client_context_lifespan.py#L10'>integration-tests/test_client_context_lifespan.py:10:8:</a> F401 [*] `prefect` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/integration-tests/test_client_context_lifespan.py#L12'>integration-tests/test_client_context_lifespan.py:12:8:</a> F401 [*] `prefect.exceptions` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py#L71'>src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:71:8:</a> F401 [*] `anyio` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/integrations/prefect-dask/prefect_dask/task_runners.py#L91'>src/integrations/prefect-dask/prefect_dask/task_runners.py:91:8:</a> F401 [*] `distributed.deploy` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/integrations/prefect-gcp/prefect_gcp/workers/cloud_run.py#L165'>src/integrations/prefect-gcp/prefect_gcp/workers/cloud_run.py:165:8:</a> F401 [*] `anyio` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/integrations/prefect-kubernetes/prefect_kubernetes/worker.py#L121'>src/integrations/prefect-kubernetes/prefect_kubernetes/worker.py:121:8:</a> F401 [*] `anyio` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/integrations/prefect-ray/tests/test_task_runners.py#L10'>src/integrations/prefect-ray/tests/test_task_runners.py:10:8:</a> F401 [*] `ray` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/integrations/prefect-ray/tests/test_task_runners.py#L16'>src/integrations/prefect-ray/tests/test_task_runners.py:16:8:</a> F401 [*] `prefect.task_engine` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/prefect/cli/profile.py#L18'>src/prefect/cli/profile.py:18:8:</a> F401 [*] `prefect.settings.profiles` imported but unused
+ <a href='https://github.com/prefecthq/prefect/blob/5d7d5eb6079b56162d99007c67fba887407d5b21/src/prefect/client/schemas/schedules.py#L9'>src/prefect/client/schemas/schedules.py:9:8:</a> F401 [*] `dateutil` imported but unused
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/7c619efba910c04005a835b110b057fc28fd6e93/cibuildwheel/__main__.py#L17'>cibuildwheel/__main__.py:17:8:</a> F401 [*] `cibuildwheel.util` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/ed055186f3a4c7713e39e40faf63c70cf8c59964/src/pip/_internal/cli/base_command.py#L6'>src/pip/_internal/cli/base_command.py:6:8:</a> F401 [*] `logging.config` imported but unused
+ <a href='https://github.com/pypa/pip/blob/ed055186f3a4c7713e39e40faf63c70cf8c59964/src/pip/_internal/vcs/__init__.py#L5'>src/pip/_internal/vcs/__init__.py:5:8:</a> F401 `pip._internal.vcs.bazaar` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/pypa/pip/blob/ed055186f3a4c7713e39e40faf63c70cf8c59964/src/pip/_internal/vcs/__init__.py#L6'>src/pip/_internal/vcs/__init__.py:6:8:</a> F401 `pip._internal.vcs.git` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/pypa/pip/blob/ed055186f3a4c7713e39e40faf63c70cf8c59964/src/pip/_internal/vcs/__init__.py#L7'>src/pip/_internal/vcs/__init__.py:7:8:</a> F401 `pip._internal.vcs.mercurial` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 245 | 245 | 0 | 0 | 0 |

</p>
</details>




---

_@dylwil3 reviewed on 2025-09-12 20:31_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__f401_import_submodules_in_function_scope.snap`:1 on 2025-09-12 20:31_

Note that we have restricted our analyses to the present scope, so this snapshot is possibly surprising. The main reason to restrict to one scope at a time is that the unused import rule already acts on the scope level. So the current behavior is already to emit two diagnostics even for something like:

```
import a

def foo():
    import a
```



---

_Marked ready for review by @dylwil3 on 2025-09-12 20:33_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:852 on 2025-09-12 20:38_

I imagine someone smarter than me could avoid so many lifetimes here... Presumably this is a code smell at this point

---

_@dylwil3 reviewed on 2025-09-12 20:38_

---

_Label `great writeup` added by @MichaReiser on 2025-09-18 10:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:654 on 2025-09-18 11:13_

What's the reason for excluding aliased imports?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:690 on 2025-09-18 11:19_

Nit: You can avoid the temporary variable by flipping the field order
```suggestion
        Self {
            used: vec![false; unused.len()],
            bindings: unused,
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:680 on 2025-09-18 11:20_

Nit: I found the name `unused` slightly mislieading. At this point, we don't know yet which bindings are or aren't used. 


```suggestion
        let bindings: Vec<_> = scope
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:695 on 2025-09-18 11:22_

You explained it in the PR summary but a future reader might not have the context. 

It wasn't immediately clear why we need to collect all bindings and why they are shadowed and what the current binding is. 

I really like the example, maybe we can move the example more to the top, then explain that `id` points to the binding of `a` created by `import a.b.c` and that said import shadows the bindings introduced by `import a` and `import a.b` (which we need to check if they're used)

This might also be a good place to document that, as of today (18 Sept 2025), Ruff's semantic index only creates a single binding for `a`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:677 on 2025-09-18 11:25_

Nit: I would move this definition after `unused_imports_from_binding`. `unused_imports_from_binding` seems more important (and provides way more context) to someone reading the code. The `MarkedBindings` in between is sort of distracting from the most important. (I think it's called newspaper style, obviously, something you can't do in Python)

Obviously, this very much comes down to personal preference. So feel free to disregard.

https://kentcdodds.com/blog/newspaper-code-structure

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:741 on 2025-09-18 11:32_

The `expect` message here is helpful in the sense that it makes it easier to find where the program is panicking if this condition isn't satisfied and debug symbols are off. However, it isn't very helpful for a future reader because it doesn't clarify **why** this invariant should hold true. 

The rust documentation provides some guidance on how to phrase your message instead: https://doc.rust-lang.org/std/result/enum.Result.html#recommended-message-style


`bindings ith shadowed bindings that aren't imports should be skipped by `unused_imports_in_scope``

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:764 on 2025-09-18 11:33_

Nit: Maybe make this a method on `qualified_name` instead, e.g. `name.root()` (and can we guarantee that it's always set)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:738 on 2025-09-18 11:37_

This makes sense to me. If you have, it might help future readers to add an example here

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:852 on 2025-09-18 11:43_

I found the `bimp` and `bname` naming here more confusing than useful. I suggest being more explicit and naming the variables `best_import`, `best_name`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:851 on 2025-09-18 11:45_

Can there be multiple bindings for the same name or could be abort the loop when we found the binding? 

It also feels a bit unnecessary that we find the best matching binding in `best_match` first and then have to loop through all bindings again to mark said binding as used. Could `best_match` return the index or even better, the best matching binding and the mutable `used` variable?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:879 on 2025-09-18 11:46_

Rename this to `bindings`. We iterate over all bindings, ignoring if they have been used or not

This is also something that you could make a method on `MarkedBindings`
```suggestion
    unused: &'a [&'b Binding<'c>],
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:854 on 2025-09-18 11:52_

I'd find some documentation on how the ranking works useful. 

What I understand is that it takes the common segments and counts how many common segments there are. 

It's not entirely clear to me why we need to include the `qname.segments().len()`. 

```py
import a.b.c
import a.b.d.e

a.b
```

Is my understanding correct that this would prefer `a.b.d.e` over `a.b.c`? What's the motivation for this? Why not use the first/last? 


```py
import a.b.c
import a.b.d.e

a.b.x.f
```

This would return `(2, 3)` for `a.b.c` and `(2, 4)` for `a.b.d.e` where both aren't valid matches. Is this handled somewhere else?



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:852 on 2025-09-18 11:55_

I don't think you need `'a` because it isn't used in the return type. Try omitting it

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:889 on 2025-09-18 11:56_


```suggestion
        .copied()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:657 on 2025-09-18 11:59_

I'm not sure if it's worth it but we could consider skipping the new logic if `id` has now shadowed bindings except itself (which should allow us to skip some unnecessayr work)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:813 on 2025-09-18 12:24_

I assume this is the same as `UnqualifiedName::from_expr`. The main reason we don't use it here is that we first need to find the top-level expression by following all ids



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:817 on 2025-09-18 12:28_

I think one bug in the current implementation is that we iterate over all bindings, without taking into account whether they're visible at the reference's point

```py
import a

a.b

import a.b
```


In preview, this reports one error that `import a` is unused which is incorrect because `import a.b` only comes after

---

_@MichaReiser reviewed on 2025-09-18 12:29_

This is a masive improvement with very low overhead. Well done.

I've a couple of nit comments but I also found a correctness bug that we need to look into. 

I haven't reviewed all tests yet.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:854 on 2025-09-19 18:31_

I'll add some documentation, but also answer here.

First, it's the opposite of what you suggest - we pick the _shortest_ path that matches. So in your example

```python
import a.b.c
import a.b.d.e

a.b
```

we would consider `a.b.c` as used, and in your next example:

```python
import a.b.c
import a.b.d.e

a.b.x.f
```

it would consider `a.b.c` as used. Also note that while it seems like...

> where both aren't valid matches

these _are_ all valid matches because of the strange behavior of Python imports described in the PR summary.

The motivation for marking the shortest as used as opposed to last one is because otherwise you wouldn't get the correct answer even in the most common situations:

```python
import a
import a.b

a.foo()
```

here both `a.b` and `a` match `a.foo` at a prefix of length `1`, so they're tied. Among those we pick the _shortest_ since we want to mark `import a` as used.

Flipping the order gives a counterexample to the heuristic of choosing the _first_ one, as well.



---

_@dylwil3 reviewed on 2025-09-19 18:31_

---

_@dylwil3 reviewed on 2025-09-19 18:33_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:851 on 2025-09-19 18:33_

We loop through because we may have repeated imports like

```python
import a
import a

a.foo()
```

so we have to mark all of them as used otherwise they will be left over and trigger this lint. But it's the job of [redefined-while-unused (F811)](https://docs.astral.sh/ruff/rules/redefined-while-unused/#redefined-while-unused-f811) to catch this situation, not `F401`

---

_@dylwil3 reviewed on 2025-09-19 18:34_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:889 on 2025-09-19 18:34_

`copied` is not a method on `Vec`. Do you want me to use `into_iter` first or something?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:813 on 2025-09-19 18:35_

Yes, we are already walking upwards anyway so I figured we could collect the name on the way instead of backtracking once we get to the top.

---

_@dylwil3 reviewed on 2025-09-19 18:35_

---

_@dylwil3 reviewed on 2025-09-19 18:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:817 on 2025-09-19 18:38_

Hmm, let me play around with this and also add some more tests - thanks for catching it!

---

_@dylwil3 reviewed on 2025-09-19 18:40_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:654 on 2025-09-19 18:40_

Just to avoid complexity - I didn't think it through very hard. It's possible we could handle them. I just worry vaguely about things like

```python
import a as b
import b

b.foo()
```

but maybe I shouldn't.

---

_@ntBre reviewed on 2025-09-19 18:47_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:889 on 2025-09-19 18:47_

Probably `iter().copied()` to avoid the `map(|v| &**v)` at the end

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:657 on 2025-09-19 19:35_

Added via

```rust
            if is_refined_submodule_import_match_enabled(settings)
                // No need to apply refined logic if there is only a single binding
                && scope.shadowed_bindings(id).nth(1).is_some()
```

is there a more idomatic way to do that? (I want to check that the iterator has at least two elements, but I don't want to use, say, `count` because there's no need to keep iterating once you've found two elements).

---

_@dylwil3 reviewed on 2025-09-19 19:35_

---

_@dylwil3 reviewed on 2025-09-19 19:45_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:738 on 2025-09-19 19:45_

In fact I don't _think_ the `else` branch is possible... marking it as `unreachable` does not cause any tests to panic. But I couldn't find an actual justification so I took the coward's way out... I'll try to dig a little and see if it's possible

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:817 on 2025-09-25 20:31_

Couldn't find a clean way to handle this "correctly" with the new logic, so instead I detect this situation now and revert to the stable behavior in that case. This means that we no longer give the ideal diagnostic for

```python
import a
a.foo()
import a.b
```

We used to mark the last import as unused, but now there's no lint error.

Note this is all related to a somewhat fishy choice of the semantic model: we copy over all references to an import binding to the _new_ binding when we have a second import statement. So, in the above example, at the end of the file we think that `a.foo()` is a reference for `import a.b`, which is a bit weird. It may be worth investigating the history of that decision and whether it could be modified to make the present logic simpler. But I think that'd be out of scope for now.

---

_@dylwil3 reviewed on 2025-09-25 20:31_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:695 on 2025-09-25 20:56_

Reworded/expanded

---

_@dylwil3 reviewed on 2025-09-25 20:56_

---

_@dylwil3 reviewed on 2025-09-25 20:57_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:764 on 2025-09-25 20:57_

> (and can we guarantee that it's always set)

Can you say more about what you had in mind here?

---

_Review requested from @MichaReiser by @dylwil3 on 2025-09-25 20:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:669 on 2025-09-26 09:44_

Did you break cargo fmt or does rustfmt not add spaces around operators (`i > 0`)?

Yeah, it looks like you broke rustfmt 😆 

Adding an empty line in front of the `matches` makes it recover for some lines but it still fails for the `matches` call. I'm not sure what's wrong with your match call that it breaks formatting but I suspect it might help rustfmt (and readers) if you extract parts of the if condition into standalone expressions (just a tiny bit)

Oh, I think rustfmt struggles with the leading comments between the different expressions in the condition. Extracting some variables (or functions) is probably your best solution

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:889 on 2025-09-26 09:54_

Nit: Using `'b` and `'c` without `'a` is a bit weird. I suggest renaming them to `'a` and `'b` (a reader otherwise wonders why `b` and `c`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:764 on 2025-09-26 09:57_

There's a hidden `unwrap` call when you access `segments[0]`. There's an implicit assumption that `segments` is never empty. 

```
binding
                .as_any_import()
                .expect("binding to be import binding since current function called after restricting to these in `unused_imports_in_scope`")
                .qualified_name()
                .segments()[0];
```

I haven't checked all code paths but I think the assumption is true for all `QualifiedName`. If so, then having a method on `QualifiedName` that returns the first segment (and documents why doing so without checking the length of segments is okay) seems generally useful (e.g. `first_segment`, `root_name`, what not :))

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:851 on 2025-09-26 09:58_

Oh I see. This would make a great inline comment

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:713 on 2025-09-26 10:01_

This documentation with the example is great! 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__f401_import_submodules_in_function_scope.snap`:7 on 2025-09-26 10:10_

I found this a bit surprising but it's the same behavior as on main. 

Which makes me wonder if we should have used the assertion helper that compares preview with stable behavior.

---

_@MichaReiser approved on 2025-09-26 10:11_

This is great and now with a 0 perf regression. Well done!

---

_@dylwil3 reviewed on 2025-09-26 13:04_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:764 on 2025-09-26 13:04_

There's one place in Ruff where it's not clear to me that this invariant is satisfied:

https://github.com/astral-sh/ruff/blob/2af8c53110227598cb4758a777667dbe18bc7f15/crates/ruff_python_ast/src/helpers.rs#L908-L918

So for now I've added an explicit `expect` here, similar to what's done elsewhere like 

https://github.com/astral-sh/ruff/blob/2af8c53110227598cb4758a777667dbe18bc7f15/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs#L494-L498

---

_@dylwil3 reviewed on 2025-09-26 13:05_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__f401_import_submodules_in_function_scope.snap`:7 on 2025-09-26 13:05_

Right now our assertion helper only works for fixtures passed as file paths, whereas here I've written the source code inline. So for now I've lazily added comments to the fixtures indicating expected behavior 😄 

---

_Merged by @dylwil3 on 2025-09-26 13:22_

---

_Closed by @dylwil3 on 2025-09-26 13:22_

---

_Branch deleted on 2025-09-26 13:22_

---
