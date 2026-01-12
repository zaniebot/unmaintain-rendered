```yaml
number: 10999
title: Adapted fix to work identical to format
type: pull_request
state: merged
author: Philipp-Thiel
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: E203_line_break
created_at: 2024-04-17T13:38:04Z
updated_at: 2024-06-08T23:29:45Z
url: https://github.com/astral-sh/ruff/pull/10999
synced_at: 2026-01-12T15:55:34Z
```

# Adapted fix to work identical to format

---

_@Philipp-Thiel_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The fix for E203 now produces the same result as ruff format in cases where a slice ends on a colon and the closing square bracket is on the following line.

Refers to https://github.com/astral-sh/ruff/issues/10973

## Test Plan

The minimal reproduction case in the ticket was added as test case producing no error. Additional cases with multiple spaces or a tab before the colon where added to make sure that the rule still finds these.


---

_Comment by @github-actions[bot] on 2024-04-17 13:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/generator.py#L758'>rasa/shared/core/generator.py:758:60:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/featurizers/test_lm_featurizer.py#L699'>tests/nlu/featurizers/test_lm_featurizer.py:699:41:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/0ec38aeffbf0b09901c875a3397b827445e7f17d/tests/tracking/test_client.py#L347'>tests/tracking/test_client.py:347:27:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/0ec38aeffbf0b09901c875a3397b827445e7f17d/tests/tracking/test_rest_tracking.py#L77'>tests/tracking/test_rest_tracking.py:77:27:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E203 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @MichaReiser on 2024-04-17 14:17_

---

_Label `rule` added by @MichaReiser on 2024-04-17 14:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:92 on 2024-04-17 14:19_

Can we add two more examples where the line end:

* in a comment
* in a line continuation



---

_Comment by @zanieb on 2024-04-17 14:59_

Are these ecosystem checks showing new false negatives?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:240 on 2024-04-17 15:03_

I think ruff's formatter doesn't always produce a whitespace. You can see it in the branch below, where it compares the whitespace before and after.  But i guess we can skip this here because we consider the newline as whitespace?

Just to confirm my understanding. This new code wouldn't warn about 

```python
Allow[
	5:
]
```

---

_@MichaReiser reviewed on 2024-04-17 15:04_

Thank you. I left an understand question and I think it might be good to add two more tests but this looks good to me.

---

_@Philipp-Thiel reviewed on 2024-04-17 15:25_

---

_Review comment by @Philipp-Thiel on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:92 on 2024-04-17 15:25_

Right, those would both be legal continuations. Didn't think about that, but will try to add those in the coming days

---

_@Philipp-Thiel reviewed on 2024-04-17 15:28_

---

_Review comment by @Philipp-Thiel on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:240 on 2024-04-17 15:28_

No this code wouldn't touch your example, because rule E203 only checks for extraneous whitespace. That's why I tried removing as little as possible, to have the lowest chance of introducing conflicts.

---

_Comment by @Philipp-Thiel on 2024-04-17 15:36_

> Are these ecosystem checks showing new false negatives?

I would say yes. They all have the same structure as the minimal reproducible example in the ticket.

---

_@Philipp-Thiel reviewed on 2024-04-21 16:23_

---

_Review comment by @Philipp-Thiel on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:92 on 2024-04-21 16:23_

I added the comment case, but can't figure out how to do the line continuation, without basically rewriting the rule since the \ doesn't show up in the token stream. I could try to adapt E502 for it, if you consider the case important.

On the bright side, both ruff format and black remove a \ in the example situation anyway so there would be no continuing conflict.

---

_@MichaReiser reviewed on 2024-04-22 08:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:92 on 2024-04-22 08:15_

You could try to use `SimpleTokenizer` when you found a potential violation to lex the text coming right after the token and see if the next token is a line continuation.

---

_Merged by @charliermarsh on 2024-06-08 23:29_

---

_Closed by @charliermarsh on 2024-06-08 23:29_

---

_Comment by @charliermarsh on 2024-06-08 23:29_

I think this looks like an improvement. The ecosystem checks are improvements, even if it's not a perfect match for the formatter (we don't check if values are complex).

---
