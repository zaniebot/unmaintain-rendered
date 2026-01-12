```yaml
number: 9658
title: "[`refurb`] Implement `metaclass_abcmeta` (`FURB180`)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: latyshev/furb180
created_at: 2024-01-27T20:49:36Z
updated_at: 2024-02-03T22:09:05Z
url: https://github.com/astral-sh/ruff/pull/9658
synced_at: 2026-01-12T15:55:29Z
```

# [`refurb`] Implement `metaclass_abcmeta` (`FURB180`)

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement [use-abc-shorthand (FURB180)](https://github.com/dosisod/refurb/blob/master/refurb/checks/readability/use_abc_shorthand.py) lint.

I changed the name to be more conformant with ruff rule-naming rules.


## Test Plan
<!-- How was it tested? -->
cargo test


---

_Comment by @github-actions[bot] on 2024-01-27 21:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+59 -0 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/metrics/validators.py#L239'>airflow/metrics/validators.py:239:21:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/models/baseoperatorlink.py#L31'>airflow/models/baseoperatorlink.py:31:24:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/models/taskmixin.py#L146'>airflow/models/taskmixin.py:146:32:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/operators/python.py#L308'>airflow/operators/python.py:308:53:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/providers/apache/beam/operators/beam.py#L52'>airflow/providers/apache/beam/operators/beam.py:52:25:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/dev/breeze/src/airflow_breeze/utils/parallel.py#L112'>dev/breeze/src/airflow_breeze/utils/parallel.py:112:35:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L266'>src/bokeh/application/application.py:266:21:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L286'>src/bokeh/application/application.py:286:22:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L62'>src/bokeh/client/states.py:62:13:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L54'>src/bokeh/colors/color.py:54:27:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommand.py#L78'>src/bokeh/command/subcommand.py:78:18:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+45 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/accounting/cost_basis/base.py#L126'>rotkehlchen/accounting/cost_basis/base.py:126:27:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/accounting/mixins/event.py#L28'>rotkehlchen/accounting/mixins/event.py:28:28:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/assets/asset.py#L221'>rotkehlchen/assets/asset.py:221:35:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/assets/asset.py#L238'>rotkehlchen/assets/asset.py:238:45:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/assets/asset.py#L248'>rotkehlchen/assets/asset.py:248:41:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/arbitrum_one/decoding/interfaces.py#L7'>rotkehlchen/chain/arbitrum_one/decoding/interfaces.py:7:50:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/ethereum/interfaces/balances.py#L36'>rotkehlchen/chain/ethereum/interfaces/balances.py:36:27:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/accounting/interfaces.py#L22'>rotkehlchen/chain/evm/accounting/interfaces.py:22:33:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/cowswap/decoder.py#L47'>rotkehlchen/chain/evm/decoding/cowswap/decoder.py:47:46:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/cowswap/interfaces.py#L25'>rotkehlchen/chain/evm/decoding/cowswap/interfaces.py:25:47:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/decoder.py#L120'>rotkehlchen/chain/evm/decoding/decoder.py:120:29:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/decoder.py#L990'>rotkehlchen/chain/evm/decoding/decoder.py:990:63:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/eas/decoder.py#L34'>rotkehlchen/chain/evm/decoding/eas/decoder.py:34:42:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/gitcoin/decoder.py#L44'>rotkehlchen/chain/evm/decoding/gitcoin/decoder.py:44:48:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/interfaces.py#L131'>rotkehlchen/chain/evm/decoding/interfaces.py:131:52:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/interfaces.py#L180'>rotkehlchen/chain/evm/decoding/interfaces.py:180:30:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/interfaces.py#L190'>rotkehlchen/chain/evm/decoding/interfaces.py:190:59:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/interfaces.py#L254'>rotkehlchen/chain/evm/decoding/interfaces.py:254:73:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/interfaces.py#L37'>rotkehlchen/chain/evm/decoding/interfaces.py:37:24:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/oneinch/decoder.py#L24'>rotkehlchen/chain/evm/decoding/oneinch/decoder.py:24:46:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/oneinch/v4/decoder.py#L39'>rotkehlchen/chain/evm/decoding/oneinch/v4/decoder.py:39:52:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/decoding/xdai_bridge/decoder.py#L31'>rotkehlchen/chain/evm/decoding/xdai_bridge/decoder.py:31:49:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/manager.py#L20'>rotkehlchen/chain/evm/manager.py:20:18:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/node_inquirer.py#L176'>rotkehlchen/chain/evm/node_inquirer.py:176:23:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/tokens.py#L118'>rotkehlchen/chain/evm/tokens.py:118:17:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/tokens.py#L390'>rotkehlchen/chain/evm/tokens.py:390:39:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/evm/transactions.py#L41'>rotkehlchen/chain/evm/transactions.py:41:23:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/optimism_superchain/decoding/decoder.py#L24'>rotkehlchen/chain/optimism_superchain/decoding/decoder.py:24:67:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/optimism_superchain/etherscan.py#L18'>rotkehlchen/chain/optimism_superchain/etherscan.py:18:46:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/optimism_superchain/node_inquirer.py#L23'>rotkehlchen/chain/optimism_superchain/node_inquirer.py:23:51:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/optimism_superchain/node_inquirer.py#L71'>rotkehlchen/chain/optimism_superchain/node_inquirer.py:71:107:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/chain/optimism_superchain/transactions.py#L23'>rotkehlchen/chain/optimism_superchain/transactions.py:23:55:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/data_import/importers/binance.py#L50'>rotkehlchen/data_import/importers/binance.py:50:20:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/data_import/importers/binance.py#L56'>rotkehlchen/data_import/importers/binance.py:56:40:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/data_import/importers/binance.py#L88'>rotkehlchen/data_import/importers/binance.py:88:42:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/data_import/utils.py#L21'>rotkehlchen/data_import/utils.py:21:28:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/db/filtering.py#L1136'>rotkehlchen/db/filtering.py:1136:63:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/rotki/rotki/blob/11ed14ee80346f1979955f6e4b74f909cff21f1b/rotkehlchen/db/filtering.py#L291'>rotkehlchen/db/filtering.py:291:21:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/649072977e976fc4d0a0bc803f5250e9010fe955/zerver/lib/notes.py#L11'>zerver/lib/notes.py:11:41:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/zulip/zulip/blob/649072977e976fc4d0a0bc803f5250e9010fe955/zerver/lib/outgoing_webhook.py#L28'>zerver/lib/outgoing_webhook.py:28:39:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/zulip/zulip/blob/649072977e976fc4d0a0bc803f5250e9010fe955/zerver/lib/queue.py#L34'>zerver/lib/queue.py:34:38:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB180 | 59 | 59 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-31 22:05_

---

_Label `rule` added by @charliermarsh on 2024-01-31 22:05_

---

_Label `preview` added by @charliermarsh on 2024-01-31 22:05_

---

_@charliermarsh approved on 2024-01-31 22:24_

Looks great!

---

_Comment by @charliermarsh on 2024-01-31 22:25_

Nice work, really thorough.

---

_@charliermarsh reviewed on 2024-01-31 22:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:70 on 2024-01-31 22:25_

I split this condition out of the `find_position`, I found it a little easier to read them when enforced separately.

---

_Merged by @charliermarsh on 2024-01-31 22:31_

---

_Closed by @charliermarsh on 2024-01-31 22:31_

---

_Branch deleted on 2024-02-03 22:09_

---
