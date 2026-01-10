```yaml
number: 18571
title: "Stabilize `unnecessary-dict-index-lookup` (`PLR1733`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-plr1733
created_at: 2025-06-08T19:25:55Z
updated_at: 2025-06-12T13:18:43Z
url: https://github.com/astral-sh/ruff/pull/18571
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `unnecessary-dict-index-lookup` (`PLR1733`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:25_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:25_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:25_

---

_Comment by @github-actions[bot] on 2025-06-08 19:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+19 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bc1d51e5388eb9fa7b28c1b8a70c5b8311bca1c1/providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/utils.py#L271'>providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/utils.py:271:31:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/apache/airflow/blob/bc1d51e5388eb9fa7b28c1b8a70c5b8311bca1c1/providers/amazon/src/airflow/providers/amazon/aws/hooks/glue.py#L407'>providers/amazon/src/airflow/providers/amazon/aws/hooks/glue.py:407:88:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/analytics/views/stats.py#L622'>analytics/views/stats.py:622:51:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/analytics/views/stats.py#L624'>analytics/views/stats.py:624:44:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/lib/subscription_info.py#L838'>zerver/lib/subscription_info.py:838:52:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L269'>astropy/utils/metadata/merge.py:269:40:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L271'>astropy/utils/metadata/merge.py:271:28:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L281'>astropy/utils/metadata/merge.py:281:29:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L283'>astropy/utils/metadata/merge.py:283:67:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L288'>astropy/utils/metadata/merge.py:288:54:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L299'>astropy/utils/metadata/merge.py:299:32:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L300'>astropy/utils/metadata/merge.py:300:22:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L300'>astropy/utils/metadata/merge.py:300:22:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L302'>astropy/utils/metadata/merge.py:302:44:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L302'>astropy/utils/metadata/merge.py:302:44:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L305'>astropy/utils/metadata/merge.py:305:59:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L310'>astropy/utils/metadata/merge.py:310:60:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L317'>astropy/utils/metadata/merge.py:317:32:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/astropy/astropy/blob/bc54dd4ea05f110b795e3774373530f1b1fab267/astropy/utils/metadata/merge.py#L319'>astropy/utils/metadata/merge.py:319:32:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1733 | 19 | 19 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:26_

---

_@ntBre approved on 2025-06-10 16:51_

Docs, tests, and ecosystem LGTM!

---

_Merged by @dylwil3 on 2025-06-12 13:18_

---

_Closed by @dylwil3 on 2025-06-12 13:18_

---

_Branch deleted on 2025-06-12 13:18_

---
