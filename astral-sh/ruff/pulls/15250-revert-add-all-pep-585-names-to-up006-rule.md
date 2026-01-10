```yaml
number: 15250
title: "Revert \"Add all PEP-585 names to UP006 rule\""
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: revert-5454-more-UP006-detection
created_at: 2025-01-04T11:08:55Z
updated_at: 2025-01-04T11:23:55Z
url: https://github.com/astral-sh/ruff/pull/15250
synced_at: 2026-01-10T20:34:00Z
```

# Revert "Add all PEP-585 names to UP006 rule"

---

_Pull request opened by @MichaReiser on 2025-01-04 11:08_

Reverts astral-sh/ruff#5454

---

_Label `bug` added by @MichaReiser on 2025-01-04 11:08_

---

_@MichaReiser reviewed on 2025-01-04 11:16_

---

_Review comment by @MichaReiser on `crates/ruff/tests/integration_test.rs`:2132 on 2025-01-04 11:16_

@dylwil3 I had to remove those tests. I think they did their job to prove that your change was working, so I don't think we have to write new ones. If we do, we should create a dummy rule instead to avoid depending on actual rule behavior. 

---

_Comment by @github-actions[bot] on 2025-01-04 11:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/cache.py#L37'>src/pdm/cli/commands/cache.py:37:47:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Iterable`
- <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/cache.py#L97'>src/pdm/cli/commands/cache.py:97:20:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Iterable`
- <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/config.py#L102'>src/pdm/cli/commands/config.py:102:36:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Mapping`
- <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/config.py#L102'>src/pdm/cli/commands/config.py:102:67:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Mapping`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2435 violations, +0 -0 fixes in 22 projects; 33 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+0 -409 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L104'>aiven/client/argx.py:104:38:</a> UP006 Use `collections.abc.Iterable` instead of `Iterable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L155'>aiven/client/argx.py:155:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L171'>aiven/client/argx.py:171:45:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L174'>aiven/client/argx.py:174:49:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L241'>aiven/client/argx.py:241:29:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L278'>aiven/client/argx.py:278:34:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L278'>aiven/client/argx.py:278:44:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L290'>aiven/client/argx.py:290:32:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L300'>aiven/client/argx.py:300:29:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L303'>aiven/client/argx.py:303:34:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
... 399 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -483 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api/auth/backend/deny_all.py#L34'>airflow/api/auth/backend/deny_all.py:34:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/parameters.py#L87'>airflow/api_connexion/parameters.py:87:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:52:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:78:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/security.py#L114'>airflow/api_connexion/security.py:114:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/security.py#L161'>airflow/api_connexion/security.py:161:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/security.py#L180'>airflow/api_connexion/security.py:180:53:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/security.py#L199'>airflow/api_connexion/security.py:199:57:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/security.py#L218'>airflow/api_connexion/security.py:218:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/airflow/api_connexion/security.py#L237'>airflow/api_connexion/security.py:237:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 473 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -167 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/scripts/check-env.py#L37'>scripts/check-env.py:37:40:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/advanced_data_type/types.py#L58'>superset/advanced_data_type/types.py:58:21:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/advanced_data_type/types.py#L59'>superset/advanced_data_type/types.py:59:23:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/cli/test_db.py#L81'>superset/cli/test_db.py:81:12:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/cli/test_db.py#L88'>superset/cli/test_db.py:88:38:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/commands/chart/export.py#L80'>superset/commands/chart/export.py:80:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/commands/dashboard/export.py#L168'>superset/commands/dashboard/export.py:168:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/commands/database/export.py#L108'>superset/commands/database/export.py:108:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/commands/dataset/export.py#L87'>superset/commands/dataset/export.py:87:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/commands/dataset/importers/v0.py#L129'>superset/commands/dataset/importers/v0.py:129:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 157 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -250 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L27'>release/action.py:27:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L27'>release/action.py:27:52:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L35'>release/action.py:35:47:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L144'>release/build.py:144:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L37'>release/credentials.py:37:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L40'>release/credentials.py:40:38:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L24'>release/pipeline.py:24:12:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L34'>release/pipeline.py:34:31:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L103'>release/ui.py:103:33:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L24'>release/ui.py:24:17:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 240 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/tests/test_patterns.py#L1122'>ibis/common/tests/test_patterns.py:1122:13:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/tests/test_patterns.py#L1125'>ibis/common/tests/test_patterns.py:1125:10:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/tests/test_patterns.py#L731'>ibis/common/tests/test_patterns.py:731:31:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -299 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/beta_decorator.py#L128'>libs/core/langchain_core/_api/beta_decorator.py:128:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/beta_decorator.py#L186'>libs/core/langchain_core/_api/beta_decorator.py:186:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/beta_decorator.py#L201'>libs/core/langchain_core/_api/beta_decorator.py:201:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/beta_decorator.py#L30'>libs/core/langchain_core/_api/beta_decorator.py:30:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/beta_decorator.py#L39'>libs/core/langchain_core/_api/beta_decorator.py:39:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/deprecation.py#L202'>libs/core/langchain_core/_api/deprecation.py:202:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/deprecation.py#L232'>libs/core/langchain_core/_api/deprecation.py:232:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/deprecation.py#L252'>libs/core/langchain_core/_api/deprecation.py:252:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/deprecation.py#L268'>libs/core/langchain_core/_api/deprecation.py:268:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/langchain-ai/langchain/blob/acddfc772e78c9b8a13b57aa7d3237a77159e7b3/libs/core/langchain_core/_api/deprecation.py#L300'>libs/core/langchain_core/_api/deprecation.py:300:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 289 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -47 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L342'>lnbits/app.py:342:46:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L352'>lnbits/app.py:352:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L332'>lnbits/core/models.py:332:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L333'>lnbits/core/models.py:333:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/auth_api.py#L340'>lnbits/core/views/auth_api.py:340:49:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/lnurl.py#L24'>lnbits/lnurl.py:24:36:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L143'>lnbits/tasks.py:143:11:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L143'>lnbits/tasks.py:143:31:</a> UP006 Use `collections.abc.Coroutine` instead of `Coroutine` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L144'>lnbits/tasks.py:144:19:</a> UP006 Use `collections.abc.Coroutine` instead of `Coroutine` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L144'>lnbits/tasks.py:144:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/c70d44c45e7f96761d1d3808c7d7401eb65d5afe/_version_helper.py#L32'>_version_helper.py:32:17:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -101 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/dev/clint/src/clint/linter.py#L110'>dev/clint/src/clint/linter.py:110:42:</a> UP006 Use `collections.abc.Iterator` instead of `Iterator` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/dev/set_matrix.py#L317'>dev/set_matrix.py:317:56:</a> UP006 Use `collections.abc.Iterator` instead of `Iterator` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/bedrock/_autolog.py#L46'>mlflow/bedrock/_autolog.py:46:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/bedrock/_autolog.py#L46'>mlflow/bedrock/_autolog.py:46:58:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/data/dataset_registry.py#L122'>mlflow/data/dataset_registry.py:122:48:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/data/dataset_registry.py#L18'>mlflow/data/dataset_registry.py:18:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/data/dataset_registry.py#L63'>mlflow/data/dataset_registry.py:63:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/data/dataset_registry.py#L93'>mlflow/data/dataset_registry.py:93:42:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/data/huggingface_dataset.py#L180'>mlflow/data/huggingface_dataset.py:180:37:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/mlflow/mlflow/blob/c059ea6122e92ced3d0e475a519784a32d2098f8/mlflow/data/huggingface_dataset.py#L180'>mlflow/data/huggingface_dataset.py:180:52:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
... 91 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L120'>src/build/_builder.py:120:14:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L120'>src/build/_builder.py:120:68:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L39'>src/build/_builder.py:39:28:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L60'>src/build/_builder.py:60:44:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L74'>src/build/_builder.py:74:47:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
- <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/_builder.py#L74'>src/build/_builder.py:74:69:</a> UP006 Use `collections.abc.Mapping` instead of `Mapping` for type annotation
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP006 | 2431 | 0 | 2431 | 0 | 0 |
| FA100 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Merged by @MichaReiser on 2025-01-04 11:23_

---

_Closed by @MichaReiser on 2025-01-04 11:23_

---

_Branch deleted on 2025-01-04 11:23_

---
