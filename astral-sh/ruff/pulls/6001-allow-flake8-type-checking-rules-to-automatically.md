```yaml
number: 6001
title: "Allow `flake8-type-checking` rules to automatically quote runtime-evaluated references"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/quote
created_at: 2023-07-23T02:03:38Z
updated_at: 2023-12-13T03:19:38Z
url: https://github.com/astral-sh/ruff/pull/6001
synced_at: 2026-01-10T23:31:11Z
```

# Allow `flake8-type-checking` rules to automatically quote runtime-evaluated references

---

_Pull request opened by @charliermarsh on 2023-07-23 02:03_

## Summary

This allows us to fix usages like:

```python
from pandas import DataFrame

def baz() -> DataFrame:
    ...
```

By quoting the `DataFrame` in `-> DataFrame`. Without quotes, moving `from pandas import DataFrame` into an `if TYPE_CHECKING:` block will fail at runtime, since Python tries to evaluate the annotation to add it to the function's `__annotations__`.

Unfortunately, this does require us to split our "annotation kind" flags into three categories, rather than two:

- `typing-only`: The annotation is only evaluated at type-checking-time.
- `runtime-evaluated`: Python will evaluate the annotation at runtime (like above) -- but we're willing to quote it.
- `runtime-required`: Python will evaluate the annotation at runtime (like above), and some library (like Pydantic) needs it to be available at runtime, so we _can't_ quote it.

This functionality is gated behind a setting (`flake8-type-checking.quote-annotations`).

Closes https://github.com/astral-sh/ruff/issues/5559.


---

_Comment by @github-actions[bot] on 2023-07-23 02:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-07-23 04:41_

This "works" on Dagster in that it generates 1834 fixes (and no syntax errors) that generally look right:

```diff
--- a/examples/assets_smoke_test/assets_smoke_test/pure_python_assets.py
+++ b/examples/assets_smoke_test/assets_smoke_test/pure_python_assets.py
@@ -1,5 +1,9 @@
+from typing import TYPE_CHECKING
+
 from dagster import SourceAsset, TableSchema, asset
-from pandas import DataFrame
+
+if TYPE_CHECKING:
+    from pandas import DataFrame

 raw_country_populations = SourceAsset(
     "raw_country_populations",
@@ -19,7 +23,7 @@


 @asset
-def country_populations(raw_country_populations) -> DataFrame:
+def country_populations(raw_country_populations) -> "DataFrame":
     country_populations = raw_country_populations.copy()
     country_populations["change"] = (
         country_populations["change"]
@@ -32,13 +36,13 @@ def country_populations(raw_country_populations) -> DataFrame:


 @asset
-def continent_stats(country_populations: DataFrame) -> DataFrame:
+def continent_stats(country_populations: "DataFrame") -> "DataFrame":
     result = country_populations.groupby("continent").agg({"pop2019": "sum", "change": "mean"})
     return result


 @asset
-def country_stats(country_populations: DataFrame, continent_stats: DataFrame) -> DataFrame:
+def country_stats(country_populations: "DataFrame", continent_stats: "DataFrame") -> "DataFrame":
     result = country_populations.join(continent_stats, on="continent", lsuffix="_continent")
     result["continent_pop_fraction"] = result["pop2019"] / result["pop2019_continent"]
     return result
```

---

_Comment by @tylerlaprade on 2023-07-23 15:59_

Yes! I was wondering yesterday if I misunderstood the purpose of the rule, because it was barely applying. When I realized I needed to add quotes, I briefly started manually adding them, but then realized it was unfeasible to do manually on even a medium-sized codebase.

---

_Comment by @charliermarsh on 2023-07-23 16:00_

(If you use `from __future__ import annotations`, you'll also get more of the expected changes, but that's not always feasible to add either depending on your use-case.)

---

_Comment by @charliermarsh on 2023-08-20 23:09_

This was blocked as it became too difficult to quote an annotation given just the reference range. It should be unblocked now that we store all expressions in a vector, since we can store an ExpressionId on all references to go from reference to Expr.

---

_Comment by @github-actions[bot] on 2023-12-10 03:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/c04bf6759d7a51dfedf54277e3ba9ac3ee6ebf3c/ibis/common/tests/test_graph_benchmarks.py#L6'>ibis/common/tests/test_graph_benchmarks.py:6:37:</a> RUF100 Unused `noqa` directive (unused: `TCH002`)
+ <a href='https://github.com/ibis-project/ibis/blob/c04bf6759d7a51dfedf54277e3ba9ac3ee6ebf3c/ibis/common/typing.py#L33'>ibis/common/typing.py:33:46:</a> RUF100 Unused `noqa` directive (unused: `TCH002`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e9ce69aaf62c6da4557aaf0cf5eb8c83cfdb81b7/analytics/views/stats.py#L13'>analytics/views/stats.py:13:31:</a> TCH002 Move third-party import `typing_extensions.TypeAlias` into a type-checking block
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |
| TCH002 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/c04bf6759d7a51dfedf54277e3ba9ac3ee6ebf3c/ibis/common/tests/test_graph_benchmarks.py#L6'>ibis/common/tests/test_graph_benchmarks.py:6:37:</a> RUF100 Unused `noqa` directive (unused: `TCH002`)
+ <a href='https://github.com/ibis-project/ibis/blob/c04bf6759d7a51dfedf54277e3ba9ac3ee6ebf3c/ibis/common/typing.py#L33'>ibis/common/typing.py:33:46:</a> RUF100 Unused `noqa` directive (unused: `TCH002`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e9ce69aaf62c6da4557aaf0cf5eb8c83cfdb81b7/analytics/views/stats.py#L13'>analytics/views/stats.py:13:31:</a> TCH002 Move third-party import `typing_extensions.TypeAlias` into a type-checking block
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |
| TCH002 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-12-10 03:35_

The diff of this on Dagster actually looks really good.

---

_Comment by @charliermarsh on 2023-12-10 03:36_

For example:
```diff
@@ -25,7 +25,6 @@
 from dagster._core.events import DagsterEvent
 from dagster._core.execution.api import create_execution_plan, scoped_job_context
 from dagster._core.execution.plan.outputs import StepOutputHandle
-from dagster._core.execution.plan.plan import ExecutionPlan
 from dagster._core.execution.plan.state import KnownExecutionState
 from dagster._core.execution.plan.step import ExecutionStep
 from dagster._core.execution.resources_init import (
@@ -34,7 +33,6 @@
 )
 from dagster._core.instance import DagsterInstance
 from dagster._core.instance.ref import InstanceRef
-from dagster._core.log_manager import DagsterLogManager
 from dagster._core.storage.dagster_run import DagsterRun, DagsterRunStatus
 from dagster._core.system_config.objects import ResolvedRunConfig, ResourceConfig
 from dagster._core.utils import make_new_run_id
@@ -47,6 +45,8 @@
 from .serialize import PICKLE_PROTOCOL

 if TYPE_CHECKING:
+    from dagster._core.execution.plan.plan import ExecutionPlan
+    from dagster._core.log_manager import DagsterLogManager
     from dagster._core.definitions.node_definition import NodeDefinition


@@ -81,8 +81,8 @@ def _setup_resources(
         self,
         resource_defs: Mapping[str, ResourceDefinition],
         resource_configs: Mapping[str, ResourceConfig],
-        log_manager: DagsterLogManager,
-        execution_plan: Optional[ExecutionPlan],
+        log_manager: "DagsterLogManager",
+        execution_plan: Optional["ExecutionPlan"],
         dagster_run: Optional[DagsterRun],
         resource_keys_to_init: Optional[AbstractSet[str]],
         instance: Optional[DagsterInstance],
```

---

_Marked ready for review by @charliermarsh on 2023-12-10 03:37_

---

_Comment by @charliermarsh on 2023-12-10 03:38_

We may want to add a setting to allow users to either (1) insert `__future__` or (2) quote annotations or (3) do neither.


---

_Comment by @charliermarsh on 2023-12-10 03:39_

@smackesey -- You may be interested in this (finally resurrected after a long break).

---

_Review requested from @zanieb by @charliermarsh on 2023-12-11 16:52_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-12-11 16:52_

---

_Comment by @dhruvmanila on 2023-12-11 19:13_

I've gone through the changes and it looks good although I'd like to give a second look at it in the evening.

---

_Comment by @smackesey on 2023-12-11 20:11_

> You may be interested in this

Definitely-- thanks for bringing it back! To spare churn in imports for other engs I like to apply changes in one PR-- is this anywhere on your agenda: https://github.com/astral-sh/ruff/issues/1107? Would be great to apply them both at once.

---

_Comment by @charliermarsh on 2023-12-12 00:45_

@smackesey - I'm still interested in supporting that but it won't be merged on the same timeline, since it requires us to build up an import map that we share across modules.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:694 on 2023-12-12 02:02_

Nit: The nesting here feels a bit heavy. Maybe move `AnnotationKind` out and implement `annotation_kind` as a constructor/factory function? 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:685 on 2023-12-12 02:03_

I would find some documenting of the kinds similar to what you wrote in the PR summary useful to better understand the code.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:692 on 2023-12-12 02:04_

The documentation mentions *and that class is marked as runtime-evaluated* but it is unclear to me where this condition is checked inside of the code.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1516 on 2023-12-12 02:09_

Nit: There's some repetitive code across the different `visit_*_annotation`. Changing the function to instead take `AnnotationKind` as a parameter and move only the code that is different for all kinds into a match statement may make the code easier to read (it becomes easier to reason about what's different).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:75 on 2023-12-12 02:11_

This seems unrelated to your PR but I'm struggling to understand what *Import is not available at runtime* means in this message. I think it presents a fact that this is the reason why you should quote the imports. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:98 on 2023-12-12 02:12_

Is it possible that the same node has moves, quotes and ignroes or could we use a single hash map and store the `kind` (Move, quotes, ignores) inside the entry?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:31 on 2023-12-12 02:16_

I think we had a similar check in typing_only_runtime_import`. It looks complicated and important to get right. Maybe it's worth to factor it out into a dedicated function to avoid repeating it.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:234 on 2023-12-12 02:17_

Are call expressions a thing in type annotations? What does the type evaluate to? The returned value? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:268 on 2023-12-12 02:19_

When would this happen? Is it because of a potential subscript or union types with string variants?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/imports.rs`:15 on 2023-12-12 02:20_

In which situations can the `parent_range` be `None`? Shouldn't an import always come from a statement. This might be worth documenting.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:996 on 2023-12-12 02:22_

We should either document that this function panics if `NodeId` is not an expression (and rename to `expect_expression`?) or return `Option` to let the caller deal with it. I would prefer the latter, assuming that it won't cause `expect` calls in too many call sites.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/reference.rs`:30 on 2023-12-12 02:25_

Do you think it would improve readability if we call it `expression_id` to make it clear that it is the expression referencing the reference rather than naming it after its type?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1660 on 2023-12-12 02:28_

This part confused me. I admint, mainly because I didn't read carefully but I first thought that what ruff does will create a runtime error. 

maybe explain why it causes a runtime error. 

Or it might be that I got confused because the documentation explains what it does and the example shows when it doesn't work (rather than when it does). 

*would cause a runtime error because the type is no longer available at runtime.*

---

_@MichaReiser reviewed on 2023-12-12 02:30_

Looks good to me, but I only reviewed if I roughly understand what's happening, not if the implemented semantics are correct (because I don't understand thme).

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:691 on 2023-12-13 01:08_

Here, with "if the annotation is in a class" we're specifically referring to the class scope i.e.,
```python
class Foo:
	bar: str
	#    ^^^
```
Is that correct? If so, it would be worth updating the comment to reflect that.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:699 on 2023-12-13 01:11_

It's unfortunate that we now have runtime **required** internally but runtime **evaluated** in the user facing option. Maybe we could use either `v0.2.0` or `v0.3.0` as a way to rename these options.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:234 on 2023-12-13 01:16_

Yes!

```
In [15]: class Foo:
    ...:     pass
    ...: 

In [16]: def foo():
    ...:     return 42
    ...: 

In [17]: def bar(f: foo()):
    ...:     pass
    ...: 

In [18]: bar.__annotations__
Out[18]: {'f': 42}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:268 on 2023-12-13 01:39_

In the function example, we've mentioned to update the internal quote:

> - When quoting `Series` in `Series[Literal["pd.Timestamp"]]`, we want `"Series[Literal['pd.Timestamp']]"`.

With this code, I think that doesn't work, right? Can we use the `Stylist` to only check for the opposite quote?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:75 on 2023-12-13 01:46_

Maybe we could use something like "Import is not _used_ at runtime" or "Import is only used for type hinting"?

---

_@dhruvmanila approved on 2023-12-13 01:51_

Thanks! This is a great improvement to the rule. Overall the implementation looks really solid apart from some minor nits.

---

_@MichaReiser reviewed on 2023-12-13 02:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:234 on 2023-12-13 02:18_

Lol, but how is this supposed to work without running the code? It seems to compete the purpose of type checking where you want to statically analyse the programs without having to run them. 

Does that mean I can extract sensitive information by adding a fishy type annotation reading local files and submitting the information to a server if any type checker feels like evaluating the type annotation 😨 

---

_@charliermarsh reviewed on 2023-12-13 02:35_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:699 on 2023-12-13 02:35_

Yes great observation! It's really unfortunate.

---

_@charliermarsh reviewed on 2023-12-13 02:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:699 on 2023-12-13 02:36_

Actually, I'm gonna change the internal names.

---

_@charliermarsh reviewed on 2023-12-13 02:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:268 on 2023-12-13 02:36_

Yeah sorry, it doesn't do that yet.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:268 on 2023-12-13 02:38_

I'll add an example.

---

_@charliermarsh reviewed on 2023-12-13 02:38_

---

_@charliermarsh reviewed on 2023-12-13 02:41_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:234 on 2023-12-13 02:41_

I don't think this actually works in practice for static analysis tools, but it is grammatically supported.

---

_@charliermarsh reviewed on 2023-12-13 02:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:31 on 2023-12-13 02:45_

Good call, done.

---

_@charliermarsh reviewed on 2023-12-13 02:47_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:98 on 2023-12-13 02:47_

You can have different actions for members in the same statement. I could use a nested hash map though...

---

_@charliermarsh reviewed on 2023-12-13 02:55_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:694 on 2023-12-13 02:55_

Good call.

---

_@charliermarsh reviewed on 2023-12-13 02:55_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:685 on 2023-12-13 02:55_

Good call, done.

---

_Merged by @charliermarsh on 2023-12-13 03:12_

---

_Closed by @charliermarsh on 2023-12-13 03:12_

---

_Branch deleted on 2023-12-13 03:12_

---
