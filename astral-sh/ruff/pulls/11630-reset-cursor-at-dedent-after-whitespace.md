```yaml
number: 11630
title: Reset cursor at dedent after whitespace
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/dedent-after-whitespace
created_at: 2024-05-31T06:04:56Z
updated_at: 2024-05-31T11:04:12Z
url: https://github.com/astral-sh/ruff/pull/11630
synced_at: 2026-01-12T15:55:38Z
```

# Reset cursor at dedent after whitespace

---

_@dhruvmanila_

## Summary

This PR fixes a bug to reset the cursor before emitting the `Dedent` token which had preceding whitespaces.

Previously, we would just use `TextRange::empty(self.offset())` because the range for each token would be computed at the place where it was emitted. Now, the lexer handles this centrally in `next_token` which means we require special handling at certain places. This is one such case.

Computing the range centrally in `next_token` has a benefit that the return type of lexer methods is a simple `TokenKind` but it does add a small complexity to handle such cases. I think it's worth doing this but if anyone thinks otherwise, feel free to comment.

## Test Plan

Add a test case and update the snapshot.


---

_Label `bug` added by @dhruvmanila on 2024-05-31 06:04_

---

_Label `parser` added by @dhruvmanila on 2024-05-31 06:04_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-31 06:04_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 06:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/dedent-after-whitespace)

### Merging #11630 will create unknown performance changes

<sub>Comparing <code>dhruv/dedent-after-whitespace</code> (b600b42) with <code>dhruv/parser-phase-2</code> (e8ab801)</sub>



### Summary


`🆕 30` new benchmarks



### Benchmarks breakdown

|     | Benchmark | `dhruv/parser-phase-2` | `dhruv/dedent-after-whitespace` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `formatter[large/dataset.py]` | N/A | 9.3 ms | N/A |
| 🆕 | `formatter[numpy/ctypeslib.py]` | N/A | 1.8 ms | N/A |
| 🆕 | `formatter[numpy/globals.py]` | N/A | 240.5 µs | N/A |
| 🆕 | `formatter[pydantic/types.py]` | N/A | 3.5 ms | N/A |
| 🆕 | `formatter[unicode/pypinyin.py]` | N/A | 649.7 µs | N/A |
| 🆕 | `lexer[large/dataset.py]` | N/A | 1.1 ms | N/A |
| 🆕 | `lexer[numpy/ctypeslib.py]` | N/A | 226 µs | N/A |
| 🆕 | `lexer[numpy/globals.py]` | N/A | 29.6 µs | N/A |
| 🆕 | `lexer[pydantic/types.py]` | N/A | 503.2 µs | N/A |
| 🆕 | `lexer[unicode/pypinyin.py]` | N/A | 76.7 µs | N/A |
| 🆕 | `linter/all-rules[large/dataset.py]` | N/A | 16.1 ms | N/A |
| 🆕 | `linter/all-rules[numpy/ctypeslib.py]` | N/A | 4 ms | N/A |
| 🆕 | `linter/all-rules[numpy/globals.py]` | N/A | 692.6 µs | N/A |
| 🆕 | `linter/all-rules[pydantic/types.py]` | N/A | 7.7 ms | N/A |
| 🆕 | `linter/all-rules[unicode/pypinyin.py]` | N/A | 2 ms | N/A |
| 🆕 | `linter/default-rules[large/dataset.py]` | N/A | 3.7 ms | N/A |
| 🆕 | `linter/default-rules[numpy/ctypeslib.py]` | N/A | 941.4 µs | N/A |
| 🆕 | `linter/default-rules[numpy/globals.py]` | N/A | 184.5 µs | N/A |
| 🆕 | `linter/default-rules[pydantic/types.py]` | N/A | 1.9 ms | N/A |
| 🆕 | `linter/default-rules[unicode/pypinyin.py]` | N/A | 358.3 µs | N/A |
| ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/ruff/branches/dhruv/dedent-after-whitespace)._


---

_Comment by @github-actions[bot] on 2024-05-31 06:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -231940 violations, +0 -0 fixes in 31 projects; 19 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -6809 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L240'>disnake/__main__.py:240:1:</a> E117 Over-indented
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L268'>disnake/__main__.py:268:1:</a> E113 Unexpected indentation
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L281'>disnake/__main__.py:281:1:</a> E117 Over-indented
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L285'>disnake/__main__.py:285:1:</a> E113 Unexpected indentation
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L290'>disnake/__main__.py:290:1:</a> E117 Over-indented
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L293'>disnake/__main__.py:293:1:</a> E117 Over-indented
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L296'>disnake/__main__.py:296:1:</a> E117 Over-indented
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L299'>disnake/__main__.py:299:1:</a> E117 Over-indented
... 4710 additional changes omitted for rule E117
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L328'>disnake/__main__.py:328:1:</a> E113 Unexpected indentation
- <a href='https://github.com/DisnakeDev/disnake/blob/6db7054c07c8ff8fc2c047c0478c488d8587de82/disnake/__main__.py#L34'>disnake/__main__.py:34:1:</a> E113 Unexpected indentation
... 6799 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+0 -115 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L102'>housewatch/api/analyze.py:102:1:</a> E113 Unexpected indentation
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L113'>housewatch/api/analyze.py:113:1:</a> E113 Unexpected indentation
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L124'>housewatch/api/analyze.py:124:1:</a> E113 Unexpected indentation
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L130'>housewatch/api/analyze.py:130:1:</a> E113 Unexpected indentation
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L133'>housewatch/api/analyze.py:133:1:</a> E113 Unexpected indentation
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L140'>housewatch/api/analyze.py:140:1:</a> E113 Unexpected indentation
... 45 additional changes omitted for rule E113
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L148'>housewatch/api/analyze.py:148:1:</a> E117 Over-indented
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L250'>housewatch/api/analyze.py:250:1:</a> E117 Over-indented
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L299'>housewatch/api/analyze.py:299:1:</a> E117 Over-indented
- <a href='https://github.com/PostHog/HouseWatch/blob/c0f95245b71a3f5d87a683e261ae7b89e391ec7a/housewatch/api/analyze.py#L313'>housewatch/api/analyze.py:313:1:</a> E117 Over-indented
... 105 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -6707 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/download_pretrained.py#L48'>.github/scripts/download_pretrained.py:48:1:</a> E113 Unexpected indentation
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/download_pretrained.py#L50'>.github/scripts/download_pretrained.py:50:1:</a> E117 Over-indented
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/download_pretrained.py#L60'>.github/scripts/download_pretrained.py:60:1:</a> E113 Unexpected indentation
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/download_pretrained.py#L67'>.github/scripts/download_pretrained.py:67:1:</a> E113 Unexpected indentation
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/download_pretrained.py#L88'>.github/scripts/download_pretrained.py:88:1:</a> E117 Over-indented
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/mr_generate_summary.py#L54'>.github/scripts/mr_generate_summary.py:54:1:</a> E113 Unexpected indentation
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/mr_publish_results.py#L107'>.github/scripts/mr_publish_results.py:107:1:</a> E117 Over-indented
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/mr_publish_results.py#L110'>.github/scripts/mr_publish_results.py:110:1:</a> E113 Unexpected indentation
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/mr_publish_results.py#L114'>.github/scripts/mr_publish_results.py:114:1:</a> E117 Over-indented
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/scripts/mr_publish_results.py#L117'>.github/scripts/mr_publish_results.py:117:1:</a> E117 Over-indented
... 6698 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+0 -2208 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L126'>docs/notebook_version_standardizer.py:126:1:</a> E117 Over-indented
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L131'>docs/notebook_version_standardizer.py:131:1:</a> E117 Over-indented
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L153'>docs/notebook_version_standardizer.py:153:1:</a> E117 Over-indented
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L160'>docs/notebook_version_standardizer.py:160:1:</a> E117 Over-indented
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L16'>docs/notebook_version_standardizer.py:16:1:</a> E117 Over-indented
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L25'>docs/notebook_version_standardizer.py:25:1:</a> E113 Unexpected indentation
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L30'>docs/notebook_version_standardizer.py:30:1:</a> E117 Over-indented
... 1456 additional changes omitted for rule E117
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L61'>docs/notebook_version_standardizer.py:61:1:</a> E113 Unexpected indentation
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/docs/notebook_version_standardizer.py#L73'>docs/notebook_version_standardizer.py:73:1:</a> E113 Unexpected indentation
- <a href='https://github.com/alteryx/featuretools/blob/eaa4e9c0ba6715b04ecc9f867963a7c472be56cb/featuretools/computational_backends/calculate_feature_matrix.py#L172'>featuretools/computational_backends/calculate_feature_matrix.py:172:1:</a> E113 Unexpected indentation
... 2198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -42650 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/__init__.py#L109'>airflow/__init__.py:109:1:</a> E117 Over-indented
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/__init__.py#L118'>airflow/__init__.py:118:1:</a> E113 Unexpected indentation
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/__init__.py#L122'>airflow/__init__.py:122:1:</a> E117 Over-indented
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/__init__.py#L126'>airflow/__init__.py:126:1:</a> E113 Unexpected indentation
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/__main__.py#L47'>airflow/__main__.py:47:1:</a> E113 Unexpected indentation
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/api/__init__.py#L37'>airflow/api/__init__.py:37:1:</a> E117 Over-indented
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/api/__init__.py#L40'>airflow/api/__init__.py:40:1:</a> E113 Unexpected indentation
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/api/__init__.py#L46'>airflow/api/__init__.py:46:1:</a> E117 Over-indented
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/api/auth/backend/kerberos_auth.py#L101'>airflow/api/auth/backend/kerberos_auth.py:101:1:</a> E117 Over-indented
- <a href='https://github.com/apache/airflow/blob/b8a83b2293f16523b40fab6035fed5f5431076af/airflow/api/auth/backend/kerberos_auth.py#L103'>airflow/api/auth/backend/kerberos_auth.py:103:1:</a> E117 Over-indented
... 23997 additional changes omitted for rule E117
... 42640 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -10085 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L110'>samcli/cli/cli_config_file.py:110:1:</a> E117 Over-indented
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L121'>samcli/cli/cli_config_file.py:121:1:</a> E117 Over-indented
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L155'>samcli/cli/cli_config_file.py:155:1:</a> E117 Over-indented
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L172'>samcli/cli/cli_config_file.py:172:1:</a> E117 Over-indented
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L244'>samcli/cli/cli_config_file.py:244:1:</a> E113 Unexpected indentation
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L310'>samcli/cli/cli_config_file.py:310:1:</a> E113 Unexpected indentation
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L315'>samcli/cli/cli_config_file.py:315:1:</a> E117 Over-indented
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L319'>samcli/cli/cli_config_file.py:319:1:</a> E113 Unexpected indentation
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L323'>samcli/cli/cli_config_file.py:323:1:</a> E113 Unexpected indentation
- <a href='https://github.com/aws/aws-sam-cli/blob/17bd987319056a0a67ee7b63989323461df1933e/samcli/cli/cli_config_file.py#L327'>samcli/cli/cli_config_file.py:327:1:</a> E113 Unexpected indentation
... 10075 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (+0 -66 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L122'>src/pytest_memray/marks.py:122:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L144'>src/pytest_memray/marks.py:144:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L164'>src/pytest_memray/marks.py:164:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L171'>src/pytest_memray/marks.py:171:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L183'>src/pytest_memray/marks.py:183:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L214'>src/pytest_memray/marks.py:214:1:</a> E113 Unexpected indentation
... 33 additional changes omitted for rule E113
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/marks.py#L217'>src/pytest_memray/marks.py:217:1:</a> E117 Over-indented
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/plugin.py#L130'>src/pytest_memray/plugin.py:130:1:</a> E117 Over-indented
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/plugin.py#L142'>src/pytest_memray/plugin.py:142:1:</a> E117 Over-indented
- <a href='https://github.com/bloomberg/pytest-memray/blob/0654e6b8c57cd98314c27484c280cb54c1b3bb94/src/pytest_memray/plugin.py#L159'>src/pytest_memray/plugin.py:159:1:</a> E117 Over-indented
... 56 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -6215 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L100'>docs/bokeh/docserver.py:100:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L97'>docs/bokeh/docserver.py:97:1:</a> E117 Over-indented
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L23'>examples/advanced/extensions/parallel_plot/parallel_plot.py:23:1:</a> E117 Over-indented
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L29'>examples/advanced/extensions/parallel_plot/parallel_plot.py:29:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L74'>examples/advanced/extensions/parallel_plot/parallel_plot.py:74:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L63'>examples/basic/data/server_sent_events_source.py:63:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/layout/multi_plot_layout.py#L13'>examples/integration/layout/multi_plot_layout.py:13:1:</a> E117 Over-indented
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L45'>examples/models/basic_plot.py:45:1:</a> E113 Unexpected indentation
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L78'>examples/models/buttons.py:78:1:</a> E113 Unexpected indentation
... 1984 additional changes omitted for rule E113
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L40'>examples/models/calendars.py:40:1:</a> E117 Over-indented
... 6205 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -2565 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L121'>admin/bootstrap.py:121:1:</a> E117 Over-indented
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L126'>admin/bootstrap.py:126:1:</a> E117 Over-indented
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L135'>admin/bootstrap.py:135:1:</a> E117 Over-indented
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L158'>admin/bootstrap.py:158:1:</a> E117 Over-indented (comment)
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L159'>admin/bootstrap.py:159:1:</a> E117 Over-indented (comment)
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L160'>admin/bootstrap.py:160:1:</a> E117 Over-indented (comment)
... 1743 additional changes omitted for rule E117
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L290'>admin/bootstrap.py:290:1:</a> E113 Unexpected indentation
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L301'>admin/bootstrap.py:301:1:</a> E113 Unexpected indentation
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/bootstrap.py#L61'>admin/bootstrap.py:61:1:</a> E113 Unexpected indentation
- <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/admin/securedrop_admin/__init__.py#L1033'>admin/securedrop_admin/__init__.py:1033:1:</a> E116 Unexpected indentation (comment)
... 2555 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -503 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/api.py#L423'>blinkpy/api.py:423:1:</a> E117 Over-indented
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/api.py#L425'>blinkpy/api.py:425:1:</a> E117 Over-indented
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/api.py#L452'>blinkpy/api.py:452:1:</a> E117 Over-indented
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/api.py#L454'>blinkpy/api.py:454:1:</a> E117 Over-indented
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/api.py#L512'>blinkpy/api.py:512:1:</a> E117 Over-indented
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/api.py#L514'>blinkpy/api.py:514:1:</a> E117 Over-indented
... 332 additional changes omitted for rule E117
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/auth.py#L148'>blinkpy/auth.py:148:1:</a> E113 Unexpected indentation
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/auth.py#L161'>blinkpy/auth.py:161:1:</a> E113 Unexpected indentation
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/auth.py#L47'>blinkpy/auth.py:47:1:</a> E113 Unexpected indentation
- <a href='https://github.com/fronzbot/blinkpy/blob/c9f49bb957e5a2798a79b75d6d597540294c0892/blinkpy/auth.py#L61'>blinkpy/auth.py:61:1:</a> E113 Unexpected indentation
... 493 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E117 | 152372 | 0 | 152372 | 0 | 0 |
| E113 | 73488 | 0 | 73488 | 0 | 0 |
| E116 | 6080 | 0 | 6080 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-31 06:27_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-05-31 06:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1010 on 2024-05-31 06:41_

I must admit that reading `self.cursor.start_token()` is hard to understand. Why are we starting a token when we're just about to return it. Your comment certainly helps. That makes me wonder if we can instead add a method on `Lexer` with a more meaningful name. 

```rust
self.update_token_start_to_current_offset() // A bi tlong
self.reset_token_start()
```

but I must admit, I don't find them much better than what we have. But maybe you have an idea for a better name?

---

_@MichaReiser reviewed on 2024-05-31 06:41_

There are in total 5 usages of `TextRange::empty(offset))` in the existing `Lexer` code. Would you mind reviewing if we need to make the same change in other places?

---

_Comment by @dhruvmanila on 2024-05-31 07:48_

> There are in total 5 usages of `TextRange::empty(offset))` in the existing `Lexer` code. Would you mind reviewing if we need to make the same change in other places?

I did review them, sorry for not mentioning in the PR description. This isn't needed in all places, it's only require if the lexer skips whitespaces. For posterity, the usages are:

https://github.com/astral-sh/ruff/blob/685d11a909451731dcaf4e1284d0f9143efae6e8/crates/ruff_python_parser/src/lexer.rs#L853-L863

⬆️ This emits any pending dedent tokens. There are no whitespaces consumed by the lexer before this, so no need to reset the cursor.

https://github.com/astral-sh/ruff/blob/685d11a909451731dcaf4e1284d0f9143efae6e8/crates/ruff_python_parser/src/lexer.rs#L1031-L1041

⬆️ We already reset the cursor before this function is called ⬇️

https://github.com/astral-sh/ruff/blob/b600b4287691ba0f95d63860957a895757e7053a/crates/ruff_python_parser/src/lexer.rs#L877-L898

---

_@dhruvmanila reviewed on 2024-05-31 07:49_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1010 on 2024-05-31 07:49_

Yeah, I agree. Let me think about this.

---

_@MichaReiser approved on 2024-05-31 08:20_

Thanks for explaining!

---

_Merged by @dhruvmanila on 2024-05-31 11:04_

---

_Closed by @dhruvmanila on 2024-05-31 11:04_

---

_Branch deleted on 2024-05-31 11:04_

---
