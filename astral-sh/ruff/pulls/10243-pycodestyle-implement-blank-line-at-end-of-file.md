```yaml
number: 10243
title: "[`pycodestyle`] Implement `blank-line-at-end-of-file` (`W391`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: too_many_newlines
created_at: 2024-03-06T05:35:27Z
updated_at: 2024-04-30T18:06:46Z
url: https://github.com/astral-sh/ruff/pull/10243
synced_at: 2026-01-10T22:37:01Z
```

# [`pycodestyle`] Implement `blank-line-at-end-of-file` (`W391`)

---

_Pull request opened by @augustelalande on 2024-03-06 05:35_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements the [blank line at end of file](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes) rule (W391) from pycodestyle. Renamed to TooManyNewlinesAtEndOfFile for clarity.

## Test Plan

New fixtures have been added

Part of #2402


---

_Renamed from "Too many newlines" to "[pycodestyle] Implement "" by @augustelalande on 2024-03-06 05:35_

---

_Renamed from "[pycodestyle] Implement "" to "[pycodestyle] Implement "blank line at end of file" (W391)" by @augustelalande on 2024-03-06 05:36_

---

_Renamed from "[pycodestyle] Implement "blank line at end of file" (W391)" to "[`pycodestyle`] Implement `blank-line-at-end-of-file` (W391)" by @augustelalande on 2024-03-06 05:36_

---

_Renamed from "[`pycodestyle`] Implement `blank-line-at-end-of-file` (W391)" to "[`pycodestyle`] Implement `blank-line-at-end-of-file` (`W391`)" by @augustelalande on 2024-03-06 05:36_

---

_Comment by @codspeed-hq[bot] on 2024-03-06 05:44_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:too_many_newlines)

### Merging #10243 will **not alter performance**

<sub>Comparing <code>augustelalande:too_many_newlines</code> (2d4e2fe) with <code>main</code> (c746912)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-03-06 05:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+27 -0 violations, +0 -0 fixes in 5 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/common/transformations/camera.py#L163'>common/transformations/camera.py:163:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/selfdrive/car/card.py#L142'>selfdrive/car/card.py:142:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/selfdrive/car/subaru/carstate.py#L229'>selfdrive/car/subaru/carstate.py:229:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/selfdrive/debug/internal/fuzz_fw_fingerprint.py#L51'>selfdrive/debug/internal/fuzz_fw_fingerprint.py:51:1:</a> W391 [*] Too many newlines at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/selfdrive/debug/test_fw_query_on_routes.py#L187'>selfdrive/debug/test_fw_query_on_routes.py:187:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/selfdrive/thermald/fan_controller.py#L38'>selfdrive/thermald/fan_controller.py:38:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/system/loggerd/tests/test_loggerd.py#L285'>system/loggerd/tests/test_loggerd.py:285:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/tools/zookeeper/disable.py#L8'>tools/zookeeper/disable.py:8:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/tools/zookeeper/enable_and_wait.py#L31'>tools/zookeeper/enable_and_wait.py:31:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/commaai/openpilot/blob/78d72d7dc390984496dd6633fd588bfd06ae1939/tools/zookeeper/ignition.py#L10'>tools/zookeeper/ignition.py:10:1:</a> W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/docker/utils/__init__.py#L13'>docker/utils/__init__.py:13:1:</a> W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/72f69acef4e73a5d9a17d82ed0402533191c0bc5/blinksync/forms.py#L122'>blinksync/forms.py:122:1:</a> W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/6086d5c99f22590c881511f52ac4ff3d659ca355/examples/example_bulkwriter.py#L418'>examples/example_bulkwriter.py:418:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/milvus-io/pymilvus/blob/6086d5c99f22590c881511f52ac4ff3d659ca355/examples/milvus_client/rbac.py#L104'>examples/milvus_client/rbac.py:104:1:</a> W391 [*] Extra newline at end of file
+ <a href='https://github.com/milvus-io/pymilvus/blob/6086d5c99f22590c881511f52ac4ff3d659ca355/examples/user.py#L93'>examples/user.py:93:1:</a> W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ docs/source/llms/llm-evaluate/notebooks/question-answering-evaluation.ipynb:cell 28:1:1: W391 [*] Extra newline at end of file
+ docs/source/llms/llm-evaluate/notebooks/rag-evaluation-llama2.ipynb:cell 22:1:1: W391 [*] Extra newline at end of file
+ docs/source/llms/llm-evaluate/notebooks/rag-evaluation.ipynb:cell 19:1:1: W391 [*] Extra newline at end of file
+ docs/source/llms/rag/notebooks/retriever-evaluation-tutorial.ipynb:cell 48:1:1: W391 [*] Extra newline at end of file
+ docs/source/traditional-ml/hyperparameter-tuning-with-child-runs/notebooks/parent-child-runs.ipynb:cell 15:1:1: W391 [*] Extra newline at end of file
+ docs/source/traditional-ml/serving-multiple-models-with-pyfunc/notebooks/MME_Tutorial.ipynb:cell 25:1:1: W391 [*] Extra newline at end of file
+ examples/evaluation/qa-evaluation.ipynb:cell 29:1:1: W391 [*] Extra newline at end of file
+ examples/evaluation/rag-evaluation.ipynb:cell 16:1:1: W391 [*] Extra newline at end of file
+ examples/h2o/random_forest.ipynb:cell 6:1:1: W391 [*] Extra newline at end of file
+ examples/llms/RAG/retriever-evaluation-tutorial.ipynb:cell 48:1:1: W391 [*] Extra newline at end of file
+ examples/rapids/mlflow_project/notebooks/rapids_mlflow.ipynb:cell 14:1:1: W391 [*] Extra newline at end of file
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 6:1:1: W391 [*] Extra newline at end of file
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| W391 | 27 | 27 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/too_many_newlines_at_end_of_file.rs`:63 on 2024-03-06 09:07_

It might be best to make this a token based rule and move it to `check_tokens`. You can then simply consume the tokens from the back and test if there's more than one `NEWLINE` token at the back. That should fix the performance regression.

Or you rewrite the rule to not use a `Regex` here (a Regex is a very powerful tool to test for some empty whitespace at the end of a file), because Regex are expensive to construct.

---

_@MichaReiser requested changes on 2024-03-06 09:08_

Thanks for your contribution.

We should convert this to a token based rule OR avoid using a regex to fix the performance regression.


---

_@augustelalande reviewed on 2024-03-06 23:53_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pycodestyle/rules/too_many_newlines_at_end_of_file.rs`:63 on 2024-03-06 23:53_

Done

---

_Review requested from @MichaReiser by @augustelalande on 2024-03-08 04:04_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-12 01:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-12 01:20_

---

_Label `rule` added by @charliermarsh on 2024-03-12 01:20_

---

_Label `preview` added by @charliermarsh on 2024-03-12 01:20_

---

_@charliermarsh approved on 2024-03-12 01:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/too_many_newlines_at_end_of_file.rs`:60 on 2024-03-12 01:36_

I removed the `Locator`. I don't think we need to guard on empty files here -- that's just for the "no trailing newline" check, since empty files would be a false positive.

---

_@charliermarsh reviewed on 2024-03-12 01:36_

---

_Merged by @charliermarsh on 2024-03-12 02:07_

---

_Closed by @charliermarsh on 2024-03-12 02:07_

---

_Branch deleted on 2024-03-12 02:45_

---

_Comment by @Avasam on 2024-04-30 18:06_

One more step towards https://github.com/astral-sh/ruff/discussions/9057 🎉

---
