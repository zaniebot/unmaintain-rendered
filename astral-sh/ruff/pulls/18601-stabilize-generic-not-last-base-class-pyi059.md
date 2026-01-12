```yaml
number: 18601
title: "Stabilize `generic-not-last-base-class` (`PYI059`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
base: brent/release-0.12.0
head: brent/stabilize-pyi059
created_at: 2025-06-09T22:41:50Z
updated_at: 2025-06-10T15:55:34Z
url: https://github.com/astral-sh/ruff/pull/18601
synced_at: 2026-01-12T15:56:22Z
```

# Stabilize `generic-not-last-base-class` (`PYI059`)

---

_@ntBre_

Summary
--

The tests were already in the right place, I just updated the documentation slightly to reflect the discussion [here](https://github.com/astral-sh/ruff/pull/18519#issuecomment-2956344254) and on Discord.

Documentation: https://docs.astral.sh/ruff/rules/generic-not-last-base-class

Tests: https://github.com/astral-sh/ruff/blob/brent/release-0.12.0/crates/ruff_linter/src/rules/flake8_pyi/mod.rs#L51-L52

I considered mentioning the MRO explicitly and linking to [this](https://docs.python.org/3/howto/mro.html) but decided that might be a bit too technical for the rule docs.

Now I see that ty links to the [glossary](https://docs.python.org/3/glossary.html#term-method-resolution-order), which would be another option.

Test Plan
--

Existing tests


---

_Review requested from @AlexWaygood by @ntBre on 2025-06-09 22:41_

---

_Label `rule` added by @ntBre on 2025-06-09 22:41_

---

_Comment by @github-actions[bot] on 2025-06-09 22:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+13 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2862555044052dc5a10e175f8b96acc1f5ccf057/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:22:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/2862555044052dc5a10e175f8b96acc1f5ccf057/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L37'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:37:23:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/2862555044052dc5a10e175f8b96acc1f5ccf057/airflow-core/src/airflow/api_fastapi/core_api/base.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/base.py:52:16:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/2862555044052dc5a10e175f8b96acc1f5ccf057/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L37'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:37:18:</a> PYI059 [*] `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/output_parsers/base.py#L31'>libs/core/langchain_core/output_parsers/base.py:31:26:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/prompts/base.py#L45'>libs/core/langchain_core/prompts/base.py:45:25:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/runnables/base.py#L111'>libs/core/langchain_core/runnables/base.py:111:15:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/stores.py#L26'>libs/core/langchain_core/stores.py:26:16:</a> PYI059 [*] `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/metadata.py#L440'>src/latch/types/metadata.py:440:25:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/metadata.py#L489'>src/latch/types/metadata.py:489:24:</a> PYI059 [*] `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/0db0486fe6a0eb6b4ebf1256fe0afe0b95e2dc36/stdlib/asyncio/queues.pyi#L28'>stdlib/asyncio/queues.pyi:28:12:</a> PYI059 [*] `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/notes.py#L12'>zerver/lib/notes.py:12:16:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/lib/queue.py#L35'>zerver/lib/queue.py:35:18:</a> PYI059 [*] `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI059 | 13 | 13 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-06-10 02:23_

related: https://github.com/astral-sh/ruff/issues/18602

---

_Comment by @AlexWaygood on 2025-06-10 12:21_

> related: #18602

IMO I think that probably _does_ block stabilisation, but I agree with you that for now we could just not offer a fix if there are any keyword or variadic arguments, stabilise it after making that change, and open a followup issue to add a fix for classes that have keyword arguments.

Variadic arguments (things like `class Foo(*args): ...` and `class Bar(**args): ...`) are rare enough in class definitions that I think it's fine for us never to offer a fix for those. But it _would_ be nice to offer a fix for `class Foo(Generic[T], Bar, metaclass=ABCMeta): ...` at some point.

---

_@AlexWaygood reviewed on 2025-06-10 12:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:17 on 2025-06-10 12:24_

I think I'd probably say something like "type checkers may not be able to infer an accurate MRO for for the class, which could lead to unexpected or inaccurate results when they analyze your code". Linking to the glossary sounds like a great idea!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:20 on 2025-06-10 14:45_

```suggestion
///
/// The rule is also applied to stub files, where it won't cause issues at
/// runtime. This is because type checkers may not be able to infer an
/// accurate [MRO] for the class, which could lead to unexpected or
/// inaccurate results when they analyze your code.
```

---

_@AlexWaygood approved on 2025-06-10 14:46_

---

_Comment by @ntBre on 2025-06-10 15:47_

Closing this in light of the discussion on https://github.com/astral-sh/ruff/pull/18611 (need to move the fix to unsafe, updating the kwarg handling), but I'll copy the docs improvements over there.

---

_Closed by @ntBre on 2025-06-10 15:47_

---
