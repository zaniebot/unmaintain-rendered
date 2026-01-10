```yaml
number: 20246
title: "Stabilize `generic-not-last-base-class` (`PYI059`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: brent/pyi059
created_at: 2025-09-04T18:11:09Z
updated_at: 2025-09-04T23:06:05Z
url: https://github.com/astral-sh/ruff/pull/20246
synced_at: 2026-01-10T17:46:21Z
```

# Stabilize `generic-not-last-base-class` (`PYI059`)

---

_Pull request opened by @ntBre on 2025-09-04 18:11_

Tests and docs look good

We nearly stabilized this last time (https://github.com/astral-sh/ruff/pull/18601), but it needed one more bug fix and a documentation improvement (https://github.com/astral-sh/ruff/pull/18611)


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 18:11_

---

_Label `rule` added by @ntBre on 2025-09-04 18:11_

---

_Comment by @github-actions[bot] on 2025-09-04 18:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+10 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L77'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:77:22:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L38'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:38:23:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/src/airflow/api_fastapi/core_api/base.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/base.py:52:16:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L37'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:37:18:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/devel-common/src/tests_common/pytest_plugin.py#L780'>devel-common/src/tests_common/pytest_plugin.py:780:15:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f1e4974fca118401d8fc72391ead6bb72c1ee395/src/latch/types/metadata.py#L437'>src/latch/types/metadata.py:437:25:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/latchbio/latch/blob/f1e4974fca118401d8fc72391ead6bb72c1ee395/src/latch/types/metadata.py#L486'>src/latch/types/metadata.py:486:24:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/07557a4316d246b4315f600fd4c9734297d6bc92/stdlib/asyncio/queues.pyi#L29'>stdlib/asyncio/queues.pyi:29:12:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1c085b1d77d03545190dcb6a9dd0f94d23a61a68/zerver/lib/notes.py#L12'>zerver/lib/notes.py:12:16:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/zulip/zulip/blob/1c085b1d77d03545190dcb6a9dd0f94d23a61a68/zerver/lib/queue.py#L35'>zerver/lib/queue.py:35:18:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI059 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-09-04 19:58_

This ecosystem hit from typeshed seems interesting, but maybe okay? Everything else looks as expected to me.

https://github.com/python/typeshed/blob/07557a4316d246b4315f600fd4c9734297d6bc92/stdlib/asyncio/queues.pyi#L27-L29

---

_Marked ready for review by @ntBre on 2025-09-04 19:58_

---

_Review requested from @AlexWaygood by @ntBre on 2025-09-04 19:58_

---

_@AlexWaygood approved on 2025-09-04 21:53_

---

_Merged by @ntBre on 2025-09-04 23:06_

---

_Closed by @ntBre on 2025-09-04 23:06_

---

_Branch deleted on 2025-09-04 23:06_

---
