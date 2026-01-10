```yaml
number: 9712
title: Stabilize some rules for v0.2.0 release
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: release/0.2.0
head: charlie/stabilize
created_at: 2024-01-30T17:52:10Z
updated_at: 2024-01-31T17:09:00Z
url: https://github.com/astral-sh/ruff/pull/9712
synced_at: 2026-01-10T22:57:09Z
```

# Stabilize some rules for v0.2.0 release

---

_Pull request opened by @charliermarsh on 2024-01-30 17:52_

## Summary

This PR stabilizes the preview rules from:

- `flake8-trio` (6 rules)
- `flake8-quotes` (1 rule)
- `pyupgrade` (1 rule)
- `flake8-pyi` (1 rule)
- `flake8-simplify` (2 rules)
- `flake8-bandit` (9 rules; 14 remain in preview)
- `flake8-type-checking` (1 rule)
- `numpy` (1 rule)
- `ruff` (4 rules, one elevated from nursery; 6 remain in preview as they were added within the last 30 days)
- `flake8-logging` (4 rules)

I see these are largely uncontroversial.


---

_Comment by @charliermarsh on 2024-01-30 17:52_

Draft, I need to remove stabilization for some of the rules based on when they were added.

---

_Comment by @codspeed-hq[bot] on 2024-01-30 17:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/stabilize)

### Merging #9712 will **degrade performances by 9.28%**

<sub>Comparing <code>charlie/stabilize</code> (a7476a8) with <code>release/0.2.0</code> (761e148)</sub>



### Summary

`❌ 4` regressions
`✅ 26` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/stabilize)._

### Benchmarks breakdown

|     | Benchmark | `release/0.2.0` | `charlie/stabilize` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 12.3 ms | 12.8 ms | -4.19% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 47.2 ms | 50.8 ms | -6.98% |
| ❌ | `linter/all-rules[large/dataset.py]` | 100.6 ms | 110.9 ms | -9.28% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 23.1 ms | 24.3 ms | -5% |


---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 18:23_

---

_Marked ready for review by @charliermarsh on 2024-01-30 18:23_

---

_Label `rule` added by @charliermarsh on 2024-01-30 18:23_

---

_Comment by @github-actions[bot] on 2024-01-30 19:12_

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




---

_Merged by @charliermarsh on 2024-01-30 19:12_

---

_Closed by @charliermarsh on 2024-01-30 19:12_

---

_Branch deleted on 2024-01-30 19:12_

---

_Added to milestone `v0.2.0` by @charliermarsh on 2024-01-31 17:09_

---
