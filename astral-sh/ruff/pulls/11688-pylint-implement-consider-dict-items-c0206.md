```yaml
number: 11688
title: "[`pylint`] Implement `consider-dict-items` (`C0206`)"
type: pull_request
state: merged
author: max-muoto
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: implement-consider-dict-items
created_at: 2024-06-02T04:53:40Z
updated_at: 2024-06-09T00:26:30Z
url: https://github.com/astral-sh/ruff/pull/11688
synced_at: 2026-01-12T15:55:38Z
```

# [`pylint`] Implement `consider-dict-items` (`C0206`)

---

_@max-muoto_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR implements the [consider dict items](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/consider-using-dict-items.html) rule from Pylint. Enabling this rule flags:

```python
ORCHESTRA = {
    "violin": "strings",
    "oboe": "woodwind",
    "tuba": "brass",
    "gong": "percussion",
}


for instrument in ORCHESTRA: 
    print(f"{instrument}: {ORCHESTRA[instrument]}")

for instrument in ORCHESTRA.keys(): 
    print(f"{instrument}: {ORCHESTRA[instrument]}")

for instrument in (inline_dict := {"foo": "bar"}): 
    print(f"{instrument}: {inline_dict[instrument]}")
```

For not using `items()` to extract the value out of the dict. We ignore the case of an assignment, as you can't modify the underlying representation with the value in the list of tuples returned.
 

## Test Plan

<!-- How was it tested? -->

`cargo test`.


---

_Renamed from "implement-consider-dict-items" to "[pylint] Implement Consider Dict Items (C0206)" by @max-muoto on 2024-06-02 16:07_

---

_Marked ready for review by @max-muoto on 2024-06-02 16:34_

---

_Comment by @github-actions[bot] on 2024-06-02 16:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+35 -0 violations, +0 -0 fixes in 8 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/particles/_special_particles.py#L260'>src/plasmapy/particles/_special_particles.py:260:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/particles/decorators.py#L677'>src/plasmapy/particles/decorators.py:677:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/particles/ionization_state_collection.py#L477'>src/plasmapy/particles/ionization_state_collection.py:477:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/plasma/grids.py#L766'>src/plasmapy/plasma/grids.py:766:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/45bf7b972121828483f930acac60aa0751e2716f/airflow/models/taskinstance.py#L3917'>airflow/models/taskinstance.py:3917:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/45bf7b972121828483f930acac60aa0751e2716f/airflow/www/views.py#L2287'>airflow/www/views.py:2287:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/45bf7b972121828483f930acac60aa0751e2716f/docs/conf.py#L471'>docs/conf.py:471:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/45bf7b972121828483f930acac60aa0751e2716f/tests/providers/amazon/aws/executors/batch/test_batch_executor.py#L677'>tests/providers/amazon/aws/executors/batch/test_batch_executor.py:677:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/45bf7b972121828483f930acac60aa0751e2716f/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L1234'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:1234:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/45bf7b972121828483f930acac60aa0751e2716f/tests/serialization/test_dag_serialization.py#L550'>tests/serialization/test_dag_serialization.py:550:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/b0ec71b1398432475fccef0bf11b11de1fc43e5b/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/e95f5a5294ff28588c14a2f3f5aae19d9210a47e/pymilvus/bulk_writer/buffer.py#L91'>pymilvus/bulk_writer/buffer.py:91:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0e90f66a88e0d4b16b143f1e563ec5fa3565469b/pandas/io/stata.py#L2131'>pandas/io/stata.py:2131:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/f174dfc9e4751f2bfd7c0e7fec73a97c8e67bdb6/rotkehlchen/chain/evm/decoding/hop/balances.py#L66'>rotkehlchen/chain/evm/decoding/hop/balances.py:66:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/rotki/rotki/blob/f174dfc9e4751f2bfd7c0e7fec73a97c8e67bdb6/rotkehlchen/data_import/importers/cryptocom.py#L446'>rotkehlchen/data_import/importers/cryptocom.py:446:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/rotki/rotki/blob/f174dfc9e4751f2bfd7c0e7fec73a97c8e67bdb6/rotkehlchen/tests/data_migrations/test_migrations.py#L150'>rotkehlchen/tests/data_migrations/test_migrations.py:150:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/corporate/views/installation_activity.py#L81'>corporate/views/installation_activity.py:81:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/tools/setup/emoji/emoji_setup_utils.py#L90'>tools/setup/emoji/emoji_setup_utils.py:90:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/actions/realm_settings.py#L237'>zerver/actions/realm_settings.py:237:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/mattermost.py#L135'>zerver/data_import/mattermost.py:135:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/mattermost.py#L165'>zerver/data_import/mattermost.py:165:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/mattermost.py#L849'>zerver/data_import/mattermost.py:849:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/rocketchat.py#L166'>zerver/data_import/rocketchat.py:166:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/rocketchat.py#L214'>zerver/data_import/rocketchat.py:214:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/rocketchat.py#L249'>zerver/data_import/rocketchat.py:249:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/data_import/rocketchat.py#L65'>zerver/data_import/rocketchat.py:65:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/lib/cache.py#L254'>zerver/lib/cache.py:254:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/lib/markdown/api_return_values_table_generator.py#L114'>zerver/lib/markdown/api_return_values_table_generator.py:114:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/migrations/0382_create_role_based_system_groups.py#L59'>zerver/migrations/0382_create_role_based_system_groups.py:59:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/migrations/0403_create_role_based_groups_for_internal_realms.py#L68'>zerver/migrations/0403_create_role_based_groups_for_internal_realms.py:68:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/tests/test_events.py#L451'>zerver/tests/test_events.py:451:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/tests/test_users.py#L723'>zerver/tests/test_users.py:723:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/53c1c4d98f16887d8a624cb5fbb07f990fb0dcfd/zerver/worker/missedmessage_emails.py#L231'>zerver/worker/missedmessage_emails.py:231:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0206 | 35 | 35 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @max-muoto on 2024-06-04 17:23_

@charliermarsh Any chance you might be able to take a look? 

---

_Comment by @charliermarsh on 2024-06-06 03:58_

Interesting, I swear we have a rule for this... But maybe we don't?  I see:

- https://docs.astral.sh/ruff/rules/dict-iter-missing-items/ -- detects `for city, population in data` (missing `.items()`).
- https://docs.astral.sh/ruff/rules/unnecessary-dict-index-lookup/ -- detects `FRUITS[fruit_name]` in a `for fruit_name, fruit_count in FRUITS.items()` loop.


---

_Label `rule` added by @charliermarsh on 2024-06-06 03:58_

---

_Label `preview` added by @charliermarsh on 2024-06-06 03:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-06 03:59_

---

_Comment by @max-muoto on 2024-06-06 04:00_

> Interesting, I swear we have a rule for this... But maybe we don't?  I see:
> 
> 
> 
> - https://docs.astral.sh/ruff/rules/dict-iter-missing-items/ -- detects `for city, population in data` (missing `.items()`).
> 
> - https://docs.astral.sh/ruff/rules/unnecessary-dict-index-lookup/ -- detects `FRUITS[fruit_name]` in a `for fruit_name, fruit_count in FRUITS.items()` loop.
> 
> 

Yeah, a little different since this one doesn't rely on you using items before.

---

_Renamed from "[pylint] Implement Consider Dict Items (C0206)" to "[`pylint`] Implement `dict-iter-missing-items` (`C0206`)" by @charliermarsh on 2024-06-06 04:25_

---

_@charliermarsh approved on 2024-06-06 04:25_

Thanks!

---

_Merged by @charliermarsh on 2024-06-06 04:28_

---

_Closed by @charliermarsh on 2024-06-06 04:28_

---

_Comment by @charliermarsh on 2024-06-06 04:28_

I reviewed the ecosystem checks and they look right to me.

---

_Comment by @max-muoto on 2024-06-06 04:29_

> Thanks!

Thanks for the clean up! Still getting used to writing more Rust code haha

---

_Comment by @charliermarsh on 2024-06-06 04:30_

No prob! One thing I did in a few places was change `x.is_none()` followed by `x.unwrap()` to `if let Some(x) = x`, which couples "checking if the condition is met" with "getting access to the value".

---

_@dhruvmanila reviewed on 2024-06-06 04:52_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0206_dict_index_missing_items.py.snap`:11 on 2024-06-06 04:52_

It would be quite useful to have a infrastructure to provide additional details here similar to Biome. That would avoid highlighting the entire `for` statement and instead we could highlight individual pieces of code with details.

---

_Renamed from "[`pylint`] Implement `dict-iter-missing-items` (`C0206`)" to "[`pylint`] Implement `consider-dict-items` (`C0206`)" by @max-muoto on 2024-06-09 00:26_

---
