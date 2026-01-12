```yaml
number: 10526
title: "[`refurb`] Support `itemgetter` in `reimplemented-operator` (`FURB118`)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: latyshev/furb118-itemgetter
created_at: 2024-03-22T16:12:39Z
updated_at: 2025-06-10T20:46:45Z
url: https://github.com/astral-sh/ruff/pull/10526
synced_at: 2026-01-12T15:55:32Z
```

# [`refurb`] Support `itemgetter` in `reimplemented-operator` (`FURB118`)

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Lint about function like expressions which are equivalent to `operator.itemgetter`. 
See: https://github.com/astral-sh/ruff/issues/1348#issuecomment-1909421747

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-22 16:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+95 -0 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L135'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:135:28:</a> FURB118 [*] Use `operator.itemgetter("SnapshotCreateTime")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/airflow/providers/weaviate/hooks/weaviate.py#L842'>airflow/providers/weaviate/hooks/weaviate.py:842:23:</a> FURB118 [*] Use `operator.itemgetter(uuid_column)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/airflow/providers_manager.py#L1297'>airflow/providers_manager.py:1297:59:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/airflow/providers_manager.py#L1301'>airflow/providers_manager.py:1301:59:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/dev/stats/get_important_pr_candidates.py#L387'>dev/stats/get_important_pr_candidates.py:387:69:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/docs/conf.py#L578'>docs/conf.py:578:26:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/docs/exts/docs_build/fetch_inventories.py#L150'>docs/exts/docs_build/fetch_inventories.py:150:33:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/docs/exts/operators_and_hooks_ref.py#L219'>docs/exts/operators_and_hooks_ref.py:219:41:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/docs/exts/operators_and_hooks_ref.py#L316'>docs/exts/operators_and_hooks_ref.py:316:52:</a> FURB118 [*] Use `operator.itemgetter("object_path")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/docs/exts/operators_and_hooks_ref.py#L324'>docs/exts/operators_and_hooks_ref.py:324:41:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/docs/exts/providers_extensions.py#L169'>docs/exts/providers_extensions.py:169:82:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/tests/api_experimental/client/test_local_client.py#L198'>tests/api_experimental/client/test_local_client.py:198:53:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/tests/operators/test_python.py#L1293'>tests/operators/test_python.py:1293:9:</a> FURB118 Use `operator.itemgetter("ds")` instead of defining a function
+ <a href='https://github.com/apache/airflow/blob/042c2acaed7c01933d37c2f8434640ce140a4b27/tests/operators/test_python.py#L808'>tests/operators/test_python.py:808:9:</a> FURB118 Use `operator.itemgetter("ds")` instead of defining a function
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/les_mis.py#L18'>examples/topics/categorical/les_mis.py:18:61:</a> FURB118 [*] Use `operator.itemgetter('group')` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1945'>src/bokeh/palettes.py:1945:56:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L461'>src/bokeh/resources.py:461:73:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/browsers.py#L76'>src/bokeh/sampledata/browsers.py:76:35:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L486'>src/bokeh/util/compiler.py:486:49:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L577'>src/bokeh/util/compiler.py:577:35:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L578'>src/bokeh/util/compiler.py:578:35:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/api/v1/schemas.py#L1989'>rotkehlchen/api/v1/schemas.py:1989:52:</a> FURB118 [*] Use `operator.itemgetter('address')` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/api/v1/schemas.py#L2016'>rotkehlchen/api/v1/schemas.py:2016:52:</a> FURB118 [*] Use `operator.itemgetter('address')` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/chain/evm/decoding/decoder.py#L384'>rotkehlchen/chain/evm/decoding/decoder.py:384:24:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/db/search_assets.py#L123'>rotkehlchen/db/search_assets.py:123:79:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/exchanges/binance.py#L1187'>rotkehlchen/exchanges/binance.py:1187:31:</a> FURB118 [*] Use `operator.itemgetter('time')` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/exchanges/bitfinex.py#L385'>rotkehlchen/exchanges/bitfinex.py:385:34:</a> FURB118 [*] Use `operator.itemgetter(id_index)` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/exchanges/poloniex.py#L189'>rotkehlchen/exchanges/poloniex.py:189:56:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/externalapis/etherscan.py#L74'>rotkehlchen/externalapis/etherscan.py:74:46:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/globaldb/cache.py#L233'>rotkehlchen/globaldb/cache.py:233:27:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/rotki/rotki/blob/228dfd77daa00c2647d71995b4b73af3165ffdc8/rotkehlchen/tests/api/test_manually_tracked_balances.py#L76'>rotkehlchen/tests/api/test_manually_tracked_balances.py:76:32:</a> FURB118 [*] Use `operator.itemgetter('label')` instead of defining a lambda
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+45 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/analytics/views/stats.py#L491'>analytics/views/stats.py:491:82:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/corporate/views/support.py#L485'>corporate/views/support.py:485:39:</a> FURB118 [*] Use `operator.itemgetter("display_order")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/tools/setup/emoji/emoji_setup_utils.py#L105'>tools/setup/emoji/emoji_setup_utils.py:105:1:</a> FURB118 Use `operator.itemgetter("has_img_google")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/actions/default_streams.py#L194'>zerver/actions/default_streams.py:194:62:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/actions/message_send.py#L436'>zerver/actions/message_send.py:436:9:</a> FURB118 [*] Use `operator.itemgetter("enable_online_push_notifications")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/actions/message_send.py#L455'>zerver/actions/message_send.py:455:9:</a> FURB118 [*] Use `operator.itemgetter("long_term_idle")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/context_processors.py#L285'>zerver/context_processors.py:285:68:</a> FURB118 [*] Use `operator.itemgetter("display_order")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/default_streams.py#L32'>zerver/lib/default_streams.py:32:37:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/display_recipient.py#L109'>zerver/lib/display_recipient.py:109:5:</a> FURB118 Use `operator.itemgetter("recipient_id")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/display_recipient.py#L112'>zerver/lib/display_recipient.py:112:5:</a> FURB118 Use `operator.itemgetter("name")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/display_recipient.py#L143'>zerver/lib/display_recipient.py:143:24:</a> FURB118 [*] Use `operator.itemgetter(0)` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/display_recipient.py#L144'>zerver/lib/display_recipient.py:144:16:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/display_recipient.py#L67'>zerver/lib/display_recipient.py:67:1:</a> FURB118 Use `operator.itemgetter("id")` instead of defining a function
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/email_notifications.py#L651'>zerver/lib/email_notifications.py:651:43:</a> FURB118 [*] Use `operator.itemgetter(1)` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L1083'>zerver/lib/events.py:1083:44:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L1084'>zerver/lib/events.py:1084:48:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L1086'>zerver/lib/events.py:1086:43:</a> FURB118 [*] Use `operator.itemgetter("name")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L1438'>zerver/lib/events.py:1438:49:</a> FURB118 [*] Use `operator.itemgetter("id")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L1703'>zerver/lib/events.py:1703:60:</a> FURB118 [*] Use `operator.itemgetter("user_id")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L837'>zerver/lib/events.py:837:21:</a> FURB118 [*] Use `operator.itemgetter("scheduled_delivery_timestamp")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L853'>zerver/lib/events.py:853:33:</a> FURB118 [*] Use `operator.itemgetter("scheduled_delivery_timestamp")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/export.py#L2444'>zerver/lib/export.py:2444:46:</a> FURB118 [*] Use `operator.itemgetter("id")` instead of defining a lambda
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/export.py#L381'>zerver/lib/export.py:381:24:</a> FURB118 [*] Use `operator.itemgetter("id")` instead of defining a lambda
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/abstracts/forms.py#L564'>indico/modules/events/abstracts/forms.py:564:55:</a> FURB118 [*] Use `operator.itemgetter(slice(3, None))` instead of defining a lambda
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/abstracts/forms.py#L598'>indico/modules/events/abstracts/forms.py:598:55:</a> FURB118 [*] Use `operator.itemgetter(slice(3, None))` instead of defining a lambda
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/editing/schemas.py#L471'>indico/modules/events/editing/schemas.py:471:34:</a> FURB118 [*] Use `operator.itemgetter('source', 'code')` instead of defining a lambda
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 95 | 95 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-07 00:54_

---

_Label `rule` added by @charliermarsh on 2024-04-07 00:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-07 00:54_

---

_@charliermarsh approved on 2024-04-07 02:20_

Thanks!

---

_Renamed from "[refurb] Support `itemgetter` in `reimplemented_operator` (FURB118) lint" to "[`refurb`] Support `itemgetter` in `reimplemented-operator` (`FURB118`)" by @charliermarsh on 2024-04-07 02:20_

---

_Label `preview` added by @charliermarsh on 2024-04-07 02:20_

---

_Merged by @charliermarsh on 2024-04-07 02:31_

---

_Closed by @charliermarsh on 2024-04-07 02:32_

---

_Comment by @ThiefMaster on 2024-04-17 15:01_

```python
-return groupby(list(self.iter_choices()), key=lambda x: x[3:])
+return groupby(list(self.iter_choices()), key=operator.itemgetter(slice(3, None)))
```

Not a fan of this change tbh. While it may have slightly better performance, it's much less readable...

It's also much longer, but that's mainly because it prefers `import operator` instead of `from operator import ...`, which is probably unrelated to your PR.

That aside I really like having a lint rule that prefers itemgetter over lambdas. Just not for slices...

---

_Branch deleted on 2025-06-10 20:46_

---
