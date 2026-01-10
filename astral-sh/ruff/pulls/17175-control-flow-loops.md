```yaml
number: 17175
title: "Control flow: loops"
type: pull_request
state: closed
author: dylwil3
labels:
  - internal
assignees: []
draft: true
base: main
head: cfg-loops
created_at: 2025-04-03T13:54:24Z
updated_at: 2025-12-10T18:00:13Z
url: https://github.com/astral-sh/ruff/pull/17175
synced_at: 2026-01-10T16:42:11Z
```

# Control flow: loops

---

_Pull request opened by @dylwil3 on 2025-04-03 13:54_

This PR implements support for loop statements in the control flow graph, as well as the associated jumps `break` and `continue`. It is simplest to review commit by commit (now that I rewrote history...)

A few comments:

1. We have ecosystem false positives for `unreachable` because we have not implemented the fallback option to "draw edges for all possible control flow" for unsupported statements. For all remaining statement types except try/catch, implementing this logic is actually just as hard as implementing the statement! So I propose to implement the remaining statements, and then implement the fallback for try/catch.
2. I tried for a bit to come up with a clever way to clean up the code and avoid repetition, but could not seem to come up with something that wouldn't itself introduce a lot more complexity. For example: it would be nice to always add statements and edges at the same time... except the cleanup step at the end of the main loop doesn't really allow for this. Or another example: it would be nice if the debug assertion that we never add statements to a block twice was actually a checkable at compile time. Again - I tried thinking through a few approaches (e.g. having a "BlockBuilder" or introduing more complex state on the CFG Builder - like a stack of statement slices  and pointers) but couldn't quite find a way to make it all work. That doesn't mean there isn't an elegant solution - just that I'm not clever enough!


---

_Comment by @MichaReiser on 2025-04-03 16:15_

I think there are a few lifetimes that can be simplified in the mermaid testing code

```rust
Subject: [PATCH] Align server indexing with CLI behavior
---
Index: crates/ruff_python_semantic/src/cfg/graph.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_semantic/src/cfg/graph.rs b/crates/ruff_python_semantic/src/cfg/graph.rs
--- a/crates/ruff_python_semantic/src/cfg/graph.rs	(revision dca1340348d73b1bf1cd10de593b1ef01d9363ec)
+++ b/crates/ruff_python_semantic/src/cfg/graph.rs	(date 1743696804592)
@@ -49,7 +49,7 @@
 
     /// Returns the [`Edges`] going out of the basic block at the given index
     pub fn outgoing(&self, block: BlockId) -> &Edges {
-        &self.blocks[block].out
+        self.blocks[block].out
     }
 
     /// Returns an iterator over the indices of the direct predecessors of the block at the given index
@@ -132,7 +132,7 @@
     }
 
     /// Returns iterator over [`Condition`]s which must be satisfied to traverse corresponding edge
-    pub fn conditions(&self) -> impl ExactSizeIterator<Item = &Condition> {
+    pub fn conditions(&self) -> impl ExactSizeIterator<Item = &Condition<'stmt>> {
         self.conditions.iter()
     }
 
@@ -140,7 +140,7 @@
         self.targets.is_empty()
     }
 
-    pub fn filter_targets_by_conditions<'a, T: FnMut(&Condition) -> bool + 'a>(
+    pub fn filter_targets_by_conditions<'a: 'stmt, T: FnMut(&Condition) -> bool + 'a>(
         &'a self,
         mut predicate: T,
     ) -> impl Iterator<Item = BlockId> + 'a {
Index: crates/ruff_python_semantic/src/cfg/visualize.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_semantic/src/cfg/visualize.rs b/crates/ruff_python_semantic/src/cfg/visualize.rs
--- a/crates/ruff_python_semantic/src/cfg/visualize.rs	(revision dca1340348d73b1bf1cd10de593b1ef01d9363ec)
+++ b/crates/ruff_python_semantic/src/cfg/visualize.rs	(date 1743696538214)
@@ -10,7 +10,7 @@
     CFGWithSource::new(graph, source).draw_graph()
 }
 
-trait MermaidGraph<'a>: DirectedGraph<'a> {
+trait MermaidGraph: DirectedGraph {
     fn draw_node(&self, node: Self::Node) -> MermaidNode;
     fn draw_edges(&self, node: Self::Node) -> impl Iterator<Item = (Self::Node, MermaidEdge)>;
 
@@ -146,7 +146,7 @@
     }
 }
 
-pub trait DirectedGraph<'a> {
+pub trait DirectedGraph {
     type Node: Idx;
 
     fn num_nodes(&self) -> usize;
@@ -154,18 +154,18 @@
     fn successors(&self, node: Self::Node) -> impl ExactSizeIterator<Item = Self::Node> + '_;
 }
 
-struct CFGWithSource<'stmt> {
+struct CFGWithSource<'source, 'stmt> {
     cfg: ControlFlowGraph<'stmt>,
-    source: &'stmt str,
+    source: &'source str,
 }
 
-impl<'stmt> CFGWithSource<'stmt> {
-    fn new(cfg: ControlFlowGraph<'stmt>, source: &'stmt str) -> Self {
+impl<'source, 'stmt> CFGWithSource<'source, 'stmt> {
+    fn new(cfg: ControlFlowGraph<'stmt>, source: &'source str) -> Self {
         Self { cfg, source }
     }
 }
 
-impl<'stmt> DirectedGraph<'stmt> for CFGWithSource<'stmt> {
+impl DirectedGraph for CFGWithSource<'_, '_> {
     type Node = BlockId;
 
     fn num_nodes(&self) -> usize {
@@ -181,7 +181,7 @@
     }
 }
 
-impl<'stmt> MermaidGraph<'stmt> for CFGWithSource<'stmt> {
+impl MermaidGraph for CFGWithSource<'_, '_> {
     fn draw_node(&self, node: Self::Node) -> MermaidNode {
         let statements: Vec<String> = self
             .cfg

```

But I don't see an obvious work around for the lifetime issues other than using an `IndexVec` stored on `ControlFlowGraph` to store all the conditions and then store the `ConditionId` in `Edges::conditions` (or to use a `Vec` in `Edges::conditions`)

---

_Comment by @dylwil3 on 2025-04-04 19:42_

Hmm... not sure what I did wrong but I applied your patch exactly and the code did not compile. This seems to be the problem:

```diff
     /// Returns the [`Edges`] going out of the basic block at the given index
     pub fn outgoing(&self, block: BlockId) -> &Edges {
-        &self.blocks[block].out
+        self.blocks[block].out
     }
```

This doesn't agree with the return signature. If we remove the borrow we get a lifetime issue. It all works fine if we switch from `SmallVec` to `Vec` though so I'm gonna do that for now, and we can revisit a more performant data structure a little later.

---

_Comment by @github-actions[bot] on 2025-04-04 21:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+164 -1 violations, +0 -0 fixes in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/converter.py#L1019'>disnake/ext/commands/converter.py:1019:9:</a> PLW0101 Unreachable code in `convert`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/slash_core.py#L56'>disnake/ext/commands/slash_core.py:56:5:</a> PLW0101 Unreachable code in `_autocomplete`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/message.py#L1328'>disnake/message.py:1328:9:</a> PLW0101 Unreachable code in `_clear_emoji`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/state.py#L2531'>disnake/state.py:2531:9:</a> PLW0101 Unreachable code in `_delay_ready`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ui/select/base.py#L234'>disnake/ui/select/base.py:234:13:</a> PLW0101 Unreachable code in `_transform_default_values`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/scripts/codemods/overloads_no_missing.py#L29'>scripts/codemods/overloads_no_missing.py:29:9:</a> PLW0101 Unreachable code in `leave_FunctionDef`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/scripts/codemods/typed_permissions.py#L84'>scripts/codemods/typed_permissions.py:84:9:</a> PLW0101 Unreachable code in `leave_ClassDef`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/airflow-core/src/airflow/example_dags/plugins/workday.py#L57'>airflow-core/src/airflow/example_dags/plugins/workday.py:57:9:</a> PLW0101 Unreachable code in `get_next_workday`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/airflow-core/src/airflow/models/dag.py#L2581'>airflow-core/src/airflow/models/dag.py:2581:5:</a> PLW0101 Unreachable code in `_run_task`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/airflow-core/tests/unit/serialization/test_dag_serialization.py#L636'>airflow-core/tests/unit/serialization/test_dag_serialization.py:636:9:</a> PLW0101 Unreachable code in `test_deserialization_across_process`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/amazon/src/airflow/providers/amazon/aws/hooks/quicksight.py#L169'>providers/amazon/src/airflow/providers/amazon/aws/hooks/quicksight.py:169:9:</a> PLW0101 Unreachable code in `wait_for_state`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/amazon/src/airflow/providers/amazon/aws/hooks/redshift_data.py#L269'>providers/amazon/src/airflow/providers/amazon/aws/hooks/redshift_data.py:269:9:</a> PLW0101 Unreachable code in `get_table_primary_key`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/amazon/src/airflow/providers/amazon/aws/hooks/sagemaker.py#L760'>providers/amazon/src/airflow/providers/amazon/aws/hooks/sagemaker.py:760:9:</a> PLW0101 Unreachable code in `check_status`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/amazon/src/airflow/providers/amazon/aws/hooks/sagemaker.py#L843'>providers/amazon/src/airflow/providers/amazon/aws/hooks/sagemaker.py:843:9:</a> PLW0101 Unreachable code in `check_training_status_with_log`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L249'>providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:249:9:</a> PLW0101 Unreachable code in `_scan_dynamodb_and_upload_to_s3`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/apache/beam/src/airflow/providers/apache/beam/hooks/beam.py#L200'>providers/apache/beam/src/airflow/providers/apache/beam/hooks/beam.py:200:5:</a> PLW0101 Unreachable code in `run_beam_command`
+ <a href='https://github.com/apache/airflow/blob/42024188b9553dd50072e717af754cb95c901972/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L659'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/utils/pod_manager.py:659:9:</a> PLW0101 Unreachable code in `await_pod_completion`
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/79afc2b545a21719eba6fce9136d3041af4e1428/superset/commands/importers/v1/utils.py#L323'>superset/commands/importers/v1/utils.py:323:5:</a> PLW0101 Unreachable code in `get_resource_mappings_batched`
+ <a href='https://github.com/apache/superset/blob/79afc2b545a21719eba6fce9136d3041af4e1428/superset/migrations/shared/native_filters.py#L95'>superset/migrations/shared/native_filters.py:95:13:</a> PLW0101 Unreachable code in `convert_filter_scopes_to_native_filters`
+ <a href='https://github.com/apache/superset/blob/79afc2b545a21719eba6fce9136d3041af4e1428/superset/tasks/cache.py#L311'>superset/tasks/cache.py:311:5:</a> PLW0101 Unreachable code in `cache_warmup`
+ <a href='https://github.com/apache/superset/blob/79afc2b545a21719eba6fce9136d3041af4e1428/superset/utils/slack.py#L88'>superset/utils/slack.py:88:5:</a> PLW0101 Unreachable code in `get_channels`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3c7943b96161c2a448c6b89874a3c6db42a2c42/tests/integration/local/common_utils.py#L34'>tests/integration/local/common_utils.py:34:5:</a> PLW0101 Unreachable code in `wait_for_local_process`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L93'>tests/support/plugins/file_server.py:93:9:</a> PLW0101 Unreachable code in `__init__`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/selenium.py#L79'>tests/support/plugins/selenium.py:79:9:</a> PLW0101 Unreachable code in `chrome`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/f1d8fc671d365fed582d169252e7c4a18907d759/securedrop/management/run.py#L125'>securedrop/management/run.py:125:9:</a> PLW0101 Unreachable code in `monitor`
+ <a href='https://github.com/freedomofpress/securedrop/blob/f1d8fc671d365fed582d169252e7c4a18907d759/securedrop/pretty_bad_protocol/_meta.py#L666'>securedrop/pretty_bad_protocol/_meta.py:666:9:</a> PLW0101 Unreachable code in `_read_response`
+ <a href='https://github.com/freedomofpress/securedrop/blob/f1d8fc671d365fed582d169252e7c4a18907d759/securedrop/pretty_bad_protocol/_meta.py#L689'>securedrop/pretty_bad_protocol/_meta.py:689:9:</a> PLW0101 Unreachable code in `_read_data`
+ <a href='https://github.com/freedomofpress/securedrop/blob/f1d8fc671d365fed582d169252e7c4a18907d759/securedrop/pretty_bad_protocol/_util.py#L137'>securedrop/pretty_bad_protocol/_util.py:137:5:</a> PLW0101 Unreachable code in `_copy_data`
+ <a href='https://github.com/freedomofpress/securedrop/blob/f1d8fc671d365fed582d169252e7c4a18907d759/securedrop/tests/test_secure_tempfile.py#L92'>securedrop/tests/test_secure_tempfile.py:92:5:</a> PLW0101 Unreachable code in `test_buffered_read`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/blinksync.py#L102'>blinksync/blinksync.py:102:5:</a> PLW0101 Unreachable code in `main`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/66ab2a5e5405ebac1f9a60dd6cc4497cc806d8b7/ibis/backends/impala/metadata.py#L153'>ibis/backends/impala/metadata.py:153:9:</a> PLW0101 Unreachable code in `_parse_schema`
+ <a href='https://github.com/ibis-project/ibis/blob/66ab2a5e5405ebac1f9a60dd6cc4497cc806d8b7/ibis/backends/impala/metadata.py#L174'>ibis/backends/impala/metadata.py:174:9:</a> PLW0101 Unreachable code in `_parse_info`
+ <a href='https://github.com/ibis-project/ibis/blob/66ab2a5e5405ebac1f9a60dd6cc4497cc806d8b7/ibis/backends/impala/metadata.py#L258'>ibis/backends/impala/metadata.py:258:9:</a> PLW0101 Unreachable code in `_parse_nested_params`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/8574442c5709c429f01369c5ce969be8065c2f2b/libs/core/langchain_core/runnables/base.py#L5137'>libs/core/langchain_core/runnables/base.py:5137:13:</a> PLW0101 Unreachable code in `astream_events`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/3d40eb57259fbea100d5022f9ab04f5bb7686857/src/latch_cli/tinyrequests.py#L111'>src/latch_cli/tinyrequests.py:111:5:</a> PLW0101 Unreachable code in `_req`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+10 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/bulk_import/example_bulkinsert_csv.py#L170'>examples/bulk_import/example_bulkinsert_csv.py:170:5:</a> PLW0101 Unreachable code in `wait_tasks_to_state`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/bulk_import/example_bulkinsert_json.py#L212'>examples/bulk_import/example_bulkinsert_json.py:212:5:</a> PLW0101 Unreachable code in `wait_tasks_to_state`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/bulk_import/example_bulkinsert_numpy.py#L214'>examples/bulk_import/example_bulkinsert_numpy.py:214:5:</a> PLW0101 Unreachable code in `wait_tasks_to_state`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/bulk_import/example_bulkinsert_withfunction.py#L170'>examples/bulk_import/example_bulkinsert_withfunction.py:170:5:</a> PLW0101 Unreachable code in `wait_tasks_to_state`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/bulk_import/example_bulkwriter.py#L407'>examples/bulk_import/example_bulkwriter.py:407:5:</a> PLW0101 Unreachable code in `call_bulkinsert`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/bulk_import/example_bulkwriter_with_nullable.py#L319'>examples/bulk_import/example_bulkwriter_with_nullable.py:319:5:</a> PLW0101 Unreachable code in `call_bulkinsert`
- <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/iterator/iterator.py#L64'>examples/iterator/iterator.py:64:9:</a> PLW0101 Unreachable code in `external_filter_func`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/iterator/iterator.py#L65'>examples/iterator/iterator.py:65:9:</a> PLW0101 Unreachable code in `external_filter_func`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/examples/orm/iterator.py#L124'>examples/orm/iterator.py:124:5:</a> PLW0101 Unreachable code in `query_iterate_collection_no_offset`
+ <a href='https://github.com/milvus-io/pymilvus/blob/b65b4f5c6e46b2b930d48f4593032b86803e848a/pymilvus/client/search_iterator.py#L150'>pymilvus/client/search_iterator.py:150:9:</a> PLW0101 Unreachable code in `next`
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/_version.py#L111'>pandas/_version.py:111:5:</a> PLW0101 Unreachable code in `run_command`
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/core/sorting.py#L208'>pandas/core/sorting.py:208:5:</a> PLW0101 Unreachable code in `get_group_index`
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/io/html.py#L1007'>pandas/io/html.py:1007:5:</a> PLW0101 Unreachable code in `_parse`
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/tests/extension/test_categorical.py#L39'>pandas/tests/extension/test_categorical.py:39:5:</a> PLW0101 Unreachable code in `make_data`
+ <a href='https://github.com/pandas-dev/pandas/blob/e5898b8d33ac943a60250e1466006c073a506e8c/pandas/tests/io/test_sql.py#L1169'>pandas/tests/io/test_sql.py:1169:5:</a> PLW0101 Unreachable code in `test_read_iris_query_string_with_parameter`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/99183d68ac491ca04c562a5de91c7287b1bd1728/cibuildwheel/oci_container.py#L461'>cibuildwheel/oci_container.py:461:9:</a> PLW0101 Unreachable code in `call`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+51 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/accounting/accountant.py#L235'>rotkehlchen/accounting/accountant.py:235:9:</a> PLW0101 Unreachable code in `process_history`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/base/modules/echo/decoder.py#L156'>rotkehlchen/chain/base/modules/echo/decoder.py:156:9:</a> PLW0101 Unreachable code in `_process_funding`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/etherscan.py#L113'>rotkehlchen/chain/ethereum/etherscan.py:113:9:</a> PLW0101 Unreachable code in `get_withdrawals`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/compound/v2/decoder.py#L384'>rotkehlchen/chain/ethereum/modules/compound/v2/decoder.py:384:9:</a> PLW0101 Unreachable code in `decode_comp_claim`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/curve/crvusd/decoder.py#L207'>rotkehlchen/chain/ethereum/modules/curve/crvusd/decoder.py:207:9:</a> PLW0101 Unreachable code in `_decode_peg_keeper_update`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/golem/decoder.py#L71'>rotkehlchen/chain/ethereum/modules/golem/decoder.py:71:9:</a> PLW0101 Unreachable code in `_decode_migration`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/lido/decoder.py#L83'>rotkehlchen/chain/ethereum/modules/lido/decoder.py:83:9:</a> PLW0101 Unreachable code in `_decode_lido_staking_in_steth`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/shutter/decoder.py#L77'>rotkehlchen/chain/ethereum/modules/shutter/decoder.py:77:9:</a> PLW0101 Unreachable code in `_decode_shutter_claim`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/yearn/decoder.py#L239'>rotkehlchen/chain/ethereum/modules/yearn/decoder.py:239:9:</a> PLW0101 Unreachable code in `_handle_transfer_events`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/ethereum/modules/yearn/utils.py#L132'>rotkehlchen/chain/ethereum/modules/yearn/utils.py:132:5:</a> PLW0101 Unreachable code in `_query_yearn_vaults`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/evm/decoding/aave/common.py#L442'>rotkehlchen/chain/evm/decoding/aave/common.py:442:9:</a> PLW0101 Unreachable code in `_decode_incentives_common`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/evm/decoding/balancer/utils.py#L103'>rotkehlchen/chain/evm/decoding/balancer/utils.py:103:5:</a> PLW0101 Unreachable code in `query_balancer_pools`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/evm/decoding/compound/v3/decoder.py#L206'>rotkehlchen/chain/evm/decoding/compound/v3/decoder.py:206:9:</a> PLW0101 Unreachable code in `_decode_supply_or_repay_event`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/evm/decoding/compound/v3/decoder.py#L356'>rotkehlchen/chain/evm/decoding/compound/v3/decoder.py:356:9:</a> PLW0101 Unreachable code in `_decode_collateral_movement`
+ <a href='https://github.com/rotki/rotki/blob/c624060abdd57e365dcf35fb5aceafae422a0d06/rotkehlchen/chain/evm/decoding/curve/decoder.py#L621'>rotkehlchen/chain/evm/decoding/curve/decoder.py:621:13:</a> PLW0101 Unreachable code in `_decode_deposit_and_stake`
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/actions/message_flags.py#L93'>zerver/actions/message_flags.py:93:5:</a> PLW0101 Unreachable code in `do_mark_all_as_read`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/actions/realm_settings.py#L671'>zerver/actions/realm_settings.py:671:9:</a> PLW0101 Unreachable code in `do_delete_all_realm_attachments`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/data_import/slack.py#L1687'>zerver/data_import/slack.py:1687:5:</a> PLW0101 Unreachable code in `get_slack_api_data`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/cache.py#L327'>zerver/lib/cache.py:327:5:</a> PLW0101 Unreachable code in `cache_delete_many`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/export.py#L2693'>zerver/lib/export.py:2693:5:</a> PLW0101 Unreachable code in `get_id_list_gently_from_database`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/export.py#L2708'>zerver/lib/export.py:2708:5:</a> PLW0101 Unreachable code in `chunkify`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/import_realm.py#L1904'>zerver/lib/import_realm.py:1904:5:</a> PLW0101 Unreachable code in `get_incoming_message_ids`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/retention.py#L179'>zerver/lib/retention.py:179:5:</a> PLW0101 Unreachable code in `run_archiving`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/soft_deactivation.py#L299'>zerver/lib/soft_deactivation.py:299:5:</a> PLW0101 Unreachable code in `do_soft_deactivate_users`
+ <a href='https://github.com/zulip/zulip/blob/18fdbc15196bbb8501435586ff60aab2c67aa345/zerver/lib/templates.py#L190'>zerver/lib/templates.py:190:5:</a> PLW0101 Unreachable code in `webpack_entry`
... 7 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0101 | 165 | 164 | 1 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @dylwil3 on 2025-04-21 19:18_

---

_Renamed from "[WIP] Cfg loops" to "Control flow: loops" by @dylwil3 on 2025-04-21 19:19_

---

_Label `internal` added by @dylwil3 on 2025-04-21 19:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:105 on 2025-04-22 15:57_

Is this the condition in a loop? Maybe `LoopCondition`? Either way, it would be great to document what this is.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:155 on 2025-04-22 15:57_

A better name for the lifetime might be `'ast`, considering that both usages aren't for statements.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:161 on 2025-04-22 15:58_

I'm a bit confused by the name. Should this simply be `StopIteration` or is the condition `true` if `next` **doesn't** return `StopIteration`?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:240 on 2025-04-22 16:01_

What's the case where `self.current == guard`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:254 on 2025-04-22 16:05_

The fact that it's the caller's responsibility to ensure the *`conditions` and `targets` must wlays be of the same length* constraint seems a bit fragile. How about an API like:

```rust
let edges = Edges::default()
    .add(body, Condition::Test(&stmt_while.test))
    .add(guard_target, Condition::Else);
```

It also makes it clearer how the targets and conditions belong together (I had to look at the source of `Edges` to understand the `conditions` variable.)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:229 on 2025-04-22 16:08_

Is the `end + 1` indexing safe? What if the loop is the last statement?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:450 on 2025-04-22 16:11_

Nit: lets separete the words. 

```suggestion
        let Some(current_block) = self.cfg.blocks.get_mut(self.current) else {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:350 on 2025-04-22 16:14_

The parser could very well create an AST where a break statement is used outside a loop. That's why I'd prefer to handle this case gracefully (simply ignore the break statement if outside a loop, treat it like any other statement). Unless there's some documentation saying that the AST must be valid.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:367 on 2025-04-22 16:15_

I guess that's the same as `start < stmts.len()`

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:486 on 2025-04-22 16:17_

Nit: I find it useful to add assertion that "stack" variables are empty once the builder phase is completed. It helped me in the past to find bugs. You could add a `asser_eq!(self.loops, &[])` to `CfgBuilder::finish`

---

_@MichaReiser approved on 2025-04-22 16:18_

---

_@dylwil3 reviewed on 2025-04-26 20:33_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/graph.rs`:229 on 2025-04-26 20:33_

yep it's safe: https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=85e85507ac6e36cf45c499bc4d438d59

---

_Converted to draft by @dylwil3 on 2025-10-24 19:40_

---

_Comment by @MichaReiser on 2025-12-10 17:41_

@dylwil3 should we close the CFG PRs. It's a shame but it doesn't look like we'll get back to them anytime soon (too much else that's on our plate).

---

_Closed by @MichaReiser on 2025-12-10 17:41_

---

_Reopened by @MichaReiser on 2025-12-10 17:41_

---

_Closed by @dylwil3 on 2025-12-10 18:00_

---
