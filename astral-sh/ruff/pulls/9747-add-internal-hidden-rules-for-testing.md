```yaml
number: 9747
title: Add internal hidden rules for testing
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/test-rules-new
created_at: 2024-01-31T19:12:26Z
updated_at: 2024-02-02T08:53:22Z
url: https://github.com/astral-sh/ruff/pull/9747
synced_at: 2026-01-10T22:57:09Z
```

# Add internal hidden rules for testing

---

_Pull request opened by @zanieb on 2024-01-31 19:12_

Updated implementation of https://github.com/astral-sh/ruff/pull/7369 which was left out in the cold.

This was motivated again following changes in #9691 and #9689 where we could not test the changes without actually deprecating or removing rules.

---

Follow-up to discussion in https://github.com/astral-sh/ruff/pull/7210

Moves integration tests from using rules that are transitively in nursery / preview groups to dedicated test rules that only exist during development. These rules always raise violations (they do not require specific file behavior). The rules are not available in production or in the documentation.

Uses features instead of `cfg(test)` for cross-crate support per https://github.com/rust-lang/cargo/issues/8379



---

_Review requested from @MichaReiser by @zanieb on 2024-01-31 19:24_

---

_@zanieb reviewed on 2024-01-31 19:25_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:232 on 2024-01-31 19:25_

Awkward, but this was really hard to do in the parent calling `check_path`.

---

_@zanieb reviewed on 2024-01-31 19:38_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1183 on 2024-01-31 19:38_

Flagging, this is testing the wrong output / selecting the wrong test rule.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/linter.rs`:232 on 2024-01-31 19:39_

Maybe a comment explaining what `fix_iteration` is? What is the significance of its value? And what is the significance of `0`?

---

_@zanieb reviewed on 2024-01-31 19:39_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:1259 on 2024-01-31 19:39_

Flagging, this is a regression in test coverage.

---

_@BurntSushi approved on 2024-01-31 19:40_

Yeah I think I buy it!

---

_@zanieb reviewed on 2024-01-31 19:42_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:232 on 2024-01-31 19:42_

I don't think this is right because we will exclude diagnostics that are not fixed... need to investigate

---

_Comment by @github-actions[bot] on 2024-01-31 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 43 project errors)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-preview --select PYI</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/spruceid/siwe-py">spruceid/siwe-py</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (error)</summary>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 43 project errors)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview --select PYI</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/spruceid/siwe-py">spruceid/siwe-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview --select ALL</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'

For more information, try '--help'.
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+479 -480 lines in 147 files in 4 projects; 1 project error; 38 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+455 -461 lines across 138 files)</summary>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/asv_bench/benchmarks/frame_methods.py#L523'>asv_bench/benchmarks/frame_methods.py~L523</a>
```diff
         self.df = DataFrame(np.random.randn(1000, 100))
 
         self.s = Series(np.arange(1028.0))
-        self.df2 = DataFrame(dict.fromkeys(range(1028), self.s))
+        self.df2 = DataFrame({i: self.s for i in range(1028)})
         self.df3 = DataFrame(np.random.randn(1000, 3), columns=list("ABC"))
 
     def time_apply_user_func(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/asv_bench/benchmarks/groupby.py#L511'>asv_bench/benchmarks/groupby.py~L511</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/asv_bench/benchmarks/groupby.py#L950'>asv_bench/benchmarks/groupby.py~L950</a>
```diff
         self.df4["jim"] = self.df4["joe"]
 
     def time_transform_lambda_max(self):
-        self.df.groupby(level="lev1").transform(max)
+        self.df.groupby(level="lev1").transform(lambda x: max(x))
 
     def time_transform_str_max(self):
         self.df.groupby(level="lev1").transform("max")
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/__init__.py#L268'>pandas/__init__.py~L268</a>
```diff
 # Pandas is not (yet) a py.typed library: the public API is determined
 # based on the documentation.
 __all__ = [
-    "NA",
     "ArrowDtype",
     "BooleanDtype",
     "Categorical",
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/__init__.py#L287'>pandas/__init__.py~L287</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/__init__.py#L307'>pandas/__init__.py~L307</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/__init__.py#L323'>pandas/__init__.py~L323</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_config/__init__.py#L7'>pandas/_config/__init__.py~L7</a>
```diff
 """
 __all__ = [
     "config",
+    "describe_option",
     "detect_console_encoding",
     "get_option",
-    "set_option",
-    "reset_option",
-    "describe_option",
     "option_context",
     "options",
+    "reset_option",
+    "set_option",
     "using_copy_on_write",
     "warn_copy_on_write",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_libs/__init__.py#L1'>pandas/_libs/__init__.py~L1</a>
```diff
 __all__ = [
+    "Interval",
     "NaT",
     "NaTType",
     "OutOfBoundsDatetime",
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_libs/__init__.py#L6'>pandas/_libs/__init__.py~L6</a>
```diff
     "Timedelta",
     "Timestamp",
     "iNaT",
-    "Interval",
 ]
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_libs/tslibs/__init__.py#L1'>pandas/_libs/tslibs/__init__.py~L1</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_testing/__init__.py#L342'>pandas/_testing/__init__.py~L342</a>
```diff
         # ensure we don't rely on the property returning a class
         # See https://github.com/pandas-dev/pandas/pull/46018 and
         # https://github.com/pandas-dev/pandas/issues/32638 and linked issues
-        return lambda *args, **kwargs: SubclassedSeries(*args, **kwargs)
+        return SubclassedSeries
 
     @property
     def _constructor_expanddim(self):
-        return lambda *args, **kwargs: SubclassedDataFrame(*args, **kwargs)
+        return SubclassedDataFrame
 
 
 class SubclassedDataFrame(DataFrame):
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_testing/__init__.py#L354'>pandas/_testing/__init__.py~L354</a>
```diff
 
     @property
     def _constructor(self):
-        return lambda *args, **kwargs: SubclassedDataFrame(*args, **kwargs)
+        return SubclassedDataFrame
 
     @property
     def _constructor_sliced(self):
-        return lambda *args, **kwargs: SubclassedSeries(*args, **kwargs)
+        return SubclassedSeries
 
 
 def convert_rows_list_to_csv_str(rows_list: list[str]) -> str:
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_testing/__init__.py#L560'>pandas/_testing/__init__.py~L560</a>
```diff
     "ALL_INT_NUMPY_DTYPES",
     "ALL_NUMPY_DTYPES",
     "ALL_REAL_NUMPY_DTYPES",
+    "BOOL_DTYPES",
+    "BYTES_DTYPES",
+    "COMPLEX_DTYPES",
+    "DATETIME64_DTYPES",
+    "ENDIAN",
+    "FLOAT_EA_DTYPES",
+    "FLOAT_NUMPY_DTYPES",
+    "NARROW_NP_DTYPES",
+    "NP_NAT_OBJECTS",
+    "NULL_OBJECTS",
+    "OBJECT_DTYPES",
+    "SIGNED_INT_EA_DTYPES",
+    "SIGNED_INT_NUMPY_DTYPES",
+    "STRING_DTYPES",
+    "TIMEDELTA64_DTYPES",
+    "UNSIGNED_INT_EA_DTYPES",
+    "UNSIGNED_INT_NUMPY_DTYPES",
+    "SubclassedDataFrame",
+    "SubclassedSeries",
     "assert_almost_equal",
     "assert_attr_equal",
     "assert_categorical_equal",
     "assert_class_equal",
     "assert_contains_all",
     "assert_copy",
+    "assert_cow_warning",
     "assert_datetime_array_equal",
     "assert_dict_equal",
     "assert_equal",
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_testing/__init__.py#L583'>pandas/_testing/__init__.py~L583</a>
```diff
     "assert_series_equal",
     "assert_sp_array_equal",
     "assert_timedelta_array_equal",
-    "assert_cow_warning",
     "at",
-    "BOOL_DTYPES",
     "box_expected",
-    "BYTES_DTYPES",
     "can_set_locale",
-    "COMPLEX_DTYPES",
     "convert_rows_list_to_csv_str",
-    "DATETIME64_DTYPES",
     "decompress_file",
-    "ENDIAN",
     "ensure_clean",
     "external_error_raised",
-    "FLOAT_EA_DTYPES",
-    "FLOAT_NUMPY_DTYPES",
     "get_cython_table_params",
     "get_dtype",
-    "getitem",
-    "get_locales",
     "get_finest_unit",
+    "get_locales",
     "get_obj",
     "get_op_from_name",
+    "getitem",
     "iat",
     "iloc",
     "loc",
     "maybe_produces_warning",
-    "NARROW_NP_DTYPES",
-    "NP_NAT_OBJECTS",
-    "NULL_OBJECTS",
-    "OBJECT_DTYPES",
     "raise_assert_detail",
     "raises_chained_assignment_error",
     "round_trip_pathlib",
     "round_trip_pickle",
-    "setitem",
     "set_locale",
     "set_timezone",
+    "setitem",
     "shares_memory",
-    "SIGNED_INT_EA_DTYPES",
-    "SIGNED_INT_NUMPY_DTYPES",
-    "STRING_DTYPES",
-    "SubclassedDataFrame",
-    "SubclassedSeries",
-    "TIMEDELTA64_DTYPES",
     "to_array",
-    "UNSIGNED_INT_EA_DTYPES",
-    "UNSIGNED_INT_NUMPY_DTYPES",
     "use_numexpr",
     "with_csv_dialect",
     "write_to_compressed",
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_testing/asserters.py#L751'>pandas/_testing/asserters.py~L751</a>
```diff
         and atol is lib.no_default
     ):
         check_exact = (
-            is_numeric_dtype(left.dtype)
-            and not is_float_dtype(left.dtype)
-            or is_numeric_dtype(right.dtype)
-            and not is_float_dtype(right.dtype)
-        )
+            is_numeric_dtype(left.dtype) and not is_float_dtype(left.dtype)
+        ) or (is_numeric_dtype(right.dtype) and not is_float_dtype(right.dtype))
     elif check_exact is lib.no_default:
         check_exact = False
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/_testing/asserters.py#L908'>pandas/_testing/asserters.py~L908</a>
```diff
         and atol is lib.no_default
     ):
         check_exact = (
-            is_numeric_dtype(left.dtype)
-            and not is_float_dtype(left.dtype)
-            or is_numeric_dtype(right.dtype)
-            and not is_float_dtype(right.dtype)
-        )
+            is_numeric_dtype(left.dtype) and not is_float_dtype(left.dtype)
+        ) or (is_numeric_dtype(right.dtype) and not is_float_dtype(right.dtype))
     elif check_exact is lib.no_default:
         check_exact = False
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/api/__init__.py#L8'>pandas/api/__init__.py~L8</a>
```diff
 )
 
 __all__ = [
-    "interchange",
     "extensions",
     "indexers",
+    "interchange",
     "types",
     "typing",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/api/extensions/__init__.py#L21'>pandas/api/extensions/__init__.py~L21</a>
```diff
 )
 
 __all__ = [
-    "no_default",
+    "ExtensionArray",
     "ExtensionDtype",
-    "register_extension_dtype",
+    "ExtensionScalarOpsMixin",
+    "no_default",
     "register_dataframe_accessor",
+    "register_extension_dtype",
     "register_index_accessor",
     "register_series_accessor",
     "take",
-    "ExtensionArray",
-    "ExtensionScalarOpsMixin",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/api/indexers/__init__.py#L10'>pandas/api/indexers/__init__.py~L10</a>
```diff
 )
 
 __all__ = [
-    "check_array_indexer",
     "BaseIndexer",
     "FixedForwardWindowIndexer",
     "VariableOffsetWindowIndexer",
+    "check_array_indexer",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/api/interchange/__init__.py#L5'>pandas/api/interchange/__init__.py~L5</a>
```diff
 from pandas.core.interchange.dataframe_protocol import DataFrame
 from pandas.core.interchange.from_dataframe import from_dataframe
 
-__all__ = ["from_dataframe", "DataFrame"]
+__all__ = ["DataFrame", "from_dataframe"]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/api/types/__init__.py#L14'>pandas/api/types/__init__.py~L14</a>
```diff
 )
 
 __all__ = [
-    "infer_dtype",
-    "union_categoricals",
     "CategoricalDtype",
     "DatetimeTZDtype",
     "IntervalDtype",
     "PeriodDtype",
+    "infer_dtype",
+    "union_categoricals",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/api/typing/__init__.py#L39'>pandas/api/typing/__init__.py~L39</a>
```diff
     "ExponentialMovingWindow",
     "ExponentialMovingWindowGroupby",
     "JsonReader",
-    "NaTType",
     "NAType",
+    "NaTType",
     "PeriodIndexResamplerGroupby",
     "Resampler",
     "Rolling",
     "RollingGroupby",
     "SeriesGroupBy",
     "StataReader",
+    "TimeGrouper",
     # See TODO above
     # "Styler",
     "TimedeltaIndexResamplerGroupby",
-    "TimeGrouper",
     "Window",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/compat/__init__.py#L180'>pandas/compat/__init__.py~L180</a>
```diff
 
 
 __all__ = [
-    "is_numpy_dev",
-    "pa_version_under10p1",
-    "pa_version_under11p0",
-    "pa_version_under13p0",
-    "pa_version_under14p0",
-    "pa_version_under14p1",
     "IS64",
     "ISMUSL",
     "PY310",
     "PY311",
     "PY312",
     "PYPY",
+    "is_numpy_dev",
+    "pa_version_under10p1",
+    "pa_version_under11p0",
+    "pa_version_under13p0",
+    "pa_version_under14p0",
+    "pa_version_under14p1",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/compat/numpy/__init__.py#L46'>pandas/compat/numpy/__init__.py~L46</a>
```diff
 
 
 __all__ = [
-    "np",
     "_np_version",
     "is_numpy_dev",
+    "np",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/_numba/extensions.py#L277'>pandas/core/_numba/extensions.py~L277</a>
```diff
     """Converts numba UnicodeCharSeq (numpy string scalar) -> unicode type (string).
     Is a no-op for other types."""
     if isinstance(x, types.UnicodeCharSeq):
-        return lambda x: str(x)
+        return str
     else:
         return lambda x: x
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/_numba/kernels/__init__.py#L16'>pandas/core/_numba/kernels/__init__.py~L16</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/api.py#L81'>pandas/core/api.py~L81</a>
```diff
 from pandas.core.frame import DataFrame  # isort:skip
 
 __all__ = [
-    "array",
+    "NA",
     "ArrowDtype",
-    "bdate_range",
     "BooleanDtype",
     "Categorical",
     "CategoricalDtype",
     "CategoricalIndex",
     "DataFrame",
     "DateOffset",
-    "date_range",
     "DatetimeIndex",
     "DatetimeTZDtype",
-    "factorize",
     "Flags",
     "Float32Dtype",
     "Float64Dtype",
     "Grouper",
     "Index",
     "IndexSlice",
+    "Int8Dtype",
     "Int16Dtype",
     "Int32Dtype",
     "Int64Dtype",
-    "Int8Dtype",
     "Interval",
     "IntervalDtype",
     "IntervalIndex",
-    "interval_range",
-    "isna",
-    "isnull",
     "MultiIndex",
-    "NA",
-    "NamedAgg",
     "NaT",
-    "notna",
-    "notnull",
+    "NamedAgg",
     "Period",
     "PeriodDtype",
     "PeriodIndex",
-    "period_range",
     "RangeIndex",
     "Series",
-    "set_eng_float_format",
     "StringDtype",
     "Timedelta",
     "TimedeltaIndex",
-    "timedelta_range",
     "Timestamp",
-    "to_datetime",
-    "to_numeric",
-    "to_timedelta",
+    "UInt8Dtype",
     "UInt16Dtype",
     "UInt32Dtype",
     "UInt64Dtype",
-    "UInt8Dtype",
+    "array",
+    "bdate_range",
+    "date_range",
+    "factorize",
+    "interval_range",
+    "isna",
+    "isnull",
+    "notna",
+    "notnull",
+    "period_range",
+    "set_eng_float_format",
+    "timedelta_range",
+    "to_datetime",
+    "to_numeric",
+    "to_timedelta",
     "unique",
     "value_counts",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/arrays/arrow/__init__.py#L4'>pandas/core/arrays/arrow/__init__.py~L4</a>
```diff
 )
 from pandas.core.arrays.arrow.array import ArrowExtensionArray
 
-__all__ = ["ArrowExtensionArray", "StructAccessor", "ListAccessor"]
+__all__ = ["ArrowExtensionArray", "ListAccessor", "StructAccessor"]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/arrays/arrow/array.py#L165'>pandas/core/arrays/arrow/array.py~L165</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/arrays/arrow/array.py#L1401'>pandas/core/arrays/arrow/array.py~L1401</a>
```diff
             pa.types.is_floating(pa_type)
             and (
                 na_value is np.nan
-                or original_na_value is lib.no_default
-                and is_float_dtype(dtype)
+                or (original_na_value is lib.no_default
+                and is_float_dtype(dtype))
             )
         ):
             result = data._pa_array.to_numpy()
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/arrays/arrow/array.py#L2505'>pandas/core/arrays/arrow/array.py~L2505</a>
```diff
 
     def _str_findall(self, pat: str, flags: int = 0) -> Self:
         regex = re.compile(pat, flags=flags)
-        predicate = lambda val: regex.findall(val)
+        predicate = regex.findall
         result = self._apply_elementwise(predicate)
         return type(self)(pa.chunked_array(result))
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/common.py#L144'>pandas/core/common.py~L144</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/computation/eval.py#L340'>pandas/core/computation/eval.py~L340</a>
```diff
 
         if engine == "numexpr" and (
             is_extension_array_dtype(parsed_expr.terms.return_type)
-            or getattr(parsed_expr.terms, "operand_types", None) is not None
-            and any(
-                is_extension_array_dtype(elem)
-                for elem in parsed_expr.terms.operand_types
+            or (
+                getattr(parsed_expr.terms, "operand_types", None) is not None
+                and any(
+                    is_extension_array_dtype(elem)
+                    for elem in parsed_expr.terms.operand_types
+                )
             )
         ):
             warnings.warn(
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/computation/expr.py#L507'>pandas/core/computation/expr.py~L507</a>
```diff
             )
 
         if self.engine != "pytables" and (
-            res.op in CMP_OPS_SYMS
-            and getattr(lhs, "is_datetime", False)
+            (res.op in CMP_OPS_SYMS and getattr(lhs, "is_datetime", False))
             or getattr(rhs, "is_datetime", False)
         ):
             # all date ops must be done in python bc numexpr doesn't work
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/computation/expr.py#L534'>pandas/core/computation/expr.py~L534</a>
```diff
         return self._maybe_evaluate_binop(op, op_class, left, right)
 
     def visit_Div(self, node, **kwargs):
-        return lambda lhs, rhs: Div(lhs, rhs)
+        return Div
 
     def visit_UnaryOp(self, node, **kwargs):
         op = self.visit(node.op)
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/computation/pytables.py#L407'>pandas/core/computation/pytables.py~L407</a>
```diff
         operand = operand.prune(klass)
 
         if operand is not None and (
-            issubclass(klass, ConditionBinOp)
-            and operand.condition is not None
-            or not issubclass(klass, ConditionBinOp)
-            and issubclass(klass, FilterBinOp)
-            and operand.filter is not None
+            (issubclass(klass, ConditionBinOp) and operand.condition is not None)
+            or (
+                not issubclass(klass, ConditionBinOp)
+                and issubclass(klass, FilterBinOp)
+                and operand.filter is not None
+            )
         ):
             return operand.invert()
         return None
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/computation/scope.py#L139'>pandas/core/computation/scope.py~L139</a>
```diff
     temps : dict
     """
 
-    __slots__ = ["level", "scope", "target", "resolvers", "temps"]
+    __slots__ = ["level", "resolvers", "scope", "target", "temps"]
     level: int
     scope: DeepChainMap
     resolvers: DeepChainMap
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/dtypes/common.py#L1676'>pandas/core/dtypes/common.py~L1676</a>
```diff
 
 
 __all__ = [
-    "classes",
     "DT64NS_DTYPE",
+    "INT64_DTYPE",
+    "TD64NS_DTYPE",
+    "classes",
     "ensure_float64",
     "ensure_python_int",
     "ensure_str",
     "infer_dtype_from_object",
-    "INT64_DTYPE",
     "is_1d_only_ea_dtype",
     "is_all_strings",
     "is_any_real_numeric_dtype",
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/dtypes/common.py#L1728'>pandas/core/dtypes/common.py~L1728</a>
```diff
     "is_unsigned_integer_dtype",
     "needs_i8_conversion",
     "pandas_dtype",
-    "TD64NS_DTYPE",
     "validate_all_hashable",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/dtypes/dtypes.py#L1058'>pandas/core/dtypes/dtypes.py~L1058</a>
```diff
         possible
         """
         if (
-            isinstance(string, str)
-            and (string.startswith(("period[", "Period[")))
-            or isinstance(string, BaseOffset)
-        ):
+            isinstance(string, str) and (string.startswith(("period[", "Period[")))
+        ) or isinstance(string, BaseOffset):
             # do not parse string like U as period[U]
             # avoid tuple to be regarded as freq
             try:
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/dtypes/dtypes.py#L1713'>pandas/core/dtypes/dtypes.py~L1713</a>
```diff
                 fill_value = (
                     other._is_na_fill_value
                     and isinstance(self.fill_value, type(other.fill_value))
-                    or isinstance(other.fill_value, type(self.fill_value))
-                )
+                ) or isinstance(other.fill_value, type(self.fill_value))
             else:
                 with warnings.catch_warnings():
                     # Ignore spurious numpy warning
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/frame.py#L3996'>pandas/core/frame.py~L3996</a>
```diff
             # GH#45316 Return view if key is not duplicated
             # Only use drop_duplicates with duplicates for performance
             if not is_mi and (
-                self.columns.is_unique
-                and key in self.columns
+                (self.columns.is_unique and key in self.columns)
                 or key in self.columns.drop_duplicates(keep=False)
             ):
                 return self._get_item_cache(key)
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/frame.py#L5193'>pandas/core/frame.py~L5193</a>
```diff
             isinstance(value, Index)
             and value.dtype == "object"
             and arr.dtype != value.dtype
-        ):  #
+        ):
             # TODO: Remove kludge in sanitize_array for string mode when enforcing
             # this deprecation
             warnings.warn(
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/frame.py#L6855'>pandas/core/frame.py~L6855</a>
```diff
         elif (
             not np.iterable(subset)
             or isinstance(subset, str)
-            or isinstance(subset, tuple)
-            and subset in self.columns
+            or (isinstance(subset, tuple) and subset in self.columns)
         ):
             subset = (subset,)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/generic.py#L6375'>pandas/core/generic.py~L6375</a>
```diff
         -------
         consolidated : same type as caller
         """
-        f = lambda: self._mgr.consolidate()
+        f = self._mgr.consolidate
         cons_data = self._protect_consolidate(f)
         return self._constructor_from_mgr(cons_data, axes=cons_data.axes).__finalize__(
             self
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/generic.py#L10373'>pandas/core/generic.py~L10373</a>
```diff
                 # self
                 cons = self._constructor_expanddim
                 df = cons(
-                    {c: self for c in other.columns}, **other._construct_axes_dict()
+                    dict.fromkeys(other.columns, self), **other._construct_axes_dict()
                 )
                 # error: Incompatible return value type (got "Tuple[DataFrame,
                 # DataFrame]", expected "Tuple[Self, NDFrameT]")
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/generic.py#L10393'>pandas/core/generic.py~L10393</a>
```diff
                 # other
                 cons = other._constructor_expanddim
                 df = cons(
-                    {c: other for c in self.columns}, **self._construct_axes_dict()
+                    dict.fromkeys(self.columns, other), **self._construct_axes_dict()
                 )
                 # error: Incompatible return value type (got "Tuple[NDFrameT,
                 # DataFrame]", expected "Tuple[Self, NDFrameT]")
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/generic.py#L10619'>pandas/core/generic.py~L10619</a>
```diff
             # CoW: Make sure reference is not kept alive
             if cond.ndim == 1 and self.ndim == 2:
                 cond = cond._constructor_expanddim(
-                    {i: cond for i in range(len(self.columns))},
+                    dict.fromkeys(range(len(self.columns)), cond),
                     copy=False,
                 )
                 cond.columns = self.columns
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/groupby/__init__.py#L8'>pandas/core/groupby/__init__.py~L8</a>
```diff
 
 __all__ = [
     "DataFrameGroupBy",
-    "NamedAgg",
-    "SeriesGroupBy",
     "GroupBy",
     "Grouper",
+    "NamedAgg",
+    "SeriesGroupBy",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/groupby/groupby.py#L927'>pandas/core/groupby/groupby.py~L927</a>
```diff
             # possibly convert to the actual key types
             # in the indices, could be a Timestamp or a np.datetime64
             if isinstance(s, datetime.datetime):
-                return lambda key: Timestamp(key)
+                return Timestamp
             elif isinstance(s, np.datetime64):
                 return lambda key: Timestamp(key).asm8
             else:
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/groupby/groupby.py#L5228'>pandas/core/groupby/groupby.py~L5228</a>
```diff
         else:
             to_coerce = [c for c, dtype in obj.dtypes.items() if dtype in dtypes_to_f32]
             if len(to_coerce):
-                shifted = shifted.astype({c: "float32" for c in to_coerce})
+                shifted = shifted.astype(dict.fromkeys(to_coerce, "float32"))
 
         return obj - shifted
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexers/__init__.py#L15'>pandas/core/indexers/__init__.py~L15</a>
```diff
 )
 
 __all__ = [
-    "is_valid_positional_slice",
+    "check_array_indexer",
+    "check_key_length",
+    "check_setitem_lengths",
+    "disallow_ndim_indexing",
+    "is_empty_indexer",
     "is_list_like_indexer",
     "is_scalar_indexer",
-    "is_empty_indexer",
-    "check_setitem_lengths",
-    "validate_indices",
-    "maybe_convert_indices",
+    "is_valid_positional_slice",
     "length_of_indexer",
-    "disallow_ndim_indexing",
+    "maybe_convert_indices",
     "unpack_1tuple",
-    "check_key_length",
-    "check_array_indexer",
     "unpack_tuple_and_ellipses",
+    "validate_indices",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexes/api.py#L47'>pandas/core/indexes/api.py~L47</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexes/base.py#L7667'>pandas/core/indexes/base.py~L7667</a>
```diff
         index_like = list(index_like)
 
     if isinstance(index_like, list):
-        if type(index_like) is not list:
+        if type(index_like) is not list:  # noqa: E721
             # must check for exactly list here because of strict type
             # check in clean_index_list
             index_like = list(index_like)
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexes/range.py#L1039'>pandas/core/indexes/range.py~L1039</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexing.py#L1236'>pandas/core/indexing.py~L1236</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexing.py#L2130'>pandas/core/indexing.py~L2130</a>
```diff
 
         is_full_setter = com.is_null_slice(pi) or com.is_full_slice(pi, len(self.obj))
 
-        is_null_setter = com.is_empty_slice(pi) or is_array_like(pi) and len(pi) == 0
+        is_null_setter = com.is_empty_slice(pi) or (is_array_like(pi) and len(pi) == 0)
 
         if is_null_setter:
             # no-op, don't cast dtype later
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/indexing.py#L2763'>pandas/core/indexing.py~L2763</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/internals/__init__.py#L11'>pandas/core/internals/__init__.py~L11</a>
```diff
 
 __all__ = [
     "Block",  # pylint: disable=undefined-all-variable
+    "BlockManager",
+    "DataManager",
     "DatetimeTZBlock",  # pylint: disable=undefined-all-variable
     "ExtensionBlock",  # pylint: disable=undefined-all-variable
-    "make_block",
-    "DataManager",
-    "BlockManager",
-    "SingleDataManager",
     "SingleBlockManager",
+    "SingleDataManager",
     "concatenate_managers",
+    "make_block",
 ]
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/internals/concat.py#L183'>pandas/core/internals/concat.py~L183</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/internals/construction.py#L408'>pandas/core/internals/construction.py~L408</a>
```diff
             else x.copy(deep=True)
             if (
                 isinstance(x, Index)
-                or isinstance(x, ABCSeries)
-                and is_1d_only_ea_dtype(x.dtype)
+                or (isinstance(x, ABCSeries) and is_1d_only_ea_dtype(x.dtype))
             )
             else x
             for x in arrays
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/internals/construction.py#L834'>pandas/core/internals/construction.py~L834</a>
```diff
 
     # assure that they are of the base dict class and not of derived
     # classes
-    data = [d if type(d) is dict else dict(d) for d in data]  # noqa: E721
+    data = [d if type(d) is dict else dict(d) for d in data]
 
     content = lib.dicts_to_array(data, list(columns))
     return content, columns
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/ops/__init__.py#L65'>pandas/core/ops/__init__.py~L65</a>
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
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/ops/__init__.py#L87'>pandas/core/ops/__init__.py~L87</a>
```diff
     "rtruediv",
     "rxor",
     "unpack_zerodim_and_defer",
-    "get_op_result_name",
-    "maybe_prepare_scalar_for_op",
-    "get_array_op",
 ]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/resample.py#L2152'>pandas/core/resample.py~L2152</a>
```diff
             raise ValueError(f"Unsupported value {convention} for `convention`")
 
         if (
-            key is None
+            (key is None
             and obj is not None
-            and isinstance(obj.index, PeriodIndex)  # type: ignore[attr-defined]
+            and isinstance(obj.index, PeriodIndex))  # type: ignore[attr-defined]
             or (
                 key is not None
                 and obj is not None
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/reshape/merge.py#L422'>pandas/core/reshape/merge.py~L422</a>
```diff
         check = set(left_by).difference(left.columns)
         if len(check) != 0:
             raise KeyError(f"{check} not found in left columns")
-        result, _ = _groupby_and_merge(left_by, left, right, lambda x, y: _merger(x, y))
+        result, _ = _groupby_and_merge(left_by, left, right, _merger)
     elif right_by is not None:
         if isinstance(right_by, str):
             right_by = [right_by]
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/reshape/merge.py#L2519'>pandas/core/reshape/merge.py~L2519</a>
```diff
             isinstance(lk.dtype, ArrowDtype)
             and (
                 is_numeric_dtype(lk.dtype.numpy_dtype)
-                or is_string_dtype(lk.dtype)
-                and not sort
+                or (is_string_dtype(lk.dtype) and not sort)
             )
         ):
             lk, _ = lk._values_for_factorize()
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/tools/datetimes.py#L126'>pandas/core/tools/datetimes.py~L126</a>
```diff
 def _guess_datetime_format_for_array(arr, dayfirst: bool | None = False) -> str | None:
     # Try to guess the format based on the first non-NaN element, return None if can't
     if (first_non_null := tslib.first_non_null(arr)) != -1:
-        if type(first_non_nan_element := arr[first_non_null]) is str:  # noqa: E721
+        if type(first_non_nan_element := arr[first_non_null]) is str:
             # GH#32264 np.str_ object
             guessed_format = guess_datetime_format(
                 first_non_nan_element, dayfirst=dayfirst
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/tools/numeric.py#L234'>pandas/core/tools/numeric.py~L234</a>
```diff
                 set(),
                 coerce_numeric=coerce_numeric,
                 convert_to_masked_nullable=dtype_backend is not lib.no_default
-                or isinstance(values_dtype, StringDtype)
-                and not values_dtype.storage == "pyarrow_numpy",
+                or (isinstance(values_dtype, StringDtype)
+                and not values_dtype.storage == "pyarrow_numpy"),
             )
         except (ValueError, TypeError):
             if errors == "raise":
```
<a href='https://github.com/pandas-dev/pandas/blob/c3014abb3bf2e15fa66d194ebd2867161527c497/pandas/core/tools/numeric.py#L247'>pandas/core/tools/numeric.py~L247</a>
```diff
         # downcasting
         values = values[~new_mask]
     elif (
-        dtype_backend is not lib.no_default
-        and new_mask is None
-        or isinstance(values_dtype, StringDtype)
-        and not values_dtype.storage == "pyarrow_numpy"
+        (dtype_backend is not lib.no_default
+        and new_mask is None)
+        or (isinstance(values_dtype, StringDtype)
+        and not values_dtype.storage == "...*[Comment body truncated]*

---

_@zanieb reviewed on 2024-01-31 21:14_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:604 on 2024-01-31 21:14_

This is ugly and I do not think it is working correctly — the snapshots are limited to a single fix now but they include diagnostics for the "fixed" test rules

---

_@zanieb reviewed on 2024-01-31 21:15_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:604 on 2024-01-31 21:15_

(Because we do not filter these downstream in `result.map`)

---

_@zanieb reviewed on 2024-01-31 22:24_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 22:24_

Why is this so awkward Rust... ?

---

_@BurntSushi reviewed on 2024-01-31 22:26_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 22:26_

I'd probably do

```rust
if !settings.rules.enabled(*test_rule) {
    continue;
}
```

but it otherwise doesn't look too bad?

---

_@zanieb reviewed on 2024-01-31 22:29_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 22:29_

I don't get why I can't call `::diagnostic` once since I know all of the objects implement `TestRule`.

I have to do some weird dynamic boxing to get to that point?

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 22:39_

I don't follow. Can you show me the code you _want_ to write? It looks like you're calling a bunch of different impls of `diagnostic` based on which `Rule` you have? Each line is doing the same function call, but it's on a different type.

---

_@BurntSushi reviewed on 2024-01-31 22:40_

---

_@zanieb reviewed on 2024-01-31 22:43_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 22:43_

`diagnostic` is provided by the `TestRule` trait which all of these rules implement. For a while I had...

```rust
        for test_rule in TEST_RULES {
            if settings.rules.enabled(*test_rule) {
                if let Some(diagnostic) =
                    implementation_for_test_rule(*test_rule).diagnostic(&locator, &indexer)
                {
                    diagnostics.push(diagnostic);
                }
            }
        }
```

but that required the slightly awkward

```rust
pub(crate) fn implementation_for_test_rule(rule: Rule) -> Box<dyn TestRule>{
    match rule {
        Rule::StableTestRule => Box::new(StableTestRule),
        Rule::StableTestRuleSafeFix => Box::new(StableTestRuleSafeFix),
        Rule::StableTestRuleUnsafeFix => Box::new(StableTestRuleUnsafeFix),
        Rule::StableTestRuleDisplayOnlyFix => Box::new(StableTestRuleDisplayOnlyFix),
        Rule::NurseryTestRule => Box::new(NurseryTestRule),
        Rule::PreviewTestRule => Box::new(PreviewTestRule),
        _ => unreachable!("All test rules must have an implementation"),
    }
}

```

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 22:44_

I was like.. why am I importing `TEST_RULES` and `implementation_for_test_rule` from `test_rules` I should just do something like `iter_test_rules() -> &[Rule, Box<dyn TestRule>]` (I actually tried implementing this as an `Iterator` too) and that's when I ran into horrible cyclical compiler errors and gave up in favor of this manual approach.

---

_@zanieb reviewed on 2024-01-31 22:44_

---

_Comment by @zanieb on 2024-01-31 22:48_

The ecosystem checks won't succeed until this is main because `RUF9XX` does not exist on the baseline binary.

---

_@BurntSushi reviewed on 2024-01-31 23:09_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 23:09_

I think any way you slice things here, you need a match statement over a `Rule`. I don't see a way around that because something has to tell the compiler which impl of `TestRule` to use, and _that_ is determined by the `Rule`. Even if you wrote `iter_test_rules() -> &[Rule, Box<dyn TestRule>]`, you'd still need a match statement in there.

I think where you ended up is fine IMO.

---

_Review comment by @charliermarsh on `crates/ruff/Cargo.toml`:58 on 2024-01-31 23:13_

@BurntSushi - Can you double-confirm for me that these won't enable in non-`cargo test` builds?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-01-31 23:14_

Wish there were something better here (e.g., what if we could use the rule source, and have a special `lint_source()` called `test`), but might be difficult.

---

_Review comment by @charliermarsh on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:211 on 2024-01-31 23:14_

Why / how do these get enabled here? Why is this necessary?

---

_@charliermarsh approved on 2024-01-31 23:15_

---

_@charliermarsh reviewed on 2024-01-31 23:16_

---

_Review comment by @charliermarsh on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:211 on 2024-01-31 23:16_

Like how does the feature end up being enabled?

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-01-31 23:39_

We could add something to the macro but it seems like more trouble than it's worth at this point

---

_@zanieb reviewed on 2024-01-31 23:39_

---

_@zanieb reviewed on 2024-01-31 23:41_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:211 on 2024-01-31 23:41_

I believe we get our ecosystem binaries from the test builds, which have the development features enabled? I think they have to be turned on in the `ruff` binary because we use that for integration tests.

It'd be nice if they were not present here, but do we want to have a separate build for the ecosystem checks?

---

_@zanieb reviewed on 2024-01-31 23:41_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:248 on 2024-01-31 23:41_

Okay thanks!

---

_@zanieb reviewed on 2024-01-31 23:56_

---

_Review comment by @zanieb on `crates/ruff/Cargo.toml`:58 on 2024-01-31 23:56_

If I do `cargo build --release --all-features` they are present

```
❯ ./target/release/ruff check example.py --select RUF
example.py:1:1: RUF900 Hey this is a stable test rule.
example.py:1:1: RUF901 [*] Hey this is a stable test rule with a safe fix.
example.py:1:1: RUF902 Hey this is a stable test rule with an unsafe fix.
example.py:1:1: RUF903 Hey this is a stable test rule with a display only fix.
Found 4 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

but they are not present without `--all-features` in the `release` or `debug` targets.

---

_@zanieb reviewed on 2024-01-31 23:58_

---

_Review comment by @zanieb on `crates/ruff/Cargo.toml`:58 on 2024-01-31 23:58_

Idk what features we include in releases, it's buried in maturin?

---

_@zanieb reviewed on 2024-02-01 00:00_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 00:00_

Should we just put them all in a separate lint group e.g. `RFT` for "Ruff test"? Seems safer and easier to filter in general?

---

_@zanieb reviewed on 2024-02-01 00:00_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 00:00_

Might end up being more work to implement overall though?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 00:13_

I'm actually wondering how these even up getting included in the schema, do we run with `--all-features` or something?

---

_@charliermarsh reviewed on 2024-02-01 00:13_

---

_@charliermarsh reviewed on 2024-02-01 00:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 00:14_

We should verify that `--select RUF9` doesn't work with the production build (i.e., selecting by a prefix that only applies to test rules).

---

_@charliermarsh reviewed on 2024-02-01 00:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 00:14_

Anyway, I'm cool with whatever here.

---

_@charliermarsh reviewed on 2024-02-01 00:15_

---

_Review comment by @charliermarsh on `crates/ruff/Cargo.toml`:58 on 2024-02-01 00:15_

We don't specify anything, so we get the default features.

---

_@charliermarsh reviewed on 2024-02-01 00:15_

---

_Review comment by @charliermarsh on `crates/ruff/Cargo.toml`:58 on 2024-02-01 00:15_

(Which should omit these.)

---

_@zanieb reviewed on 2024-02-01 03:16_

---

_Review comment by @zanieb on `crates/ruff/Cargo.toml`:58 on 2024-02-01 03:16_

Oh great so I think we're good then

---

_@zanieb reviewed on 2024-02-01 03:18_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 03:18_

Huh this raises a preview warning

```
❯ ./target/debug/ruff check example.py --select RUF9
warning: Selection `RUF9` has no effect because the `--preview` flag was not included.
```

but doesn't do anything else.. `RUF8` errors

```
❯ ./target/debug/ruff check example.py --select RUF8
error: invalid value 'RUF8' for '--select <RULE_CODE>'
```

I guess I'll fix that next..

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 03:23_

Yeah this is actually confusing... why are they included in the schema in the first place?

---

_@zanieb reviewed on 2024-02-01 03:23_

---

_@charliermarsh reviewed on 2024-02-01 03:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 03:36_

They shouldn't even _exist_ when we run `cargo dev generate-all`, right? 

---

_@zanieb reviewed on 2024-02-01 04:20_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rule_selector.rs`:293 on 2024-02-01 04:20_

Okay this was a bug in the proc macro 🎉 
https://github.com/astral-sh/ruff/pull/9747/commits/8247b8582b4b80900a0ef7e984a035283b4558b6 fixes it (but has a mediocre implementation I'm fixing up)

---

_Comment by @zanieb on 2024-02-01 04:34_

The schema generation tests use `--all-features` even though the real schema generation does not 😮‍💨 

---

_Comment by @charliermarsh on 2024-02-01 04:36_

Noooooo

---

_@BurntSushi reviewed on 2024-02-01 12:24_

---

_Review comment by @BurntSushi on `crates/ruff/Cargo.toml`:58 on 2024-02-01 12:24_

Yeah features enabled in `dev-dependencies` should definitely not cross over into standard builds. But yeah as Zanie mentioned, if you have anything that's using `--all-features`, then they will get enabled in that case.

---

_Merged by @zanieb on 2024-02-01 14:44_

---

_Closed by @zanieb on 2024-02-01 14:44_

---

_Branch deleted on 2024-02-01 14:44_

---

_@MichaReiser reviewed on 2024-02-02 08:53_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:152 on 2024-02-02 08:53_

Can we add another run command that runs the test of `ruff_dev` but with the default features only? We otherwise lose the schema test. 

Although I'm seeing test failures locally even when not using `--all-features`, making that test rather annoying. 

A workaround could be to export a `are_testing_rules_enabled` and conditionally implement it depending on the presence of the feature flag and not run the schema test when it's enabled. But that still means that the test won't fail anymore locally and still requires an explicit cargo test `ruff_dev` run :(

---
