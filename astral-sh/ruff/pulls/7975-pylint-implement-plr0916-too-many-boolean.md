```yaml
number: 7975
title: "[pylint] Implement PLR0916 (`too-many-boolean-expressions`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
assignees: []
merged: true
base: main
head: add-PLR0916
created_at: 2023-10-16T06:31:03Z
updated_at: 2023-10-17T04:50:36Z
url: https://github.com/astral-sh/ruff/pull/7975
synced_at: 2026-01-12T15:55:25Z
```

# [pylint] Implement PLR0916 (`too-many-boolean-expressions`)

---

_@diceroll123_

## Summary

Add [R0916](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/too-many-boolean-expressions.html), no autofix available.

See: #970 

## Test Plan

`cargo test` and manually.

---

_Comment by @github-actions[bot] on 2023-10-16 07:05_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+10, -0, 0 error(s))

<details><summary>rotki (+10, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/assets/utils.py#L137'>rotkehlchen/assets/utils.py:137:9:</a> PLR0916 Too many Boolean expressions (7 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/base_bridge/decoder.py#L73'>rotkehlchen/chain/ethereum/modules/base_bridge/decoder.py:73:17:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L194'>rotkehlchen/chain/ethereum/modules/compound/decoder.py:194:17:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L268'>rotkehlchen/chain/ethereum/modules/compound/decoder.py:268:17:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L279'>rotkehlchen/chain/ethereum/modules/compound/decoder.py:279:17:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L202'>rotkehlchen/chain/ethereum/modules/curve/decoder.py:202:17:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L635'>rotkehlchen/chain/ethereum/modules/curve/decoder.py:635:17:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/liquity/decoder.py#L139'>rotkehlchen/chain/ethereum/modules/liquity/decoder.py:139:21:</a> PLR0916 Too many Boolean expressions (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/data_import/importers/binance.py#L293'>rotkehlchen/data_import/importers/binance.py:293:17:</a> PLR0916 Too many Boolean expressions (7 > 5)
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/exchanges/kraken.py#L1160'>rotkehlchen/exchanges/kraken.py:1160:25:</a> PLR0916 Too many Boolean expressions (7 > 5)
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR0916 | 10 | 10 | 0 |



---

_Label `rule` added by @charliermarsh on 2023-10-16 13:37_

---

_Renamed from "add PLR0916 rule" to "[pylint] Implement PLR0916 (`too-many-boolean-expressions`)" by @charliermarsh on 2023-10-17 04:31_

---

_Merged by @charliermarsh on 2023-10-17 04:44_

---

_Closed by @charliermarsh on 2023-10-17 04:44_

---
