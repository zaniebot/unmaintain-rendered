```yaml
number: 10910
title: "Expect `,` for parenthesized with items and not at `)`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
  - fuzzer
assignees: []
merged: true
base: dhruv/parser
head: dhruv/with-item-expect-comma
created_at: 2024-04-12T14:24:31Z
updated_at: 2024-04-15T12:59:54Z
url: https://github.com/astral-sh/ruff/pull/10910
synced_at: 2026-01-10T22:37:01Z
```

# Expect `,` for parenthesized with items and not at `)`

---

_Pull request opened by @dhruvmanila on 2024-04-12 14:24_

## Summary

This PR fixes a bug in parenthesized with-items parsing where the parser should expect a comma but it didn't.

This is the case in the speculative parsing loop of parenthesized with-items. Once the parser finds any token other than comma, it breaks out of the loop. Then, once the parser has determined the with item kind, it needs to expect a comma if it's not the end of parenthesized with-items.

## Test Plan

Add inline test cases, verify the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-04-12 14:24_

---

_Label `parser` added by @dhruvmanila on 2024-04-12 14:24_

---

_Label `fuzzer` added by @dhruvmanila on 2024-04-12 14:24_

---

_Comment by @github-actions[bot] on 2024-04-12 14:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+33 -92 violations, +0 -0 fixes in 10 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/d9bbcd295832eab0a2da6bf81e0ae00b9777308a/aiven/client/client.py#L159'>aiven/client/client.py:159:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/models/taskinstance.py#L1653'>airflow/models/taskinstance.py:1653:21:</a> PLR6104 Use `/=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/providers/apache/hive/hooks/hive.py#L346'>airflow/providers/apache/hive/hooks/hive.py:346:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/providers/apache/hive/hooks/hive.py#L348'>airflow/providers/apache/hive/hooks/hive.py:348:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/providers/elasticsearch/log/es_task_handler.py#L207'>airflow/providers/elasticsearch/log/es_task_handler.py:207:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/providers/fab/auth_manager/security_manager/override.py#L2041'>airflow/providers/fab/auth_manager/security_manager/override.py:2041:21:</a> PLR6104 Use `%=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/providers/microsoft/azure/utils.py#L184'>airflow/providers/microsoft/azure/utils.py:184:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/airflow/utils/operator_helpers.py#L112'>airflow/utils/operator_helpers.py:112:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3196'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3196:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L257'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:257:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/95a136e7fda4ada07174020c37bf373e1c3c7d88/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L419'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:419:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/cw_logs/cw_log_puller.py#L136'>samcli/lib/observability/cw_logs/cw_log_puller.py:136:17:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/xray_traces/xray_event_puller.py#L163'>samcli/lib/observability/xray_traces/xray_event_puller.py:163:21:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/xray_traces/xray_events.py#L52'>samcli/lib/observability/xray_traces/xray_events.py:52:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/xray_traces/xray_events.py#L87'>samcli/lib/observability/xray_traces/xray_events.py:87:13:</a> PLR1730 [*] Replace `if` statement with `max` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/server.py#L294'>src/bokeh/embed/server.py:294:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L307'>src/bokeh/server/tornado.py:307:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L422'>src/bokeh/server/tornado.py:422:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_util.py#L462'>securedrop/pretty_bad_protocol/_util.py:462:5:</a> PLR6104 Use `%=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/e31eded9bdc50f821d7d4f33620420e3f8bf00a4/ibis/backends/druid/__init__.py#L61'>ibis/backends/druid/__init__.py:61:9:</a> PLR6104 Use `|=` to perform an augmented assignment directly
- <a href='https://github.com/ibis-project/ibis/blob/e31eded9bdc50f821d7d4f33620420e3f8bf00a4/ibis/backends/exasol/__init__.py#L105'>ibis/backends/exasol/__init__.py:105:9:</a> PLR6104 Use `|=` to perform an augmented assignment directly
- <a href='https://github.com/ibis-project/ibis/blob/e31eded9bdc50f821d7d4f33620420e3f8bf00a4/ibis/common/annotations.py#L287'>ibis/common/annotations.py:287:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/ibis-project/ibis/blob/e31eded9bdc50f821d7d4f33620420e3f8bf00a4/ibis/common/annotations.py#L289'>ibis/common/annotations.py:289:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -47 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/_testing/contexts.py#L113'>pandas/_testing/contexts.py:113:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/compat/numpy/function.py#L107'>pandas/compat/numpy/function.py:107:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/compat/numpy/function.py#L168'>pandas/compat/numpy/function.py:168:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/compat/numpy/function.py#L200'>pandas/compat/numpy/function.py:200:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/compat/numpy/function.py#L229'>pandas/compat/numpy/function.py:229:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/masked.py#L693'>pandas/core/arrays/masked.py:693:13:</a> PLR6104 Use `|=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/groupby/groupby.py#L1973'>pandas/core/groupby/groupby.py:1973:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/groupby/groupby.py#L4502'>pandas/core/groupby/groupby.py:4502:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/datetimes.py#L665'>pandas/core/indexes/datetimes.py:665:13:</a> PLR6104 Use `&=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/multi.py#L3590'>pandas/core/indexes/multi.py:3590:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/range.py#L455'>pandas/core/indexes/range.py:455:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/methods/selectn.py#L171'>pandas/core/methods/selectn.py:171:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/window/numba_.py#L152'>pandas/core/window/numba_.py:152:29:</a> PLR6104 Use `*=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/window/numba_.py#L327'>pandas/core/window/numba_.py:327:29:</a> PLR6104 Use `*=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/window/rolling.py#L827'>pandas/core/window/rolling.py:827:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/window/rolling.py#L828'>pandas/core/window/rolling.py:828:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/window/rolling.py#L829'>pandas/core/window/rolling.py:829:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/io/formats/format.py#L344'>pandas/io/formats/format.py:344:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/chain/bitcoin/bch/utils.py#L132'>rotkehlchen/chain/bitcoin/bch/utils.py:132:97:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR6104`)
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/chain/bitcoin/bch/utils.py#L58'>rotkehlchen/chain/bitcoin/bch/utils.py:58:93:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR6104`)
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/chain/ethereum/graph.py#L71'>rotkehlchen/chain/ethereum/graph.py:71:88:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR6104`)
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/chain/ethereum/modules/eth2/beacon.py#L76'>rotkehlchen/chain/ethereum/modules/eth2/beacon.py:76:105:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR6104`)
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/chain/evm/decoding/utils.py#L42'>rotkehlchen/chain/evm/decoding/utils.py:42:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/chain/evm/node_inquirer.py#L301'>rotkehlchen/chain/evm/node_inquirer.py:301:173:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR6104`)
+ <a href='https://github.com/rotki/rotki/blob/46b11abf1ef51a5b26e41a909abafa5303c0f286/rotkehlchen/db/accounting_rules.py#L268'>rotkehlchen/db/accounting_rules.py:268:120:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR6104`)
... 21 additional changes omitted for rule RUF100
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/analytics/lib/fixtures.py#L77'>analytics/lib/fixtures.py:77:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/tools/lib/pretty_print.py#L134'>tools/lib/pretty_print.py:134:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/zerver/lib/cache.py#L185'>zerver/lib/cache.py:185:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/zerver/lib/ccache.py#L43'>zerver/lib/ccache.py:43:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/zerver/lib/ccache.py#L45'>zerver/lib/ccache.py:45:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/zerver/lib/ccache.py#L70'>zerver/lib/ccache.py:70:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 6 additional changes omitted for rule PLR6104
+ <a href='https://github.com/zulip/zulip/blob/352e2ebc5a3d4fd563b4ff5f9f6e95260f995ab9/zerver/tests/test_openapi.py#L344'>zerver/tests/test_openapi.py:344:13:</a> PLR1730 [*] Replace `if` statement with `val = min(v, val)`
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/core/logger.py#L122'>indico/core/logger.py:122:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/modules/attachments/blueprint.py#L100'>indico/modules/attachments/blueprint.py:100:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/modules/attachments/blueprint.py#L59'>indico/modules/attachments/blueprint.py:59:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/modules/attachments/blueprint.py#L61'>indico/modules/attachments/blueprint.py:61:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/modules/attachments/blueprint.py#L98'>indico/modules/attachments/blueprint.py:98:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/modules/events/management/blueprint.py#L79'>indico/modules/events/management/blueprint.py:79:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 9 additional changes omitted for rule PLR6104
+ <a href='https://github.com/indico/indico/blob/19f74fc977002ed7017f25f4838d31482bd177fb/indico/modules/receipts/controllers/templates.py#L218'>indico/modules/receipts/controllers/templates.py:218:17:</a> PLR1730 [*] Replace `if` statement with `max_index = max(index, max_index)`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6104 | 92 | 0 | 92 | 0 | 0 |
| RUF100 | 26 | 26 | 0 | 0 | 0 |
| PLR1730 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-04-12 15:18_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-12 15:18_

---

_@dhruvmanila reviewed on 2024-04-12 15:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2078 on 2024-04-12 15:19_

This is only the case for the first "gap" as any other missing commas will be highlighted by the list parsing in `parse_with_items`.

---

_@MichaReiser approved on 2024-04-15 08:51_

---

_Merged by @dhruvmanila on 2024-04-15 12:40_

---

_Closed by @dhruvmanila on 2024-04-15 12:40_

---

_Branch deleted on 2024-04-15 12:40_

---
