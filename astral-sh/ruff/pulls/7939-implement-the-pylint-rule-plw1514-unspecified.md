```yaml
number: 7939
title: Implement the pylint rule PLW1514 (unspecified-encoding)
type: pull_request
state: merged
author: shager
labels:
  - rule
assignees: []
merged: true
base: main
head: add-PLW1514
created_at: 2023-10-13T10:50:12Z
updated_at: 2023-10-17T08:25:22Z
url: https://github.com/astral-sh/ruff/pull/7939
synced_at: 2026-01-12T02:32:41Z
```

# Implement the pylint rule PLW1514 (unspecified-encoding)

---

_Pull request opened by @shager on 2023-10-13 10:50_

## Summary

Implemented the pylint rule W1514 ( unspecified-encoding).
See also: https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/unspecified-encoding.html


## Test Plan

Tested it with the submitted test case.
Additionally, we tested the new ruff rule (PLW1514) on our proprietary Python code base.


---

_Comment by @github-actions[bot] on 2023-10-13 12:36_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+60, -0, 0 error(s))

<details><summary>rotki (+60, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/package.py#L477'>package.py:477:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/package.py#L484'>package.py:484:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/package.py#L512'>package.py:512:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/packaging/docker/entrypoint.py#L60'>packaging/docker/entrypoint.py:60:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/airdrops.py#L301'>rotkehlchen/chain/ethereum/airdrops.py:301:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/airdrops.py#L309'>rotkehlchen/chain/ethereum/airdrops.py:309:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/airdrops.py#L331'>rotkehlchen/chain/ethereum/airdrops.py:331:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/airdrops.py#L334'>rotkehlchen/chain/ethereum/airdrops.py:334:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/chain/ethereum/modules/dxdaomesa/decoder.py#L45'>rotkehlchen/chain/ethereum/modules/dxdaomesa/decoder.py:45:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/db/dbhandler.py#L276'>rotkehlchen/db/dbhandler.py:276:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/externalapis/cryptocompare.py#L830'>rotkehlchen/externalapis/cryptocompare.py:830:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/externalapis/cryptocompare.py#L848'>rotkehlchen/externalapis/cryptocompare.py:848:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/assets_management.py#L36'>rotkehlchen/globaldb/assets_management.py:36:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v2_v3.py#L546'>rotkehlchen/globaldb/upgrades/v2_v3.py:546:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v3_v4.py#L137'>rotkehlchen/globaldb/upgrades/v3_v4.py:137:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v3_v4.py#L153'>rotkehlchen/globaldb/upgrades/v3_v4.py:153:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v3_v4.py#L155'>rotkehlchen/globaldb/upgrades/v3_v4.py:155:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v3_v4.py#L307'>rotkehlchen/globaldb/upgrades/v3_v4.py:307:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v3_v4.py#L320'>rotkehlchen/globaldb/upgrades/v3_v4.py:320:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/globaldb/upgrades/v4_v5.py#L40'>rotkehlchen/globaldb/upgrades/v4_v5.py:40:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_evm_transactions.py#L111'>rotkehlchen/tests/api/test_evm_transactions.py:111:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_history.py#L408'>rotkehlchen/tests/api/test_history.py:408:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L205'>rotkehlchen/tests/api/test_snapshots.py:205:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L212'>rotkehlchen/tests/api/test_snapshots.py:212:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L221'>rotkehlchen/tests/api/test_snapshots.py:221:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L228'>rotkehlchen/tests/api/test_snapshots.py:228:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L237'>rotkehlchen/tests/api/test_snapshots.py:237:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L244'>rotkehlchen/tests/api/test_snapshots.py:244:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L253'>rotkehlchen/tests/api/test_snapshots.py:253:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L261'>rotkehlchen/tests/api/test_snapshots.py:261:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L271'>rotkehlchen/tests/api/test_snapshots.py:271:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L278'>rotkehlchen/tests/api/test_snapshots.py:278:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L328'>rotkehlchen/tests/api/test_snapshots.py:328:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L353'>rotkehlchen/tests/api/test_snapshots.py:353:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L377'>rotkehlchen/tests/api/test_snapshots.py:377:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L396'>rotkehlchen/tests/api/test_snapshots.py:396:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L572'>rotkehlchen/tests/api/test_snapshots.py:572:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/api/test_snapshots.py#L572'>rotkehlchen/tests/api/test_snapshots.py:572:79:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/conftest.py#L141'>rotkehlchen/tests/conftest.py:141:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/exchanges/test_kraken.py#L207'>rotkehlchen/tests/exchanges/test_kraken.py:207:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/external_apis/test_coingecko.py#L93'>rotkehlchen/tests/external_apis/test_coingecko.py:93:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/external_apis/test_coingecko.py#L96'>rotkehlchen/tests/external_apis/test_coingecko.py:96:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L356'>rotkehlchen/tests/unit/globaldb/test_upgrades.py:356:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/unit/globaldb/test_upgrades.py#L51'>rotkehlchen/tests/unit/globaldb/test_upgrades.py:51:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/unit/test_cost_basis.py#L968'>rotkehlchen/tests/unit/test_cost_basis.py:968:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/unit/test_eth2.py#L159'>rotkehlchen/tests/unit/test_eth2.py:159:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/accounting.py#L270'>rotkehlchen/tests/utils/accounting.py:270:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/accounting.py#L294'>rotkehlchen/tests/utils/accounting.py:294:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/history.py#L1204'>rotkehlchen/tests/utils/history.py:1204:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/kraken.py#L499'>rotkehlchen/tests/utils/kraken.py:499:14:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/mock.py#L245'>rotkehlchen/tests/utils/mock.py:245:22:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/mock.py#L282'>rotkehlchen/tests/utils/mock.py:282:18:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/tests/utils/mock.py#L294'>rotkehlchen/tests/utils/mock.py:294:22:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/rotkehlchen/utils/snapshots.py#L120'>rotkehlchen/utils/snapshots.py:120:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/tools/profiling/graph.py#L297'>tools/profiling/graph.py:297:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/tools/profiling/graph.py#L306'>tools/profiling/graph.py:306:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/tools/scripts/generate_changelog.py#L24'>tools/scripts/generate_changelog.py:24:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/tools/scripts/generate_minimized_db_schema.py#L61'>tools/scripts/generate_minimized_db_schema.py:61:6:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/tools/scripts/pylint_useless_suppression.py#L60'>tools/scripts/pylint_useless_suppression.py:60:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/rotki/rotki/blob/2168547cb2009af0fa118ef4bd9a5c3a7663303c/tools/scripts/pylint_useless_suppression.py#L72'>tools/scripts/pylint_useless_suppression.py:72:10:</a> PLW1514 `open` in text mode without explicit `encoding` argument
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLW1514 | 60 | 60 | 0 |



---

_Label `rule` added by @charliermarsh on 2023-10-13 19:36_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-10-17 02:09_

---

_@charliermarsh reviewed on 2023-10-17 03:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:135 on 2023-10-17 03:19_

This implementation overall looks really nice, but where this list come from? E.g., it looks like Pylint flags some of the `pathlib` methods -- should we flag those here too?

---

_@charliermarsh reviewed on 2023-10-17 03:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:135 on 2023-10-17 03:24_

Ahh, I guess we can't include the `pathlib` cases since those are instance methods.

---

_@charliermarsh approved on 2023-10-17 03:40_

---

_Merged by @charliermarsh on 2023-10-17 03:49_

---

_Closed by @charliermarsh on 2023-10-17 03:49_

---

_@shager reviewed on 2023-10-17 08:25_

---

_Review comment by @shager on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:135 on 2023-10-17 08:25_

Exactly, we tried `pathlib` as well but it didn't work.
We also tried to detect something like this:
```python
foo = open
foo("test.txt")
```
But we didn't manage to get this one right, so we omitted it. =(



---
