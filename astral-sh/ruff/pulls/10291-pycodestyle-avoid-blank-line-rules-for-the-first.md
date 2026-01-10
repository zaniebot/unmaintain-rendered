```yaml
number: 10291
title: "[`pycodestyle`] Avoid blank line rules for the first logical line in cell"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - rule
assignees: []
merged: true
base: main
head: hoel/fix_10228
created_at: 2024-03-08T03:41:54Z
updated_at: 2024-03-25T13:09:42Z
url: https://github.com/astral-sh/ruff/pull/10291
synced_at: 2026-01-10T22:47:01Z
```

# [`pycodestyle`] Avoid blank line rules for the first logical line in cell

---

_Pull request opened by @hoel-bagard on 2024-03-08 03:41_

## Summary

Closes #10228

The PR makes the blank lines rules keep track of the cell status when running on a notebook, and makes the rules not trigger when the line is the first of the cell.

## Test Plan

The example given in #10228 is added as a fixture, along with a few tests from the main blank lines fixtures.

---

_Comment by @github-actions[bot] on 2024-03-08 03:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -111 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -111 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- docs/source/deep-learning/keras/quickstart/quickstart_keras.ipynb:cell 21:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/deep-learning/keras/quickstart/quickstart_keras.ipynb:cell 22:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/deep-learning/pytorch/quickstart/pytorch_quickstart.ipynb:cell 14:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/deep-learning/pytorch/quickstart/pytorch_quickstart.ipynb:cell 16:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/deep-learning/pytorch/quickstart/pytorch_quickstart.ipynb:cell 23:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/deep-learning/pytorch/quickstart/pytorch_quickstart.ipynb:cell 25:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/deep-learning/pytorch/quickstart/pytorch_quickstart.ipynb:cell 27:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/deep-learning/tensorflow/quickstart/quickstart_tensorflow.ipynb:cell 30:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/deep-learning/tensorflow/quickstart/quickstart_tensorflow.ipynb:cell 9:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/getting-started/logging-first-model/notebooks/logging-first-model.ipynb:cell 14:3:1: E305 [*] Expected 2 blank lines after class or function definition, found (1)
- docs/source/llms/custom-pyfunc-for-llms/notebooks/custom-pyfunc-advanced-llm.ipynb:cell 11:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/custom-pyfunc-for-llms/notebooks/custom-pyfunc-advanced-llm.ipynb:cell 9:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/langchain/notebooks/langchain-retriever.ipynb:cell 11:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/langchain/notebooks/langchain-retriever.ipynb:cell 13:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/langchain/notebooks/langchain-retriever.ipynb:cell 19:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/langchain/notebooks/langchain-retriever.ipynb:cell 21:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/langchain/notebooks/langchain-retriever.ipynb:cell 9:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/llm-evaluate/notebooks/rag-evaluation-llama2.ipynb:cell 14:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/llm-evaluate/notebooks/rag-evaluation-llama2.ipynb:cell 16:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/llm-evaluate/notebooks/rag-evaluation.ipynb:cell 10:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/llm-evaluate/notebooks/rag-evaluation.ipynb:cell 13:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (1)
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 16:2:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 18:2:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 22:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 24:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 28:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 30:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 32:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 33:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 35:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 36:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/openai/notebooks/openai-embeddings-generation.ipynb:cell 13:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/openai/notebooks/openai-embeddings-generation.ipynb:cell 15:3:1: E305 [*] Expected 2 blank lines after class or function definition, found (1)
- docs/source/llms/openai/notebooks/openai-embeddings-generation.ipynb:cell 9:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/mlflow-e2e-evaluation.ipynb:cell 22:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/mlflow-e2e-evaluation.ipynb:cell 26:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/mlflow-e2e-evaluation.ipynb:cell 28:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/mlflow-e2e-evaluation.ipynb:cell 30:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 22:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 23:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 28:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 29:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 53:1:1: E302 [*] Expected 2 blank lines, found 0
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 8:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/rag/notebooks/retriever-evaluation-tutorial.ipynb:cell 22:2:1: E302 [*] Expected 2 blank lines, found 0
... 37 additional changes omitted for rule E302
- docs/source/llms/rag/notebooks/retriever-evaluation-tutorial.ipynb:cell 24:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/rag/notebooks/retriever-evaluation-tutorial.ipynb:cell 45:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/sentence-transformers/tutorials/paraphrase-mining/paraphrase-mining-sentence-transformers.ipynb:cell 8:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/sentence-transformers/tutorials/semantic-search/semantic-search-sentence-transformers.ipynb:cell 8:1:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
- docs/source/llms/sentence-transformers/tutorials/semantic-similarity/semantic-similarity-sentence-transformers.ipynb:cell 8:2:1: E305 [*] Expected 2 blank lines after class or function definition, found (0)
... 61 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E302 | 62 | 0 | 62 | 0 | 0 |
| E305 | 49 | 0 | 49 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @hoel-bagard on 2024-03-08 04:34_

---

_Review requested from @dhruvmanila by @dhruvmanila on 2024-03-08 04:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:522 on 2024-03-08 07:54_

You may want to use `containing_range` here. `contains` is `O(n)`, `containing_range` is `O(log(n))`. But I wonder if we could do better by keeping an iterator with the cell offsets (they're sorted) and peek at the first offset and:

* while it is smaller than the `line_start`, call next 
* if it is not `None`, you know its inside of the cell. 
This gives you `O(n)` performance

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:528 on 2024-03-08 07:56_

Is it okay that we set `is_cell_first_non_comment_line` to `true` even if `line_is_comment_only` is true?

---

_@MichaReiser reviewed on 2024-03-08 07:57_

---

_@hoel-bagard reviewed on 2024-03-08 08:04_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:528 on 2024-03-08 08:04_

It's fine since if a cell starts with a comment, we don't want to run the check on that comment (since it's the first line).
However the naming is pretty bad indeed, I'll try to improve it.

Edit: renamed in [f29a53d](https://github.com/astral-sh/ruff/pull/10291/commits/f29a53dc47ced8c51b3dd2b303d9b79b0b370524).

---

_@hoel-bagard reviewed on 2024-03-09 11:04_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:522 on 2024-03-09 11:04_

@MichaReiser It's not exactly what you said, but I gave it a try in 3f703afa6. What do you think ?

---

_Review requested from @MichaReiser by @hoel-bagard on 2024-03-11 13:25_

---

_Comment by @MichaReiser on 2024-03-18 13:38_

@dhruvmanila would you mind taking a look

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:407 on 2024-03-18 13:40_

I wonder if this panics for an empty file (that contains no new lines). It might be safer to use `cell_offsets.get(1..).unwrap_or_default().iter().peekable()`

---

_@MichaReiser approved on 2024-03-18 13:41_

---

_@hoel-bagard reviewed on 2024-03-18 14:27_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:407 on 2024-03-18 14:27_

Done in [05a7473](https://github.com/astral-sh/ruff/pull/10291/commits/05a747381f5efb2509fd4c1c2512dd55c604fb09).

---

_Comment by @dhruvmanila on 2024-03-18 14:44_

(Sorry, will look at it today.)

---

_@dhruvmanila requested changes on 2024-03-19 08:23_

I've started looking into it and testing out the code for the `E30` rules. I've noticed a few things:

### `E303`

For the following two cells:
```python
# There are two blank lines after `function1`
def function1():
	pass

 
```

```python
 
def function2():
	pass
# There is one blank line before `function2`
```

This is still raising the `E303` error. I guess this is because we `continue` without checking for the cell boundary here:

https://github.com/astral-sh/ruff/blob/461cdad53a1569d6b6dfa242734b140bcad1b7b7/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs#L430-L437

I think the intent for the fix should be to reset the count of blank lines such that the number of empty lines before the `function2` gives us 1 instead of 3.

### `E304`

I don't really have a good way of testing this unless we actually define the decorator and function definition in separate cells like so:

```python
# One empty line after the decorator
@decorator
 
```

```
def function():
	pass
```

But this is highly unlikely to come up in a real world and in the future we would be making sure that this raises a syntax error (#10264). Nevertheless this suffers from the same problem as mentioned earlier. I'm assuming the root cause is the same.

---

_Comment by @hoel-bagard on 2024-03-22 13:51_

@dhruvmanila Thanks for the review. I've fixed the `E303` issue you found.

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-03-25 07:58_

---

_@dhruvmanila approved on 2024-03-25 11:08_

Thank you!

---

_Renamed from "[`pycodestyle`] Fix blank line rules adding blank lines at the beginning of cells." to "[`pycodestyle`] Avoid blank line rules for the first logical line in cell" by @dhruvmanila on 2024-03-25 11:09_

---

_Label `rule` added by @dhruvmanila on 2024-03-25 11:09_

---

_Merged by @dhruvmanila on 2024-03-25 11:19_

---

_Closed by @dhruvmanila on 2024-03-25 11:19_

---

_Branch deleted on 2024-03-25 13:09_

---
