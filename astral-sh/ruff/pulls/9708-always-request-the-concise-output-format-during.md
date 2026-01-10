```yaml
number: 9708
title: Always request the concise output format during ecosystem checks
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: release/0.2.0
head: zb/ecosystem-fix
created_at: 2024-01-30T16:32:43Z
updated_at: 2024-01-30T16:46:03Z
url: https://github.com/astral-sh/ruff/pull/9708
synced_at: 2026-01-10T22:57:09Z
```

# Always request the concise output format during ecosystem checks

---

_Pull request opened by @zanieb on 2024-01-30 16:32_

Fixes a regression in the ecosystem checks from https://github.com/astral-sh/ruff/pull/9687 which was causing them to run for multiple hours due to the size of the output.

We need the concise format for comparisons.

We should probably update the ecosystem checks to actually diff the full output in the future because that'd be nice.

---

_Label `ci` added by @zanieb on 2024-01-30 16:33_

---

_Comment by @zanieb on 2024-01-30 16:43_

I think the ecosystem checks cannot succeed here because the baseline in #9680 is from before #9687 which does have the required format :D

---

_@charliermarsh approved on 2024-01-30 16:43_

---

_Merged by @zanieb on 2024-01-30 16:44_

---

_Closed by @zanieb on 2024-01-30 16:44_

---

_Branch deleted on 2024-01-30 16:44_

---

_Comment by @github-actions[bot] on 2024-01-30 16:46_

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
ℹ️ ecosystem check **detected format changes**. (+217 -205 lines in 87 files in 2 projects; 2 project errors; 39 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+189 -181 lines across 80 files)</summary>
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/api/extensions/__init__.py#L21'>pandas/api/extensions/__init__.py~L21</a>
```diff
 )
 
 __all__ = [
-    "ExtensionArray",
-    "ExtensionDtype",
-    "ExtensionScalarOpsMixin",
     "no_default",
-    "register_dataframe_accessor",
+    "ExtensionDtype",
     "register_extension_dtype",
+    "register_dataframe_accessor",
     "register_index_accessor",
     "register_series_accessor",
     "take",
+    "ExtensionArray",
+    "ExtensionScalarOpsMixin",
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/api/interchange/__init__.py#L5'>pandas/api/interchange/__init__.py~L5</a>
```diff
 from pandas.core.interchange.dataframe_protocol import DataFrame
 from pandas.core.interchange.from_dataframe import from_dataframe
 
-__all__ = ["DataFrame", "from_dataframe"]
+__all__ = ["from_dataframe", "DataFrame"]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/api/types/__init__.py#L14'>pandas/api/types/__init__.py~L14</a>
```diff
 )
 
 __all__ = [
+    "infer_dtype",
+    "union_categoricals",
     "CategoricalDtype",
     "DatetimeTZDtype",
     "IntervalDtype",
     "PeriodDtype",
-    "infer_dtype",
-    "union_categoricals",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/api/typing/__init__.py#L39'>pandas/api/typing/__init__.py~L39</a>
```diff
     "ExponentialMovingWindow",
     "ExponentialMovingWindowGroupby",
     "JsonReader",
-    "NAType",
     "NaTType",
+    "NAType",
     "PeriodIndexResamplerGroupby",
     "Resampler",
     "Rolling",
     "RollingGroupby",
     "SeriesGroupBy",
     "StataReader",
-    "TimeGrouper",
     # See TODO above
     # "Styler",
     "TimedeltaIndexResamplerGroupby",
+    "TimeGrouper",
     "Window",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/_numba/extensions.py#L277'>pandas/core/_numba/extensions.py~L277</a>
```diff
     """Converts numba UnicodeCharSeq (numpy string scalar) -> unicode type (string).
     Is a no-op for other types."""
     if isinstance(x, types.UnicodeCharSeq):
-        return str
+        return lambda x: str(x)
     else:
         return lambda x: x
 
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/_numba/kernels/__init__.py#L16'>pandas/core/_numba/kernels/__init__.py~L16</a>
```diff
 )
 
 __all__ = [
+    "sliding_mean",
     "grouped_mean",
-    "grouped_min_max",
+    "sliding_sum",
     "grouped_sum",
+    "sliding_var",
     "grouped_var",
-    "sliding_mean",
     "sliding_min_max",
-    "sliding_sum",
-    "sliding_var",
+    "grouped_min_max",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/common.py#L144'>pandas/core/common.py~L144</a>
```diff
     elif isinstance(key, list):
         # check if np.array(key).dtype would be bool
         if len(key) > 0:
-            if type(key) is not list:
+            if type(key) is not list:  # noqa: E721
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/ops/__init__.py#L65'>pandas/core/ops/__init__.py~L65</a>
```diff
 __all__ = [
     "ARITHMETIC_BINOPS",
     "arithmetic_op",
-    "comp_method_OBJECT_ARRAY",
     "comparison_op",
-    "fill_binop",
-    "get_array_op",
-    "get_op_result_name",
+    "comp_method_OBJECT_ARRAY",
     "invalid_comparison",
+    "fill_binop",
     "kleene_and",
     "kleene_or",
     "kleene_xor",
     "logical_op",
     "make_flex_doc",
-    "maybe_prepare_scalar_for_op",
     "radd",
     "rand_",
     "rdiv",
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/core/ops/__init__.py#L90'>pandas/core/ops/__init__.py~L90</a>
```diff
     "rtruediv",
     "rxor",
     "unpack_zerodim_and_defer",
+    "get_op_result_name",
+    "maybe_prepare_scalar_for_op",
+    "get_array_op",
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/arithmetic/test_timedelta64.py#L1788'>pandas/tests/arithmetic/test_timedelta64.py~L1788</a>
```diff
         tm.assert_equal(result, expected)
 
         # same thing buts let's be explicit about calling __rfloordiv__
-        result = td1.__rfloordiv__(scalar_td)
+        result = scalar_td // td1
         tm.assert_equal(result, expected)
 
     def test_td64arr_floordiv_int(self, box_with_array):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/arithmetic/test_timedelta64.py#L2100'>pandas/tests/arithmetic/test_timedelta64.py~L2100</a>
```diff
         xbox = get_upcast_box(tdi, ser)
         expected = tm.box_expected(expected, xbox)
 
-        result = ser.__rtruediv__(tdi)
+        result = tdi / ser
         if box is DataFrame:
             assert result is NotImplemented
         else:
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/constructors/test_from_records.py#L26'>pandas/tests/frame/constructors/test_from_records.py~L26</a>
```diff
     def test_from_records_dt64tz_frame(self):
         # GH#51162 don't lose tz when calling from_records with DataFrame input
         dti = date_range("2016-01-01", periods=10, tz="US/Pacific")
-        df = DataFrame(dict.fromkeys(range(4), dti))
+        df = DataFrame({i: dti for i in range(4)})
         with tm.assert_produces_warning(FutureWarning):
             res = DataFrame.from_records(df)
         tm.assert_frame_equal(res, df)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/indexing/test_indexing.py#L892'>pandas/tests/frame/indexing/test_indexing.py~L892</a>
```diff
 
     def test_setitem_frame_float(self, float_frame):
         piece = float_frame.loc[float_frame.index[:2], ["A", "B"]]
-        float_frame.loc[float_frame.index[-2]:, ["A", "B"]] = piece.values
+        float_frame.loc[float_frame.index[-2] :, ["A", "B"]] = piece.values
         result = float_frame.loc[float_frame.index[-2:], ["A", "B"]].values
         expected = piece.values
         tm.assert_almost_equal(result, expected)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/indexing/test_indexing.py#L1188'>pandas/tests/frame/indexing/test_indexing.py~L1188</a>
```diff
         df.loc[df.index[:3], "E"] = np.array([5 * one_hour] * 3, dtype="m8[ns]")
         df["F"] = np.timedelta64("NaT")
         df.loc[df.index[:-1], "F"] = np.array([6 * one_hour] * 3, dtype="m8[ns]")
-        df.loc[df.index[-3]:, "G"] = date_range("20130101", periods=3)
+        df.loc[df.index[-3] :, "G"] = date_range("20130101", periods=3)
         df["H"] = np.datetime64("NaT")
         result = df.dtypes
         expected = Series(
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/methods/test_nlargest.py#L157'>pandas/tests/frame/methods/test_nlargest.py~L157</a>
```diff
         result = df.nlargest(n, order)
         expected = df.sort_values(order, ascending=False).head(n)
         if Version(np.__version__) >= Version("1.25") and (
-            (order == ["a"] and n in (1, 2, 3, 4)) or (order == ["a", "b"]) and n == 5
+            (order == ["a"] and n in (1, 2, 3, 4)) or ((order == ["a", "b"]) and n == 5)
         ):
             request.applymarker(
                 pytest.mark.xfail(
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/methods/test_quantile.py#L11'>pandas/tests/frame/methods/test_quantile.py~L11</a>
```diff
 import pandas._testing as tm
 
 
-@pytest.fixture(
-    params=[["linear", "single"], ["nearest", "table"]], ids=lambda x: "-".join(x)
-)
+@pytest.fixture(params=[["linear", "single"], ["nearest", "table"]], ids="-".join)
 def interp_method(request):
     """(interpolation, method) arguments for quantile"""
     return request.param
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/methods/test_set_axis.py#L116'>pandas/tests/frame/methods/test_set_axis.py~L116</a>
```diff
         # wrong length
         msg = (
             f"Length mismatch: Expected axis has {len(obj)} elements, "
-            f"new values have {len(obj)-1} elements"
+            f"new values have {len(obj) - 1} elements"
         )
         with pytest.raises(ValueError, match=msg):
             obj.index = np.arange(len(obj) - 1)
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_arithmetic.py#L288'>pandas/tests/frame/test_arithmetic.py~L288</a>
```diff
             columns=["A", "B", "C"],
         )
 
-        result = df == None
+        result = df.__eq__(None)
         assert not result.any().any()
 
     def test_df_string_comparison(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_constructors.py#L214'>pandas/tests/frame/test_constructors.py~L214</a>
```diff
     @pytest.mark.parametrize(
         "constructor",
         [
-            DataFrame,
+            lambda: DataFrame(),
             lambda: DataFrame(None),
             lambda: DataFrame(()),
             lambda: DataFrame([]),
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_repr.py#L38'>pandas/tests/frame/test_repr.py~L38</a>
```diff
         index1 = ["\u03c3", "\u03c4", "\u03c5", "\u03c6"]
         cols = ["\u03c8"]
         df = DataFrame(data, columns=cols, index=index1)
-        assert type(repr(df)) is str
+        assert type(df.__repr__()) is str  # noqa: E721
 
         ser = df[cols[0]]
-        assert type(repr(ser)) is str
+        assert type(ser.__repr__()) is str  # noqa: E721
 
     def test_repr_bytes_61_lines(self):
         # GH#12857
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_subclass.py#L535'>pandas/tests/frame/test_subclass.py~L535</a>
```diff
             columns=["first", "last", "variable", "value"],
         )
 
-        df.apply(check_row_subclass)
-        df.apply(check_row_subclass, axis=1)
+        df.apply(lambda x: check_row_subclass(x))
+        df.apply(lambda x: check_row_subclass(x), axis=1)
 
         expected = tm.SubclassedDataFrame(
             [
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_subclass.py#L548'>pandas/tests/frame/test_subclass.py~L548</a>
```diff
             columns=["first", "last", "variable", "value"],
         )
 
-        result = df.apply(stretch, axis=1)
+        result = df.apply(lambda x: stretch(x), axis=1)
         assert isinstance(result, tm.SubclassedDataFrame)
         tm.assert_frame_equal(result, expected)
 
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L1144'>pandas/tests/groupby/aggregate/test_aggregate.py~L1144</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "func", [lambda s: s.mean(), np.mean, np.nanmean]
+    "func", [lambda s: s.mean(), lambda s: np.mean(s), lambda s: np.nanmean(s)]
 )
 def test_multiindex_custom_func(func):
     # GH 31777
```
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L1330'>pandas/tests/groupby/aggregate/test_aggregate.py~L1330</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/aggregate/test_aggregate.py#L1340'>pandas/tests/groupby/aggregate/test_aggregate.py~L1340</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/groupby/test_apply.py#L15...*[Comment body truncated]*

---
