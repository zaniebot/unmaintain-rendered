```yaml
number: 9539
title: "[`refurb`] - add `bool-literal-compare` with fix (`FURB149`)"
type: pull_request
state: closed
author: diceroll123
labels:
  - rule
assignees: []
base: main
head: add-FURB149
created_at: 2024-01-15T22:57:16Z
updated_at: 2024-03-28T14:07:28Z
url: https://github.com/astral-sh/ruff/pull/9539
synced_at: 2026-01-10T22:47:01Z
```

# [`refurb`] - add `bool-literal-compare` with fix (`FURB149`)

---

_Pull request opened by @diceroll123 on 2024-01-15 22:57_

## Summary

Add [`bool-literal-compare` (`FURB149`)](https://github.com/dosisod/refurb/blob/master/refurb/checks/readability/no_is_bool_compare.py) with fix

See: #1348 

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-01-15 23:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1673 -0 violations, +0 -0 fixes in 5 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+616 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/cli/commands/celery_command.py#L90'>airflow/cli/commands/celery_command.py:90:8:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/cli/commands/connection_command.py#L371'>airflow/cli/commands/connection_command.py:371:8:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/cli/commands/dag_command.py#L139'>airflow/cli/commands/dag_command.py:139:8:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/cli/commands/scheduler_command.py#L83'>airflow/cli/commands/scheduler_command.py:83:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/cli/commands/triggerer_command.py#L39'>airflow/cli/commands/triggerer_command.py:39:8:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/configuration.py#L318'>airflow/configuration.py:318:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/jobs/triggerer_job_runner.py#L192'>airflow/jobs/triggerer_job_runner.py:192:8:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/jobs/triggerer_job_runner.py#L281'>airflow/jobs/triggerer_job_runner.py:281:14:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/models/abstractoperator.py#L193'>airflow/models/abstractoperator.py:193:12:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/models/abstractoperator.py#L193'>airflow/models/abstractoperator.py:193:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/models/dagrun.py#L1473'>airflow/models/dagrun.py:1473:17:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/models/taskinstance.py#L335'>airflow/models/taskinstance.py:335:8:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/models/taskinstance.py#L990'>airflow/models/taskinstance.py:990:8:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/operators/python.py#L282'>airflow/operators/python.py:282:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/providers/amazon/aws/operators/emr.py#L1481'>airflow/providers/amazon/aws/operators/emr.py:1481:20:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/providers/apache/kafka/operators/consume.py#L103'>airflow/providers/apache/kafka/operators/consume.py:103:12:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/providers/atlassian/jira/sensors/jira.py#L125'>airflow/providers/atlassian/jira/sensors/jira.py:125:12:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/apache/airflow/blob/2e3b1758246169b278e7d409ecff98532ec18c18/airflow/providers/celery/cli/celery_command.py#L106'>airflow/providers/celery/cli/celery_command.py:106:8:</a> FURB149 Comparison to `False` should be `not cond`
... 598 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+367 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L116'>src/bokeh/command/bootstrap.py:116:8:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L118'>src/bokeh/command/bootstrap.py:118:10:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L623'>src/bokeh/server/tornado.py:623:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L625'>src/bokeh/server/tornado.py:625:40:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_custom_action.py#L46'>tests/integration/tools/test_custom_action.py:46:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L108'>tests/unit/bokeh/application/handlers/test_code_runner.py:108:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L116'>tests/unit/bokeh/application/handlers/test_code_runner.py:116:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L179'>tests/unit/bokeh/application/handlers/test_code_runner.py:179:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L45'>tests/unit/bokeh/application/handlers/test_code_runner.py:45:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L49'>tests/unit/bokeh/application/handlers/test_code_runner.py:49:16:</a> FURB149 Comparison to `False` should be `not cond`
... 357 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+633 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/package.py#L863'>package.py:863:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/packaging/docker/entrypoint.py#L124'>packaging/docker/entrypoint.py:124:8:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/debugimporter/json.py#L107'>rotkehlchen/accounting/debugimporter/json.py:107:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/debugimporter/json.py#L107'>rotkehlchen/accounting/debugimporter/json.py:107:56:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/debugimporter/json.py#L45'>rotkehlchen/accounting/debugimporter/json.py:45:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/export/csv.py#L133'>rotkehlchen/accounting/export/csv.py:133:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/export/csv.py#L182'>rotkehlchen/accounting/export/csv.py:182:46:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/export/csv.py#L184'>rotkehlchen/accounting/export/csv.py:184:43:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/export/csv.py#L200'>rotkehlchen/accounting/export/csv.py:200:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/export/csv.py#L316'>rotkehlchen/accounting/export/csv.py:316:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/pot.py#L165'>rotkehlchen/accounting/pot.py:165:20:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/pot.py#L182'>rotkehlchen/accounting/pot.py:182:12:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/pot.py#L386'>rotkehlchen/accounting/pot.py:386:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/rules.py#L60'>rotkehlchen/accounting/rules.py:60:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/accounting/structures/processed_event.py#L207'>rotkehlchen/accounting/structures/processed_event.py:207:36:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/api/rest.py#L1288'>rotkehlchen/api/rest.py:1288:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/api/rest.py#L1302'>rotkehlchen/api/rest.py:1302:12:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/rotki/rotki/blob/a774835fb888a1f07408aed509b1cb130a152f94/rotkehlchen/api/rest.py#L1622'>rotkehlchen/api/rest.py:1622:8:</a> FURB149 Comparison to `False` should be `not cond`
... 615 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L1675'>corporate/lib/stripe.py:1675:20:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L1779'>corporate/lib/stripe.py:1779:24:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L3562'>corporate/lib/stripe.py:3562:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L3594'>corporate/lib/stripe.py:3594:20:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L3956'>corporate/lib/stripe.py:3956:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L3986'>corporate/lib/stripe.py:3986:20:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L4389'>corporate/lib/stripe.py:4389:16:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L4419'>corporate/lib/stripe.py:4419:20:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/actions/create_realm.py#L216'>zerver/actions/create_realm.py:216:40:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/actions/scheduled_messages.py#L144'>zerver/actions/scheduled_messages.py:144:12:</a> FURB149 Comparison to `True` should be `cond`
... 21 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+26 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/cli/devserver.py#L208'>indico/cli/devserver.py:208:14:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/cli/devserver.py#L70'>indico/cli/devserver.py:70:16:</a> FURB149 Comparison to `True` should be `cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/core/db/sqlalchemy/links.py#L217'>indico/core/db/sqlalchemy/links.py:217:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/core/db/sqlalchemy/links.py#L244'>indico/core/db/sqlalchemy/links.py:244:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/core/db/sqlalchemy/links.py#L258'>indico/core/db/sqlalchemy/links.py:258:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/core/db/sqlalchemy/links.py#L272'>indico/core/db/sqlalchemy/links.py:272:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/core/db/sqlalchemy/links.py#L286'>indico/core/db/sqlalchemy/links.py:286:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/core/db/sqlalchemy/links.py#L300'>indico/core/db/sqlalchemy/links.py:300:30:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/modules/events/editing/service.py#L259'>indico/modules/events/editing/service.py:259:14:</a> FURB149 Comparison to `False` should be `not cond`
+ <a href='https://github.com/indico/indico/blob/66a8cc31965a87289a7b34cbeb6e83e7cdc0c75f/indico/modules/events/fields_test.py#L57'>indico/modules/events/fields_test.py:57:12:</a> FURB149 Comparison to `False` should be `not cond`
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB149 | 1673 | 1673 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @diceroll123 on 2024-01-16 03:07_

---

_Marked ready for review by @diceroll123 on 2024-01-16 03:25_

---

_Comment by @tjkuson on 2024-01-21 17:24_

What overlap does this have with [`true-false-comparison` (`E712`)](https://docs.astral.sh/ruff/rules/true-false-comparison/)?

---

_Comment by @diceroll123 on 2024-01-21 17:27_

Hrm, in theory, FURB149 could fix it before E712 fixes it, or, E712 fixes it and then FURB149 fixes it afterward.

---

_@charliermarsh reviewed on 2024-01-24 02:53_

This looks pretty similar to [`true-false-comparison`](https://docs.astral.sh/ruff/rules/true-false-comparison/), do you mind comparing the two rules?

---

_Comment by @charliermarsh on 2024-01-24 02:54_

Ugh, sorry, I missed @tjkuson comment 🤦 

---

_Comment by @charliermarsh on 2024-01-24 02:54_

But do you know if the rules catch the same cases, @diceroll123?

---

_Comment by @diceroll123 on 2024-01-24 06:32_

Made an update; I fixed an edge case or two, and narrowed it to disallow chained comparisons while doing the testing below, and after some consideration I've also made the fixes both set to unsafe.

Semantically speaking, I suppose FURB149 is more unsafe than E712, but they definitely don't do the same thing. The major difference is dropping the explicit equivalence operator in favor of the hand-wavy truthiness/falsiness. Falseyness? idk

Here's a comparison of both E712 and FURB149 autofixes ran on the same file:

Running E712 on the E712.py text fixture (with fixes) gives this:
![image](https://github.com/astral-sh/ruff/assets/1243957/f5602751-90f4-44c3-9dde-6c63d9f3fef7)

---

Running FURB149 on the same file with fixes:

![image](https://github.com/astral-sh/ruff/assets/1243957/c8823e0e-f037-44c4-85fe-ff52ba1f356a)


---

_Review requested from @charliermarsh by @diceroll123 on 2024-03-15 05:02_

---

_Comment by @MichaReiser on 2024-03-28 14:07_

Thanks @diceroll123  for working on this PR and writing up the difference to `true-false-comparision`. 

I understand that this rull and `bool-literal-compare` identify the same code smell but provide different fixes. We should only have one rule, or the experience for users is confusing (what fix am I supposed to use and why?) I see two solutions, but this requires more design before opening a follow-up PR:

* An option is to extend `bool-literal-compare` to specify the preferred fix. 
* Wait until Ruff has access to types and use the type information to decide if the safe fix is to fix to `x is True` or `x`. 

I'm happy to check in with the team to see what they think of adding an option to `true-false-comparision` to direct the fix but I'll close this PR because Ruff should only have a single rule per code-smell.

---

_Closed by @MichaReiser on 2024-03-28 14:07_

---

_Label `rule` added by @MichaReiser on 2024-03-28 14:07_

---
