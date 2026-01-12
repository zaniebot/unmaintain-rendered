```yaml
number: 17121
title: "Control flow: `return` and `raise`"
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: cfg-jumps
created_at: 2025-04-01T13:45:24Z
updated_at: 2025-04-03T13:30:29Z
url: https://github.com/astral-sh/ruff/pull/17121
synced_at: 2026-01-12T15:56:00Z
```

# Control flow: `return` and `raise`

---

_@dylwil3_

We add support for `return` and `raise` statements in the control flow graph: we simply add an edge to the terminal block, push the statements to the current block, and proceed.

This implementation will have to be modified somewhat once we add support for `try` statements - then we will need to check whether to _defer_ the jump. But for now this will do!

Also in this PR: We fix the `unreachable` diagnostic range so that it lumps together consecutive unreachable blocks.


---

_Label `internal` added by @dylwil3 on 2025-04-01 13:45_

---

_Comment by @github-actions[bot] on 2025-04-01 13:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/src/airflow/models/mappedoperator.py#L71'>airflow-core/src/airflow/models/mappedoperator.py:71:9:</a> PLW0101 Unreachable code in `expand_start_from_trigger`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/src/airflow/triggers/base.py#L103'>airflow-core/src/airflow/triggers/base.py:103:9:</a> PLW0101 Unreachable code in `run`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_baseoperator.py#L213'>airflow-core/tests/unit/models/test_baseoperator.py:213:21:</a> PLW0101 Unreachable code in `my_work`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L1171'>airflow-core/tests/unit/models/test_mappedoperator.py:1171:17:</a> PLW0101 Unreachable code in `my_work`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L1209'>airflow-core/tests/unit/models/test_mappedoperator.py:1209:25:</a> PLW0101 Unreachable code in `my_work`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L1256'>airflow-core/tests/unit/models/test_mappedoperator.py:1256:25:</a> PLW0101 Unreachable code in `my_work`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L565'>airflow-core/tests/unit/models/test_mappedoperator.py:565:21:</a> PLW0101 Unreachable code in `my_setup`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L636'>airflow-core/tests/unit/models/test_mappedoperator.py:636:21:</a> PLW0101 Unreachable code in `other_setup`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L652'>airflow-core/tests/unit/models/test_mappedoperator.py:652:21:</a> PLW0101 Unreachable code in `my_setup`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L678'>airflow-core/tests/unit/models/test_mappedoperator.py:678:21:</a> PLW0101 Unreachable code in `other_setup`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L851'>airflow-core/tests/unit/models/test_mappedoperator.py:851:21:</a> PLW0101 Unreachable code in `my_work`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_mappedoperator.py#L868'>airflow-core/tests/unit/models/test_mappedoperator.py:868:21:</a> PLW0101 Unreachable code in `my_work`
+ <a href='https://github.com/apache/airflow/blob/bc04e4e31e062264f9ed515b9914b7c4831d1b18/airflow-core/tests/unit/models/test_taskinstance.py#L3423'>airflow-core/tests/unit/models/test_taskinstance.py:3423:13:</a> PLW0101 Unreachable code in `on_finish_callable`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/8c8bca68b25464c8064952534c7e8d4769186120/libs/core/tests/unit_tests/runnables/test_fallbacks.py#L267'>libs/core/tests/unit_tests/runnables/test_fallbacks.py:267:5:</a> PLW0101 Unreachable code in `_generate_immediate_error`
+ <a href='https://github.com/langchain-ai/langchain/blob/8c8bca68b25464c8064952534c7e8d4769186120/libs/core/tests/unit_tests/runnables/test_fallbacks.py#L295'>libs/core/tests/unit_tests/runnables/test_fallbacks.py:295:5:</a> PLW0101 Unreachable code in `_agenerate_immediate_error`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/62c4d98a8256e7f179d69632fa00d24aa440bf57/src/latch_cli/snakemake/single_task_snakemake.py#L236'>src/latch_cli/snakemake/single_task_snakemake.py:236:5:</a> PLW0101 Unreachable code in `empty_generator`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/0f037ba3cb1a23466d4d5ae111240ca85170da57/examples/iterator/iterator.py#L64'>examples/iterator/iterator.py:64:9:</a> PLW0101 Unreachable code in `external_filter_func`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/2382a9912a025c71eb9802c2efe566457b524a7c/astropy/table/tests/test_bst.py#L18'>astropy/table/tests/test_bst.py:18:5:</a> PLW0101 Unreachable code in `tree`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0101 | 18 | 18 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW0101_unreachable.py.snap`:8 on 2025-04-01 14:10_

We need multiline span diagnostics so that we can highlight the "why". (CC: @ntBre might be a good first rule to port)

With our current noqa system. Would a user have to put the noqa on the last line? Would that be confusing if they have

```py
return 1

if a:
	pass
else:
	other # noqa: unreachable
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:197 on 2025-04-01 14:12_

Nit: Too much math for my brain ;)
```suggestion
        // `start < end`. Since `end < stmts.len()` we conclude that
        // `start <= stmts.len()`. It is therefore always safe to use `start` as
```

And should it be `start < stmts.len`. If not, then indexing isn't safe because `stmts[len]` panics

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/cfg/graph.rs`:238 on 2025-04-01 14:15_

This seems a bit repetitive. We may want to abstract this somehow (e.g. store `stmts` in the current block and have a `self.branch(offset)` or similar). But I'm fine deferring this a little longer

---

_@MichaReiser approved on 2025-04-01 14:16_

Doing this incrementally makes it convenient to review. Thank you.

---

_@dylwil3 reviewed on 2025-04-01 15:10_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/cfg/graph.rs`:197 on 2025-04-01 15:10_

Unfortunately the math is necessary to be correct! `start <= end + 1` is not the same as `start < end`, and I kept the second inequality as `<=` so it would be easier to combine them.

It can happen that `start == stmts.len()` but only upon exiting the loop. This is safe because the only other place we use `start` to slice is in the form `stmts[start..]`. This is safe even if `start = stmts.len()`. 

I'll add some clarification to the comment

---

_@dylwil3 reviewed on 2025-04-01 18:40_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW0101_unreachable.py.snap`:8 on 2025-04-01 18:40_

I think it's the opposite: noqa comments go at the _start_ of the range. So I think it would be 

```python
return 1

if a: # noqa: unreachable
	pass
else:
	other 
```

[Playground example with existing rule](https://play.ruff.rs/6b0806b8-b409-4951-8cfc-a82c5359106e)


---

_Merged by @dylwil3 on 2025-04-03 13:30_

---

_Closed by @dylwil3 on 2025-04-03 13:30_

---
