```yaml
number: 13780
title: "[DO NOT LAND] just testing CI"
type: pull_request
state: closed
author: carljm
labels: []
assignees: []
draft: true
base: main
head: cjm/test-ci
created_at: 2024-10-16T18:18:29Z
updated_at: 2024-10-16T20:41:51Z
url: https://github.com/astral-sh/ruff/pull/13780
synced_at: 2026-01-10T20:59:37Z
```

# [DO NOT LAND] just testing CI

---

_Pull request opened by @carljm on 2024-10-16 18:18_

_No description provided._

---

_Closed by @AlexWaygood on 2024-10-16 19:48_

---

_Reopened by @AlexWaygood on 2024-10-16 19:48_

---

_Closed by @MichaReiser on 2024-10-16 19:49_

---

_Reopened by @MichaReiser on 2024-10-16 19:50_

---

_Comment by @MichaReiser on 2024-10-16 20:00_

The jobs are passing after cleaning the cache. I'm not sure what happened that corrupted the runner caches of multiple jobs https://github.com/astral-sh/ruff/actions/runs/11372809791

---

_Comment by @github-actions[bot] on 2024-10-16 20:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 54 project errors)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/spruceid/siwe-py">spruceid/siwe-py</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/agronholm/anyio">agronholm/anyio</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/encode/httpx">encode/httpx</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (error)</summary>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 54 project errors)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/spruceid/siwe-py">spruceid/siwe-py</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/agronholm/anyio">agronholm/anyio</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/encode/httpx">encode/httpx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/ruff/ruff/ruff)
/home/runner/work/ruff/ruff/ruff: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /home/runner/work/ruff/ruff/ruff)
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-10-16 20:05_

Looks like we've been downgraded to ubuntu 22...

---

_Comment by @AlexWaygood on 2024-10-16 20:07_

> Looks like we've been downgraded to ubuntu 22...

Should we pin the Ubuntu image rather than just using `ubuntu-latest`?

---

_Closed by @carljm on 2024-10-16 20:39_

---

_Branch deleted on 2024-10-16 20:41_

---
