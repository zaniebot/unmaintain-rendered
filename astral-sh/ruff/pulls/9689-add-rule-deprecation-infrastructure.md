```yaml
number: 9689
title: Add rule deprecation infrastructure
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: release/0.2.0
head: zb/deprecated
created_at: 2024-01-29T21:02:37Z
updated_at: 2024-01-30T17:39:19Z
url: https://github.com/astral-sh/ruff/pull/9689
synced_at: 2026-01-12T15:55:30Z
```

# Add rule deprecation infrastructure

---

_@zanieb_

Adds a new `Deprecated` rule group in addition to `Stable` and `Preview`.

Deprecated rules:
- Warn on explicit selection without preview
- Error on explicit selection with preview
- Are excluded when selected by prefix with preview

Deprecates `TRY200`, `ANN101`, and `ANN102` as a proof of concept. We can consider deprecating them separately.

---

_Added to milestone `v0.2.0` by @zanieb on 2024-01-29 21:15_

---

_Comment by @github-actions[bot] on 2024-01-29 21:25_

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
ℹ️ ecosystem check **detected format changes**. (+324 -298 lines in 95 files in 3 projects; 2 project errors; 38 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+304 -284 lines across 86 files)</summary>
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
-            (engine == "numba" and method in _numba_unsupported_methods)
+            engine == "numba"
+            and method in _numba_unsupported_methods
             or ncols > 1
             or application == "transformation"
             or dtype == "datetime"
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/asv_bench/benchmarks/groupby.py#L950'>asv_bench/benchmarks/groupby.py~L950</a>
```diff
         self.df4["jim"] = self.df4["joe"]
 
     def time_transform_lambda_max(self):
-        self.df.groupby(level="lev1").transform(max)
+        self.df.groupby(level="lev1").transform(lambda x: max(x))
 
     def time_transform_str_max(self):
         self.df.groupby(level="lev1").transform("max")
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/__init__.py#L279'>pandas/__init__.py~L279</a>
```diff
 # Pandas is not (yet) a py.typed library: the public API is determined
 # based on the documentation.
 __all__ = [
-    "NA",
     "ArrowDtype",
     "BooleanDtype",
     "Categorical",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/__init__.py#L298'>pandas/__init__.py~L298</a>
```diff
     "HDFStore",
     "Index",
     "IndexSlice",
-    "Int8Dtype",
     "Int16Dtype",
     "Int32Dtype",
     "Int64Dtype",
+    "Int8Dtype",
     "Interval",
     "IntervalDtype",
     "IntervalIndex",
     "MultiIndex",
+    "NA",
     "NaT",
     "NamedAgg",
     "Period",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/__init__.py#L318'>pandas/__init__.py~L318</a>
```diff
     "Timedelta",
     "TimedeltaIndex",
     "Timestamp",
-    "UInt8Dtype",
     "UInt16Dtype",
     "UInt32Dtype",
     "UInt64Dtype",
+    "UInt8Dtype",
     "api",
     "array",
     "arrays",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/__init__.py#L334'>pandas/__init__.py~L334</a>
```diff
     "errors",
     "eval",
     "factorize",
-    "from_dummies",
     "get_dummies",
+    "from_dummies",
     "get_option",
     "infer_freq",
     "interval_range",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_libs/__init__.py#L1'>pandas/_libs/__init__.py~L1</a>
```diff
 __all__ = [
+    "Interval",
     "NaT",
     "NaTType",
     "OutOfBoundsDatetime",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_libs/__init__.py#L6'>pandas/_libs/__init__.py~L6</a>
```diff
     "Timedelta",
     "Timestamp",
     "iNaT",
-    "Interval",
 ]
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_libs/tslibs/__init__.py#L1'>pandas/_libs/tslibs/__init__.py~L1</a>
```diff
 __all__ = [
-    "dtypes",
-    "localize_pydatetime",
+    "BaseOffset",
+    "IncompatibleFrequency",
     "NaT",
     "NaTType",
-    "iNaT",
-    "nat_strings",
     "OutOfBoundsDatetime",
     "OutOfBoundsTimedelta",
-    "IncompatibleFrequency",
     "Period",
     "Resolution",
+    "Tick",
     "Timedelta",
-    "normalize_i8_timestamps",
-    "is_date_array_normalized",
-    "dt64arr_to_periodarr",
+    "Timestamp",
+    "add_overflowsafe",
+    "astype_overflowsafe",
     "delta_to_nanoseconds",
+    "dt64arr_to_periodarr",
+    "dtypes",
+    "get_resolution",
+    "get_supported_dtype",
+    "get_unit_from_dtype",
+    "guess_datetime_format",
+    "iNaT",
     "ints_to_pydatetime",
     "ints_to_pytimedelta",
-    "get_resolution",
-    "Timestamp",
-    "tz_convert_from_utc_single",
-    "tz_convert_from_utc",
-    "to_offset",
-    "Tick",
-    "BaseOffset",
-    "tz_compare",
+    "is_date_array_normalized",
+    "is_supported_dtype",
     "is_unitless",
-    "astype_overflowsafe",
-    "get_unit_from_dtype",
+    "localize_pydatetime",
+    "nat_strings",
+    "normalize_i8_timestamps",
     "periods_per_day",
     "periods_per_second",
-    "guess_datetime_format",
-    "add_overflowsafe",
-    "get_supported_dtype",
-    "is_supported_dtype",
+    "to_offset",
+    "tz_compare",
+    "tz_convert_from_utc",
+    "tz_convert_from_utc_single",
 ]
 
 from pandas._libs.tslibs import dtypes  # pylint: disable=import-self
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_testing/asserters.py#L751'>pandas/_testing/asserters.py~L751</a>
```diff
         and atol is lib.no_default
     ):
         check_exact = (
-            is_numeric_dtype(left.dtype)
-            and not is_float_dtype(left.dtype)
-            or is_numeric_dtype(right.dtype)
-            and not is_float_dtype(right.dtype)
+            (is_numeric_dtype(left.dtype)
+            and not is_float_dtype(left.dtype))
+            or (is_numeric_dtype(right.dtype)
+            and not is_float_dtype(right.dtype))
         )
     elif check_exact is lib.no_default:
         check_exact = False
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/_testing/asserters.py#L908'>pandas/_testing/asserters.py~L908</a>
```diff
         and atol is lib.no_default
     ):
         check_exact = (
-            is_numeric_dtype(left.dtype)
-            and not is_float_dtype(left.dtype)
-            or is_numeric_dtype(right.dtype)
-            and not is_float_dtype(right.dtype)
+            (is_numeric_dtype(left.dtype)
+            and not is_float_dtype(left.dtype))
+            or (is_numeric_dtype(right.dtype)
+            and not is_float_dtype(right.dtype))
         )
     elif check_exact is lib.no_default:
         check_exact = False
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/dtypes/dtypes.py#L1058'>pandas/core/dtypes/dtypes.py~L1058</a>
```diff
         possible
         """
         if (
-            isinstance(string, str)
-            and (string.startswith(("period[", "Period[")))
+            (isinstance(string, str)
+            and (string.startswith(("period[", "Period["))))
             or isinstance(string, BaseOffset)
         ):
             # do not parse string like U as period[U]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/dtypes/dtypes.py#L1711'>pandas/core/dtypes/dtypes.py~L1711</a>
```diff
                 # i.e. we want to treat any floating-point NaN as equal, but
                 # not a floating-point NaN and a datetime NaT.
                 fill_value = (
-                    other._is_na_fill_value
-                    and isinstance(self.fill_value, type(other.fill_value))
+                    (other._is_na_fill_value
+                    and isinstance(self.fill_value, type(other.fill_value)))
                     or isinstance(other.fill_value, type(self.fill_value))
                 )
             else:
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/indexes/api.py#L47'>pandas/core/indexes/api.py~L47</a>
```diff
 
 
 __all__ = [
-    "CategoricalIndex",
-    "DatetimeIndex",
     "Index",
-    "IntervalIndex",
-    "InvalidIndexError",
     "MultiIndex",
-    "NaT",
-    "PeriodIndex",
+    "CategoricalIndex",
+    "IntervalIndex",
     "RangeIndex",
+    "InvalidIndexError",
     "TimedeltaIndex",
+    "PeriodIndex",
+    "DatetimeIndex",
     "_new_Index",
-    "all_indexes_same",
-    "default_index",
+    "NaT",
     "ensure_index",
     "ensure_index_from_sequences",
     "get_objs_combined_axis",
+    "union_indexes",
     "get_unanimous_names",
+    "all_indexes_same",
+    "default_index",
     "safe_sort_index",
-    "union_indexes",
 ]
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/indexes/base.py#L7670'>pandas/core/indexes/base.py~L7670</a>
```diff
         index_like = list(index_like)
 
     if isinstance(index_like, list):
-        if type(index_like) is not list:
+        if type(index_like) is not list:  # noqa: E721
             # must check for exactly list here because of strict type
             # check in clean_index_list
             index_like = list(index_like)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/indexes/range.py#L1039'>pandas/core/indexes/range.py~L1039</a>
```diff
     @unpack_zerodim_and_defer("__floordiv__")
     def __floordiv__(self, other):
         if is_integer(other) and other != 0:
-            if len(self) == 0 or (self.start % other == 0 and self.step % other == 0):
+            if len(self) == 0 or self.start % other == 0 and self.step % other == 0:
                 start = self.start // other
                 step = self.step // other
                 stop = start + len(self) * step
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/indexing.py#L1234'>pandas/core/indexing.py~L1234</a>
```diff
         if isinstance(key, bool) and not (
             is_bool_dtype(ax.dtype)
             or ax.dtype.name == "boolean"
-            or isinstance(ax, MultiIndex)
-            and is_bool_dtype(ax.get_level_values(0).dtype)
+            or (
+                isinstance(ax, MultiIndex)
+                and is_bool_dtype(ax.get_level_values(0).dtype)
+            )
         ):
             raise KeyError(
                 f"{key}: boolean label can not be used without a boolean index"
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/indexing.py#L2128'>pandas/core/indexing.py~L2128</a>
```diff
 
         is_full_setter = com.is_null_slice(pi) or com.is_full_slice(pi, len(self.obj))
 
-        is_null_setter = com.is_empty_slice(pi) or is_array_like(pi) and len(pi) == 0
+        is_null_setter = com.is_empty_slice(pi) or (is_array_like(pi) and len(pi) == 0)
 
         if is_null_setter:
             # no-op, don't cast dtype later
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/indexing.py#L2761'>pandas/core/indexing.py~L2761</a>
```diff
     """
     Check if the indexer is or contains a dict or set, which is no longer allowed.
     """
-    if (
-        isinstance(key, set)
-        or isinstance(key, tuple)
-        and any(isinstance(x, set) for x in key)
+    if isinstance(key, set) or (
+        isinstance(key, tuple) and any(isinstance(x, set) for x in key)
     ):
         raise TypeError(
             "Passing a set as an indexer is not supported. Use a list instead."
         )
 
-    if (
-        isinstance(key, dict)
-        or isinstance(key, tuple)
-        and any(isinstance(x, dict) for x in key)
+    if isinstance(key, dict) or (
+        isinstance(key, tuple) and any(isinstance(x, dict) for x in key)
     ):
         raise TypeError(
             "Passing a dict as an indexer is not supported. Use a list instead."
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/internals/construction.py#L490'>pandas/core/internals/construction.py~L490</a>
```diff
                 else x.copy(deep=True)
                 if (
                     isinstance(x, Index)
-                    or isinstance(x, ABCSeries)
-                    and is_1d_only_ea_dtype(x.dtype)
+                    or (isinstance(x, ABCSeries) and is_1d_only_ea_dtype(x.dtype))
                 )
                 else x
                 for x in arrays
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/internals/construction.py#L919'>pandas/core/internals/construction.py~L919</a>
```diff
 
     # assure that they are of the base dict class and not of derived
     # classes
-    data = [d if type(d) is dict else dict(d) for d in data]  # noqa: E721
+    data = [d if type(d) is dict else dict(d) for d in data]
 
     content = lib.dicts_to_array(data, list(columns))
     return content, columns
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/excel/__init__.py#L8'>pandas/io/excel/__init__.py~L8</a>
```diff
 from pandas.io.excel._util import register_writer
 from pandas.io.excel._xlsxwriter import XlsxWriter as _XlsxWriter
 
-__all__ = ["ExcelFile", "ExcelWriter", "read_excel"]
+__all__ = ["read_excel", "ExcelWriter", "ExcelFile"]
 
 
 register_writer(_OpenpyxlWriter)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/excel/_odswriter.py#L270'>pandas/io/excel/_odswriter.py~L270</a>
```diff
         style_key = json.dumps(style)
         if style_key in self._style_dict:
             return self._style_dict[style_key]
-        name = f"pd{len(self._style_dict) + 1}"
+        name = f"pd{len(self._style_dict)+1}"
         self._style_dict[style_key] = name
         odf_style = Style(name=name, family="table-cell")
         if "font" in style:
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/format.py#L562'>pandas/io/formats/format.py~L562</a>
```diff
             result = {}
         elif isinstance(col_space, (int, str)):
             result = {"": col_space}
-            result.update(dict.fromkeys(self.frame.columns, col_space))
+            result.update({column: col_space for column in self.frame.columns})
         elif isinstance(col_space, Mapping):
             for column in col_space.keys():
                 if column not in self.frame.columns and column != "":
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style.py#L2450'>pandas/io/formats/style.py~L2450</a>
```diff
                 for i, level in enumerate(levels_):
                     styles.append(
                         {
-                            "selector": f"thead tr:nth-child({level + 1}) th",
+                            "selector": f"thead tr:nth-child({level+1}) th",
                             "props": props
                             + (
                                 f"top:{i * pixel_size}px; height:{pixel_size}px; "
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style.py#L2461'>pandas/io/formats/style.py~L2461</a>
```diff
                 if not all(name is None for name in self.index.names):
                     styles.append(
                         {
-                            "selector": f"thead tr:nth-child({obj.nlevels + 1}) th",
+                            "selector": f"thead tr:nth-child({obj.nlevels+1}) th",
                             "props": props
                             + (
                                 f"top:{(len(levels_)) * pixel_size}px; "
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style.py#L2481'>pandas/io/formats/style.py~L2481</a>
```diff
                     styles.extend(
                         [
                             {
-                                "selector": f"thead tr th:nth-child({level + 1})",
+                                "selector": f"thead tr th:nth-child({level+1})",
                                 "props": props_ + "z-index:3 !important;",
                             },
                             {
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style.py#L4023'>pandas/io/formats/style.py~L4023</a>
```diff
         if end > start:
             cell_css += "background: linear-gradient(90deg,"
             if start > 0:
-                cell_css += f" transparent {start * 100:.1f}%, {color} {start * 100:.1f}%,"
-            cell_css += f" {color} {end * 100:.1f}%, transparent {end * 100:.1f}%)"
+                cell_css += f" transparent {start*100:.1f}%, {color} {start*100:.1f}%,"
+            cell_css += f" {color} {end*100:.1f}%, transparent {end*100:.1f}%)"
         return cell_css
 
     def css_calc(x, left: float, right: float, align: str, color: str | list | tuple):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style_render.py#L938'>pandas/io/formats/style_render.py~L938</a>
```diff
                     idx_len = d["index_lengths"].get((lvl, r), None)
                     if idx_len is not None:  # i.e. not a sparsified entry
                         d["clines"][rn + idx_len].append(
-                            f"\\cline{{{lvln + 1}-{len(visible_index_levels) + data_len}}}"
+                            f"\\cline{{{lvln+1}-{len(visible_index_levels)+data_len}}}"
                         )
 
     def format(
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style_render.py#L1192'>pandas/io/formats/style_render.py~L1192</a>
```diff
         data = self.data.loc[subset]
 
         if not isinstance(formatter, dict):
-            formatter = dict.fromkeys(data.columns, formatter)
+            formatter = {col: formatter for col in data.columns}
 
         cis = self.columns.get_indexer_for(data.columns)
         ris = self.index.get_indexer_for(data.index)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style_render.py#L1377'>pandas/io/formats/style_render.py~L1377</a>
```diff
             return self  # clear the formatter / revert to default and avoid looping
 
         if not isinstance(formatter, dict):
-            formatter = dict.fromkeys(levels_, formatter)
+            formatter = {level: formatter for level in levels_}
         else:
             formatter = {
                 obj._get_level_number(level): formatter_
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style_render.py#L1827'>pandas/io/formats/style_render.py~L1827</a>
```diff
     """
     # Get initial func from input string, input callable, or from default factory
     if isinstance(formatter, str):
-        func_0 = formatter.format
+        func_0 = lambda x: formatter.format(x)
     elif callable(formatter):
         func_0 = formatter
     elif formatter is None:
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/style_render.py#L2318'>pandas/io/formats/style_render.py~L2318</a>
```diff
         if value[0] == "#" and len(value) == 7:  # color is hex code
             return command, f"[HTML]{{{value[1:].upper()}}}{arg}"
         if value[0] == "#" and len(value) == 4:  # color is short hex code
-            val = f"{value[1].upper() * 2}{value[2].upper() * 2}{value[3].upper() * 2}"
+            val = f"{value[1].upper()*2}{value[2].upper()*2}{value[3].upper()*2}"
             return command, f"[HTML]{{{val}}}{arg}"
         elif value[:3] == "rgb":  # color is rgb or rgba
             r = re.findall("(?<=\\()[0-9\\s%]+(?=,)", value)[0].strip()
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/formats/xml.py#L269'>pandas/io/formats/xml.py~L269</a>
```diff
         nmsp_dict: dict[str, str] = {}
         if self.namespaces:
             nmsp_dict = {
-                f"xmlns{p if p == '' else f':{p}'}": n
+                f"xmlns{p if p=='' else f':{p}'}": n
                 for p, n in self.namespaces.items()
                 if n != self.prefix_uri[1:-1]
             }
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/json/__init__.py#L7'>pandas/io/json/__init__.py~L7</a>
```diff
 from pandas.io.json._table_schema import build_table_schema
 
 __all__ = [
-    "build_table_schema",
-    "read_json",
-    "to_json",
     "ujson_dumps",
     "ujson_loads",
+    "read_json",
+    "to_json",
+    "build_table_schema",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/json/_json.py#L373'>pandas/io/json/_json.py~L373</a>
```diff
 
         # TODO: Do this timedelta properly in objToJSON.c See GH #15137
         if (
-            ((obj.ndim == 1)
-            and (obj.name in set(obj.index.names)))
+            (obj.ndim == 1)
+            and (obj.name in set(obj.index.names))
             or len(obj.columns.intersection(obj.index.names))
         ):
             msg = "Overlapping names between the index and columns"
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/parsers/base_parser.py#L498'>pandas/io/parsers/base_parser.py~L498</a>
```diff
                     index_converter = converters.get(self.index_names[i]) is not None
 
             try_num_bool = not (
-                (cast_type and is_string_dtype(cast_type)) or index_converter
+                cast_type and is_string_dtype(cast_type) or index_converter
             )
 
             arr, _ = self._infer_types(
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/parsers/python_parser.py#L880'>pandas/io/parsers/python_parser.py~L880</a>
```diff
             for line in lines
             if (
                 len(line) > 1
-                or (len(line) == 1
-                and (not isinstance(line[0], str) or line[0].strip()))
+                or len(line) == 1
+                and (not isinstance(line[0], str) or line[0].strip())
             )
         ]
         return ret
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/sql.py#L1904'>pandas/io/sql.py~L1904</a>
```diff
                 # Type[str], Type[float], Type[int], Type[complex], Type[bool],
                 # Type[object]]]]"; expected type "Union[ExtensionDtype, str,
                 # dtype[Any], Type[object]]"
-                dtype = dict.fromkeys(frame, dtype)  # type: ignore[misc]
+                dtype = {col_name: dtype for col_name in frame}  # type: ignore[misc]
             else:
                 dtype = cast(dict, dtype)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/sql.py#L2847'>pandas/io/sql.py~L2847</a>
```diff
                 # Type[str], Type[float], Type[int], Type[complex], Type[bool],
                 # Type[object]]]]"; expected type "Union[ExtensionDtype, str,
                 # dtype[Any], Type[object]]"
-                dtype = dict.fromkeys(frame, dtype)  # type: ignore[misc]
+                dtype = {col_name: dtype for col_name in frame}  # type: ignore[misc]
             else:
                 dtype = cast(dict, dtype)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/stata.py#L2810'>pandas/io/stata.py~L2810</a>
```diff
         # ds_format - just use 114
         self._write_bytes(struct.pack("b", 114))
         # byteorder
-        self._write((byteorder == ">" and "\x01") or "\x02")
+        self._write(byteorder == ">" and "\x01" or "\x02")
         # filetype
         self._write("\x01")
         # unused
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/io/stata.py#L3351'>pandas/io/stata.py~L3351</a>
```diff
         # ds_format - 117
         bio.write(self._tag(bytes(str(self._dta_version), "utf-8"), "release"))
         # byteorder
-        bio.write(self._tag((byteorder == ">" and "MSF") or "LSF", "byteorder"))
+        bio.write(self._tag(byteorder == ">" and "MSF" or "LSF", "byteorder"))
         # number of vars, 2 bytes in 117 and 118, 4 byte in 119
         nvar_type = "H" if self._dta_version <= 118 else "I"
         bio.write(self._tag(struct.pack(byteorder + nvar_type, self.nvar), "K"))
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/plotting/__init__.py#L79'>pandas/plotting/__init__.py~L79</a>
```diff
 
 __all__ = [
     "PlotAccessor",
-    "andrews_curves",
-    "autocorrelation_plot",
-    "bootstrap_plot",
     "boxplot",
     "boxplot_frame",
     "boxplot_frame_groupby",
-    "deregister_matplotlib_converters",
     "hist_frame",
     "hist_series",
-    "lag_plot",
+    "scatter_matrix",
+    "radviz",
+    "andrews_curves",
+    "bootstrap_plot",
     "parallel_coordinates",
+    "lag_plot",
+    "autocorrelation_plot",
+    "table",
     "plot_params",
-    "radviz",
     "register_matplotlib_converters",
-    "scatter_matrix",
-    "table",
+    "deregister_matplotlib_converters",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/plotting/_matplotlib/__init__.py#L74'>pandas/plotting/_matplotlib/__init__.py~L74</a>
```diff
 
 
 __all__ = [
-    "andrews_curves",
-    "autocorrelation_plot",
-    "bootstrap_plot",
+    "plot",
+    "hist_series",
+    "hist_frame",
     "boxplot",
     "boxplot_frame",
     "boxplot_frame_groupby",
-    "deregister",
-    "hist_frame",
-    "hist_series",
+    "table",
+    "andrews_curves",
+    "autocorrelation_plot",
+    "bootstrap_plot",
     "lag_plot",
     "parallel_coordinates",
-    "plot",
     "radviz",
-    "register",
     "scatter_matrix",
-    "table",
+    "register",
+    "deregister",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/plotting/_matplotlib/hist.py#L262'>pandas/plotting/_matplotlib/hist.py~L262</a>
```diff
 
     @classmethod
     # error: Signature of "_plot" incompatible with supertype "MPLPlot"
-    def _plot(  # type: ignore[override]
+    def _plot(  #  type: ignore[override]
         cls,
         ax: Axes,
         y: np.ndarray,
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/testing.py#L13'>pandas/testing.py~L13</a>
```diff
 __all__ = [
     "assert_extension_array_equal",
     "assert_frame_equal",
-    "assert_index_equal",
     "assert_series_equal",
+    "assert_index_equal",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_frame_apply.py#L211'>pandas/tests/apply/test_frame_apply.py~L211</a>
```diff
 def test_apply_broadcast_scalars_axis1(float_frame):
     result = float_frame.apply(np.mean, axis=1, result_type="broadcast")
     m = float_frame.mean(axis=1)
-    expected = DataFrame(dict.fromkeys(float_frame.columns, m))
+    expected = DataFrame({c: m for c in float_frame.columns})
     tm.assert_frame_equal(result, expected)
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_frame_apply.py#L238'>pandas/tests/apply/test_frame_apply.py~L238</a>
```diff
     )
     m = list(range(len(float_frame.index)))
     expected = DataFrame(
-        dict.fromkeys(float_frame.columns, m),
+        {c: m for c in float_frame.columns},
         dtype="float64",
         index=float_frame.index,
     )
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/apply/test_frame_apply.py#L1077'>pandas/tests/apply/test_frame_apply.py~L1077</a>
```diff
 
 @pytest.mark.parametrize(
     "box",
-    [list, tuple, lambda x: np.array(x, dtype="int64")],
+    [lambda x: list(x), lambda x: tuple(x), lambda x: np.array(x, dtype="int64")],
     ids=["list", "tuple", "array"],
 )
 def test_consistency_for_boxed(box, int_frame_const_col):
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
 
    ...*[Comment body truncated]*

---

_Review requested from @charliermarsh by @zanieb on 2024-01-29 21:33_

---

_Comment by @timleslie on 2024-01-29 21:55_

**Question:** Is there a planned mechanism to explain **why** a particularly rule was deprecated? It might be helpful for a brief explanation to be available in the docs or a verbose error message.

---

_Comment by @charliermarsh on 2024-01-29 22:07_

Yeah, we should probably add a section to the documentation for that particular rule.

---

_Comment by @T-256 on 2024-01-29 22:13_

> **Question:** Is there a planned mechanism to explain **why** a particularly rule was deprecated? It might be helpful for a brief explanation to be available in the docs or a verbose error message.

Also, it would be nice to mention planned version (if any) of its drop time:
`... and planned to completely remove it in v0.3.0`

---

_Comment by @zanieb on 2024-01-29 22:23_

> Question: Is there a planned mechanism to explain why a particularly rule was deprecated? 

Yeah this would be nice. I'm not sure how to do this with our macro but I should try to figure it out.

> Also, it would be nice to mention planned version (if any) of its drop time

I don't think we can feasibly include "will be dropped in" versions — they're too likely to go out of date since builds are static. It sounds nice, but hard to maintain.

---

_Comment by @zanieb on 2024-01-30 00:16_

I've added deprecation status messages to:

- The top of the documentation for the rule
- The rule table

I've also added specific explanations for the deprecations to each rule's documentation.

---

_@charliermarsh reviewed on 2024-01-30 00:54_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_rules_table.rs`:107 on 2024-01-30 00:54_

I kind of preferred what we had before over a separate section, but I assume you tried to just extend the previous layout and this looked better?

---

_@charliermarsh reviewed on 2024-01-30 00:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:431 on 2024-01-30 00:57_

I'm less certain on `ANN102`. I would never enable it myself, but could it have value?

---

_@zanieb reviewed on 2024-01-30 01:15_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:107 on 2024-01-30 01:15_

For the legend specifically? I didn't like how much blank space there was so I tried removing that and it felt awkward. I find having all of sentences weirdly verbose for documenting the meanings of the icons.

One thing we could do here is link to the legend from the icons in the rule table. I've tried clicking them multiple times :D

---

_@zanieb reviewed on 2024-01-30 01:17_

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:431 on 2024-01-30 01:17_

I can't see why. It think the annotations here are either `self: Self` or `cls: type[Self]`. Are there situations in which they would differ for the `cls` case? cc @AlexWaygood 

---

_@AlexWaygood reviewed on 2024-01-30 07:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/codes.rs`:431 on 2024-01-30 07:52_

PEP-673 has a really annoying restriction that says you can't use `typing.Self` in metaclasses, so you have to use a custom TypeVar for `__new__` methods in metaclasses (in typeshed we work around this in a somewhat-hacky way: we have a custom TypeVar that is literally just defined as `Self = TypeVar("Self")`, and use that for these situations: https://github.com/python/typeshed/blob/3881b1b81a8118aa6fc2d2e9616c8f541e5101ed/stdlib/builtins.pyi#L194-L196)

You also might need to use a custom type variable for `cls` if you can't use `typing.Self` for whatever reason -- maybe you support older Pythons and you can't declare a dependency on `typing_extensions`?

But I agree that this seems like a pretty pointless rule, honestly. The vast majority of the time, you can leave these unannotated, and no type checker is going to care if you leave them unannotated. If you need to annotate `cls`, you'll know that you need to do so without a lint rule telling you about it, I think

---

_@charliermarsh reviewed on 2024-01-30 17:07_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_rules_table.rs`:107 on 2024-01-30 17:07_

That's interesting. We could even put it at the bottom I guess.

---

_@charliermarsh approved on 2024-01-30 17:08_

---

_Merged by @zanieb on 2024-01-30 17:38_

---

_Closed by @zanieb on 2024-01-30 17:38_

---

_Branch deleted on 2024-01-30 17:38_

---

_@zanieb reviewed on 2024-01-30 17:39_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:107 on 2024-01-30 17:39_

Let's focus on this separately #9711 

---
