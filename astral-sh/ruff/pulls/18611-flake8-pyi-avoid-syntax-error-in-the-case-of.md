```yaml
number: 18611
title: "[`flake8-pyi`] Avoid syntax error in the case of starred and keyword arguments (`PYI059`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/fix-pyi059-syntax-error
created_at: 2025-06-10T14:30:18Z
updated_at: 2025-06-10T16:27:08Z
url: https://github.com/astral-sh/ruff/pull/18611
synced_at: 2026-01-12T15:56:22Z
```

# [`flake8-pyi`] Avoid syntax error in the case of starred and keyword arguments (`PYI059`)

---

_@ntBre_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/18602 by:
1. Avoiding a fix when `*args` are present
2. Inserting the `Generic` base class right before the first keyword argument, if one is present

In an intermediate commit, I also had special handling to avoid a fix in the `**kwargs` case, but this is treated (roughly) as a normal keyword, and I believe handling it properly falls out of the other keyword fix.

I also updated the `add_argument` utility function to insert new arguments right before the keyword argument list instead of at the very end of the argument list. This changed a couple of snapshots unrelated to `PYI059`, but there shouldn't be any functional changes to other rules because all other calls to `add_argument` were adding a keyword argument anyway.

## Test Plan

Existing PYI059 cases, plus new tests based on the issue


---

_Review requested from @AlexWaygood by @ntBre on 2025-06-10 14:30_

---

_Label `bug` added by @ntBre on 2025-06-10 14:30_

---

_Label `fixes` added by @ntBre on 2025-06-10 14:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:147 on 2025-06-10 14:41_

should we be adding this logic to `add_argument`, so that it's also fixed for other rules that use that utility? If we don't want to do that in this PR, is it worth opening a followup issue for that?

---

_Comment by @github-actions[bot] on 2025-06-10 14:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -26 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:22:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:22:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L37'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:37:23:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L37'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:37:23:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/core_api/base.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/base.py:52:16:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/core_api/base.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/base.py:52:16:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L37'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:37:18:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/8da0160cf9ecda249230c417d09ed248fd6db095/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L37'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:37:18:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/output_parsers/base.py#L31'>libs/core/langchain_core/output_parsers/base.py:31:26:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/output_parsers/base.py#L31'>libs/core/langchain_core/output_parsers/base.py:31:26:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/prompts/base.py#L45'>libs/core/langchain_core/prompts/base.py:45:25:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/prompts/base.py#L45'>libs/core/langchain_core/prompts/base.py:45:25:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/runnables/base.py#L111'>libs/core/langchain_core/runnables/base.py:111:15:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/runnables/base.py#L111'>libs/core/langchain_core/runnables/base.py:111:15:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/stores.py#L26'>libs/core/langchain_core/stores.py:26:16:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/stores.py#L26'>libs/core/langchain_core/stores.py:26:16:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/metadata.py#L440'>src/latch/types/metadata.py:440:25:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/metadata.py#L440'>src/latch/types/metadata.py:440:25:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/metadata.py#L489'>src/latch/types/metadata.py:489:24:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/metadata.py#L489'>src/latch/types/metadata.py:489:24:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/0db0486fe6a0eb6b4ebf1256fe0afe0b95e2dc36/stdlib/asyncio/queues.pyi#L28'>stdlib/asyncio/queues.pyi:28:12:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/python/typeshed/blob/0db0486fe6a0eb6b4ebf1256fe0afe0b95e2dc36/stdlib/asyncio/queues.pyi#L28'>stdlib/asyncio/queues.pyi:28:12:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/notes.py#L12'>zerver/lib/notes.py:12:16:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/notes.py#L12'>zerver/lib/notes.py:12:16:</a> PYI059 `Generic[]` should always be the last base class
- <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/queue.py#L35'>zerver/lib/queue.py:35:18:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/queue.py#L35'>zerver/lib/queue.py:35:18:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI059 | 26 | 0 | 0 | 0 | 26 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:149 on 2025-06-10 14:43_

Hmm... it's unrelated to your PR, but I'm not sure that this should be a safe fix :-(

Moving the position of `Generic[]` in the bases tuple could change the behaviour of the code by changing the class's MRO, and it's _possible_ that the code was actually working before the fix. (Having `Generic[]` as not-the-last base class can _sometimes_ work, it's just... never advisable.)

Even if we're okay with the fix changing the behaviour of the code, is there a possibility it could delete comments in some situations? We discussed this in the original PR so I think we have good test coverage for it, but at that point in time we didn't have as rigorous a policy for whether a fix should be deemed to be unsafe if it removed comments: https://github.com/astral-sh/ruff/pull/11233#discussion_r1589099207

---

_@AlexWaygood approved on 2025-06-10 14:44_

Nice!

---

_@ntBre reviewed on 2025-06-10 14:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:147 on 2025-06-10 14:49_

Yeah, I actually checked the other callers of `add_argument`, and they were all adding keyword arguments! So I think it makes sense to fix this or add a note to the docs at the very least.

I don't mind doing it here, but I guess it might churn some snapshots to move the existing calls to the front of the kwarg list.

---

_@ntBre reviewed on 2025-06-10 14:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:149 on 2025-06-10 14:56_

I was wondering about that too, thanks for bringing it up.

It definitely removes comments ([playground](https://play.ruff.rs/8b475e04-00a8-40c6-83b4-eb6d805b3f1e)) trailing the `Generic` argument.

I'd be fine marking this always unsafe and adding a `## Fix safety` section saying that it can change the MRO and also delete comments, if that sounds good to you.

This seems like a lot of changes for a rule about to be stabilized. How are you feeling about that? I'm definitely fine leaving this in preview for another cycle.

---

_@AlexWaygood reviewed on 2025-06-10 14:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:149 on 2025-06-10 14:59_

> I'd be fine marking this always unsafe and adding a `## Fix safety` section saying that it can change the MRO and also delete comments, if that sounds good to you.

That sounds good to me, yeah!

> This seems like a lot of changes for a rule about to be stabilized. How are you feeling about that? I'm definitely fine leaving this in preview for another cycle.

Agreed -- let's make the changes proposed in this PR and the docs updates proposed in https://github.com/astral-sh/ruff/pull/18601, but leave it in preview for now?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:147 on 2025-06-10 15:00_

I'm happy with doing it here or postponing to a followup -- either is fine by me!

---

_@AlexWaygood reviewed on 2025-06-10 15:00_

---

_@ntBre reviewed on 2025-06-10 16:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:147 on 2025-06-10 16:08_

I went ahead and did it here. It might have even shortened the net diff!

You're welcome to review again of course, but otherwise I'll just merge this when CI finishes. Thanks for the help!

---

_@AlexWaygood approved on 2025-06-10 16:17_

Excellent!

---

_Merged by @ntBre on 2025-06-10 16:27_

---

_Closed by @ntBre on 2025-06-10 16:27_

---

_Branch deleted on 2025-06-10 16:27_

---
