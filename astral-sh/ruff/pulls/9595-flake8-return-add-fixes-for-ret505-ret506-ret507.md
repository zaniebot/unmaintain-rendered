```yaml
number: 9595
title: "[`flake8-return`] - Add fixes for (`RET505`, `RET506`, `RET507`, `RET508`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: add-fixes-to-RET505,506,507,508
created_at: 2024-01-21T07:11:43Z
updated_at: 2024-01-25T07:28:44Z
url: https://github.com/astral-sh/ruff/pull/9595
synced_at: 2026-01-10T22:57:09Z
```

# [`flake8-return`] - Add fixes for (`RET505`, `RET506`, `RET507`, `RET508`)

---

_Pull request opened by @diceroll123 on 2024-01-21 07:11_

## Summary

add fix for RET505, RET506, RET507, RET508

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-01-21 07:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-fixes-to-RET505,506,507,508)

### Merging #9595 will **degrade performances by 22.56%**

<sub>Comparing <code>diceroll123:add-fixes-to-RET505,506,507,508</code> (c89ce6f) with <code>main</code> (a42600e)</sub>



### Summary

`❌ 2 (👁 2)` regressions
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `diceroll123:add-fixes-to-RET505,506,507,508` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-with-preview-rules[pydantic/types.py]` | 56.8 ms | 64.5 ms | -11.94% |
| 👁 | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 26.6 ms | 34.4 ms | -22.56% |


---

_Converted to draft by @diceroll123 on 2024-01-21 07:19_

---

_Comment by @github-actions[bot] on 2024-01-21 07:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +1884 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +1118 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/__init__.py#L95'>airflow/__init__.py:95:5:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/__init__.py#L95'>airflow/__init__.py:95:5:</a> RET506 [*] Unnecessary `elif` after `raise` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/config_endpoint.py#L119'>airflow/api_connexion/endpoints/config_endpoint.py:119:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/config_endpoint.py#L119'>airflow/api_connexion/endpoints/config_endpoint.py:119:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/config_endpoint.py#L85'>airflow/api_connexion/endpoints/config_endpoint.py:85:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/config_endpoint.py#L85'>airflow/api_connexion/endpoints/config_endpoint.py:85:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/dag_run_endpoint.py#L429'>airflow/api_connexion/endpoints/dag_run_endpoint.py:429:5:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/dag_run_endpoint.py#L429'>airflow/api_connexion/endpoints/dag_run_endpoint.py:429:5:</a> RET505 [*] Unnecessary `else` after `return` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L47'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:47:9:</a> RET506 Unnecessary `else` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L47'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:47:9:</a> RET506 [*] Unnecessary `else` after `raise` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/schemas/common_schema.py#L122'>airflow/api_connexion/schemas/common_schema.py:122:9:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/api_connexion/schemas/common_schema.py#L122'>airflow/api_connexion/schemas/common_schema.py:122:9:</a> RET505 [*] Unnecessary `elif` after `return` statement
... 815 additional changes omitted for rule RET505
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/cli/commands/dag_command.py#L237'>airflow/cli/commands/dag_command.py:237:5:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/cli/commands/dag_command.py#L237'>airflow/cli/commands/dag_command.py:237:5:</a> RET506 [*] Unnecessary `elif` after `raise` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/cli/commands/dag_command.py#L258'>airflow/cli/commands/dag_command.py:258:5:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/cli/commands/dag_command.py#L258'>airflow/cli/commands/dag_command.py:258:5:</a> RET506 [*] Unnecessary `elif` after `raise` statement
... 267 additional changes omitted for rule RET506
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/dag_processing/manager.py#L567'>airflow/dag_processing/manager.py:567:17:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/dag_processing/manager.py#L567'>airflow/dag_processing/manager.py:567:17:</a> RET508 [*] Unnecessary `elif` after `break` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/amazon/aws/hooks/sagemaker.py#L1164'>airflow/providers/amazon/aws/hooks/sagemaker.py:1164:21:</a> RET508 Unnecessary `else` after `break` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/amazon/aws/hooks/sagemaker.py#L1164'>airflow/providers/amazon/aws/hooks/sagemaker.py:1164:21:</a> RET508 [*] Unnecessary `else` after `break` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/amazon/aws/secrets/secrets_manager.py#L232'>airflow/providers/amazon/aws/secrets/secrets_manager.py:232:13:</a> RET507 Unnecessary `elif` after `continue` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/amazon/aws/secrets/secrets_manager.py#L232'>airflow/providers/amazon/aws/secrets/secrets_manager.py:232:13:</a> RET507 [*] Unnecessary `elif` after `continue` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L625'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:625:13:</a> RET507 Unnecessary `elif` after `continue` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L625'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:625:13:</a> RET507 [*] Unnecessary `elif` after `continue` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/snowflake/operators/snowflake.py#L523'>airflow/providers/snowflake/operators/snowflake.py:523:13:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/snowflake/operators/snowflake.py#L523'>airflow/providers/snowflake/operators/snowflake.py:523:13:</a> RET508 [*] Unnecessary `elif` after `break` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/serialization/serialized_objects.py#L982'>airflow/serialization/serialized_objects.py:982:13:</a> RET507 Unnecessary `elif` after `continue` statement
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/serialization/serialized_objects.py#L982'>airflow/serialization/serialized_objects.py:982:13:</a> RET507 [*] Unnecessary `elif` after `continue` statement
- <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/utils/helpers.py#L349'>airflow/utils/helpers.py:349:13:</a> RET507 Unnecessary `elif` after `continue` statement
... 1089 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +306 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/examples/server/app/clustering/main.py#L88'>examples/server/app/clustering/main.py:88:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/examples/server/app/clustering/main.py#L88'>examples/server/app/clustering/main.py:88:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/release/checks.py#L105'>release/checks.py:105:9:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/release/checks.py#L105'>release/checks.py:105:9:</a> RET505 [*] Unnecessary `else` after `return` statement
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/release/checks.py#L119'>release/checks.py:119:9:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/release/checks.py#L119'>release/checks.py:119:9:</a> RET505 [*] Unnecessary `else` after `return` statement
... 239 additional changes omitted for rule RET505
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/application/application.py#L169'>src/bokeh/application/application.py:169:9:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/application/application.py#L169'>src/bokeh/application/application.py:169:9:</a> RET506 [*] Unnecessary `elif` after `raise` statement
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/application/handlers/directory.py#L154'>src/bokeh/application/handlers/directory.py:154:9:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/application/handlers/directory.py#L154'>src/bokeh/application/handlers/directory.py:154:9:</a> RET506 [*] Unnecessary `elif` after `raise` statement
... 296 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +38 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L383'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:383:13:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L383'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:383:13:</a> RET508 [*] Unnecessary `elif` after `break` statement
- <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L501'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:501:13:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L501'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:501:13:</a> RET508 [*] Unnecessary `elif` after `break` statement
- <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/CheckpointFirewall/Scripts/CheckpointFWBackupStatus/CheckpointFWBackupStatus.py#L30'>Packs/CheckpointFirewall/Scripts/CheckpointFWBackupStatus/CheckpointFWBackupStatus.py:30:21:</a> RET508 Unnecessary `else` after `break` statement
+ <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/CheckpointFirewall/Scripts/CheckpointFWBackupStatus/CheckpointFWBackupStatus.py#L30'>Packs/CheckpointFirewall/Scripts/CheckpointFWBackupStatus/CheckpointFWBackupStatus.py:30:21:</a> RET508 [*] Unnecessary `else` after `break` statement
- <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/CheckpointFirewall/Scripts/CheckpointFWCreateBackup/CheckpointFWCreateBackup.py#L30'>Packs/CheckpointFirewall/Scripts/CheckpointFWCreateBackup/CheckpointFWCreateBackup.py:30:17:</a> RET508 Unnecessary `else` after `break` statement
+ <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/CheckpointFirewall/Scripts/CheckpointFWCreateBackup/CheckpointFWCreateBackup.py#L30'>Packs/CheckpointFirewall/Scripts/CheckpointFWCreateBackup/CheckpointFWCreateBackup.py:30:17:</a> RET508 [*] Unnecessary `else` after `break` statement
- <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/DeveloperTools/Scripts/WaitAndCompleteTask/WaitAndCompleteTask.py#L117'>Packs/DeveloperTools/Scripts/WaitAndCompleteTask/WaitAndCompleteTask.py:117:9:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/DeveloperTools/Scripts/WaitAndCompleteTask/WaitAndCompleteTask.py#L117'>Packs/DeveloperTools/Scripts/WaitAndCompleteTask/WaitAndCompleteTask.py:117:9:</a> RET508 [*] Unnecessary `elif` after `break` statement
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +422 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/analytics/views/activity_common.py#L81'>analytics/views/activity_common.py:81:5:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/analytics/views/activity_common.py#L81'>analytics/views/activity_common.py:81:5:</a> RET505 [*] Unnecessary `else` after `return` statement
- <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/analytics/views/activity_common.py#L88'>analytics/views/activity_common.py:88:5:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/analytics/views/activity_common.py#L88'>analytics/views/activity_common.py:88:5:</a> RET505 [*] Unnecessary `else` after `return` statement
- <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/analytics/views/stats.py#L505'>analytics/views/stats.py:505:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/analytics/views/stats.py#L505'>analytics/views/stats.py:505:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
... 363 additional changes omitted for rule RET505
- <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/corporate/lib/stripe.py#L4293'>corporate/lib/stripe.py:4293:9:</a> RET506 Unnecessary `else` after `raise` statement
+ <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/corporate/lib/stripe.py#L4293'>corporate/lib/stripe.py:4293:9:</a> RET506 [*] Unnecessary `else` after `raise` statement
- <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/corporate/tests/test_stripe.py#L460'>corporate/tests/test_stripe.py:460:13:</a> RET506 Unnecessary `else` after `raise` statement
+ <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/corporate/tests/test_stripe.py#L460'>corporate/tests/test_stripe.py:460:13:</a> RET506 [*] Unnecessary `else` after `raise` statement
- <a href='https://github.com/zulip/zulip/blob/866a6106644adb9ff16820578490716f73481802/corporate/views/remote_billing_page.py#L104'>corporate/views/remote_billing_page.py:104:9:</a> RET506 Unnecessary `else` after `raise` statement
... 411 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RET505 | 1434 | 0 | 0 | 1434 | 0 |
| RET506 | 372 | 0 | 0 | 372 | 0 |
| RET508 | 50 | 0 | 0 | 50 | 0 |
| RET507 | 28 | 0 | 0 | 28 | 0 |

</p>
</details>




---

_Marked ready for review by @diceroll123 on 2024-01-21 08:21_

---

_Renamed from "[`flake8-return`] - add fix for RET505, RET506, RET507, RET508" to "[`flake8-return`] - Add fixes for (`RET505`, `RET506`, `RET507`, `RET508`)" by @diceroll123 on 2024-01-21 17:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:740 on 2024-01-24 16:12_

I don't expect this to exist in practice but assuming that the `else` header ends on the same line as the `else` keyword is not always true. For example if you have.

```python
if True:
    return

else\
    :\
    # comment
    pass
```

it then keeps `:\\n   # comment` where it should not. We could use `SimpleTokenizer` to detect this but I don't think it's worth doing. 

However, I think it would be good to add a few more tests that demonstrate how the fix deals with comments in different positions. 

```python
if True:
    return
else:
    # comment
    pass
```

```python
if True:
    return
else: # comment
    pass
```


---

_@MichaReiser approved on 2024-01-24 16:18_

Thanks, that's a very useful fix. 

I looked at the performance regression and it's caused by `adjust_indentation` calling into libCST. The regression is fine because it only applies to files with that specific violation. It's not that it introduces a slow down for projects that have no violations. 

There's one edge case that the code isn't handling correctly but I don't think it's worth addressing. But feel free to give it a try if you want. 

Would you mind adding a few more tests that demonstrate how the rule handles comments? We should then be good to merge this. 

---

_Label `rule` added by @MichaReiser on 2024-01-24 16:18_

---

_@diceroll123 reviewed on 2024-01-25 00:19_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:740 on 2024-01-25 00:19_

I've tweaked it to work with that line continuation that I didn't account for, and the code now keeps all comments after the else's `:` 😄 

---

_@MichaReiser approved on 2024-01-25 07:28_

Nice!

---

_Merged by @MichaReiser on 2024-01-25 07:28_

---

_Closed by @MichaReiser on 2024-01-25 07:28_

---

_Label `autofix` added by @MichaReiser on 2024-01-25 07:28_

---

_Label `rule` removed by @MichaReiser on 2024-01-25 07:28_

---
