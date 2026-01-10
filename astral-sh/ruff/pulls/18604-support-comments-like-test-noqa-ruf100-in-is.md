```yaml
number: 18604
title: "support comments like `# test # noqa: RUF100` in `is_pragma_comment` "
type: pull_request
state: closed
author: CodeMan62
labels:
  - rule
assignees: []
base: main
head: codeman/18470
created_at: 2025-06-10T05:12:31Z
updated_at: 2025-06-23T07:25:24Z
url: https://github.com/astral-sh/ruff/pull/18604
synced_at: 2026-01-10T18:39:08Z
```

# support comments like `# test # noqa: RUF100` in `is_pragma_comment` 

---

_Pull request opened by @CodeMan62 on 2025-06-10 05:12_

## Summary
the [`is_pragma_comment`](https://github.com/astral-sh/ruff/blob/ff6f0b6ab8fef4df5a14651cf9ef86f016ec0367/crates/ruff_python_trivia/src/pragmas.rs#L13-L30)

## Test Plan
manually tested with `cargo test`
closes #18470 

---

_Review requested from @MichaReiser by @CodeMan62 on 2025-06-10 05:39_

---

_Comment by @github-actions[bot] on 2025-06-10 05:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+66 -1 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/brokers/kafka.py#L95'>rasa/core/brokers/kafka.py:95:111:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L219'>rasa/core/channels/hangouts.py:219:113:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L274'>rasa/core/channels/hangouts.py:274:98:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L275'>rasa/core/channels/hangouts.py:275:92:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/mattermost.py#L94'>rasa/core/channels/mattermost.py:94:95:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/formats/luis.py#L39'>rasa/shared/nlu/training_data/formats/luis.py:39:92:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L37'>tests/core/channels/test_hangouts.py:37:104:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L54'>tests/core/channels/test_hangouts.py:54:108:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L67'>tests/core/channels/test_hangouts.py:67:97:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/93583220a7aa4debeaf3fd4a6cade9f771ee8c2f/superset/config.py#L1302'>superset/config.py:1302:115:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/pandas/conftest.py#L1985'>pandas/conftest.py:1985:136:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+55 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/accounting/cost_basis/base.py#L687'>rotkehlchen/accounting/cost_basis/base.py:687:104:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L3616'>rotkehlchen/api/rest.py:3616:40:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L1786'>rotkehlchen/api/v1/schemas.py:1786:107:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/hop/constants.py#L40'>rotkehlchen/chain/arbitrum_one/modules/hop/constants.py:40:143:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L36'>rotkehlchen/chain/ethereum/modules/convex/constants.py:36:125:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L37'>rotkehlchen/chain/ethereum/modules/convex/constants.py:37:146:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L46'>rotkehlchen/chain/ethereum/modules/convex/constants.py:46:116:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L47'>rotkehlchen/chain/ethereum/modules/convex/constants.py:47:110:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/curve/constants.py#L38'>rotkehlchen/chain/ethereum/modules/curve/constants.py:38:102:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/l2/loopring.py#L170'>rotkehlchen/chain/ethereum/modules/l2/loopring.py:170:121:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/sushiswap/decoder.py#L26'>rotkehlchen/chain/ethereum/modules/sushiswap/decoder.py:26:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/uniswap/v2/common.py#L343'>rotkehlchen/chain/ethereum/modules/uniswap/v2/common.py:343:91:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/uniswap/v2/decoder.py#L29'>rotkehlchen/chain/ethereum/modules/uniswap/v2/decoder.py:29:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/aave/common.py#L258'>rotkehlchen/chain/evm/decoding/aave/common.py:258:123:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/aave/v3/decoder.py#L70'>rotkehlchen/chain/evm/decoding/aave/v3/decoder.py:70:97:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L123'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py:123:99:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L190'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py:190:98:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L249'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py:249:99:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/decoder.py#L351'>rotkehlchen/chain/evm/decoding/curve/decoder.py:351:125:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/decoder.py#L503'>rotkehlchen/chain/evm/decoding/curve/decoder.py:503:121:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/morpho/decoder.py#L197'>rotkehlchen/chain/evm/decoding/morpho/decoder.py:197:96:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/odos/common.py#L138'>rotkehlchen/chain/evm/decoding/odos/common.py:138:96:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/socket_bridge/decoder.py#L87'>rotkehlchen/chain/evm/decoding/socket_bridge/decoder.py:87:102:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/uniswap/v3/constants.py#L9'>rotkehlchen/chain/evm/decoding/uniswap/v3/constants.py:9:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/node_inquirer.py#L151'>rotkehlchen/chain/evm/node_inquirer.py:151:109:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/node_inquirer.py#L415'>rotkehlchen/chain/evm/node_inquirer.py:415:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/transactions.py#L812'>rotkehlchen/chain/evm/transactions.py:812:101:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/data_migrations/migrations/migrations_18.py#L174'>rotkehlchen/data_migrations/migrations/migrations_18.py:174:100:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/db/dbhandler.py#L551'>rotkehlchen/db/dbhandler.py:551:143:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/db/eth2.py#L684'>rotkehlchen/db/eth2.py:684:96:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/db/evmtx.py#L71'>rotkehlchen/db/evmtx.py:71:98:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/exchanges/coinbase.py#L80'>rotkehlchen/exchanges/coinbase.py:80:154:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/externalapis/beaconchain/service.py#L419'>rotkehlchen/externalapis/beaconchain/service.py:419:102:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/externalapis/etherscan.py#L820'>rotkehlchen/externalapis/etherscan.py:820:134:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/globaldb/upgrades/v3_v4.py#L168'>rotkehlchen/globaldb/upgrades/v3_v4.py:168:84:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/globaldb/upgrades/v3_v4.py#L170'>rotkehlchen/globaldb/upgrades/v3_v4.py:170:82:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/server.py#L62'>rotkehlchen/server.py:62:199:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tasks/calendar.py#L434'>rotkehlchen/tasks/calendar.py:434:143:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tests/api/test_accounting_rules.py#L41'>rotkehlchen/tests/api/test_accounting_rules.py:41:154:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tests/db/test_db.py#L942'>rotkehlchen/tests/db/test_db.py:942:99:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tests/fixtures/accounting.py#L37'>rotkehlchen/tests/fixtures/accounting.py:37:93:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/actions/message_summary.py#L172'>zerver/actions/message_summary.py:172:101:</a> E501 Line too long (132 > 100)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 66 | 66 | 0 | 0 | 0 |
| E501 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+66 -1 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/brokers/kafka.py#L95'>rasa/core/brokers/kafka.py:95:111:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L219'>rasa/core/channels/hangouts.py:219:113:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L274'>rasa/core/channels/hangouts.py:274:98:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L275'>rasa/core/channels/hangouts.py:275:92:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/mattermost.py#L94'>rasa/core/channels/mattermost.py:94:95:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/formats/luis.py#L39'>rasa/shared/nlu/training_data/formats/luis.py:39:92:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L37'>tests/core/channels/test_hangouts.py:37:104:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L54'>tests/core/channels/test_hangouts.py:54:108:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L67'>tests/core/channels/test_hangouts.py:67:97:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/93583220a7aa4debeaf3fd4a6cade9f771ee8c2f/superset/config.py#L1302'>superset/config.py:1302:115:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/pandas/conftest.py#L1985'>pandas/conftest.py:1985:136:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+55 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/accounting/cost_basis/base.py#L687'>rotkehlchen/accounting/cost_basis/base.py:687:104:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L3616'>rotkehlchen/api/rest.py:3616:40:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L1786'>rotkehlchen/api/v1/schemas.py:1786:107:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/hop/constants.py#L40'>rotkehlchen/chain/arbitrum_one/modules/hop/constants.py:40:143:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L36'>rotkehlchen/chain/ethereum/modules/convex/constants.py:36:125:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L37'>rotkehlchen/chain/ethereum/modules/convex/constants.py:37:146:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L46'>rotkehlchen/chain/ethereum/modules/convex/constants.py:46:116:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/convex/constants.py#L47'>rotkehlchen/chain/ethereum/modules/convex/constants.py:47:110:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/curve/constants.py#L38'>rotkehlchen/chain/ethereum/modules/curve/constants.py:38:102:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/l2/loopring.py#L170'>rotkehlchen/chain/ethereum/modules/l2/loopring.py:170:121:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/sushiswap/decoder.py#L26'>rotkehlchen/chain/ethereum/modules/sushiswap/decoder.py:26:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/uniswap/v2/common.py#L343'>rotkehlchen/chain/ethereum/modules/uniswap/v2/common.py:343:91:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/ethereum/modules/uniswap/v2/decoder.py#L29'>rotkehlchen/chain/ethereum/modules/uniswap/v2/decoder.py:29:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/aave/common.py#L258'>rotkehlchen/chain/evm/decoding/aave/common.py:258:123:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/aave/v3/decoder.py#L70'>rotkehlchen/chain/evm/decoding/aave/v3/decoder.py:70:97:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L123'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py:123:99:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L190'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py:190:98:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L249'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py:249:99:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/decoder.py#L351'>rotkehlchen/chain/evm/decoding/curve/decoder.py:351:125:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/curve/decoder.py#L503'>rotkehlchen/chain/evm/decoding/curve/decoder.py:503:121:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/morpho/decoder.py#L197'>rotkehlchen/chain/evm/decoding/morpho/decoder.py:197:96:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/odos/common.py#L138'>rotkehlchen/chain/evm/decoding/odos/common.py:138:96:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/socket_bridge/decoder.py#L87'>rotkehlchen/chain/evm/decoding/socket_bridge/decoder.py:87:102:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/decoding/uniswap/v3/constants.py#L9'>rotkehlchen/chain/evm/decoding/uniswap/v3/constants.py:9:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/node_inquirer.py#L151'>rotkehlchen/chain/evm/node_inquirer.py:151:109:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/node_inquirer.py#L415'>rotkehlchen/chain/evm/node_inquirer.py:415:138:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/evm/transactions.py#L812'>rotkehlchen/chain/evm/transactions.py:812:101:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/data_migrations/migrations/migrations_18.py#L174'>rotkehlchen/data_migrations/migrations/migrations_18.py:174:100:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/db/dbhandler.py#L551'>rotkehlchen/db/dbhandler.py:551:143:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/db/eth2.py#L684'>rotkehlchen/db/eth2.py:684:96:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/db/evmtx.py#L71'>rotkehlchen/db/evmtx.py:71:98:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/exchanges/coinbase.py#L80'>rotkehlchen/exchanges/coinbase.py:80:154:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/externalapis/beaconchain/service.py#L419'>rotkehlchen/externalapis/beaconchain/service.py:419:102:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/externalapis/etherscan.py#L820'>rotkehlchen/externalapis/etherscan.py:820:134:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/globaldb/upgrades/v3_v4.py#L168'>rotkehlchen/globaldb/upgrades/v3_v4.py:168:84:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/globaldb/upgrades/v3_v4.py#L170'>rotkehlchen/globaldb/upgrades/v3_v4.py:170:82:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/server.py#L62'>rotkehlchen/server.py:62:199:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tasks/calendar.py#L434'>rotkehlchen/tasks/calendar.py:434:143:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tests/api/test_accounting_rules.py#L41'>rotkehlchen/tests/api/test_accounting_rules.py:41:154:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tests/db/test_db.py#L942'>rotkehlchen/tests/db/test_db.py:942:99:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/tests/fixtures/accounting.py#L37'>rotkehlchen/tests/fixtures/accounting.py:37:93:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/ac509eaa66e805557368c1890ac40751835b7070/zerver/actions/message_summary.py#L172'>zerver/actions/message_summary.py:172:101:</a> E501 Line too long (132 > 100)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 66 | 66 | 0 | 0 | 0 |
| E501 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+625 -1773 lines in 212 files in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+625 -1773 lines across 212 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/accounting/accountant.py#L78'>rotkehlchen/accounting/accountant.py~L78</a>
```diff
         self.processable_events_cache_signatures: DefaultLRUCache[int, list[int]] = (
             DefaultLRUCache(default_factory=list, maxsize=PROCESSABLE_EVENTS_CACHE_SIZE)
         )  # noqa: E501
-        self.ignored_asset_ids: set[str] = (
-            set()
-        )  # populated in process_history so that we load them in memory once during accounting and not reload them from the DB for every single event processing  # noqa: E501
+        self.ignored_asset_ids: set[str] = set()  # populated in process_history so that we load them in memory once during accounting and not reload them from the DB for every single event processing  # noqa: E501
 
     def activate_premium_status(self, premium: Premium) -> None:
         self.premium = premium
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/accounting/debugimporter/json.py#L52'>rotkehlchen/accounting/debugimporter/json.py~L52</a>
```diff
         events: list[AccountingEventMixin] = []
         try:
             for event in debug_data["events"]:
-                if (
-                    "extra_data" not in event
-                ):  # May be missing for non EVM events in old debug data  # noqa: E501
+                if "extra_data" not in event:  # May be missing for non EVM events in old debug data  # noqa: E501
                     event["extra_data"] = None
                 event_type = AccountingEventType.deserialize(event["accounting_event_type"])
                 match event_type:
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/accounting/history_base_entries.py#L105'>rotkehlchen/accounting/history_base_entries.py~L105</a>
```diff
                 )
                 return 1
 
-            in_event = cast(
-                "HistoryBaseEntry", next(events_iterator)
-            )  # guaranteed by the if check  # noqa: E501
+            in_event = cast("HistoryBaseEntry", next(events_iterator))  # guaranteed by the if check  # noqa: E501
             next_event = events_iterator.peek(None)
             if (
                 next_event
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/accounting/history_base_entries.py#L115'>rotkehlchen/accounting/history_base_entries.py~L115</a>
```diff
                 and next_event.event_identifier == event.event_identifier
                 and next_event.event_subtype == HistoryEventSubType.FEE
             ):  # noqa: E501
-                fee_event = cast(
-                    "HistoryBaseEntry", next(events_iterator)
-                )  # guaranteed by if check  # noqa: E501
+                fee_event = cast("HistoryBaseEntry", next(events_iterator))  # guaranteed by if check  # noqa: E501
 
             with self.pot.database.conn.read_ctx() as cursor:
                 if (
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L2017'>rotkehlchen/api/rest.py~L2017</a>
```diff
                 blockchain=blockchain,
                 accounts=accounts,
             )
-            balances_update = self.rotkehlchen.chains_aggregator.get_balances_update(
-                chain=None
-            )  # return full update of balances  # noqa: E501
+            balances_update = self.rotkehlchen.chains_aggregator.get_balances_update(chain=None)  # return full update of balances  # noqa: E501
         except EthSyncError as e:
             return {"result": None, "message": str(e), "status_code": HTTPStatus.CONFLICT}
         except InputError as e:
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L2791'>rotkehlchen/api/rest.py~L2791</a>
```diff
     ) -> dict[str, Any]:
         blockchain_addresses: dict[SupportedBlockchain, ListOfBlockchainAddresses]
         with self.rotkehlchen.data.db.conn.read_ctx() as cursor:
-            if (
-                accounts is None or len(accounts) == 0
-            ):  # No accounts specified. Get all accounts from DB.  # noqa: E501
+            if accounts is None or len(accounts) == 0:  # No accounts specified. Get all accounts from DB.  # noqa: E501
                 blockchain_addresses = {
                     chain: addr_list
                     for chain in CHAINS_WITH_TRANSACTIONS
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L3984'>rotkehlchen/api/rest.py~L3984</a>
```diff
                         "assets.json could not be found in the provided zip file."
                     )  # noqa: E501
 
-                with (
-                    tempfile.TemporaryDirectory(ignore_cleanup_errors=True) as tempdir
-                ):  # needed on windows, see https://tinyurl.com/tmp-win-err  # noqa: E501
+                with tempfile.TemporaryDirectory(ignore_cleanup_errors=True) as tempdir:  # needed on windows, see https://tinyurl.com/tmp-win-err  # noqa: E501
                     zip_file.extract("assets.json", tempdir)
                     import_assets_from_file(
                         path=Path(tempdir) / "assets.json",
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L5020'>rotkehlchen/api/rest.py~L5020</a>
```diff
             filename = generate_events_export_filename(
                 filter_query=filter_query, use_localtime=settings.display_date_in_localtime
             )  # noqa: E501
-            if (
-                directory_path is None
-            ):  # file will be downloaded later via download_history_events_csv endpoint  # noqa: E501
+            if directory_path is None:  # file will be downloaded later via download_history_events_csv endpoint  # noqa: E501
                 file_path = Path(tempfile.mkdtemp()) / filename
                 dict_to_csv_file(
                     path=file_path,
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest.py#L5223'>rotkehlchen/api/rest.py~L5223</a>
```diff
             if token.protocol == SPAM_PROTOCOL:  # remove the spam protocol if it was set
                 set_token_spam_protocol(write_cursor=write_cursor, token=token, is_spam=False)
 
-        with (
-            self.rotkehlchen.data.db.user_write() as write_cursor
-        ):  # remove it from the ignored assets  # noqa: E501
+        with self.rotkehlchen.data.db.user_write() as write_cursor:  # remove it from the ignored assets  # noqa: E501
             self.rotkehlchen.data.db.remove_from_ignored_assets(
                 write_cursor=write_cursor,
                 asset=token,
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest_helpers/history_events.py#L29'>rotkehlchen/api/rest_helpers/history_events.py~L29</a>
```diff
     - InputError
     """
     if (
-        (
-            result
-            := write_cursor.execute(  # Get the event_identifier, selecting by the identifier of the first event.  # noqa: E501
-                "SELECT event_identifier FROM history_events WHERE identifier=?",
-                (events[0].identifier,),
-            ).fetchone()
-        )
-        is not None
-    ):
+        result := write_cursor.execute(  # Get the event_identifier, selecting by the identifier of the first event.  # noqa: E501
+            "SELECT event_identifier FROM history_events WHERE identifier=?",
+            (events[0].identifier,),
+        ).fetchone()
+    ) is not None:
         event_identifier = result[0]
     else:
         raise InputError(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/rest_helpers/history_events.py#L113'>rotkehlchen/api/rest_helpers/history_events.py~L113</a>
```diff
             events_db.edit_history_event(write_cursor=write_cursor, event=event)
             edited_identifiers.append(event.identifier)
 
-    if (
-        identifiers != edited_identifiers
-    ):  # There are identifiers with no corresponding events - these events need to be deleted.  # noqa: E501
+    if identifiers != edited_identifiers:  # There are identifiers with no corresponding events - these events need to be deleted.  # noqa: E501
         write_cursor.executemany(
             "DELETE FROM history_events WHERE identifier=?",
             [(identifier,) for identifier in set(identifiers) - set(edited_identifiers)],
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/server.py#L196'>rotkehlchen/api/server.py~L196</a>
```diff
     ("/assets/counterpartymappings", CounterpartyAssetMappingsResource),
     ("/tags", TagsResource),
     ("/exchanges/binance/pairs", BinanceAvailableMarkets),
-    (
-        "/exchanges/<string:location>/savings",
-        BinanceSavingsResource,
-    ),  # this can only be Binance/BinanceUS  # noqa: E501
+    ("/exchanges/<string:location>/savings", BinanceSavingsResource),  # this can only be Binance/BinanceUS  # noqa: E501
     ("/exchanges/binance/pairs/<string:name>", BinanceUserMarkets),
     ("/exchanges/data", ExchangesDataResource),
     ("/exchanges/data/<string:location>", ExchangesDataResource, "named_exchanges_data_resource"),
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/resources.py#L920'>rotkehlchen/api/v1/resources.py~L920</a>
```diff
     def make_add_schema(self) -> AssetSchema:
         return AssetSchema(
             identifier_required=False,
-            disallowed_asset_types=[
-                AssetType.CUSTOM_ASSET
-            ],  # custom assets are handled on a separate endpoint  # noqa: E501
+            disallowed_asset_types=[AssetType.CUSTOM_ASSET],  # custom assets are handled on a separate endpoint  # noqa: E501
             coingecko=self.rest_api.rotkehlchen.coingecko,
             cryptocompare=self.rest_api.rotkehlchen.cryptocompare,
         )
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/resources.py#L930'>rotkehlchen/api/v1/resources.py~L930</a>
```diff
     def make_edit_schema(self) -> AssetSchema:
         return AssetSchema(
             identifier_required=True,
-            disallowed_asset_types=[
-                AssetType.CUSTOM_ASSET
-            ],  # custom assets are handled on a separate endpoint  # noqa: E501
+            disallowed_asset_types=[AssetType.CUSTOM_ASSET],  # custom assets are handled on a separate endpoint  # noqa: E501
             coingecko=self.rest_api.rotkehlchen.coingecko,
             cryptocompare=self.rest_api.rotkehlchen.cryptocompare,
             is_edit=True,
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/resources.py#L944'>rotkehlchen/api/v1/resources.py~L944</a>
```diff
 
     @require_loggedin_user()
     @resource_parser.use_kwargs(make_add_schema, location="json")
-    def put(
-        self, asset: AssetWithOracles
-    ) -> Response:  # is asset with oracles since we disallow custom assets  # noqa: E501
+    def put(self, asset: AssetWithOracles) -> Response:  # is asset with oracles since we disallow custom assets  # noqa: E501
         return self.rest_api.add_user_asset(asset)
 
     @resource_parser.use_kwargs(make_edit_schema, location="json")
-    def patch(
-        self, asset: AssetWithOracles
-    ) -> Response:  # is asset with oracles since we disallow custom assets  # noqa: E501
+    def patch(self, asset: AssetWithOracles) -> Response:  # is asset with oracles since we disallow custom assets  # noqa: E501
         return self.rest_api.edit_user_asset(asset)
 
     @require_loggedin_user()
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L257'>rotkehlchen/api/v1/schemas.py~L257</a>
```diff
 
 class DBOrderBySchema(Schema):
     order_by_attributes = DelimitedOrNormalList(EmptyAsNoneStringField(), load_default=None)
-    ascending = DelimitedOrNormalList(
-        fields.Boolean(), load_default=None
-    )  # most recent first by default  # noqa: E501
+    ascending = DelimitedOrNormalList(fields.Boolean(), load_default=None)  # most recent first by default  # noqa: E501
 
     @validates_schema
     def validate_order_by_schema(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L715'>rotkehlchen/api/v1/schemas.py~L715</a>
```diff
         }
 
         filter_query: HistoryEventFilterQuery | (EvmEventFilterQuery | EthStakingEventFilterQuery)
-        if (
-            should_query_evm_event or location_labels is not None
-        ):  # use evm event filter since only evm events have the "address" column for counterparty checks  # noqa: E501
+        if should_query_evm_event or location_labels is not None:  # use evm event filter since only evm events have the "address" column for counterparty checks  # noqa: E501
             filter_query = EvmEventFilterQuery.make(
                 **common_arguments,
                 tx_hashes=data["tx_hashes"],
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L920'>rotkehlchen/api/v1/schemas.py~L920</a>
```diff
         location_label = EmptyAsNoneStringField(load_default=None)
         blockchain = EmptyAsNoneStringField(load_default=None)
         unique_id = EmptyAsNoneStringField(required=False, load_default=None)
-        address = EmptyAsNoneStringField(
-            required=False, load_default=None
-        )  # It can be an address for any chain not only the supported ones so we validate it as string.  # noqa: E501
-        transaction_id = EmptyAsNoneStringField(
-            required=False, load_default=None
-        )  # It can be a transaction from any chain. We don't do any special validation on it.  # noqa: E501
+        address = EmptyAsNoneStringField(required=False, load_default=None)  # It can be an address for any chain not only the supported ones so we validate it as string.  # noqa: E501
+        transaction_id = EmptyAsNoneStringField(required=False, load_default=None)  # It can be a transaction from any chain. We don't do any special validation on it.  # noqa: E501
         event_identifier = EmptyAsNoneStringField(required=False, load_default=None)
         asset = AssetField(required=True, expected_type=Asset, form_with_incomplete_data=True)
         fee_asset = AssetField(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L3524'>rotkehlchen/api/v1/schemas.py~L3524</a>
```diff
 class UserNotesPutSchema(Schema):
     title = NonEmptyStringField(required=True)
     content = NonEmptyStringField(required=True)  # frontend requires a non empty description
-    location = fields.String(
-        required=True
-    )  # empty string here means that it's a global location. TODO: Change  # noqa: E501
+    location = fields.String(required=True)  # empty string here means that it's a global location. TODO: Change  # noqa: E501
     is_pinned = fields.Boolean(required=True)
 
 
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L4080'>rotkehlchen/api/v1/schemas.py~L4080</a>
```diff
     ) -> dict[str, Any]:  # noqa: E501
         return {
             "calendar": CalendarEntry(
-                identifier=data.get(
-                    "identifier", 0
-                ),  # not present here but used in UpdateCalendarSchema  # noqa: E501
+                identifier=data.get("identifier", 0),  # not present here but used in UpdateCalendarSchema  # noqa: E501
                 name=data["name"],
                 timestamp=data["timestamp"],
                 description=data["description"],
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/api/v1/schemas.py#L4167'>rotkehlchen/api/v1/schemas.py~L4167</a>
```diff
     @staticmethod
     def _process_reminder(data: dict[str, Any]) -> ReminderEntry:
         return ReminderEntry(
-            identifier=data.get(
-                "identifier", 0
-            ),  # not present when creating a new reminder but used in UpdateCalendarReminderSchema. Using default 0 since it is ignored when doing the creation  # noqa: E501
+            identifier=data.get("identifier", 0),  # not present when creating a new reminder but used in UpdateCalendarReminderSchema. Using default 0 since it is ignored when doing the creation  # noqa: E501
             secs_before=data["secs_before"],
             event_id=data["event_id"],
             acknowledged=data["acknowledged"],
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/assets/converters.py#L447'>rotkehlchen/assets/converters.py~L447</a>
```diff
     woo_name = GlobalDBHandler.get_assetid_from_exchange_name(
         exchange=Location.WOO,
         symbol=woo_name,
-        default=woo_name.split("_", maxsplit=1)[
-            -1
-        ],  # some woo assets are prefixed with the network for deposits/withdrawals  # noqa: E501
+        default=woo_name.split("_", maxsplit=1)[-1],  # some woo assets are prefixed with the network for deposits/withdrawals  # noqa: E501
     )
     return symbol_to_asset_or_token(
         GlobalDBHandler.get_assetid_from_exchange_name(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/assets/resolver.py#L175'>rotkehlchen/assets/resolver.py~L175</a>
```diff
             # Check if the version in the packaged globaldb is correct
             globaldb = AssetResolver._globaldb
             packaged_asset = globaldb.resolve_asset(identifier=identifier, use_packaged_db=True)
-            if isinstance(
-                packaged_asset, expected_type
-            ):  # it's what was requested. So fix local global db  # noqa: E501
+            if isinstance(packaged_asset, expected_type):  # it's what was requested. So fix local global db  # noqa: E501
                 resolved_asset = globaldb.resolve_asset_from_packaged_and_store(
                     identifier=identifier
                 )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/balances/manual.py#L22'>rotkehlchen/balances/manual.py~L22</a>
```diff
     location: Location
     tags: list[str] | None
     balance_type: BalanceType
-    asset_is_missing: bool = field(
-        default=False
-    )  # set to true when the asset points to an asset that is unknown to the db  # noqa: E501
+    asset_is_missing: bool = field(default=False)  # set to true when the asset points to an asset that is unknown to the db  # noqa: E501
 
 
 @dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False)
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/aggregator.py#L591'>rotkehlchen/chain/aggregator.py~L591</a>
```diff
                 xpub_manager.check_for_new_xpub_addresses(blockchain=blockchain)  # type: ignore # is checked in the if
         else:  # all chains
             for chain in SupportedBlockchain:
-                if (
-                    chain.is_evm() and len(self.accounts.get(chain)) == 0
-                ):  # don't check eth2 and bitcoin since we might need to query new addresses  # noqa: E501
+                if chain.is_evm() and len(self.accounts.get(chain)) == 0:  # don't check eth2 and bitcoin since we might need to query new addresses  # noqa: E501
                     continue
 
                 query_method = f"query_{chain.get_key()}_balances"
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/manager.py#L28'>rotkehlchen/chain/arbitrum_one/manager.py~L28</a>
```diff
                 database=node_inquirer.database,
                 evm_inquirer=node_inquirer,
                 token_exceptions={
-                    string_to_evm_address(
-                        "0x0043d7c85C8b96a49A72A92C0B48CdC4720437d7"
-                    ),  # Constant Inflow Token  # noqa: E501
-                    string_to_evm_address(
-                        "0x051e766e2d8dc65ae2bFCF084A50AD0447634227"
-                    ),  # Constant Outflow Token  # noqa: E501
+                    string_to_evm_address("0x0043d7c85C8b96a49A72A92C0B48CdC4720437d7"),  # Constant Inflow Token  # noqa: E501
+                    string_to_evm_address("0x051e766e2d8dc65ae2bFCF084A50AD0447634227"),  # Constant Outflow Token  # noqa: E501
                 },
             ),
             transactions_decoder=ArbitrumOneTransactionDecoder(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py#L46'>rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py~L46</a>
```diff
 TOKEN_WITHDRAWAL_INITIATED: Final = (
     b"0s\xa7N\xcbr\x8d\x10\xbew\x9f\xe1\x9at\xa1B\x8e F\x8f[M\x16{\xf9\xc7=\x90g\x84}s"  # noqa: E501
 )
-L2_TO_L1_TX: Final = b">z\xaf\xa7}\xbf\x18k\x7f\xd4\x88\x00k\xef\xf8\x93tL\xaa<Oo)\x9e\x8ap\x9f\xa2\x08st\xfc"  # https://github.com/OffchainLabs/bold/blob/4c45c226da2662f357450fcce9270bb00324eb57/contracts/src/precompiles/ArbSys.sol#L113  # noqa: E501
+L2_TO_L1_TX: Final = (
+    b">z\xaf\xa7}\xbf\x18k\x7f\xd4\x88\x00k\xef\xf8\x93tL\xaa<Oo)\x9e\x8ap\x9f\xa2\x08st\xfc"  # https://github.com/OffchainLabs/bold/blob/4c45c226da2662f357450fcce9270bb00324eb57/contracts/src/precompiles/ArbSys.sol#L113  # noqa: E501
+)
 DEPOSIT_TX_TYPE: Final = 100  # A deposit of ETH from L1 to L2 via the Arbitrum bridge.
 ERC20_DEPOSIT_FINALIZED: Final = b'\xc7\xf2\xe9\xc5\\@\xa5\x0f\xbc!}\xfcp\xcd9\xa2"\x94\r\xfab\x14Z\xa0\xcaI\xeb\x955\xd4\xfc\xb2'  # noqa: E501
 WITHDRAW_ETH_METHOD: Final = b"%\xe1`c"
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py#L186'>rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py~L186</a>
```diff
             return DEFAULT_DECODING_OUTPUT
 
         from_address = bytes_to_address(context.tx_log.topics[1])
-        to_address = bytes_to_address(
-            context.transaction.input_data[4:]
-        )  # only argument of input data is destination address # noqa: E501
+        to_address = bytes_to_address(context.transaction.input_data[4:])  # only argument of input data is destination address # noqa: E501
 
         if not self.base.any_tracked([from_address, to_address]):
             return DEFAULT_DECODING_OUTPUT
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py#L324'>rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py~L324</a>
```diff
     def decoding_by_tx_type(self) -> dict[int, list[tuple[int, Callable]]]:
         return {
             DEPOSIT_TX_TYPE: [
-                (
-                    0,
-                    self._decode_eth_deposit_event,
-                ),  # We need this rule because these transactions contain no logs and consequently we can only run a decoder as a post decoding rule  # noqa: E501
+                (0, self._decode_eth_deposit_event),  # We need this rule because these transactions contain no logs and consequently we can only run a decoder as a post decoding rule  # noqa: E501
             ],
         }
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/curve/decoder.py#L30'>rotkehlchen/chain/arbitrum_one/modules/curve/decoder.py~L30</a>
```diff
             aave_pools=set(),
             curve_deposit_contracts={
                 DEPOSIT_AND_STAKE_ZAP,
-                string_to_evm_address(
-                    "0xF97c707024ef0DD3E77a0824555a46B622bfB500"
-                ),  # Tricrypto Zap  # noqa: E501
+                string_to_evm_address("0xF97c707024ef0DD3E77a0824555a46B622bfB500"),  # Tricrypto Zap  # noqa: E501
                 string_to_evm_address("0x25e2e8d104BC1A70492e2BE32dA7c1f8367F9d2c"),  # EURs Zap
                 string_to_evm_address("0x7544Fe3d184b6B55D6B36c3FCA1157eE0Ba30287"),  # MetaUSD Zap
                 string_to_evm_address("0x803A2B40c5a9BB2B86DD630B274Fa2A9202874C2"),  # MetaBTC Zap
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/gmx/balances.py#L129'>rotkehlchen/chain/arbitrum_one/modules/gmx/balances.py~L129</a>
```diff
             log.error(f"Failed to query GMX balances due to {e!s}")
             return balances
 
-        for idx, result in enumerate(
-            call_output
-        ):  # each iteration is a different address with its positions  # noqa: E501
+        for idx, result in enumerate(call_output):  # each iteration is a different address with its positions  # noqa: E501
             pos_information = reader_contract.decode(
                 result=result,
                 method_name="getPositions",
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/gmx/balances.py#L160'>rotkehlchen/chain/arbitrum_one/modules/gmx/balances.py~L160</a>
```diff
                     continue
 
                 position_collateral_value = token_normalized_value_decimals(
-                    token_amount=pos_result[1]
-                    + pos_result[8],  # sum the collateral + delta (gain or loss)  # noqa: E501
+                    token_amount=pos_result[1] + pos_result[8],  # sum the collateral + delta (gain or loss)  # noqa: E501
                     token_decimals=GMX_USD_DECIMALS,
                 )
                 asset_amount = round(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/gmx/decoder.py#L165'>rotkehlchen/chain/arbitrum_one/modules/gmx/decoder.py~L165</a>
```diff
                 and event.event_type == base_event_type
                 and (
                     (verb_text == "increase" and event.amount == transferred_amount)
-                    or verb_text
-                    == "decrease"  # when it is a decrease we don't know the amount that will be received  # noqa: E501
+                    or verb_text == "decrease"  # when it is a decrease we don't know the amount that will be received  # noqa: E501
                 )
                 and _asset_matches(token=path_token, event_asset=event.asset)
             ):
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/umami/decoder.py#L39'>rotkehlchen/chain/arbitrum_one/modules/umami/decoder.py~L39</a>
```diff
     b"q\xba\xb6\\\xed.WPwZ\x06\x13\xbe\x06}\xf4\x8e\xf0l\xf9*In\xbfvc\xae\x06`\x92IT"  # noqa: E501
 )
 DEPOSIT_EXECUTION_FOUR_BYTES: Final = b"\xdb\x10\xc3\xb9"
-UMAMI_DEPOSIT_WITHDRAWAL_FEE_PERCENTAGE: Final = FVal(
-    "0.0015"
-)  # this is an estimate since the actual percentage is dynamic -- 0.15%  # noqa: E501
+UMAMI_DEPOSIT_WITHDRAWAL_FEE_PERCENTAGE: Final = FVal("0.0015")  # this is an estimate since the actual percentage is dynamic -- 0.15%  # noqa: E501
 
 
 class FoundEventType(Enum):
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/arbitrum_one/modules/zerox/constants.py#L4'>rotkehlchen/chain/arbitrum_one/modules/zerox/constants.py~L4</a>
```diff
 
 # Skipped versions v1.1, v1.2, and v1.4 because these contracts don't have transactions
 SETTLER_ROUTERS: Final = {
-    string_to_evm_address(
-        "0x2b6625AAFC65373e5a82A0349F777FA11F7F04d1"
-    ),  # V1.3 commit: 0x336fda7ac33e46626cba703a82a53ad517aa8336000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0xc8e09d4Ac2bf8b83a842068A0A6e79E118414a1d"
-    ),  # V1.5 commit: 0xa5a3b402765eb2940a6e29efa81a58e222d0ae6a000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0x845a4dae544147FFe8cB536079b58ee43F320067"
-    ),  # V1.6 commit: 0x3ae13a6a1d3eea900d733ebc1d1ba9d772e6b415000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0xf3E01012Ce60BB95AE294D5b24a9Fc3af245b53b"
-    ),  # V1.7 commit: 0xa6f39ee20f0c4dfe1265f5d203dfc4f3f05ca003000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0x6dB4d3B89b06d3C8Bd2074Ee1F1B440baf122eAd"
-    ),  # V1.8 commit: 0xffc129424fbe525c124e52cff5225afbfb610534000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0xB254ee265261675528bdDb0796741c0C65a4C158"
-    ),  # V1.9 commit: 0xed3a62a794e1598537d99e3f7d44d08f2ff2cac8000000000000000000000000  # noqa: E501
+    string_to_evm_address("0x2b6625AAFC65373e5a82A0349F777FA11F7F04d1"),  # V1.3 commit: 0x336fda7ac33e46626cba703a82a53ad517aa8336000000000000000000000000  # noqa: E501
+    string_to_evm_address("0xc8e09d4Ac2bf8b83a842068A0A6e79E118414a1d"),  # V1.5 commit: 0xa5a3b402765eb2940a6e29efa81a58e222d0ae6a000000000000000000000000  # noqa: E501
+    string_to_evm_address("0x845a4dae544147FFe8cB536079b58ee43F320067"),  # V1.6 commit: 0x3ae13a6a1d3eea900d733ebc1d1ba9d772e6b415000000000000000000000000  # noqa: E501
+    string_to_evm_address("0xf3E01012Ce60BB95AE294D5b24a9Fc3af245b53b"),  # V1.7 commit: 0xa6f39ee20f0c4dfe1265f5d203dfc4f3f05ca003000000000000000000000000  # noqa: E501
+    string_to_evm_address("0x6dB4d3B89b06d3C8Bd2074Ee1F1B440baf122eAd"),  # V1.8 commit: 0xffc129424fbe525c124e52cff5225afbfb610534000000000000000000000000  # noqa: E501
+    string_to_evm_address("0xB254ee265261675528bdDb0796741c0C65a4C158"),  # V1.9 commit: 0xed3a62a794e1598537d99e3f7d44d08f2ff2cac8000000000000000000000000  # noqa: E501
 }
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/base/manager.py#L28'>rotkehlchen/chain/base/manager.py~L28</a>
```diff
                 database=node_inquirer.database,
                 evm_inquirer=node_inquirer,
                 token_exceptions={
-                    string_to_evm_address(
-                        "0x2d51962A9EE4D3C2819EF585eab7412c2a2C31Ac"
-                    ),  # Constant Inflow Token  # noqa: E501
-                    string_to_evm_address(
-                        "0xD3C78bb5a16Ea4ab584844eeb8F90Ac710c16355"
-                    ),  # Constant Outflow Token  # noqa: E501
+                    string_to_evm_address("0x2d51962A9EE4D3C2819EF585eab7412c2a2C31Ac"),  # Constant Inflow Token  # noqa: E501
+                    string_to_evm_address("0xD3C78bb5a16Ea4ab584844eeb8F90Ac710c16355"),  # Constant Outflow Token  # noqa: E501
                 },
             ),
             transactions_decoder=BaseTransactionDecoder(
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/base/modules/basenames/decoder.py#L194'>rotkehlchen/chain/base/modules/basenames/decoder.py~L194</a>
```diff
                 event.notes = f"Register {self.display_name} name {fullname} for {event.amount} ETH until {self.timestamp_to_date(expires)}"  # noqa: E501
                 event.extra_data = {"name": fullname, "expires": expires}
                 spend_event = event
-            elif (
-                event.event_type == HistoryEventType.RECEIVE
-                and tokenid_belongs_to_collection(
-                    token_identifier=event.asset.identifier,
-                    collection_identifier="eip155:8453/erc721:0x03c4738Ee98aE44591e1A4A4F3CaB6641d95DD9a",  # Basenames NFTs  # noqa: E501
-                )
+            elif event.event_type == HistoryEventType.RECEIVE and tokenid_belongs_to_collection(
+                token_identifier=event.asset.identifier,
+                collection_identifier="eip155:8453/erc721:0x03c4738Ee98aE44591e1A4A4F3CaB6641d95DD9a",  # Basenames NFTs  # noqa: E501
             ):
                 event.event_type = HistoryEventType.TRADE
                 event.event_subtype = HistoryEventSubType.RECEIVE
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/base/modules/morpho/decoder.py#L22'>rotkehlchen/chain/base/modules/morpho/decoder.py~L22</a>
```diff
             base_tools=base_tools,
             msg_aggregator=msg_aggregator,
             bundlers={
-                string_to_evm_address(
-                    "0x23055618898e202386e6c13955a58D3C68200BFB"
-                ),  # ChainAgnosticBundlerV2  # noqa: E501
-                string_to_evm_address(
-                    "0x123f3167a416cA19365dE03a65e0AF3532af7223"
-                ),  # CompoundV2MigrationBundlerV2  # noqa: E501
-                string_to_evm_address(
-                    "0x1f8076e2EB6f10b12e6886f30D4909A91969F7dA"
-                ),  # CompoundV3MigrationBundlerV2  # noqa: E501
-                string_to_evm_address(
-                    "0xcAe2929baBc60Be34818EaA5F40bF69265677108"
-                ),  # AaveV3MigrationBundlerV2  # noqa: E501
+                string_to_evm_address("0x23055618898e202386e6c13955a58D3C68200BFB"),  # ChainAgnosticBundlerV2  # noqa: E501
+                string_to_evm_address("0x123f3167a416cA19365dE03a65e0AF3532af7223"),  # CompoundV2MigrationBundlerV2  # noqa: E501
+                string_to_evm_address("0x1f8076e2EB6f10b12e6886f30D4909A91969F7dA"),  # CompoundV3MigrationBundlerV2  # noqa: E501
+                string_to_evm_address("0xcAe2929baBc60Be34818EaA5F40bF69265677108"),  # AaveV3MigrationBundlerV2  # noqa: E501
             },
             weth=A_WETH_BASE,
         )
```
<a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/chain/base/modules/zerox/constants.py#L4'>rotkehlchen/chain/base/modules/zerox/constants.py~L4</a>
```diff
 
 # Skipped version v1.1, v1.3 and v1.5 because these contracts dosn't have transactions
 SETTLER_ROUTERS: Final = {
-    string_to_evm_address(
-        "0x3d868Cd3f8f24DA361364E442411DAe23CD79cC4"
-    ),  # V1.2 commit: 0x7426193dacc58467ab4b85bb1fa7acbc0a4764d4000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0x55873e4b1Dd63ab3Fea3CA47c10277655Ac2DcE0"
-    ),  # V1.4 commit: 0x336fda7ac33e46626cba703a82a53ad517aa8336000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0x163631Ebf9550476156d78748dFF6b1C691d89e1"
-    ),  # V1.6 commit: 0xa5a3b402765eb2940a6e29efa81a58e222d0ae6a000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0xf15c6EC20e5863351D3bBC9E742f9208E3A343fF"
-    ),  # V1.7 commit: 0x3ae13a6a1d3eea900d733ebc1d1ba9d772e6b415000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0xBc3c5cA50b6A215edf00815965485527f26F5dA8"
-    ),  # V1.8 commit: 0xa6f39ee20f0c4dfe1265f5d203dfc4f3f05ca003000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0x6A57A0579E91A5B7ce9c2d08b93E1A9b995f974f"
-    ),  # V1.9 commit: 0xffc129424fbe525c124e52cff5225afbfb610534000000000000000000000000  # noqa: E501
-    string_to_evm_address(
-        "0x5C9bdC801a600c006c388FC032dCb27355154cC9"
-    ),  # V1.10 commit: 0xed3a62a794e1598537d99e3f7d44d08f2ff2cac8000000000000000000000000  # noqa: E501
+    string_to_evm_address("0x3d868Cd3f8f24DA361364E442411DAe23CD79cC4"),  # V1.2 commit: 0x7426193dacc58467ab4b85bb1fa7acbc0a4764d4000000000000000000000000  # noqa: E501
+    string_to_evm_address("0x55873e4b1Dd63ab3Fea3CA47c10277655Ac2DcE0"),  # V1.4 commit: 0x336fda7ac33e46626cba703a82a53ad517aa8336000000000000000000000000  # noqa: E501
+    string_to_evm_address("0x163631Ebf9550476156d78748dFF6b1C691d89e1"),  # V1.6 commit: 0xa5a3b402765eb2940a6e29efa81a58e222d0ae6a000000000000000000000000  # noqa: E501
+...*[Comment body truncated]*

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:29 on 2025-06-10 06:30_

Something went wrong here when merging. This PR removes the `ty` pragma comment again.

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:17 on 2025-06-10 06:31_

Having a second look at this. It would be better to handle nested comments in https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_linter/src/rules/pycodestyle/overlong.rs#L128-L133 because the formatter already handles nested comments. 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:26 on 2025-06-10 06:32_

Can you tell me more about this change? We should avoid using `collect` in this method. It's called very frequently from the formatter.

---

_@MichaReiser requested changes on 2025-06-10 06:32_

---

_@MichaReiser reviewed on 2025-06-10 06:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__ruf100_0.snap`:244 on 2025-06-10 06:33_

It seems wrong that this is now flagged

---

_Comment by @MichaReiser on 2025-06-10 06:34_

Thank you for working on this

This is a very disruptive change. We need to gate this behind preview. 

1. Add a new function to [`preview.rs`](https://github.com/astral-sh/ruff/blob/4f8a005f8f1c74d9e372812763a3308dede6bb8e/crates/ruff_linter/src/preview.rs#L10-L14), similar to the function already in there
2. Only apply the new behavior when the function returns true


We should also add some test demonstrating the new functionality

---

_Label `rule` added by @MichaReiser on 2025-06-10 06:34_

---

_@CodeMan62 reviewed on 2025-06-10 12:51_

---

_Review comment by @CodeMan62 on `crates/ruff_python_trivia/src/pragmas.rs`:17 on 2025-06-10 12:51_

So we just simply revert the changes and make changes in overlong.rs right?? 

---

_@CodeMan62 reviewed on 2025-06-11 04:21_

---

_Review comment by @CodeMan62 on `crates/ruff_python_trivia/src/pragmas.rs`:29 on 2025-06-11 04:21_

strange

---

_@CodeMan62 reviewed on 2025-06-11 04:27_

---

_Review comment by @CodeMan62 on `crates/ruff_python_trivia/src/pragmas.rs`:26 on 2025-06-11 04:27_

So we are lowercasing every byte in the comment then collecting them into a temporary  `Vec<u8>` then scanning the `Vec` and looking for exact sequence which is b"noqa". if we have to avoing using collect then we can create a helper function here 

---

_Comment by @CodeMan62 on 2025-06-11 04:52_

> 1. Add a new function to [`preview.rs`](https://github.com/astral-sh/ruff/blob/4f8a005f8f1c74d9e372812763a3308dede6bb8e/crates/ruff_linter/src/preview.rs#L10-L14), similar to the function already in there
> 2. Only apply the new behavior when the function returns true
> 
> We should also add some test demonstrating the new functionality

sounds good i am doing all these 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:26 on 2025-06-11 06:22_

I'd prefer to keep what we had before unless there's a reason why we have to change it in this PR

---

_@MichaReiser reviewed on 2025-06-11 06:22_

---

_@MichaReiser reviewed on 2025-06-11 06:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:17 on 2025-06-11 06:22_

Yes. Sorry for my incorrect suggestion in the issue. I only recently became aware that this function is also used by the formatter.

---

_@CodeMan62 reviewed on 2025-06-11 08:35_

---

_Review comment by @CodeMan62 on `crates/ruff_python_trivia/src/pragmas.rs`:17 on 2025-06-11 08:35_

sounds good to me will do it 

---

_@CodeMan62 reviewed on 2025-06-12 16:37_

---

_Review comment by @CodeMan62 on `crates/ruff_python_trivia/src/pragmas.rs`:17 on 2025-06-12 16:37_

needs review now 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/overlong.rs`:126 on 2025-06-13 06:42_

It's not clear to me how this fixes that `# comment # noqa: test` should only count up to the start of the `# noqa`. 

1. I suggest you add a new test demonstrating the behavior we want
2. I think you have to split the `comment` by `#`.

---

_@MichaReiser reviewed on 2025-06-13 06:42_

---

_@CodeMan62 reviewed on 2025-06-13 08:29_

---

_Review comment by @CodeMan62 on `crates/ruff_linter/src/rules/pycodestyle/overlong.rs`:126 on 2025-06-13 08:29_

Okk :+1: 

---

_Comment by @CodeMan62 on 2025-06-15 18:53_

I will do as you said in last review @MichaReiser  but i have one thing 
```rust
pub fn is_pragma_comment(comment: &str) -> bool {
    let Some(content) = comment.strip_prefix('#') else {
        return false;
    };
    let trimmed = content.trim_start();

    // Case-insensitive match against `noqa` (which doesn't require a trailing colon).
    matches!(
        trimmed.as_bytes(),
        [b'n' | b'N', b'o' | b'O', b'q' | b'Q', b'a' | b'A', ..]
    ) ||
        // Case-insensitive match against pragmas that don't require a trailing colon.
        trimmed.starts_with("nosec") ||
        // Case-sensitive match against a variety of pragmas that _do_ require a trailing colon.
        trimmed
        .split_once(':')
        .is_some_and(|(maybe_pragma, _)| matches!(maybe_pragma, "isort" | "type" | "pyright" | "pylint" | "flake8" | "ruff" | "ty"))
}

``` 
in this function we have split `:` from `comment` i think we should add split `comment` by `#` what is your opinion here 

---

_Comment by @MichaReiser on 2025-06-16 06:14_

It's possible to handle this inside `is_pragma_comment` but you would have to change the return type to be more than just a `bool` because both the formatter and linter need to know the width before the first pragmac comment

---

_Review requested from @MichaReiser by @CodeMan62 on 2025-06-19 17:55_

---

_@CodeMan62 reviewed on 2025-06-19 17:58_

everything is done now 

---

_@MichaReiser reviewed on 2025-06-20 06:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/overlong.rs`:129 on 2025-06-20 06:22_

I don't think this fix is correct and it shows in the ecosystem results. 

The idea is to only count the characters up to, but not including, the first pragma comment that we see in the line length computation. 

---

_Review requested from @MichaReiser by @CodeMan62 on 2025-06-21 10:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:382 on 2025-06-23 07:22_

I'd expect it to account for the width up to but not including the start of the pragma comment. E.g. it should include the width of the first part in `# this counts to the width # noqa: RUF` 

It's important the logic here is consistent with the logic in the linter. We should add tests to ensure this is the case.

---

_@MichaReiser reviewed on 2025-06-23 07:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:17 on 2025-06-23 07:22_

We can simplify this to `Option<usize>` where it returns `None` if it isn't a pragma comment and `Some(relative_offset)` otherwise

---

_@MichaReiser reviewed on 2025-06-23 07:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:66 on 2025-06-23 07:23_

I think this can be simplified using `split`

---

_@MichaReiser reviewed on 2025-06-23 07:23_

---

_Comment by @MichaReiser on 2025-06-23 07:25_

Thanks for your work on this but I'll close this PR because there's still a lot that needs to be changed and the repetitive reviews are taking up a lot of my time. 

---

_Closed by @MichaReiser on 2025-06-23 07:25_

---
