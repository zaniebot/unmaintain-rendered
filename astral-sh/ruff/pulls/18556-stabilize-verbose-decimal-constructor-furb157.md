```yaml
number: 18556
title: "Stabilize `verbose-decimal-constructor` (`FURB157`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-furb157
created_at: 2025-06-08T19:02:57Z
updated_at: 2025-06-09T17:44:16Z
url: https://github.com/astral-sh/ruff/pull/18556
synced_at: 2026-01-12T15:56:21Z
```

# Stabilize `verbose-decimal-constructor` (`FURB157`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:02_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:02_

---

_Comment by @github-actions[bot] on 2025-06-08 19:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/teradata/tests/unit/teradata/transfers/test_teradata_to_teradata.py#L86'>providers/teradata/tests/unit/teradata/transfers/test_teradata_to_teradata.py:86:33:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/teradata/tests/unit/teradata/transfers/test_teradata_to_teradata.py#L86'>providers/teradata/tests/unit/teradata/transfers/test_teradata_to_teradata.py:86:58:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/teradata/tests/unit/teradata/transfers/test_teradata_to_teradata.py#L86'>providers/teradata/tests/unit/teradata/transfers/test_teradata_to_teradata.py:86:83:</a> FURB157 [*] Verbose expression in `Decimal` constructor
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/c23ffce05a1b59281825af9e1d29ea78fd17c38d/tests/units/utils/test_serializers.py#L194'>tests/units/utils/test_serializers.py:194:26:</a> FURB157 [*] Verbose expression in `Decimal` constructor
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB157 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:30_

---

_@ntBre approved on 2025-06-09 16:54_

---

_Merged by @dylwil3 on 2025-06-09 17:44_

---

_Closed by @dylwil3 on 2025-06-09 17:44_

---

_Branch deleted on 2025-06-09 17:44_

---
