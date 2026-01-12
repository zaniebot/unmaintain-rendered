```yaml
number: 9691
title: Add rule removal infrastructure
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: release/0.2.0
head: zb/removed
created_at: 2024-01-29T22:13:22Z
updated_at: 2024-01-30T17:52:21Z
url: https://github.com/astral-sh/ruff/pull/9691
synced_at: 2026-01-12T15:55:30Z
```

# Add rule removal infrastructure

---

_@zanieb_

Similar to https://github.com/astral-sh/ruff/pull/9689 — retains removed rules for better error messages and documentation but removed rules _cannot_ be used in any context.

Removes PLR1706 as a useful test case and something we want to accomplish in #9680 anyway. The rule was in preview so we do not need to deprecate it first.

Closes https://github.com/astral-sh/ruff/issues/9007

## Test plan

<img width="1110" alt="Rules table" src="https://github.com/astral-sh/ruff/assets/2586601/ac9fa682-623c-44aa-8e51-d8ab0d308355">

<img width="1110" alt="Rule page" src="https://github.com/astral-sh/ruff/assets/2586601/05850b2d-7ca5-49bb-8df8-bb931bab25cd">



---

_Comment by @github-actions[bot] on 2024-01-29 22:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 43 project errors)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --no-preview --select PYI</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/spruceid/siwe-py">spruceid/siwe-py</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (error)</summary>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 43 project errors)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select PYI</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/spruceid/siwe-py">spruceid/siwe-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'concise' for '--output-format <OUTPUT_FORMAT>'
  [possible values: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure, sarif]

For more information, try '--help'.
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+240 -221 lines in 82 files in 2 projects; 2 project errors; 39 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+213 -200 lines across 75 files)</summary>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/asv_bench/benchmarks/frame_methods.py#L523'>asv_bench/benchmarks/frame_methods.py~L523</a>
```diff
         self.df = DataFrame(np.random.randn(1000, 100))
 
         self.s = Series(np.arange(1028.0))
-        self.df2 = DataFrame(dict.fromkeys(range(1028), self.s))
+        self.df2 = DataFrame({i: self.s for i in range(1028)})
         self.df3 = DataFrame(np.random.randn(1000, 3), columns=list("ABC"))
 
     def time_apply_user_func(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/asv_bench/benchmarks/groupby.py#L511'>asv_bench/benchmarks/groupby.py~L511</a>
```diff
         # grouping on multiple columns
         # and we lack kernels for a bunch of methods
         if (
-            (engine == "numba"
-            and method in _numba_unsupported_methods)
+            engine == "numba"
+            and method in _numba_unsupported_methods
             or ncols > 1
             or application == "transformation"
             or dtype == "datetime"
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/asv_bench/benchmarks/groupby.py#L951'>asv_bench/benchmarks/groupby.py~L951</a>
```diff
         self.df4["jim"] = self.df4["joe"]
 
     def time_transform_lambda_max(self):
-        self.df.groupby(level="lev1").transform(max)
+        self.df.groupby(level="lev1").transform(lambda x: max(x))
 
     def time_transform_str_max(self):
         self.df.groupby(level="lev1").transform("max")
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/asv_bench/benchmarks/io/csv.py#L337'>asv_bench/benchmarks/io/csv.py~L337</a>
```diff
         if thousands is not None:
             fmt = f":{thousands}"
             fmt = "{" + fmt + "}"
-            df = df.map(fmt.format)
+            df = df.map(lambda x: fmt.format(x))
         df.to_csv(self.fname, sep=sep)
 
     def time_thousands(self, sep, thousands, engine):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/asv_bench/benchmarks/io/style.py#L13'>asv_bench/benchmarks/io/style.py~L13</a>
```diff
     def setup(self, cols, rows):
         self.df = DataFrame(
             np.random.randn(rows, cols),
-            columns=[f"float_{i + 1}" for i in range(cols)],
-            index=[f"row_{i + 1}" for i in range(rows)],
+            columns=[f"float_{i+1}" for i in range(cols)],
+            index=[f"row_{i+1}" for i in range(rows)],
         )
 
     def time_apply_render(self, cols, rows):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_libs/__init__.py#L1'>pandas/_libs/__init__.py~L1</a>
```diff
 __all__ = [
-    "Interval",
     "NaT",
     "NaTType",
     "OutOfBoundsDatetime",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_libs/__init__.py#L7'>pandas/_libs/__init__.py~L7</a>
```diff
     "Timedelta",
     "Timestamp",
     "iNaT",
+    "Interval",
 ]
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_libs/tslibs/__init__.py#L1'>pandas/_libs/tslibs/__init__.py~L1</a>
```diff
 __all__ = [
-    "BaseOffset",
-    "IncompatibleFrequency",
+    "dtypes",
+    "localize_pydatetime",
     "NaT",
     "NaTType",
+    "iNaT",
+    "nat_strings",
     "OutOfBoundsDatetime",
     "OutOfBoundsTimedelta",
+    "IncompatibleFrequency",
     "Period",
     "Resolution",
-    "Tick",
     "Timedelta",
-    "Timestamp",
-    "add_overflowsafe",
-    "astype_overflowsafe",
-    "delta_to_nanoseconds",
+    "normalize_i8_timestamps",
+    "is_date_array_normalized",
     "dt64arr_to_periodarr",
-    "dtypes",
-    "get_resolution",
-    "get_supported_dtype",
-    "get_unit_from_dtype",
-    "guess_datetime_format",
-    "iNaT",
+    "delta_to_nanoseconds",
     "ints_to_pydatetime",
     "ints_to_pytimedelta",
-    "is_date_array_normalized",
-    "is_supported_dtype",
+    "get_resolution",
+    "Timestamp",
+    "tz_convert_from_utc_single",
+    "tz_convert_from_utc",
+    "to_offset",
+    "Tick",
+    "BaseOffset",
+    "tz_compare",
     "is_unitless",
-    "localize_pydatetime",
-    "nat_strings",
-    "normalize_i8_timestamps",
+    "astype_overflowsafe",
+    "get_unit_from_dtype",
     "periods_per_day",
     "periods_per_second",
-    "to_offset",
-    "tz_compare",
-    "tz_convert_from_utc",
-    "tz_convert_from_utc_single",
+    "guess_datetime_format",
+    "add_overflowsafe",
+    "get_supported_dtype",
+    "is_supported_dtype",
 ]
 
 from pandas._libs.tslibs import dtypes  # pylint: disable=import-self
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/api/__init__.py#L8'>pandas/api/__init__.py~L8</a>
```diff
 )
 
 __all__ = [
+    "interchange",
     "extensions",
     "indexers",
-    "interchange",
     "types",
     "typing",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/api/indexers/__init__.py#L10'>pandas/api/indexers/__init__.py~L10</a>
```diff
 )
 
 __all__ = [
+    "check_array_indexer",
     "BaseIndexer",
     "FixedForwardWindowIndexer",
     "VariableOffsetWindowIndexer",
-    "check_array_indexer",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/_numba/extensions.py#L277'>pandas/core/_numba/extensions.py~L277</a>
```diff
     """Converts numba UnicodeCharSeq (numpy string scalar) -> unicode type (string).
     Is a no-op for other types."""
     if isinstance(x, types.UnicodeCharSeq):
-        return lambda x: str(x)
+        return str
     else:
         return lambda x: x
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/_numba/kernels/__init__.py#L16'>pandas/core/_numba/kernels/__init__.py~L16</a>
```diff
 )
 
 __all__ = [
-    "sliding_mean",
     "grouped_mean",
-    "sliding_sum",
+    "grouped_min_max",
     "grouped_sum",
-    "sliding_var",
     "grouped_var",
+    "sliding_mean",
     "sliding_min_max",
-    "grouped_min_max",
+    "sliding_sum",
+    "sliding_var",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/arrays/__init__.py#L23'>pandas/core/arrays/__init__.py~L23</a>
```diff
 
 __all__ = [
     "ArrowExtensionArray",
+    "ExtensionArray",
+    "ExtensionOpsMixin",
+    "ExtensionScalarOpsMixin",
     "ArrowStringArray",
     "BaseMaskedArray",
     "BooleanArray",
     "Categorical",
     "DatetimeArray",
-    "ExtensionArray",
-    "ExtensionOpsMixin",
-    "ExtensionScalarOpsMixin",
     "FloatingArray",
     "IntegerArray",
     "IntervalArray",
     "NumpyExtensionArray",
     "PeriodArray",
+    "period_array",
     "SparseArray",
     "StringArray",
     "TimedeltaArray",
-    "period_array",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/arrays/arrow/__init__.py#L4'>pandas/core/arrays/arrow/__init__.py~L4</a>
```diff
 )
 from pandas.core.arrays.arrow.array import ArrowExtensionArray
 
-__all__ = ["ArrowExtensionArray", "StructAccessor", "ListAccessor"]
+__all__ = ["ArrowExtensionArray", "ListAccessor", "StructAccessor"]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/arrays/arrow/array.py#L165'>pandas/core/arrays/arrow/array.py~L165</a>
```diff
         "rmul": lambda x, y: pc.multiply_checked(y, x),
         "truediv": lambda x, y: pc.divide(*cast_for_truediv(x, y)),
         "rtruediv": lambda x, y: pc.divide(*cast_for_truediv(y, x)),
-        "floordiv": lambda x, y: floordiv_compat(x, y),
+        "floordiv": floordiv_compat,
         "rfloordiv": lambda x, y: floordiv_compat(y, x),
         "mod": NotImplemented,
         "rmod": NotImplemented,
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/arrays/arrow/array.py#L1401'>pandas/core/arrays/arrow/array.py~L1401</a>
```diff
             pa.types.is_floating(pa_type)
             and (
                 na_value is np.nan
-                or original_na_value is lib.no_default
-                and is_float_dtype(dtype)
+                or (original_na_value is lib.no_default and is_float_dtype(dtype))
             )
         ):
             result = data._pa_array.to_numpy()
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/arrays/arrow/array.py#L2505'>pandas/core/arrays/arrow/array.py~L2505</a>
```diff
 
     def _str_findall(self, pat: str, flags: int = 0) -> Self:
         regex = re.compile(pat, flags=flags)
-        predicate = lambda val: regex.findall(val)
+        predicate = regex.findall
         result = self._apply_elementwise(predicate)
         return type(self)(pa.chunked_array(result))
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/common.py#L144'>pandas/core/common.py~L144</a>
```diff
     elif isinstance(key, list):
         # check if np.array(key).dtype would be bool
         if len(key) > 0:
-            if type(key) is not list:  # noqa: E721
+            if type(key) is not list:
                 # GH#42461 cython will raise TypeError if we pass a subclass
                 key = list(key)
             return lib.is_bool_list(key)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/dtypes/common.py#L1676'>pandas/core/dtypes/common.py~L1676</a>
```diff
 
 
 __all__ = [
-    "DT64NS_DTYPE",
-    "INT64_DTYPE",
-    "TD64NS_DTYPE",
     "classes",
+    "DT64NS_DTYPE",
     "ensure_float64",
     "ensure_python_int",
     "ensure_str",
     "infer_dtype_from_object",
+    "INT64_DTYPE",
     "is_1d_only_ea_dtype",
     "is_all_strings",
     "is_any_real_numeric_dtype",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/dtypes/common.py#L1729'>pandas/core/dtypes/common.py~L1729</a>
```diff
     "is_unsigned_integer_dtype",
     "needs_i8_conversion",
     "pandas_dtype",
+    "TD64NS_DTYPE",
     "validate_all_hashable",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/dtypes/dtypes.py#L1058'>pandas/core/dtypes/dtypes.py~L1058</a>
```diff
         possible
         """
         if (
-            (isinstance(string, str)
-            and (string.startswith(("period[", "Period["))))
+            isinstance(string, str)
+            and (string.startswith(("period[", "Period[")))
             or isinstance(string, BaseOffset)
         ):
             # do not parse string like U as period[U]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/dtypes/dtypes.py#L1711'>pandas/core/dtypes/dtypes.py~L1711</a>
```diff
                 # i.e. we want to treat any floating-point NaN as equal, but
                 # not a floating-point NaN and a datetime NaT.
                 fill_value = (
-                    (other._is_na_fill_value
-                    and isinstance(self.fill_value, type(other.fill_value)))
+                    other._is_na_fill_value
+                    and isinstance(self.fill_value, type(other.fill_value))
                     or isinstance(other.fill_value, type(self.fill_value))
                 )
             else:
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/internals/__init__.py#L14'>pandas/core/internals/__init__.py~L14</a>
```diff
 )
 
 __all__ = [
+    "ArrayManager",
     "Block",  # pylint: disable=undefined-all-variable
+    "BlockManager",
+    "DataManager",
     "DatetimeTZBlock",  # pylint: disable=undefined-all-variable
     "ExtensionBlock",  # pylint: disable=undefined-all-variable
-    "make_block",
-    "DataManager",
-    "ArrayManager",
-    "BlockManager",
-    "SingleDataManager",
-    "SingleBlockManager",
     "SingleArrayManager",
+    "SingleBlockManager",
+    "SingleDataManager",
     "concatenate_managers",
+    "make_block",
 ]
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/internals/concat.py#L221'>pandas/core/internals/concat.py~L221</a>
```diff
         for i, indexer in indexers.items():
             mgr = mgr.reindex_indexer(
                 axes[i],
-                indexers[i],
+                indexer,
                 axis=i,
                 copy=False,
                 only_slice=True,  # only relevant for i==0
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/ops/__init__.py#L65'>pandas/core/ops/__init__.py~L65</a>
```diff
 __all__ = [
     "ARITHMETIC_BINOPS",
     "arithmetic_op",
-    "comparison_op",
     "comp_method_OBJECT_ARRAY",
-    "invalid_comparison",
+    "comparison_op",
     "fill_binop",
+    "get_array_op",
+    "get_op_result_name",
+    "invalid_comparison",
     "kleene_and",
     "kleene_or",
     "kleene_xor",
     "logical_op",
     "make_flex_doc",
+    "maybe_prepare_scalar_for_op",
     "radd",
     "rand_",
     "rdiv",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/ops/__init__.py#L87'>pandas/core/ops/__init__.py~L87</a>
```diff
     "rtruediv",
     "rxor",
     "unpack_zerodim_and_defer",
-    "get_op_result_name",
-    "maybe_prepare_scalar_for_op",
-    "get_array_op",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_frame_apply_relabeling.py#L65'>pandas/tests/apply/test_frame_apply_relabeling.py~L65</a>
```diff
             cat=("B", max),
             dat=("C", "min"),
             f=("B", np.sum),
-            kk=("B", min),
+            kk=("B", lambda x: min(x)),
         )
     expected = pd.DataFrame(
         {
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_series_apply.py#L395'>pandas/tests/apply/test_series_apply.py~L395</a>
```diff
     tm.assert_series_equal(result, expected)
 
 
-@pytest.mark.parametrize("func", [str, str])
+@pytest.mark.parametrize("func", [str, lambda x: str(x)])
 def test_apply_map_evaluate_lambdas_the_same(string_series, func, by_row):
     # test that we are evaluating row-by-row first if by_row="compat"
     # else vectorized evaluation
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_series_apply.py#L413'>pandas/tests/apply/test_series_apply.py~L413</a>
```diff
     # in the future, the result will be a Series class.
 
     with tm.assert_produces_warning(FutureWarning):
-        result = string_series.agg(type)
+        result = string_series.agg(lambda x: type(x))
     assert isinstance(result, Series) and len(result) == len(string_series)
 
     with tm.assert_produces_warning(FutureWarning):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_series_apply.py#L696'>pandas/tests/apply/test_series_apply.py~L696</a>
```diff
 def test_series_apply_unpack_nested_data():
     # GH#55189
     ser = Series([[1, 2, 3], [4, 5, 6, 7]])
-    result = ser.apply(Series)
+    result = ser.apply(lambda x: Series(x))
     expected = DataFrame({0: [1.0, 4.0], 1: [2.0, 5.0], 2: [3.0, 6.0], 3: [np.nan, 7]})
     tm.assert_frame_equal(result, expected)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/arithmetic/test_period.py#L1483'>pandas/tests/arithmetic/test_period.py~L1483</a>
```diff
             lambda obj, ng: ng + obj,
             lambda obj, ng: obj - ng,
             lambda obj, ng: ng - obj,
-            lambda obj, ng: np.add(obj, ng),
+            np.add,
             lambda obj, ng: np.add(ng, obj),
-            lambda obj, ng: np.subtract(obj, ng),
+            np.subtract,
             lambda obj, ng: np.subtract(ng, obj),
         ],
     )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/arrays/sparse/test_arithmetics.py#L404'>pandas/tests/arrays/sparse/test_arithmetics.py~L404</a>
```diff
     # GH#27910
     arr = SparseArray([0, 1], fill_value=0)
     df = pd.DataFrame([[1, 2], [3, 4]])
-    result = arr + df
+    result = arr.__add__(df)
     assert result is NotImplemented
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/arrays/string_/test_string.py#L192'>pandas/tests/arrays/string_/test_string.py~L192</a>
```diff
 def test_add_strings(dtype):
     arr = pd.array(["a", "b", "c", "d"], dtype=dtype)
     df = pd.DataFrame([["t", "y", "v", "w"]], dtype=object)
-    assert arr + df is NotImplemented
+    assert arr.__add__(df) is NotImplemented
 
     result = arr + df
     expected = pd.DataFrame([["at", "by", "cv", "dw"]]).astype(dtype)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/arrays/string_/test_string.py#L208'>pandas/tests/arrays/string_/test_string.py~L208</a>
```diff
     arr = pd.array(["a", "b", np.nan, np.nan], dtype=dtype)
     df = pd.DataFrame([["x", np.nan, "y", np.nan]])
 
-    assert arr + df is NotImplemented
+    assert arr.__add__(df) is NotImplemented
 
     result = arr + df
     expected = pd.DataFrame([["ax", np.nan, np.nan, np.nan]]).astype(dtype)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/base/test_conversion.py#L49'>pandas/tests/base/test_conversion.py~L49</a>
```diff
         [
             lambda x: x.tolist(),
             lambda x: x.to_list(),
-            list,
-            lambda x: list(iter(x)),
+            lambda x: list(x),
+            lambda x: list(x.__iter__()),
         ],
         ids=["tolist", "to_list", "list", "iter"],
     )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/base/test_conversion.py#L81'>pandas/tests/base/test_conversion.py~L81</a>
```diff
         [
             lambda x: x.tolist(),
             lambda x: x.to_list(),
-            list,
-            lambda x: list(iter(x)),
+            lambda x: list(x),
+            lambda x: list(x.__iter__()),
         ],
         ids=["tolist", "to_list", "list", "iter"],
     )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/base/test_conversion.py#L131'>pandas/tests/base/test_conversion.py~L131</a>
```diff
         [
             lambda x: x.tolist(),
             lambda x: x.to_list(),
-            list,
-            lambda x: list(iter(x)),
+            lambda x: list(x),
+            lambda x: list(x.__iter__()),
         ],
         ids=["tolist", "to_list", "list", "iter"],
     )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/copy_view/index/test_timedeltaindex.py#L16'>pandas/tests/copy_view/index/test_timedeltaindex.py~L16</a>
```diff
 @pytest.mark.parametrize(
     "cons",
     [
-        TimedeltaIndex,
+        lambda x: TimedeltaIndex(x),
         lambda x: TimedeltaIndex(TimedeltaIndex(x)),
     ],
 )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/copy_view/test_array.py#L15'>pandas/tests/copy_view/test_array.py~L15</a>
```diff
 
 @pytest.mark.parametrize(
     "method",
-    [lambda ser: ser.values, np.asarray],
+    [lambda ser: ser.values, lambda ser: np.asarray(ser)],
     ids=["values", "asarray"],
 )
 def test_series_values(using_copy_on_write, method):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/copy_view/test_array.py#L45'>pandas/tests/copy_view/test_array.py~L45</a>
```diff
 
 @pytest.mark.parametrize(
     "method",
-    [lambda df: df.values, np.asarray],
+    [lambda df: df.values, lambda df: np.asarray(df)],
     ids=["values", "asarray"],
 )
 def test_dataframe_values(using_copy_on_write, method):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/copy_view/test_constructors.py#L343'>pandas/tests/copy_view/test_constructors.py~L343</a>
```diff
     arr = np.array([[1, 2], [3, 4]])
     df = DataFrame(arr, copy=copy)
 
-    if (using_copy_on_write and copy is not False) or copy is True:
+    if using_copy_on_write and copy is not False or copy is True:
         assert not np.shares_memory(get_array(df, 0), arr)
     else:
         assert np.shares_memory(get_array(df, 0), arr)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/copy_view/test_functions.py#L201'>pandas/tests/copy_view/test_functions.py~L201</a>
```diff
     "func",
     [
         lambda df1, df2, **kwargs: df1.merge(df2, **kwargs),
-        merge,
+        lambda df1, df2, **kwargs: merge(df1, df2, **kwargs),
     ],
 )
 def test_merge_on_key(using_copy_on_write, func):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/copy_view/test_methods.py#L388'>pandas/tests/copy_view/test_methods.py~L388</a>
```diff
         lambda idx: idx,
         lambda idx: idx.view(),
         lambda idx: idx.copy(),
-        list,
+        lambda idx: list(idx),
     ],
     ids=["identical", "view", "copy", "values"],
 )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/dtypes/test_common.py#L119'>pandas/tests/dtypes/test_common.py~L119</a>
```diff
 }
 
 
-@pytest.mark.parametrize("name1,dtype1", list(dtypes.items()), ids=str)
-@pytest.mark.parametrize("name2,dtype2", list(dtypes.items()), ids=str)
+@pytest.mark.parametrize("name1,dtype1", list(dtypes.items()), ids=lambda x: str(x))
+@pytest.mark.parametrize("name2,dtype2", list(dtypes.items()), ids=lambda x: str(x))
 def test_dtype_equal(name1, dtype1, name2, dtype2):
     # match equal to self, but not equal to other
     assert com.is_dtype_equal(dtype1, dtype1)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/dtypes/test_common.py#L128'>pandas/tests/dtypes/test_common.py~L128</a>
```diff
         assert not com.is_dtype_equal(dtype1, dtype2)
 
 
-@pytest.mark.parametrize("name,dtype", list(dtypes.items()), ids=str)
+@pytest.mark.parametrize("name,dtype", list(dtypes.items()), ids=lambda x: str(x))
 def test_pyarrow_string_import_error(name, dtype):
     # GH-44276
     assert not com.is_dtype_equal(dtype, "string[pyarrow]")
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/dtypes/test_dtypes.py#L227'>pandas/tests/dtypes/test_dtypes.py~L227</a>
```diff
 
     def test_repr(self):
         cat = Categorical(pd.Index([1, 2, 3], dtype="int32"))
-        result = repr(cat.dtype)
+        result = cat.dtype.__repr__()
         expected = (
             "CategoricalDtype(categories=[1, 2, 3], ordered=False, "
             "categories_dtype=int32)"
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/extension/base/getitem.py#L139'>pandas/tests/extension/base/getitem.py~L139</a>
```diff
                 "index out of bounds",  # pyarrow
                 "Out of bounds access",  # Sparse
                 f"loc must be an integer between -{ub} and {ub}",  # Sparse
-                f"index {ub+1} is out of bounds for axis 0 with size {ub}",
-                f"index -{ub+1} is out of bounds for axis 0 with size {ub}",
+                f"index {ub + 1} is out of bounds for axis 0 with size {ub}",
+                f"index -{ub + 1} is out of bounds for axis 0 with size {ub}",
             ]
         )
         with pytest.raises(IndexError, match=msg):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L940'>pandas/tests/groupby/aggregate/test_aggregate.py~L940</a>
```diff
             [5.5, 7.5],
         ),
         (
-            (("y", "A"), max),
+            (("y", "A"), lambda x: max(x)),
             (("y", "A"), lambda x: 1),
             (("y", "B"), np.mean),
             [1, 3],
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L1143'>pandas/tests/groupby/aggregate/test_aggregate.py~L1143</a>
```diff
         tm.assert_frame_equal(left, right)
 
 
-@pytest.mark.parametrize("func", [lambda s: s.mean(), np.mean, np.nanmean])
+@pytest.mark.parametrize(
+    "func", [lambda s: s.mean(), lambda s: np.mean(s), lambda s: np.nanmean(s)]
+)
 def test_multiindex_custom_func(func):
     # GH 31777
     data = [[1, 4, 2], [5, 7, 1]]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L1328'>pandas/tests/groupby/aggregate/test_aggregate.py~L1328</a>
```diff
             height_sqr_min=("height", lambda x: np.min(x**2)),
             height_max=("height", "max"),
             weight_max=("weight", "max"),
-            height_max_2=("height", np.max),
-            weight_min=("weight", np.min),
+            height_max_2=("height", lambda x: np.max(x)),
+            weight_min=("weight", lambda x: np.min(x)),
         )
         tm.assert_frame_equal(result1, expected)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L1338'>pandas/tests/groupby/aggregate/test_aggregate.py~L1338</a>
```diff
             height_sqr_min=pd.NamedAgg(column="height", aggfunc=lambda x: np.min(x**2)),
             height_max=pd.NamedAgg(column="height", aggfunc="max"),
             weight_max=pd.NamedAgg(column="weight", aggfunc="max"),
-            height_max_2=pd.NamedAgg(column="height", aggfunc=np.max),
-            weight_min=pd.NamedAgg(column="weight", aggfunc=np.min),
+            height_max_2=pd.NamedAgg(column="height", aggfunc=lambda x: np.max(x)),
+            weight_min=pd.NamedAgg(column="weight", aggfunc=lambda x: np.min(x)),
         )
         tm.assert_frame_equal(result2, expected)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_cython.py#L221'>pandas/tests/groupby/aggregate/test_cython.py~L221</a>
```diff
     result = g._cython_agg_general(op, alt=None, numeric_only=True)
 
     g = df.groupby(pd.cut(df[0], grps), observed=observed)
-    expected = g.agg(targop)
+    expected = g.agg(lambda x: targop(x))
     tm.assert_frame_equal(result, expected)
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_other.py#L402'>pandas/tests/groupby/aggregate/test_other.py~L402</a>
```diff
     equiv_callables = [
         sum,
         np.sum,
-        sum,
+        lambda x: sum(x),
         lambda x: x.sum(),
         partial(sum),
         fn_class(),
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_other.py#L482'>pandas/tests/groupby/aggregate/test_other.py~L482</a>
```diff
     df = DataFrame({"a": 1, "b": [ts + dt.timedelta(minutes=nn) for nn in range(10)]})
 
     result1 = df.groupby("a")["b"].agg("min").iloc[0]
-    result2 = df.groupby("a")["b"].agg(np.min).iloc[0]
+    result2 = df.groupby("a")["b"].agg(lambda x: np.min(x)).iloc[0]
     result3 = df.groupby("a")["b"].min().iloc[0]
 
     assert result1 == ts
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_other.py#L544'>pandas/tests/groupby/aggregate/test_other.py~L544</a>
```diff
     [
         (tuple, tuple),
         (list, list),
-        (tuple, tuple),
-        (list, list),
+        (lambda x: tuple(x), tuple),
+        (lambda x: list(x), list),
     ],
 )
 def test_agg_structs_dataframe(structure, cast_as):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_other.py#L566'>pandas/tests/groupby/aggregate/test_other.py~L566</a>
```diff
     [
         (tuple, tuple),
         (list, list),
-        (tuple, tuple),
-        (list, list),
+        (lambda x: tuple(x), tuple),
+        (lambda x: list(x), list),
     ],
 )
 def test_agg_structs_series(structure, cast_as):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_other.py#L604'>pandas/tests/groupby/aggregate/test_other.py~L604</a>
```diff
     # GH 18473
     df = DataFrame({"A": [str(x) for x in range(3)], "B": [str(x) for x in range(3)]})
     grouped = df.groupby("A", as_index=False, sort=False)
-    result = grouped.agg({"B": list})
+    result = grouped.agg({"B": lambda x: list(x)})
     expected = DataFrame(
         {"A": [str(x) for x in range(3)], "B": [[str(x)] for x in range(3)]}
     )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/test_apply.py#L1593'>pandas/tests/groupby/test_apply.py~L1593</a>
```diff
     tm.assert_frame_equal(result, expected)
 
     with tm.assert_produces_warning(DeprecationWarning, match=msg):
-        expected2 = gb.apply(npfunc)
+        expected2 = gb.apply(lambda x: npfunc(x))
     tm.assert_frame_equal(result, expected2)
 
     if f != sum:
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/test_categorical.py#L144'>pandas/tests/groupby/test_categorical.py~L144</a>
```diff
         result = df.a.groupby(c, observed=False).transform(sum)
     tm.assert_series_equal(result, df["a"])
 
-    tm.assert_series_equal(df.a.groupby(c, observed=False).transform(np.sum), df["a"])
+    tm.assert_series_equal(
+        df.a.groupby(c, observed=False).transform(lambda xs: np.sum(xs)), df["a"]
+    )
     msg = "using DataFrameGroupBy.sum"
     with tm.assert_produces_warning(FutureWarning, match=msg):
         # GH#53425
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/test_categorical.py#L162'>pandas/tests/groupby/test_categorical.py~L162</a>
```diff
         # GH#53425
         result3 = gbc.transform(max)
     result4 = gbc.transform(np.maximum.reduce)
-    result5 = gbc.transform(np.maximum.reduce)
+    result5 = gbc.transform(lambda xs: np.maximum.reduce(xs))
     tm.assert_frame_equal(result2, df[["a"]], c...*[Comment body truncated]*

---

_@charliermarsh approved on 2024-01-29 23:59_

That's nice!

---

_Comment by @charliermarsh on 2024-01-30 00:00_

Although, we should also remove the files and tests for the rule. And we should probably ensure it doesn't end up in the docs?


---

_Added to milestone `v0.2.0` by @zanieb on 2024-01-30 00:06_

---

_@zanieb reviewed on 2024-01-30 00:15_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:36 on 2024-01-30 00:15_

Can we get rid of more here? I'm not sure.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-30 00:15_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_rules_table.rs`:32 on 2024-01-30 00:51_

My vote would be to remove rules from this table entirely. What's the motivation for keeping them around?

---

_@charliermarsh reviewed on 2024-01-30 00:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_selector.rs`:217 on 2024-01-30 00:51_

Do these show up in the JSON Schema? (I think they shouldn't.)

---

_@charliermarsh reviewed on 2024-01-30 00:51_

---

_@zanieb reviewed on 2024-01-30 01:09_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:217 on 2024-01-30 01:09_

Yeah let's remove them from there..

---

_@zanieb reviewed on 2024-01-30 01:12_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:32 on 2024-01-30 01:12_

I don't think there's another way to navigate to them. The motivation for keeping them is to

1. Document why they were removed
2. Provide a reference for people on previous versions of Ruff

I don't think I expect to keep them around forever; this was an easy path forward though. When we do rule recategorization (#1774), I think we should look at this again?

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_docs.rs`:48 on 2024-01-30 07:24_

I find the terminology here a bit confusing. Does this apply to rules that are in their deprecation period or only to rules that have been removed after being deprecated? 


---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_rules_table.rs`:32 on 2024-01-30 07:27_

It seems fair to me to list the rules here when we document the removal on the details page. I do see this as obsolete if we have a versioned documentation. 

You may want to consider listing these rules *last* and designing them visually less prominent (e.g., graying them out).

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:932 on 2024-01-30 07:30_

Nit: I prefer to use spaces for alignment in terminals because tabs can be awkwardly large in the terminal, although it has the downside that we don't respect the users configured indentation. 

---

_@MichaReiser reviewed on 2024-01-30 07:31_

Could you add two screenshots of the documentation to your test plan:
1.  showing the generated rules table
2. the details page 



---

_@MichaReiser reviewed on 2024-01-30 07:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:51 on 2024-01-30 07:33_

I remember that this needs to be a `format!` call because we extract it somewhere (or was it message). @charliermarsh do you remember the details? Do we need to preserve the `message` and `fix_title`?

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:932 on 2024-01-30 15:28_

I'd rather be consistent with what we have elsewhere

---

_@zanieb reviewed on 2024-01-30 15:28_

---

_@zanieb reviewed on 2024-01-30 15:29_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:51 on 2024-01-30 15:29_

It needs to be a `format!` call for the `derive_message_formats` macro but I've hard-coded the message formats.

---

_@zanieb reviewed on 2024-01-30 15:29_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_docs.rs`:48 on 2024-01-30 15:29_

This is actually wrong! It should be `rule.is_removed()`.

---

_@zanieb reviewed on 2024-01-30 15:30_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_docs.rs`:48 on 2024-01-30 15:30_

```suggestion
            if rule.is_removed() {
```

---

_@zanieb reviewed on 2024-01-30 15:31_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:932 on 2024-01-30 15:31_

```suggestion
                    message.push_str("\n    - ");
```

---

_@zanieb reviewed on 2024-01-30 15:32_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:32 on 2024-01-30 15:32_

I wanted to gray them out but it actually seems like a huge pain to do it inside the markdown table. I think this will become obsolete when we do more work on the documentation but I do not think now is the time to try to build that out.

---

_Comment by @zanieb on 2024-01-30 15:35_

@MichaReiser added those — I'm not in love with them but I think they're a good incremental step forward.

---

_Comment by @MichaReiser on 2024-01-30 16:25_

Thank you @zanieb. 

What do you think of crossing out the rule names for rules that have been removed? The icon is very distant and hard to spot. 

E.g. ~~and-or-tenary~~

---

_Comment by @zanieb on 2024-01-30 16:51_

@MichaReiser I guess that's an option. Could I populate the message section instead? e.g. "_This rule has been removed_"?

---

_@MichaReiser approved on 2024-01-30 16:59_

> @MichaReiser I guess that's an option. Could I populate the message section instead? e.g. "_This rule has been removed_"?

My preferred solution would be to have both but we can iterate on this later.

---

_Comment by @zanieb on 2024-01-30 17:36_

Here's the latest rule table <img width="1110" alt="Screenshot 2024-01-30 at 11 36 16" src="https://github.com/astral-sh/ruff/assets/2586601/ac9fa682-623c-44aa-8e51-d8ab0d308355">


---

_Merged by @zanieb on 2024-01-30 17:45_

---

_Closed by @zanieb on 2024-01-30 17:45_

---

_Branch deleted on 2024-01-30 17:45_

---
