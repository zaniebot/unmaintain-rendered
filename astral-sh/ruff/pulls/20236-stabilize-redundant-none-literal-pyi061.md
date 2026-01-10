```yaml
number: 20236
title: "Stabilize `redundant-none-literal` (`PYI061`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: brent/pyi061
created_at: 2025-09-04T14:30:45Z
updated_at: 2025-10-07T14:19:03Z
url: https://github.com/astral-sh/ruff/pull/20236
synced_at: 2026-01-10T17:34:34Z
```

# Stabilize `redundant-none-literal` (`PYI061`)

---

_Pull request opened by @ntBre on 2025-09-04 14:30_

Tests and docs look good


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 14:30_

---

_Label `rule` added by @ntBre on 2025-09-04 14:30_

---

_Comment by @github-actions[bot] on 2025-09-04 14:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+21 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3d5d8f37400d1d66675943292ce833e492351d94/airflow-core/src/airflow/serialization/serialized_objects.py#L3239'>airflow-core/src/airflow/serialization/serialized_objects.py:3239:36:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/bc91a4811c454511af50a2fbd95f48f024b5738a/libs/langchain/langchain/chat_models/base.py#L40'>libs/langchain/langchain/chat_models/base.py:40:34:</a> PYI061 [*] Use `None` rather than `Literal[None]`
+ <a href='https://github.com/langchain-ai/langchain/blob/bc91a4811c454511af50a2fbd95f48f024b5738a/libs/langchain/langchain/chat_models/base.py#L48'>libs/langchain/langchain/chat_models/base.py:48:20:</a> PYI061 [*] Use `None` rather than `Literal[None]`
+ <a href='https://github.com/langchain-ai/langchain/blob/bc91a4811c454511af50a2fbd95f48f024b5738a/libs/langchain/langchain/chat_models/base.py#L51'>libs/langchain/langchain/chat_models/base.py:51:34:</a> PYI061 [*] Use `None` rather than `Literal[None]`
+ <a href='https://github.com/langchain-ai/langchain/blob/bc91a4811c454511af50a2fbd95f48f024b5738a/libs/langchain_v1/langchain/chat_models/base.py#L36'>libs/langchain_v1/langchain/chat_models/base.py:36:34:</a> PYI061 [*] Use `None` rather than `Literal[None]`
+ <a href='https://github.com/langchain-ai/langchain/blob/bc91a4811c454511af50a2fbd95f48f024b5738a/libs/langchain_v1/langchain/chat_models/base.py#L44'>libs/langchain_v1/langchain/chat_models/base.py:44:20:</a> PYI061 [*] Use `None` rather than `Literal[None]`
+ <a href='https://github.com/langchain-ai/langchain/blob/bc91a4811c454511af50a2fbd95f48f024b5738a/libs/langchain_v1/langchain/chat_models/base.py#L47'>libs/langchain_v1/langchain/chat_models/base.py:47:34:</a> PYI061 [*] Use `None` rather than `Literal[None]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f1e4974fca118401d8fc72391ead6bb72c1ee395/src/latch/types/metadata.py#L496'>src/latch/types/metadata.py:496:45:</a> PYI061 [*] Use `Optional[Literal[...]]` rather than `Literal[None, ...]` 
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/3c14b71c8f965ff316b22dfb3d87839361f55297/pandas/core/groupby/groupby.py#L4324'>pandas/core/groupby/groupby.py:4324:39:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/pandas-dev/pandas/blob/3c14b71c8f965ff316b22dfb3d87839361f55297/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/pandas-dev/pandas/blob/3c14b71c8f965ff316b22dfb3d87839361f55297/pandas/io/html.py#L1045'>pandas/io/html.py:1045:28:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/pandas-dev/pandas/blob/3c14b71c8f965ff316b22dfb3d87839361f55297/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/billing_page.py#L329'>corporate/views/billing_page.py:329:24:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L155'>corporate/views/remote_billing_page.py:155:26:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L156'>corporate/views/remote_billing_page.py:156:42:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L157'>corporate/views/remote_billing_page.py:157:48:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L676'>corporate/views/remote_billing_page.py:676:26:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L677'>corporate/views/remote_billing_page.py:677:42:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L678'>corporate/views/remote_billing_page.py:678:48:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/corporate/views/remote_billing_page.py#L67'>corporate/views/remote_billing_page.py:67:5:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
+ <a href='https://github.com/zulip/zulip/blob/a47c8ce15f5657dcf690ad4087047205606e2d3d/zerver/lib/event_types.py#L906'>zerver/lib/event_types.py:906:62:</a> PYI061 [*] Use `None` rather than `Literal[None]`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI061 | 21 | 21 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-09-04 17:49_

I can kind of see why people would prefer `Literal[1, 2, 3, None]` over `Literal[1, 2, 3] | None` or `Optional[Literal[1, 2, 3]]`, but these all look like true positives to me. If you prefer the other style, I guess you just wouldn't activate the rule. `Literal[None]` -> `None` seems less contentious, but it does appear in the ecosystem check, mostly for `@overload`s.

---

_Marked ready for review by @ntBre on 2025-09-04 17:50_

---

_Review requested from @AlexWaygood by @ntBre on 2025-09-04 17:50_

---

_@AlexWaygood approved on 2025-09-05 13:55_

---

_Comment by @ntBre on 2025-09-05 14:05_

Just to record the decision here, we're aware of https://github.com/astral-sh/ruff/issues/20265, but I agree with Alex that it doesn't need to block stabilization.

---

_Merged by @ntBre on 2025-09-05 14:05_

---

_Closed by @ntBre on 2025-09-05 14:05_

---

_Branch deleted on 2025-09-05 14:05_

---

_Comment by @user27182 on 2025-10-06 23:13_

> I can kind of see why people would prefer `Literal[1, 2, 3, None]` over `Literal[1, 2, 3] | None` or `Optional[Literal[1, 2, 3]]`, but these all look like true positives to me.

This is more than a style preference, as it modifies the output of `typing.get_args`, see #20729. This auto-fix caused test failures in the `pyvista` repo when trying to update the `ruff` pre-commit hook:
https://github.com/pyvista/pyvista/pull/7992/commits/d908bd738be48e4e514a07d87237b038a4bdbc26


---

_Comment by @ntBre on 2025-10-07 14:19_

Ah that's good to know. Thank you for the report!

---
