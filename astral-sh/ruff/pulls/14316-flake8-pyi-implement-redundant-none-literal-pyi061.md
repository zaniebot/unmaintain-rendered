```yaml
number: 14316
title: "[`flake8-pyi`] Implement `redundant-none-literal` (`PYI061`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: redundant-none-literal
created_at: 2024-11-13T12:28:03Z
updated_at: 2024-11-18T08:47:55Z
url: https://github.com/astral-sh/ruff/pull/14316
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-pyi`] Implement `redundant-none-literal` (`PYI061`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

`Literal[None]` can be simplified into `None` in type annotations.

Surprising to see that this is not that rare:
- https://github.com/langchain-ai/langchain/blob/master/libs/langchain/langchain/chat_models/base.py#L54
- https://github.com/sqlalchemy/sqlalchemy/blob/main/lib/sqlalchemy/sql/annotation.py#L69
- https://github.com/jax-ml/jax/blob/main/jax/numpy/__init__.pyi#L961
- https://github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/inference/_common.py#L179

## Test Plan

`cargo test`

Reviewed all ecosystem results, and they are true positives.

---

_Comment by @github-actions[bot] on 2024-11-13 12:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+14 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/models/dag.py#L1038'>airflow/models/dag.py:1038:36:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/fae3e8034faf66eb8ef00bcbed73d48e4ef791d3/pandas/core/groupby/groupby.py#L4069'>pandas/core/groupby/groupby.py:4069:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/fae3e8034faf66eb8ef00bcbed73d48e4ef791d3/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/fae3e8034faf66eb8ef00bcbed73d48e4ef791d3/pandas/io/html.py#L1027'>pandas/io/html.py:1027:28:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/fae3e8034faf66eb8ef00bcbed73d48e4ef791d3/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/billing_page.py#L326'>corporate/views/billing_page.py:326:24:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L678'>corporate/views/remote_billing_page.py:678:26:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:42:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:48:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI061 | 14 | 14 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @sbrugman on 2024-11-13 13:51_

---

_Marked ready for review by @sbrugman on 2024-11-13 14:25_

---

_Comment by @MichaReiser on 2024-11-13 16:32_

Hi @sbrugman Thanks for another PR. It would help me review your PRs and avoid unnecessary work if you could fill out the Test plan. E.g. did you review the ecosystem changes already? If so, then I don't have to do so :)

---

_Comment by @MichaReiser on 2024-11-13 16:34_

Is there some previous issue where we discussed this rule? I'm asking because the rule itself would be a better fit for `flake8-annotations`... except that it isn't an upstream rule

---

_Comment by @sbrugman on 2024-11-13 16:44_

@MichaReiser I've updated the test plan above, will do so for the other issues as well. 

You're right that these rules fit better in another category, but since there is no upstream rule I've placed them here. 
I don't think we should hold back on contributing new rules, especially with the new categorisation upcoming.

I'm ok moving the rules if preferred. 

---

_Label `rule` added by @MichaReiser on 2024-11-13 16:56_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-13 16:56_

---

_Label `preview` added by @MichaReiser on 2024-11-13 16:56_

---

_Comment by @sbrugman on 2024-11-13 21:49_

It looks like this could actually be in flake8-pyi:
https://github.com/PyCQA/flake8-pyi/blob/main/ERRORCODES.md#Y061

---

_Converted to draft by @sbrugman on 2024-11-14 07:32_

---

_Renamed from "[`ruff`] Implement `redundant-none-literal` (`RUF037`)" to "[`flake8-pyi`] Implement `redundant-none-literal` (`PYI061`)" by @sbrugman on 2024-11-14 07:32_

---

_Marked ready for review by @sbrugman on 2024-11-14 07:45_

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-14 07:45_

---

_Label `needs-decision` removed by @MichaReiser on 2024-11-14 07:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:109 on 2024-11-14 07:58_

 I'm surprised that setting the same fix works :laughing: 

---

_@MichaReiser approved on 2024-11-14 07:59_

LGTM, @AlexWaygood could you sign off the rule :)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:14 on 2024-11-14 13:07_

The motivation of this rule is purely for stylistic consistency: it doesn't detect a bug or anything that's incorrect, it just makes your annotations more concise and enforces a consistent style. I'd prefer to say that more explicitly here, like we do in [the flake8-pyi docs](https://github.com/PyCQA/flake8-pyi/blob/main/ERRORCODES.md#Y061):

```suggestion
/// `Literal[None]` is legal as a type annotation, but means the same thing as `None`.
/// For stylistic consistency, prefer using `None`, which is more concise. 
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:22 on 2024-11-14 13:14_

```suggestion
/// from typing import Literal
///
/// Literal[None]
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:30 on 2024-11-14 13:14_

```suggestion
/// from typing import Literal
///
/// None
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:32 on 2024-11-14 13:15_

nit: the name of this field feels a little opaque to me. Maybe something like `other_literal_elements_seen`? 

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI061.py`:1 on 2024-11-14 13:18_

Could you add a test that does have a comment in the middle of a `Literal` slice, to show that this produces a fix that's marked as unsafe?

---

_@AlexWaygood reviewed on 2024-11-14 13:18_

Thanks! A couple of nitpicks, but looks great overall

---

_@sbrugman reviewed on 2024-11-14 15:06_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI061.py`:1 on 2024-11-14 15:06_

Good call. I realised that now it's mapped to an existing `flake8-pyi` rule, I could also look at those test cases.

It turned out that `comment_ranges().has_comments()` also marks a fix as unsafe on a trailing comment, which is safe.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI061.pyi`:37 on 2024-11-14 15:09_

```suggestion
Literal[None]  # PYI061 None inside "Literal[]" expression. Replace with "None"
Literal[True, None]  # PYI061 None inside "Literal[]" expression. Replace with "Literal[True] | None"
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI061.pyi`:52 on 2024-11-14 15:10_

These test cases made sense in the context of flake8-pyi's test suite, but I wonder if they make sense here? flake8-pyi has a pretty different code structure to us; in Ruff, we tend to consider each rule in isolation, rather than changing whether one diagnostic will be emitted depending on whether another diagnostic for a different rule is going to be emitted on the same expression

---

_@AlexWaygood approved on 2024-11-14 15:11_

Thank you! Looks great, just a couple of nits about the test remaining

---

_Review requested from @carljm by @charliermarsh on 2024-11-16 18:17_

---

_Review requested from @sharkdp by @charliermarsh on 2024-11-16 18:17_

---

_Merged by @charliermarsh on 2024-11-16 18:22_

---

_Closed by @charliermarsh on 2024-11-16 18:22_

---

_Branch deleted on 2024-11-18 08:47_

---
