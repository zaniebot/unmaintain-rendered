```yaml
number: 17064
title: "Control flow graph: setup"
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: cfg
created_at: 2025-03-30T21:19:27Z
updated_at: 2025-04-01T10:53:43Z
url: https://github.com/astral-sh/ruff/pull/17064
synced_at: 2026-01-10T19:40:36Z
```

# Control flow graph: setup

---

_Pull request opened by @dylwil3 on 2025-03-30 21:19_

This PR contains the scaffolding for a new control flow graph implementation, along with its application to the `unreachable` rule. At the moment, the implementation is a maximal over-approximation: no control flow is modeled and all statements are counted as reachable. With each additional statement type we support, this approximation will improve.

So this PR just contains:
- A `ControlFlowGraph` struct and builder
- Support for printing the flow graph as a Mermaid graph
- Snapshot tests for the actual graphs
- (a very bad!) reimplementation of `unreachable` using the new structs
- Snapshot tests for `unreachable`

# Instructions for Viewing Mermaid snapshots
Unfortunately I don't know how to convince GitHub to render the Mermaid graphs in the snapshots. However, you can view these locally in VSCode if you install an extension that supports Mermaid graphs in Markdown, and then add this to your `settings.json`:

```json
  "files.associations": {
"*.md.snap": "markdown",
  }
  ```

---

_Label `do-not-merge` added by @dylwil3 on 2025-03-30 21:19_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-03-30 21:19_

---

_Comment by @github-actions[bot] on 2025-03-30 21:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -25 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/src/airflow/models/mappedoperator.py#L71'>airflow-core/src/airflow/models/mappedoperator.py:71:9:</a> PLW0101 Unreachable code in `expand_start_from_trigger`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/src/airflow/triggers/base.py#L103'>airflow-core/src/airflow/triggers/base.py:103:9:</a> PLW0101 Unreachable code in `run`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/src/airflow/triggers/testing.py#L52'>airflow-core/src/airflow/triggers/testing.py:52:13:</a> PLW0101 Unreachable code in `run`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_baseoperator.py#L213'>airflow-core/tests/unit/models/test_baseoperator.py:213:21:</a> PLW0101 Unreachable code in `my_work`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L1171'>airflow-core/tests/unit/models/test_mappedoperator.py:1171:17:</a> PLW0101 Unreachable code in `my_work`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L1209'>airflow-core/tests/unit/models/test_mappedoperator.py:1209:25:</a> PLW0101 Unreachable code in `my_work`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L1256'>airflow-core/tests/unit/models/test_mappedoperator.py:1256:25:</a> PLW0101 Unreachable code in `my_work`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L565'>airflow-core/tests/unit/models/test_mappedoperator.py:565:21:</a> PLW0101 Unreachable code in `my_setup`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L636'>airflow-core/tests/unit/models/test_mappedoperator.py:636:21:</a> PLW0101 Unreachable code in `other_setup`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L652'>airflow-core/tests/unit/models/test_mappedoperator.py:652:21:</a> PLW0101 Unreachable code in `my_setup`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L678'>airflow-core/tests/unit/models/test_mappedoperator.py:678:21:</a> PLW0101 Unreachable code in `other_setup`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L851'>airflow-core/tests/unit/models/test_mappedoperator.py:851:21:</a> PLW0101 Unreachable code in `my_work`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_mappedoperator.py#L868'>airflow-core/tests/unit/models/test_mappedoperator.py:868:21:</a> PLW0101 Unreachable code in `my_work`
- <a href='https://github.com/apache/airflow/blob/d346df13639da34e0420cd6d9550d6e41c8cd2c1/airflow-core/tests/unit/models/test_taskinstance.py#L3423'>airflow-core/tests/unit/models/test_taskinstance.py:3423:13:</a> PLW0101 Unreachable code in `on_finish_callable`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/abbe222dcee90ef2dcc8218a2e151b4ed7935715/admin/securedrop_admin/__init__.py#L78'>admin/securedrop_admin/__init__.py:78:5:</a> PLW0101 Unreachable code in `openssh_version`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/ecff055096bc137bc10d7933d71016e2af56c06d/libs/core/langchain_core/runnables/base.py#L5093'>libs/core/langchain_core/runnables/base.py:5093:13:</a> PLW0101 Unreachable code in `astream_events`
- <a href='https://github.com/langchain-ai/langchain/blob/ecff055096bc137bc10d7933d71016e2af56c06d/libs/core/tests/unit_tests/runnables/test_fallbacks.py#L267'>libs/core/tests/unit_tests/runnables/test_fallbacks.py:267:5:</a> PLW0101 Unreachable code in `_generate_immediate_error`
- <a href='https://github.com/langchain-ai/langchain/blob/ecff055096bc137bc10d7933d71016e2af56c06d/libs/core/tests/unit_tests/runnables/test_fallbacks.py#L295'>libs/core/tests/unit_tests/runnables/test_fallbacks.py:295:5:</a> PLW0101 Unreachable code in `_agenerate_immediate_error`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/62c4d98a8256e7f179d69632fa00d24aa440bf57/src/latch_cli/snakemake/single_task_snakemake.py#L236'>src/latch_cli/snakemake/single_task_snakemake.py:236:5:</a> PLW0101 Unreachable code in `empty_generator`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/0f037ba3cb1a23466d4d5ae111240ca85170da57/examples/iterator/iterator.py#L64'>examples/iterator/iterator.py:64:9:</a> PLW0101 Unreachable code in `external_filter_func`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/83979d6a0c5a223bac2af8ef706c5ff8d432bcca/pandas/io/parsers/python_parser.py#L1314'>pandas/io/parsers/python_parser.py:1314:25:</a> PLW0101 Unreachable code in `_get_lines`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/33e2e262d688ed2951ec5174e51545fda918e6f2/src/_pytest/config/__init__.py#L1870'>src/_pytest/config/__init__.py:1870:9:</a> PLW0101 Unreachable code in `_assertion_supported`
- <a href='https://github.com/pytest-dev/pytest/blob/33e2e262d688ed2951ec5174e51545fda918e6f2/testing/code/test_code.py#L151'>testing/code/test_code.py:151:17:</a> PLW0101 Unreachable code in `test_bad_getsource`
- <a href='https://github.com/pytest-dev/pytest/blob/33e2e262d688ed2951ec5174e51545fda918e6f2/testing/code/test_code.py#L167'>testing/code/test_code.py:167:17:</a> PLW0101 Unreachable code in `test_getsource`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/2382a9912a025c71eb9802c2efe566457b524a7c/astropy/table/tests/test_bst.py#L18'>astropy/table/tests/test_bst.py:18:5:</a> PLW0101 Unreachable code in `tree`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0101 | 25 | 0 | 25 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-03-31 10:23_

I don't think I follow your PR approach. Is the idea that you want to reuse this PR as the target of stacked PRs later on? If so, what's the reason for merging the stacked PRs into this PR over directly merging to main (the `unreachable` rule has never been shipped)? 

To phrase it differently. What prevents us from merging this to main?


---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/unreachable.py`:1 on 2025-03-31 10:24_

Do we want to preserve those test cases somewhere/somehow? 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:20 on 2025-03-31 10:29_

Can you explain what `empty` means (as part of the comment)? Is it that the `BlockId` is empty or is it that the block is empty? 

It might be sufficient to always speak of blocks instead of nodes here (it's clear to me what an empty block is, I don't know what an empty node is)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:116 on 2025-03-31 10:32_

I think it's worth documenting that `conditions` and `targets` must always be the exact same length.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:232 on 2025-03-31 10:35_

This line isn't clear to me and the comment doesn't add much. What's this line about? Aren't we always setting all statements?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:275 on 2025-03-31 10:36_

Nit: `set_current_block_stmts` and `set_current_block_edges` to make it clear that this only mutates the current block and not the state of the entire builder.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:176 on 2025-03-31 10:38_

What do you think of always creating an empty block for the start of the CFG. Could this simplify the `set_stmts` and `set_edgs` function because we can create a `add_block(stmts, edges)` function?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/mod.rs`:20 on 2025-03-31 10:38_

Is this the same as?

```suggestion
        let path = PathBuf::from("resources/test/fixtures/cfg").join(filename);
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/mod.rs`:31 on 2025-03-31 10:40_

Should the test fail if the module body contains any non function statements (it might not be obvious that the framework only captures function level statements)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:81 on 2025-03-31 10:45_

Can you add some documentation what `parents` are in a basic block context? Is it the incoming basic blocks? 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:115 on 2025-03-31 10:46_

I don't think this has to defer this PR further but I wonder if we could use a structure similar to https://docs.rs/petgraph/latest/petgraph/graph/struct.Graph.html where `Node` and `Edge`s avoid allocations by roughly using an arena for nodes and edges and links between nodes and edges (all fixed size, using a fixed array)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:13 on 2025-03-31 10:48_

It would be great to have some module or struct level documentation explaining what a CFG is, what are basic blocks, how are they connected, and how the data is represented. 

You may want to wait with writing this documentation if we end up changing the representation of `Nodes` (`Block`) and `Edges`

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:15 on 2025-03-31 10:49_

Nit: The all uppercase breaks my mind but I'm not sure why. I'm sort of leaning towards avoiding the `cfg` abbreviation and just using the full name `ControlFlowGraph` (also less confusing with rusts `cfg!` macro

---

_@MichaReiser approved on 2025-03-31 10:50_

This overall looks good to me. I left a few in detail comments. 

The biggest one is whether we want to consider an alternative representation for `Block` and `Edge`s that avoids the nested vectors, which is also something we can consider doing as a separate PR (and maybe even better to get an understanding for the performance characteristics)

---

_Comment by @dylwil3 on 2025-03-31 13:21_

> I don't think I follow your PR approach. Is the idea that you want to reuse this PR as the target of stacked PRs later on? If so, what's the reason for merging the stacked PRs into this PR over directly merging to main (the `unreachable` rule has never been shipped)?
> 
> To phrase it differently. What prevents us from merging this to main?

Nothing prevents us from merging into main except that the code would just be sitting there not used for anything for a bit. If it makes things less complicated I'm happy to just do sequential PRs directly into main!

---

_@dylwil3 reviewed on 2025-03-31 13:22_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/graph.rs`:15 on 2025-03-31 13:22_

That's fine with me! do you also want me to change the name of the submodule itself to `control_flow_graph` or `control_flow` or something instead of `cfg`?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:15 on 2025-03-31 13:48_

Ha, I think the module name is fine... I know, I don't make much sense

---

_@MichaReiser reviewed on 2025-03-31 13:48_

---

_Comment by @MichaReiser on 2025-03-31 13:49_

> Nothing prevents us from merging into main except that the code would just be sitting there not used for anything for a bit. If it makes things less complicated I'm happy to just do sequential PRs directly into main!

That seems not much different from the unreachable code that's already on main. I'm all for merging to main because it makes our live easier.

---

_@dylwil3 reviewed on 2025-03-31 13:52_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/graph.rs`:176 on 2025-03-31 13:52_

I think we could certainly make the initial and final blocks empty. It may also be possible to always arrange that we set statements and edges on a block at the same time - but I'm not sure. I don't think we can call this `add_block` though because we will often need to create a block (and assign it an index) before we populate it.

I think it may make sense to wait until some or all of the statement kinds have been implemented, and then see if it's possible to do some clean up like this. Does that sound okay?

---

_@dylwil3 reviewed on 2025-03-31 13:59_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/graph.rs`:13 on 2025-03-31 13:59_

I agree! I plan on writing detailed documentation of the spec and the implementation, but was deferring it until things were more solidified. A draft of the spec documentation I had in mind is in the tracking issue https://github.com/astral-sh/ruff/issues/17065

---

_Label `do-not-merge` removed by @dylwil3 on 2025-03-31 17:22_

---

_Label `internal` added by @dylwil3 on 2025-03-31 17:22_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/mod.rs`:31 on 2025-04-01 10:37_

By framework do you mean the framework of the test or of the control flow graph? The control flow graph should work the same for modules as well, it's just the snapshot test that is organized this way so that you can have more than one graph per file.

I've made the test fail as you suggest, but may revert that later if it makes sense to allow, say, import statements for certain examples.

---

_@dylwil3 reviewed on 2025-04-01 10:37_

---

_@dylwil3 reviewed on 2025-04-01 10:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/unreachable.py`:1 on 2025-04-01 10:38_

I'll let `git` do the preserving! I plan to bring them back by the end of the series of PRs

---

_@dylwil3 reviewed on 2025-04-01 10:43_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/graph.rs`:115 on 2025-04-01 10:43_

This is a very interesting idea! I think for now I will defer this and do some perf experiments in the end. But I will keep this in mind and try to write the implementation in a way that would not make such a refactor too painful.

The previous implementation got around this by keeping their graph bivalent at the cost of having more nodes. Another idea is to make the array sizes used in the `SmallVec`'s bigger. It's not clear to me (but maybe it is to you!) the relative performance of these three options - and it's even less clear how much of a difference it makes on real world Python code. I think we'd have a better idea once the full implementation is in place and we can do some benchmarking.

---

_Renamed from "Reimplement the control flow graph" to "Control flow graph: setup" by @dylwil3 on 2025-04-01 10:53_

---

_Merged by @dylwil3 on 2025-04-01 10:53_

---

_Closed by @dylwil3 on 2025-04-01 10:53_

---
