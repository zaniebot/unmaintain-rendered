```yaml
number: 16658
title: "[`ruff`] Stabilize `if-key-in-dict-del` (`RUF051`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/ruf051-0.10
created_at: 2025-03-12T03:13:42Z
updated_at: 2025-03-12T12:37:13Z
url: https://github.com/astral-sh/ruff/pull/16658
synced_at: 2026-01-12T15:55:55Z
```

# [`ruff`] Stabilize `if-key-in-dict-del` (`RUF051`)

---

_@ntBre_

Summary
--

Stabilizes RUF051. The tests and docs looked good.

Test Plan
--

1 closed documentation issue from 4 days after the rule was added and 1 typo fix from the same day it was added, but no other issues or PRs.



---

_Label `rule` added by @ntBre on 2025-03-12 03:13_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-12 03:13_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 03:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf051-0.10)

### Merging #16658 will **degrade performances by 10.81%**

<sub>Comparing <code>brent/ruf051-0.10</code> (19819c7) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf051-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 03:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+18 -0 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/docs/build_docs.py#L636'>docs/build_docs.py:636:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/docs/build_docs.py#L638'>docs/build_docs.py:638:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L666'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:666:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L680'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:680:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/23c3e7a02728ab325e8c3b23d665f574c452819b/task-sdk/src/airflow/sdk/definitions/baseoperator.py#L1268'>task-sdk/src/airflow/sdk/definitions/baseoperator.py:1268:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/a3f3a35c2000771af5f4db474125d771ea722383/superset/dashboards/schemas.py#L184'>superset/dashboards/schemas.py:184:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/c4729900cb7b59ce75bb577cbb73ad48c28c56c4/reflex/vars/base.py#L1773'>reflex/vars/base.py:1773:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0f5246400bf971983c49fc67adcfb693b8540f8d/corporate/tests/test_stripe.py#L663'>corporate/tests/test_stripe.py:663:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/0f5246400bf971983c49fc67adcfb693b8540f8d/zerver/lib/markdown/__init__.py#L2512'>zerver/lib/markdown/__init__.py:2512:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/0f5246400bf971983c49fc67adcfb693b8540f8d/zerver/lib/test_runner.py#L103'>zerver/lib/test_runner.py:103:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/0f5246400bf971983c49fc67adcfb693b8540f8d/zerver/tornado/event_queue.py#L551'>zerver/tornado/event_queue.py:551:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/0f5246400bf971983c49fc67adcfb693b8540f8d/zerver/tornado/handlers.py#L42'>zerver/tornado/handlers.py:42:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/modeling/core.py#L189'>astropy/modeling/core.py:189:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF051 | 18 | 18 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-12 03:31_

These all look like true positives.

---

_@MichaReiser approved on 2025-03-12 07:34_

---

_Merged by @ntBre on 2025-03-12 12:37_

---

_Closed by @ntBre on 2025-03-12 12:37_

---

_Branch deleted on 2025-03-12 12:37_

---
