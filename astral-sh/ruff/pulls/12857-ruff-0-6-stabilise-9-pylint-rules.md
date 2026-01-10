```yaml
number: 12857
title: "[Ruff 0.6] Stabilise 9 pylint rules"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: ruff-0.6
head: pylint-stabilisations
created_at: 2024-08-13T12:07:32Z
updated_at: 2024-08-13T13:41:58Z
url: https://github.com/astral-sh/ruff/pull/12857
synced_at: 2026-01-10T21:38:32Z
```

# [Ruff 0.6] Stabilise 9 pylint rules

---

_Pull request opened by @AlexWaygood on 2024-08-13 12:07_

## Summary

Stabilise the following rules for Ruff 0.6:
- `singledispatch-method` (`PLE1519`)
- `singledispatchmethod-function` (`PLE1520`)
- `bad-staticmethod-argument` (`PLW0211`)
- `if-stmt-min-max` (`PLR1730`)
- `invalid-bytes-return-type` (`PLE0308`)
- `invalid-hash-return-type` (`PLE0309`)
- `invalid-index-return-type` (`PLE0305`)
- `invalid-length-return-type` (`E303`)
- ~~`non-augmented-assignment` (`PLR6104`)~~
- `self-or-cls-assignment` (`PLW0642`)

These rules all seem fairly uncontroversial, they've all been in preview for >3 months (and haven't changed much in a while), and none of them have any significant issues open about them that should block stabilisation IMO.

## Test Plan

`cargo test`, but the ecoystem report will be the real judge...


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-13 12:07_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-08-13 12:07_

---

_Comment by @github-actions[bot] on 2024-08-13 12:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+66 -0 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/particles/particle_class.py#L845'>src/plasmapy/particles/particle_class.py:845:21:</a> PLR1730 [*] Replace `if` statement with `max_charge = max(charge, max_charge)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/cli/commands/kubernetes_command.py#L91'>airflow/cli/commands/kubernetes_command.py:91:5:</a> PLR1730 [*] Replace `if` statement with `min_pending_minutes = max(min_pending_minutes, 5)`
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/models/taskinstance.py#L2652'>airflow/models/taskinstance.py:2652:13:</a> PLR1730 [*] Replace `if` statement with `min_backoff = max(min_backoff, 1)`
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/providers/cncf/kubernetes/cli/kubernetes_command.py#L84'>airflow/providers/cncf/kubernetes/cli/kubernetes_command.py:84:5:</a> PLR1730 [*] Replace `if` statement with `min_pending_minutes = max(min_pending_minutes, 5)`
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/tests/cluster_policies/__init__.py#L103'>tests/cluster_policies/__init__.py:103:5:</a> PLR1730 [*] Replace `if` statement with `min` call
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/tests/secrets/test_cache.py#L43'>tests/secrets/test_cache.py:43:25:</a> PLW0211 First argument of a static method should not be named `self`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/observability/cw_logs/cw_log_puller.py#L136'>samcli/lib/observability/cw_logs/cw_log_puller.py:136:17:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/observability/xray_traces/xray_event_puller.py#L163'>samcli/lib/observability/xray_traces/xray_event_puller.py:163:21:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/observability/xray_traces/xray_events.py#L52'>samcli/lib/observability/xray_traces/xray_events.py:52:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/observability/xray_traces/xray_events.py#L87'>samcli/lib/observability/xray_traces/xray_events.py:87:13:</a> PLR1730 [*] Replace `if` statement with `max` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Confusing assignment to `cls` argument in class method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/pymilvus/bulk_writer/buffer.py#L270'>pymilvus/bulk_writer/buffer.py:270:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/pymilvus/bulk_writer/buffer.py#L272'>pymilvus/bulk_writer/buffer.py:272:13:</a> PLR1730 [*] Replace `if` statement with `min` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/pymilvus/orm/iterator.py#L54'>pymilvus/orm/iterator.py:54:9:</a> PLR1730 [*] Replace `if` statement with `min` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+51 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1118'>pandas/core/arrays/datetimelike.py:1118:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1129'>pandas/core/arrays/datetimelike.py:1129:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1131'>pandas/core/arrays/datetimelike.py:1131:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1185'>pandas/core/arrays/datetimelike.py:1185:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1187'>pandas/core/arrays/datetimelike.py:1187:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1260'>pandas/core/arrays/datetimelike.py:1260:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1274'>pandas/core/arrays/datetimelike.py:1274:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1480'>pandas/core/arrays/datetimelike.py:1480:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1699'>pandas/core/arrays/datetimelike.py:1699:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2131'>pandas/core/arrays/datetimelike.py:2131:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2153'>pandas/core/arrays/datetimelike.py:2153:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L456'>pandas/core/arrays/datetimelike.py:456:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L778'>pandas/core/arrays/datetimelike.py:778:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L979'>pandas/core/arrays/datetimelike.py:979:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7827'>pandas/core/frame.py:7827:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7840'>pandas/core/frame.py:7840:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8183'>pandas/core/frame.py:8183:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8195'>pandas/core/frame.py:8195:21:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8235'>pandas/core/frame.py:8235:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L3468'>pandas/core/generic.py:3468:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L3629'>pandas/core/generic.py:3629:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L7992'>pandas/core/generic.py:7992:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L7995'>pandas/core/generic.py:7995:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L7998'>pandas/core/generic.py:7998:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L9235'>pandas/core/generic.py:9235:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L9242'>pandas/core/generic.py:9242:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/generic.py#L9245'>pandas/core/generic.py:9245:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/groupby/grouper.py#L258'>pandas/core/groupby/grouper.py:258:13:</a> PLW0642 Confusing assignment to `cls` argument in class method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/indexes/base.py#L1412'>pandas/core/indexes/base.py:1412:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/indexes/base.py#L2256'>pandas/core/indexes/base.py:2256:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/indexes/base.py#L3067'>pandas/core/indexes/base.py:3067:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/84be0a300b6dc9f445ccc570ffd1d0471133d463/zerver/tests/test_openapi.py#L321'>zerver/tests/test_openapi.py:321:13:</a> PLR1730 [*] Replace `if` statement with `val = min(v, val)`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0642 | 52 | 52 | 0 | 0 | 0 |
| PLR1730 | 13 | 13 | 0 | 0 | 0 |
| PLW0211 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+52 -52 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Confusing assignment to `cls` argument in class method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Invalid assignment to `cls` argument in class method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+51 -51 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1118'>pandas/core/arrays/datetimelike.py:1118:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1118'>pandas/core/arrays/datetimelike.py:1118:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1129'>pandas/core/arrays/datetimelike.py:1129:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1129'>pandas/core/arrays/datetimelike.py:1129:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1131'>pandas/core/arrays/datetimelike.py:1131:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1131'>pandas/core/arrays/datetimelike.py:1131:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1185'>pandas/core/arrays/datetimelike.py:1185:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1185'>pandas/core/arrays/datetimelike.py:1185:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1187'>pandas/core/arrays/datetimelike.py:1187:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1187'>pandas/core/arrays/datetimelike.py:1187:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1260'>pandas/core/arrays/datetimelike.py:1260:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1260'>pandas/core/arrays/datetimelike.py:1260:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1274'>pandas/core/arrays/datetimelike.py:1274:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1274'>pandas/core/arrays/datetimelike.py:1274:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1480'>pandas/core/arrays/datetimelike.py:1480:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1480'>pandas/core/arrays/datetimelike.py:1480:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1699'>pandas/core/arrays/datetimelike.py:1699:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1699'>pandas/core/arrays/datetimelike.py:1699:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2131'>pandas/core/arrays/datetimelike.py:2131:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2131'>pandas/core/arrays/datetimelike.py:2131:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2153'>pandas/core/arrays/datetimelike.py:2153:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2153'>pandas/core/arrays/datetimelike.py:2153:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L456'>pandas/core/arrays/datetimelike.py:456:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L456'>pandas/core/arrays/datetimelike.py:456:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L778'>pandas/core/arrays/datetimelike.py:778:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L778'>pandas/core/arrays/datetimelike.py:778:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L979'>pandas/core/arrays/datetimelike.py:979:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L979'>pandas/core/arrays/datetimelike.py:979:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7827'>pandas/core/frame.py:7827:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7827'>pandas/core/frame.py:7827:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7840'>pandas/core/frame.py:7840:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7840'>pandas/core/frame.py:7840:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8183'>pandas/core/frame.py:8183:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8183'>pandas/core/frame.py:8183:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8195'>pandas/core/frame.py:8195:21:</a> PLW0642 Confusing assignment to `self` argument in instance method
... 53 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0642 | 104 | 52 | 52 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-08-13 12:25_

The vast majority of the ecosystem hit here are PLR6104. I'll split that one out into its own PR so that it can be considered in isolation.

(Edit: this has now been done; the PR for that is https://github.com/astral-sh/ruff/pull/12858)

---

_Renamed from "[Ruff 0.6] Stabilise 10 pylint rules" to "[Ruff 0.6] Stabilise 9 pylint rules" by @AlexWaygood on 2024-08-13 12:29_

---

_Comment by @AlexWaygood on 2024-08-13 12:49_

There's a lot of `PLW0642` hits on `pandas`, but I think they're all true positives. `pandas` apparently does a lot of `self = foo` assignments, but I think these could easily be replaced with other variable names, which would improve the readability of the code.

---

_Comment by @MichaReiser on 2024-08-13 13:06_

LGTM. My only feedback for `PLW0642` is that I find the message: *Invalid assignment to* somewhat confusing. I'm not familiar enough with Python but the assignment itself isn't *invalid*. It's just not recommended to change the `cls` or `self` variable but I don't think it breaks anything, it's just unexpected.

---

_@MichaReiser approved on 2024-08-13 13:07_

---

_Comment by @AlexWaygood on 2024-08-13 13:10_

> LGTM. My only feedback for `PLW0642` is that I find the message: _Invalid assignment to_ somewhat confusing. I'm not familiar enough with Python but the assignment itself isn't _invalid_. It's just not recommended to change the `cls` or `self` variable but I don't think it breaks anything, it's just unexpected.

Yeah, that's a great point. I think I'll change it to "confusing". It's not an _invalid_ assignment; you could just write the code in a clearer/less confusing way by using a different variable name.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_or_cls_assignment.rs`:63 on 2024-08-13 13:37_

This is clever ;)

---

_Merged by @AlexWaygood on 2024-08-13 13:38_

---

_Closed by @AlexWaygood on 2024-08-13 13:38_

---

_Branch deleted on 2024-08-13 13:38_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_or_cls_assignment.rs`:57 on 2024-08-13 13:39_

Another option could be "Re-assigned `{}` argument in {method_type} method"

---

_@dhruvmanila reviewed on 2024-08-13 13:39_

---

_@AlexWaygood reviewed on 2024-08-13 13:41_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/self_or_cls_assignment.rs`:57 on 2024-08-13 13:41_

Oh, that's better. Much more descriptive. I'll make another PR 😄

---
