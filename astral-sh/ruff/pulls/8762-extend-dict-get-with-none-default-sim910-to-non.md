```yaml
number: 8762
title: "Extend `dict-get-with-none-default` (`SIM910`) to non-literals"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/SIM9
created_at: 2023-11-18T23:51:22Z
updated_at: 2023-11-19T00:29:04Z
url: https://github.com/astral-sh/ruff/pull/8762
synced_at: 2026-01-10T23:40:55Z
```

# Extend `dict-get-with-none-default` (`SIM910`) to non-literals

---

_Pull request opened by @charliermarsh on 2023-11-18 23:51_

## Summary

Ensures that we can catch cases like:

```python
ages = {"Tom": 23, "Maria": 23, "Dog": 11}
age = ages.get("Cat", None)
```

Previously, the rule was somewhat useless, as it only checked for literal accesses.

Closes https://github.com/astral-sh/ruff/issues/8760.


---

_Label `bug` added by @charliermarsh on 2023-11-18 23:51_

---

_Comment by @github-actions[bot] on 2023-11-19 00:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/api/v1/schemas.py#L1582'>rotkehlchen/api/v1/schemas.py:1582:17:</a> SIM910 [*] Use `data.get('label')` instead of `data.get('label', None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/chain/balances.py#L174'>rotkehlchen/chain/balances.py:174:27:</a> SIM910 [*] Use `xpub_mappings.get(account)` instead of `xpub_mappings.get(account, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/chain/bitcoin/hdkey.py#L125'>rotkehlchen/chain/bitcoin/hdkey.py:125:14:</a> SIM910 [*] Use `VERSION_BYTES.get(prefix)` instead of `VERSION_BYTES.get(prefix, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/chain/ethereum/modules/aave/aave.py#L127'>rotkehlchen/chain/ethereum/modules/aave/aave.py:127:32:</a> SIM910 [*] Use `reserve_cache.get(reserve_address)` instead of `reserve_cache.get(reserve_address, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/chain/ethereum/modules/yearn/vaultsv2.py#L122'>rotkehlchen/chain/ethereum/modules/yearn/vaultsv2.py:122:27:</a> SIM910 [*] Use `roi_cache.get(vault_address)` instead of `roi_cache.get(vault_address, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/chain/ethereum/modules/yearn/vaultsv2.py#L123'>rotkehlchen/chain/ethereum/modules/yearn/vaultsv2.py:123:27:</a> SIM910 [*] Use `pps_cache.get(vault_address)` instead of `pps_cache.get(vault_address, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/exchanges/coinbase.py#L619'>rotkehlchen/exchanges/coinbase.py:619:27:</a> SIM910 [*] Use `raw_data.get('payout_at')` instead of `raw_data.get('payout_at', None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/exchanges/coinbase.py#L768'>rotkehlchen/exchanges/coinbase.py:768:27:</a> SIM910 [*] Use `raw_data.get('payout_at')` instead of `raw_data.get('payout_at', None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/exchanges/independentreserve.py#L136'>rotkehlchen/exchanges/independentreserve.py:136:13:</a> SIM910 [*] Use `IR_TO_WORLD.get(symbol)` instead of `IR_TO_WORLD.get(symbol, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/exchanges/utils.py#L32'>rotkehlchen/exchanges/utils.py:32:11:</a> SIM910 [*] Use `mapping.get(key)` instead of `mapping.get(key, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/rotkehlchen.py#L636'>rotkehlchen/rotkehlchen.py:636:25:</a> SIM910 [*] Use `xpub_mappings.get(xpub_entry)` instead of `xpub_mappings.get(xpub_entry, None)`
+ <a href='https://github.com/rotki/rotki/blob/7c08bd0997b043a95b9dde3fcf3c18c1f36f52ba/rotkehlchen/serialization/schemas.py#L216'>rotkehlchen/serialization/schemas.py:216:35:</a> SIM910 [*] Use `data.get('underlying_tokens')` instead of `data.get('underlying_tokens', None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+842 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/airflow/io/store/__init__.py#L134'>airflow/io/store/__init__.py:134:21:</a> SIM910 [*] Use `_STORE_CACHE.get(alias)` instead of `_STORE_CACHE.get(alias, None)`
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/airflow/providers/amazon/aws/hooks/glue_crawler.py#L126'>airflow/providers/amazon/aws/hooks/glue_crawler.py:126:32:</a> SIM910 [*] Use `crawler_tags.get(key)` instead of `crawler_tags.get(key, None)`
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/airflow/providers/apache/livy/operators/livy.py#L214'>airflow/providers/apache/livy/operators/livy.py:214:12:</a> SIM910 [*] Use `event.get("log_lines")` instead of `event.get("log_lines", None)`
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/tests/hooks/test_package_index.py#L70'>tests/hooks/test_package_index.py:70:24:</a> SIM910 [*] Use `testdata.get("host")` instead of `testdata.get("host", None)`
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/tests/hooks/test_package_index.py#L71'>tests/hooks/test_package_index.py:71:25:</a> SIM910 [*] Use `testdata.get("login")` instead of `testdata.get("login", None)`
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/tests/hooks/test_package_index.py#L72'>tests/hooks/test_package_index.py:72:28:</a> SIM910 [*] Use `testdata.get("password")` instead of `testdata.get("password", None)`
+ <a href='https://github.com/apache/airflow/blob/99534e47f330ce0efb96402629dda5b2a4f16e8f/tests/hooks/test_package_index.py#L73'>tests/hooks/test_package_index.py:73:35:</a> SIM910 [*] Use `testdata.get("expected_result")` instead of `testdata.get("expected_result", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/code_runner.py#L108'>src/bokeh/application/handlers/code_runner.py:108:25:</a> SIM910 [*] Use `d.get('__doc__')` instead of `d.get('__doc__', None)`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/multi_root_static_handler.py#L62'>src/bokeh/server/views/multi_root_static_handler.py:62:25:</a> SIM910 [*] Use `root.get(name)` instead of `root.get(name, None)`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokeh_palette.py#L113'>src/bokeh/sphinxext/bokeh_palette.py:113:9:</a> SIM910 [*] Use `_globals.get("palette")` instead of `_globals.get("palette", None)`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/util/compiler.py#L306'>src/bokeh/util/compiler.py:306:14:</a> SIM910 [*] Use `_bundle_cache.get(key)` instead of `_bundle_cache.get(key, None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+803 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Documentation/common_server_docs.py#L178'>Documentation/common_server_docs.py:178:32:</a> SIM910 [*] Use `PY_IRREGULAR_FUNCS.get(a)` instead of `PY_IRREGULAR_FUNCS.get(a, None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Documentation/common_server_docs.py#L185'>Documentation/common_server_docs.py:185:32:</a> SIM910 [*] Use `PY_IRREGULAR_FUNCS.get(a)` instead of `PY_IRREGULAR_FUNCS.get(a, None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py#L304'>Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py:304:19:</a> SIM910 [*] Use `args.get('instance_id')` instead of `args.get('instance_id', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py#L305'>Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py:305:16:</a> SIM910 [*] Use `args.get('port')` instead of `args.get('port', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py#L306'>Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py:306:16:</a> SIM910 [*] Use `args.get('protocol')` instead of `args.get('protocol', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py#L307'>Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py:307:17:</a> SIM910 [*] Use `args.get('public_ip')` instead of `args.get('public_ip', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py#L308'>Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py:308:19:</a> SIM910 [*] Use `args.get('assume_role')` instead of `args.get('assume_role', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py#L309'>Packs/AWS-Enrichment-Remediation/Scripts/AWSRecreateSG/AWSRecreateSG.py:309:14:</a> SIM910 [*] Use `args.get('region')` instead of `args.get('region', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L100'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:100:26:</a> SIM910 [*] Use `args.get('first_observed_at_start')` instead of `args.get('first_observed_at_start', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L101'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:101:24:</a> SIM910 [*] Use `args.get('first_observed_at_end')` instead of `args.get('first_observed_at_end', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L103'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:103:29:</a> SIM910 [*] Use `args.get('date_range_unit')` instead of `args.get('date_range_unit', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L109'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:109:26:</a> SIM910 [*] Use `args.get('last_observed_at_start')` instead of `args.get('last_observed_at_start', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L110'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:110:24:</a> SIM910 [*] Use `args.get('last_observed_at_end')` instead of `args.get('last_observed_at_end', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L112'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:112:29:</a> SIM910 [*] Use `args.get('date_range_unit')` instead of `args.get('date_range_unit', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L116'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:116:26:</a> SIM910 [*] Use `args.get('created_at_start')` instead of `args.get('created_at_start', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L117'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:117:24:</a> SIM910 [*] Use `args.get('created_at_end')` instead of `args.get('created_at_end', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L119'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:119:29:</a> SIM910 [*] Use `args.get('date_range_unit')` instead of `args.get('date_range_unit', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L123'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:123:26:</a> SIM910 [*] Use `args.get('updated_at_start')` instead of `args.get('updated_at_start', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L124'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:124:24:</a> SIM910 [*] Use `args.get('updated_at_end')` instead of `args.get('updated_at_end', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L126'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:126:29:</a> SIM910 [*] Use `args.get('date_range_unit')` instead of `args.get('date_range_unit', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L130'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:130:26:</a> SIM910 [*] Use `args.get('severity_label_value')` instead of `args.get('severity_label_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L131'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:131:31:</a> SIM910 [*] Use `args.get('severity_label_comparison')` instead of `args.get('severity_label_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L135'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:135:26:</a> SIM910 [*] Use `args.get('title_value')` instead of `args.get('title_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L136'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:136:31:</a> SIM910 [*] Use `args.get('title_comparison')` instead of `args.get('title_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L140'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:140:26:</a> SIM910 [*] Use `args.get('description_value')` instead of `args.get('description_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L141'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:141:31:</a> SIM910 [*] Use `args.get('description_comparison')` instead of `args.get('description_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L145'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:145:26:</a> SIM910 [*] Use `args.get('recommendation_text_value')` instead of `args.get('recommendation_text_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L146'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:146:31:</a> SIM910 [*] Use `args.get('recommendation_text_comparison')` instead of `args.get('recommendation_text_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L150'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:150:26:</a> SIM910 [*] Use `args.get('source_url_value')` instead of `args.get('source_url_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L151'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:151:31:</a> SIM910 [*] Use `args.get('source_url_comparison')` instead of `args.get('source_url_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L155'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:155:24:</a> SIM910 [*] Use `args.get('product_fields_key')` instead of `args.get('product_fields_key', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L156'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:156:26:</a> SIM910 [*] Use `args.get('product_fields_value')` instead of `args.get('product_fields_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L157'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:157:31:</a> SIM910 [*] Use `args.get('product_fields_comparison')` instead of `args.get('product_fields_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L161'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:161:26:</a> SIM910 [*] Use `args.get('product_name_value')` instead of `args.get('product_name_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L162'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:162:31:</a> SIM910 [*] Use `args.get('product_name_comparison')` instead of `args.get('product_name_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L166'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:166:26:</a> SIM910 [*] Use `args.get('company_name_value')` instead of `args.get('company_name_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L167'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:167:31:</a> SIM910 [*] Use `args.get('company_name_comparison')` instead of `args.get('company_name_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L171'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:171:24:</a> SIM910 [*] Use `args.get('user_defined_fields_key')` instead of `args.get('user_defined_fields_key', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L172'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:172:26:</a> SIM910 [*] Use `args.get('user_defined_fields_value')` instead of `args.get('user_defined_fields_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L173'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:173:31:</a> SIM910 [*] Use `args.get('user_defined_fields_comparison')` instead of `args.get('user_defined_fields_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L177'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:177:26:</a> SIM910 [*] Use `args.get('malware_name_value')` instead of `args.get('malware_name_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L178'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:178:31:</a> SIM910 [*] Use `args.get('malware_name_comparison')` instead of `args.get('malware_name_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L182'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:182:26:</a> SIM910 [*] Use `args.get('malware_type_value')` instead of `args.get('malware_type_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L183'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:183:31:</a> SIM910 [*] Use `args.get('malware_type_comparison')` instead of `args.get('malware_type_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L187'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:187:26:</a> SIM910 [*] Use `args.get('malware_path_value')` instead of `args.get('malware_path_value', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L188'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:188:31:</a> SIM910 [*] Use `args.get('malware_path_comparison')` instead of `args.get('malware_path_comparison', None)`
+ <a href='https://github.com/demisto/content/blob/f07891be7bf3c6ac6f0b3fb0eafb9802c9002af9/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L192'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py:192:26:</a> SIM910 [*] Use `args.get('malware_state_value')` instead of `args.get('malware_state_value', None)`
... 756 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/bedc7950b24c37809e36a585b7985d5aa5e3e458/ibis/backends/pandas/execution/temporal.py#L123'>ibis/backends/pandas/execution/temporal.py:123:11:</a> SIM910 [*] Use `OFFSET_CLASS.get(unit)` instead of `OFFSET_CLASS.get(unit, None)`
+ <a href='https://github.com/ibis-project/ibis/blob/bedc7950b24c37809e36a585b7985d5aa5e3e458/ibis/backends/pandas/execution/temporal.py#L135'>ibis/backends/pandas/execution/temporal.py:135:11:</a> SIM910 [*] Use `OFFSET_CLASS.get(unit)` instead of `OFFSET_CLASS.get(unit, None)`
+ <a href='https://github.com/ibis-project/ibis/blob/bedc7950b24c37809e36a585b7985d5aa5e3e458/ibis/backends/pandas/execution/temporal.py#L147'>ibis/backends/pandas/execution/temporal.py:147:11:</a> SIM910 [*] Use `OFFSET_CLASS.get(unit)` instead of `OFFSET_CLASS.get(unit, None)`
+ <a href='https://github.com/ibis-project/ibis/blob/bedc7950b24c37809e36a585b7985d5aa5e3e458/ibis/backends/pandas/execution/temporal.py#L159'>ibis/backends/pandas/execution/temporal.py:159:11:</a> SIM910 [*] Use `OFFSET_CLASS.get(unit)` instead of `OFFSET_CLASS.get(unit, None)`
+ <a href='https://github.com/ibis-project/ibis/blob/bedc7950b24c37809e36a585b7985d5aa5e3e458/ibis/backends/postgres/datatypes.py#L76'>ibis/backends/postgres/datatypes.py:76:25:</a> SIM910 [*] Use `_postgres_interval_fields.get(field)` instead of `_postgres_interval_fields.get(field, None)`
+ <a href='https://github.com/ibis-project/ibis/blob/bedc7950b24c37809e36a585b7985d5aa5e3e458/ibis/backends/tests/test_map.py#L194'>ibis/backends/tests/test_map.py:194:46:</a> SIM910 [*] Use `value.get(x)` instead of `value.get(x, None)`
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 842 | 842 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-11-19 00:06_

This needs to be gated behind `preview`.

---

_Label `bug` removed by @charliermarsh on 2023-11-19 00:16_

---

_Label `rule` added by @charliermarsh on 2023-11-19 00:16_

---

_Label `preview` added by @charliermarsh on 2023-11-19 00:16_

---

_Merged by @charliermarsh on 2023-11-19 00:21_

---

_Closed by @charliermarsh on 2023-11-19 00:21_

---

_Branch deleted on 2023-11-19 00:21_

---
