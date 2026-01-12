```yaml
number: 14564
title: "[`ruff`] `d.update({k: v})` (`RUF042`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - needs-decision
assignees: []
base: main
head: RUF042
created_at: 2024-11-24T05:05:32Z
updated_at: 2024-12-08T00:37:02Z
url: https://github.com/astral-sh/ruff/pull/14564
synced_at: 2026-01-12T15:55:48Z
```

# [`ruff`] `d.update({k: v})` (`RUF042`)

---

_@InSyncWithFoo_

## Summary

Resolves #13533.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-24 06:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+71 -0 violations, +0 -0 fixes in 9 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/twilio.py#L58'>rasa/core/channels/twilio.py:58:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L131'>rasa/core/migrate.py:131:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L143'>rasa/core/migrate.py:143:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L177'>rasa/core/migrate.py:177:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L179'>rasa/core/migrate.py:179:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L181'>rasa/core/migrate.py:181:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L185'>rasa/core/migrate.py:185:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L393'>rasa/shared/core/domain.py:393:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L421'>rasa/shared/core/domain.py:421:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L426'>rasa/shared/core/domain.py:426:13:</a> RUF042 [*] `dict.update` with single dictionary argument
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+34 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/abstractoperator.py#L369'>airflow/models/abstractoperator.py:369:17:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/serialization/serialized_objects.py#L1279'>airflow/serialization/serialized_objects.py:1279:25:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/serialization/serialized_objects.py#L1549'>airflow/serialization/serialized_objects.py:1549:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/utils/file.py#L214'>airflow/utils/file.py:214:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/stats/get_important_pr_candidates.py#L371'>dev/stats/get_important_pr_candidates.py:371:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/helm_tests/other/test_keda.py#L140'>helm_tests/other/test_keda.py:140:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/amazon/aws/hooks/sagemaker.py#L1285'>providers/src/airflow/providers/amazon/aws/hooks/sagemaker.py:1285:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/amazon/aws/hooks/sagemaker.py#L1289'>providers/src/airflow/providers/amazon/aws/hooks/sagemaker.py:1289:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L328'>providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:328:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L329'>providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:329:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L330'>providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:330:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L333'>providers/src/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:333:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/marketing_platform/hooks/search_ads.py#L125'>providers/src/airflow/providers/google/marketing_platform/hooks/search_ads.py:125:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/marketing_platform/hooks/search_ads.py#L127'>providers/src/airflow/providers/google/marketing_platform/hooks/search_ads.py:127:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/marketing_platform/hooks/search_ads.py#L195'>providers/src/airflow/providers/google/marketing_platform/hooks/search_ads.py:195:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/openlineage/plugins/adapter.py#L457'>providers/src/airflow/providers/openlineage/plugins/adapter.py:457:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/openlineage/plugins/adapter.py#L465'>providers/src/airflow/providers/openlineage/plugins/adapter.py:465:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/openlineage/plugins/adapter.py#L484'>providers/src/airflow/providers/openlineage/plugins/adapter.py:484:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/openlineage/plugins/adapter.py#L488'>providers/src/airflow/providers/openlineage/plugins/adapter.py:488:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/openlineage/plugins/adapter.py#L496'>providers/src/airflow/providers/openlineage/plugins/adapter.py:496:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/tests/amazon/aws/hooks/test_sagemaker.py#L183'>providers/tests/amazon/aws/hooks/test_sagemaker.py:183:1:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/tests/amazon/aws/hooks/test_sagemaker.py#L189'>providers/tests/amazon/aws/hooks/test_sagemaker.py:189:1:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/tests/amazon/aws/sensors/test_sagemaker_training.py#L41'>providers/tests/amazon/aws/sensors/test_sagemaker_training.py:41:1:</a> RUF042 [*] `dict.update` with single dictionary argument
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/0166db96632a5de1e8fcf807b2a0d64943cfb6a2/superset/sql_lab.py#L512'>superset/sql_lab.py:512:17:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/superset/blob/0166db96632a5de1e8fcf807b2a0d64943cfb6a2/superset/sql_lab.py#L538'>superset/sql_lab.py:538:17:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/superset/blob/0166db96632a5de1e8fcf807b2a0d64943cfb6a2/superset/views/core.py#L692'>superset/views/core.py:692:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/apache/superset/blob/0166db96632a5de1e8fcf807b2a0d64943cfb6a2/superset/views/utils.py#L227'>superset/views/utils.py:227:25:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/34495134958e89ef7c854d2e31967bb580ab5823/examples/orm/collection.py#L163'>examples/orm/collection.py:163:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/milvus-io/pymilvus/blob/34495134958e89ef7c854d2e31967bb580ab5823/examples/orm/partition.py#L98'>examples/orm/partition.py:98:9:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/c2d46b49aa8dff2624125941f600c1e7a7dd85e3/rotkehlchen/exchanges/bitstamp.py#L259'>rotkehlchen/exchanges/bitstamp.py:259:21:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/rotki/rotki/blob/c2d46b49aa8dff2624125941f600c1e7a7dd85e3/rotkehlchen/exchanges/bitstamp.py#L414'>rotkehlchen/exchanges/bitstamp.py:414:17:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/rotki/rotki/blob/c2d46b49aa8dff2624125941f600c1e7a7dd85e3/rotkehlchen/exchanges/poloniex.py#L189'>rotkehlchen/exchanges/poloniex.py:189:13:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/corporate/views/remote_billing_page.py#L529'>corporate/views/remote_billing_page.py:529:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/corporate/views/remote_billing_page.py#L538'>corporate/views/remote_billing_page.py:538:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/corporate/views/remote_billing_page.py#L548'>corporate/views/remote_billing_page.py:548:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/corporate/views/remote_billing_page.py#L552'>corporate/views/remote_billing_page.py:552:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/tests/test_markdown.py#L1829'>zerver/tests/test_markdown.py:1829:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/tests/test_markdown.py#L1867'>zerver/tests/test_markdown.py:1867:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/tests/test_markdown.py#L1909'>zerver/tests/test_markdown.py:1909:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/tests/test_markdown.py#L1945'>zerver/tests/test_markdown.py:1945:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/tests/test_markdown.py#L1981'>zerver/tests/test_markdown.py:1981:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/tests/test_markdown.py#L2011'>zerver/tests/test_markdown.py:2011:13:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/c5ee75d0f54cef0e7fa8e402989eef86f553ace3/indico/web/forms/widgets.py#L144'>indico/web/forms/widgets.py:144:9:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/263143935b8ed378d9fd72a717b1b16d54a7fef3/src/pdm/formats/uv.py#L219'>src/pdm/formats/uv.py:219:9:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/pdm-project/pdm/blob/263143935b8ed378d9fd72a717b1b16d54a7fef3/src/pdm/project/config.py#L42'>src/pdm/project/config.py:42:17:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/4e3ad4b94f5183a8c78cc0c6ee636c6a224dc14b/astropy/io/registry/core.py#L218'>astropy/io/registry/core.py:218:21:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/astropy/astropy/blob/4e3ad4b94f5183a8c78cc0c6ee636c6a224dc14b/astropy/samp/integrated_client.py#L400'>astropy/samp/integrated_client.py:400:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/astropy/astropy/blob/4e3ad4b94f5183a8c78cc0c6ee636c6a224dc14b/astropy/samp/integrated_client.py#L402'>astropy/samp/integrated_client.py:402:13:</a> RUF042 [*] `dict.update` with single dictionary argument
+ <a href='https://github.com/astropy/astropy/blob/4e3ad4b94f5183a8c78cc0c6ee636c6a224dc14b/astropy/visualization/wcsaxes/core.py#L320'>astropy/visualization/wcsaxes/core.py:320:13:</a> RUF042 [*] `dict.update` with single dictionary argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF042 | 71 | 71 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:22 on 2024-11-25 08:34_

I suggest removing the mention of performance as discussed in the issue. There's some evidance that directly assigning is faster, but it isn't the rule's primary motivation and it only matters if this code is in a hot loop. 

```suggestion
/// Calling `dict.update()` with a single-item dictionary
/// is equivalent to setting the item directly, which is simpler and more concise. 
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:43 on 2024-11-25 08:36_

It's unclear to me what *Literal dictionary* means in this context. 
```suggestion
        "`dict.update` with single dictionary argument".to_string()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:65 on 2024-11-25 08:36_

```suggestion
    let key_expr = locator.slice(key);
    let value_expr = locator.slice(value);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:63 on 2024-11-25 08:36_

```suggestion
    let dict_ref_expr = locator.slice(dict_ref);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:69 on 2024-11-25 08:36_

The fix should be marked as unsafe if the replacement range contains any comments.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:88 on 2024-11-25 08:38_

Nit

```suggestion
    let Expr::Attribute(ExprAttribute { value, attr, .. }) = &func else {
        return None;
    };
```

---

_@MichaReiser reviewed on 2024-11-25 08:40_

Thank you. 

I think we have to align on the rule's scope first. @AlexWaygood [mentioned](https://github.com/astral-sh/ruff/issues/13533#issuecomment-2396495345) that the rule could possibly be extended to cover more cases than just `update`. In that case, the rule should be named differently. 

---

_Label `needs-decision` added by @MichaReiser on 2024-11-25 08:40_

---

_@InSyncWithFoo reviewed on 2024-11-25 10:08_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/dict_update_single.rs`:88 on 2024-11-25 10:08_

I don't think this compiles. `func` is of type `&Box<Expr>`.

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-11-27 09:22_

---

_@AlexWaygood requested changes on 2024-11-27 13:39_

Yeah, as I mentioned in https://github.com/astral-sh/ruff/issues/13533#issuecomment-2396495345, I would reframe this rule. Rather than calling it `DictUpdateSingleItemDict`, I would call it `UnnecessaryIntermediateRepresentation`. Rather than saying in the docs that it "Checks for `dictionary.update({single: item})`", I would say that it "Checks for situations where collections are unnecessarily created and then immediately discarded". The advantage of this framing is that we can then expand the rule in future PRs so that it also flags statements such as the following, which are all really examples of the same antipattern, just in different ways. None of them are currently flagged by Ruff:

```py
lst = []
lst += ["value"]
lst += ["value 1", "value 2"]

s = set()
s |= {"value"}
s |= {"value 1", "value 2"}

d = {}
d |= {"key": "value"}
```

These can all be written more efficiently as follows:

```py
lst = []
lst.append("value")
lst.extend(("value 1", "value 2"))

s = set()
s.add("value")
s.update(("value 1", "value 2"))

d = {}
d["key"] = "value"
```

(For the `lst.extend()` and `s.update()` ones in that snippet: note that Python can evaluate constant tuples at compile time, which it cannot do for lists and sets.)

I think you should also add a `## Known issues` section to this rule's docs that says similar things to the `## Known issues` section of our `repeated-append` rule: https://docs.astral.sh/ruff/rules/repeated-append/#known-problems.

---

_Comment by @MichaReiser on 2024-12-03 07:38_

@InSyncWithFoo are you planning to address @AlexWaygood's feedback? I'd otherwise close this PR and mention it as a good starting point for anyone else interested in finishing it.

---

_Comment by @InSyncWithFoo on 2024-12-03 12:44_

@MichaReiser As a matter of fact, I do. I already have a half-working branch locally.

---

_Closed by @InSyncWithFoo on 2024-12-08 00:37_

---

_Branch deleted on 2024-12-08 00:37_

---
