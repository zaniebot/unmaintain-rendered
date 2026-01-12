```yaml
number: 15308
title: "[`pycodestyle`] Handle each cell separately for `too-many-newlines-at-end-of-file` (`W391`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
  - notebook
assignees: []
merged: true
base: main
head: newlines-notebook
created_at: 2025-01-06T23:21:48Z
updated_at: 2025-01-09T16:50:40Z
url: https://github.com/astral-sh/ruff/pull/15308
synced_at: 2026-01-12T15:55:50Z
```

# [`pycodestyle`] Handle each cell separately for `too-many-newlines-at-end-of-file` (`W391`)

---

_@dylwil3_

Jupyter notebooks are converted into source files by joining with newlines, which confuses the check [too-many-newlines-at-end-of-file (W391)](https://docs.astral.sh/ruff/rules/too-many-newlines-at-end-of-file/#too-many-newlines-at-end-of-file-w391). This PR introduces logic to apply the check cell-wise (and, in particular, correctly handles empty cells.)

Closes #13763


---

_Label `rule` added by @dylwil3 on 2025-01-06 23:21_

---

_Label `preview` added by @dylwil3 on 2025-01-06 23:21_

---

_Label `notebook` added by @dylwil3 on 2025-01-06 23:21_

---

_Comment by @dylwil3 on 2025-01-06 23:23_

One aesthetic question is whether internal cells should still be allowed to have a single trailing newline (at the moment this is what the fix produces/allows).

---

_Comment by @github-actions[bot] on 2025-01-06 23:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -17 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- docs/notebooks/dispersion/stix_dispersion.ipynb:cell 34:1:1: W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ examples/output/jupyter/push_notebook/Basic Usage.ipynb:cell 14:1:1: W391 [*] Extra newline at end of cell
- examples/output/jupyter/push_notebook/Basic Usage.ipynb:cell 14:1:1: W391 [*] Too many newlines at end of file
- examples/output/jupyter/push_notebook/Continuous Updating.ipynb:cell 8:1:1: W391 [*] Extra newline at end of file
- examples/output/jupyter/push_notebook/Jupyter Interactors.ipynb:cell 8:1:1: W391 [*] Extra newline at end of file
- examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 28:1:1: W391 [*] Extra newline at end of file
+ examples/server/api/notebook_embed.ipynb:cell 7:1:1: W391 [*] Extra newline at end of cell
- examples/server/api/notebook_embed.ipynb:cell 7:1:1: W391 [*] Too many newlines at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- docs/source/llms/llm-evaluate/notebooks/question-answering-evaluation.ipynb:cell 29:1:1: W391 [*] Extra newline at end of file
- docs/source/llms/llm-evaluate/notebooks/rag-evaluation.ipynb:cell 19:1:1: W391 [*] Extra newline at end of file
- docs/source/llms/rag/notebooks/retriever-evaluation-tutorial.ipynb:cell 50:1:1: W391 [*] Extra newline at end of file
- docs/source/traditional-ml/hyperparameter-tuning-with-child-runs/notebooks/parent-child-runs.ipynb:cell 15:1:1: W391 [*] Extra newline at end of file
- docs/source/traditional-ml/serving-multiple-models-with-pyfunc/notebooks/MME_Tutorial.ipynb:cell 25:1:1: W391 [*] Extra newline at end of file
- examples/evaluation/qa-evaluation.ipynb:cell 29:1:1: W391 [*] Extra newline at end of file
- examples/evaluation/rag-evaluation.ipynb:cell 16:1:1: W391 [*] Extra newline at end of file
- examples/h2o/random_forest.ipynb:cell 6:1:1: W391 [*] Extra newline at end of file
- examples/llms/RAG/retriever-evaluation-tutorial.ipynb:cell 48:1:1: W391 [*] Extra newline at end of file
- examples/rapids/mlflow_project/notebooks/rapids_mlflow.ipynb:cell 14:1:1: W391 [*] Extra newline at end of file
- examples/sklearn_elasticnet_wine/train.ipynb:cell 6:1:1: W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| W391 | 19 | 2 | 17 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__preview__W391_W391.ipynb.snap`:4 on 2025-01-07 08:09_

I think we should change the message to: Too many newlines at the end of the cell

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/too_many_newlines_at_end_of_file.rs`:10 on 2025-01-07 08:10_

We should extend the rule documentation to mention its behavior in a notebook context

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/too_many_newlines_at_end_of_file.rs`:75 on 2025-01-07 08:15_

It feels like the only part that needs sharing between notebooks and regular documents is line 98 to 142. Would it make sense to extract this logic into its own function instead of "faking" a notebook for normal documents? 

This would have the advantage that you can move the `cell_offsets.iter().rev` into `collect_trailing_newlines_diagnostics` (you probably want to rename that method), which, in turn, makes the comment about `offset_iter` being reverse obsolete.

I'd also expect that doing this would reduce the diff size.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/too_many_newlines_at_end_of_file.rs`:96 on 2025-01-07 08:17_

Nit: You could use `tokens_iter.peeking_take_while`

---

_@MichaReiser reviewed on 2025-01-07 08:18_

---

_Comment by @MichaReiser on 2025-01-07 08:19_

> One aesthetic question is whether internal cells should still be allowed to have a single trailing newline (at the moment this is what the fix produces/allows).

I think we should and doing so would be consistent with the formatter. 

It would be very annoying if you write a cell with a newline at the end, hit save and Ruff fixes away the newline before you had a chance to write the next content.

---

_Review requested from @MichaReiser by @dylwil3 on 2025-01-07 17:37_

---

_@MichaReiser approved on 2025-01-08 07:28_

Thanks

---

_Merged by @dylwil3 on 2025-01-09 16:50_

---

_Closed by @dylwil3 on 2025-01-09 16:50_

---
