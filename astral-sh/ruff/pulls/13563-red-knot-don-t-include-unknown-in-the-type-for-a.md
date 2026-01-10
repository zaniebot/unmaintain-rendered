```yaml
number: 13563
title: "[red-knot] don't include Unknown in the type for a conditionally-defined import"
type: pull_request
state: merged
author: pilleye
labels:
  - ty
assignees: []
merged: true
base: main
head: pilleye/public-known
created_at: 2024-09-30T03:02:57Z
updated_at: 2024-10-18T04:09:10Z
url: https://github.com/astral-sh/ruff/pull/13563
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] don't include Unknown in the type for a conditionally-defined import

---

_Pull request opened by @pilleye on 2024-09-30 03:02_

## Summary

Fixes the bug described in #13514 where an unbound public type defaulted to the type or `Unknown`, whereas it should only be the type if unbound.

## Test Plan

Added a new test case


---

_Review requested from @carljm by @pilleye on 2024-09-30 03:02_

---

_Review requested from @MichaReiser by @pilleye on 2024-09-30 03:02_

---

_Review requested from @AlexWaygood by @pilleye on 2024-09-30 03:02_

---

_Renamed from "[red-knot] don't include Unknown in the type for a conditionally-defined import #13514" to "[red-knot] don't include Unknown in the type for a conditionally-defined import" by @pilleye on 2024-09-30 03:07_

---

_@pilleye reviewed on 2024-09-30 03:10_

---

_Review comment by @pilleye on `crates/red_knot_python_semantic/src/types.rs`:61 on 2024-09-30 03:10_

below this is the case where `use_def.has_public_declarations(symbol)` is false, what would be a situation where we fall into that case?

---

_Comment by @github-actions[bot] on 2024-09-30 03:19_

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

_@Slyces reviewed on 2024-09-30 07:23_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:61 on 2024-09-30 07:23_

```python
if flag:
    x = 1
```
See my comment on #13514

---

_Label `red-knot` added by @AlexWaygood on 2024-09-30 11:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5496 on 2024-09-30 20:39_

We can now remove this comment, and the similar one in two below tests, because the lack of `sys.version_info` handling no longer results in a spurious `Unknown` here.

---

_@carljm approved on 2024-09-30 20:40_

Awesome, thank you!! Just a few comment removals and this looks good to go.

---

_Comment by @carljm on 2024-09-30 21:35_

Oh, I didn't notice tests are failing. We'll need to update the tests, and I'll need to look at the resulting assertion changes to make sure we aren't getting any undesired side effects from this. 

---

_Comment by @pilleye on 2024-09-30 22:09_

Ah, I missed those tests on the latest version, will fix tonight.

---

_Comment by @pilleye on 2024-10-01 01:16_

~~There seems to be a weird side-effect where we're narrowing `Unbound | Unknown` types to just `Unknown` instead of `Unbound`: https://github.com/pilleye/ruff/blob/pilleye/public-known/crates/red_knot_python_semantic/src/types/infer.rs#L5313~~

Misread the case

---

_Comment by @carljm on 2024-10-01 01:25_

So one thing I'm realizing here is that we are using `assert_public_ty` in some tests where we really intend to be testing "what is the local type of this variable at the end of the scope" -- this was natural because those used to be the same thing, but in this PR we are definitively making them different.

So for tests like `basic_for_loop`, for example, what we should really do instead of using `assert_public_ty` is to add a `reveal_type(x)` as the last line in the test code, and then `assert_file_diagnostics` that we get the expected type revealed there, which will include `Unbound`. This will be a bit more verbose for now, but it'll get better in the upcoming test framework.

---

_Comment by @pilleye on 2024-10-01 01:50_

Working through a global-related bugs right now.

We seem to be incorrectly shadowing global builtins with `Never` here: https://github.com/pilleye/ruff/blob/pilleye/public-known/crates/red_knot_python_semantic/src/types/infer.rs#L4431

This is currently narrowed to `Literal[1]`.


---

_Comment by @carljm on 2024-10-01 02:48_

Ah ok, so that's going to be due to the use of `global_symbol_ty` in `TypeInferenceBuilder::lookup_name`. We need to use a function there that _does_ union with unbound (if unbound is possible), so that we can replace unbound with the builtin definition. So the set of functions in `types.rs`: `global_symbol_ty`, `symbol_ty`, `symbol_ty_by_name`, `symbol_ty_by_id` are going to have to be split between the import case and the global-lookup-within-the-module case.

I would probably keep those functions as is, and add a new `global_symbol_lookup` function that a) still inserts `Type::Unbound` in case unbound is possible, and b) should also use only definitions, not declared types, even if there are declarations (so it's like just the second case in `symbol_ty_by_id`). (We may want a new test to verify (b) as well.) We may not even need `global_symbol_lookup` to be a function; I think the only callsite will be in `TypeInferenceBuilder::lookup_name`, so it can probably just be inlined there, since it shouldn't be more than a couple lines.

Let me know if any of that doesn't make sense! Thanks for working through this. I knew the first version of this PR was too simple to be true 😆 

---

_Comment by @pilleye on 2024-10-01 05:39_

Haha the first version seemed suspiciously easy

I think I got the tests working, but definitely feels way jankier than I would want. Would appreciate comments!

I think trying to genericize `symbol_ty_by_id` adds unnecessary complexity, so I might just inline that code as you said.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3369 on 2024-10-01 18:31_

I don't think this is a behavior change we want. If you import a fully undeclared/undefined name, you should get a diagnostic, but the imported name should have type `Unknown` in this case. Otherwise you'll just get lots of useless cascading errors about doing invalid things with `Never`.

There is currently a special case in `TypeInferenceBuilder::infer_import_from_definition` where we transform `Unbound` to `Unknown` -- that's why this test exists. That special case becomes obsolete with this PR (we'll never get `Unbound` there anymore), but I think the special case should be updated to check for `Never`, emit a diagnostic (use `self.add_diagnostic`), and transform the `Never` to `Unknown`. (It's also fine if you want to just add a TODO for the diagnostic and we can add it in a later PR; we'll also want a similar diagnostic that's currently missing in `infer_import_definition`, for a nonexistent module.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4267 on 2024-10-01 18:32_

nit: it's only maybe unbound
```suggestion
    fn resolve_unannotated_maybe_unbound_public() -> anyhow::Result<()> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4256 on 2024-10-01 18:32_

```suggestion
    fn resolve_maybe_undeclared_maybe_unbound_public() -> anyhow::Result<()> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-01 18:34_

This is one of those cases where part of the whole purpose of the test is actually to show that we know which names might be unbound, so we should really update the test such that it doesn't use (only)`assert_public_ty`, because it's really not the public type we want to assert on.

I earlier suggested an approach using `reveal_type`, and I think that's actually a fine option in the tests below where you use it.

I realized a simpler approach in some of these tests (particularly the global-scope ones) might be to keep these `assert_public_ty` but add another helper `assert_may_be_unbound` (which uses the use-def map directly) and explicitly assert here that `r` and `s` may be unbound. This way we actually get both the assertion on the public type, and an assertion that locally we do know the name might be unbound.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4647 on 2024-10-01 18:37_

same as above, should have an assertion in some form that `y` can be unbound here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4771 on 2024-10-01 18:44_

So on further thought and discussion with the team, I think I pointed you in the wrong direction here yesterday; sorry about that! I think we should always use the same logic, respecting both declarations and definitions. (Maybe we will want to consider using only definitions for a not-global nonlocal reference, but that's not a change we should make in this PR.)

I still think the test case you added here is useful! But I would want the assertion to change such that when you add the annotation, you get `Literal[copyright] | int` instead of `Literal[copyright] | Literal[1]`.

And if the assertion changes, it's probably clearer to be a fully separate test rather than a `#[test_case]`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5496 on 2024-10-01 18:46_

another case where it's important that we assert on may-be-unbound

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5602 on 2024-10-01 18:46_

and another

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5688 on 2024-10-01 18:49_

we should assert on maybe-unbound here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5717 on 2024-10-01 18:49_

and here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5864 on 2024-10-01 18:50_

This comment (including the two previous lines GH won't let me comment on) can be removed.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5890 on 2024-10-01 18:50_

And this one can be removed too

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:104 on 2024-10-01 18:52_

```suggestion
/// Shorthand for `symbol_ty_by_id` that looks up a global symbol from the context of being in that
/// module, including fallback to globals if (possibly) not defined.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:115 on 2024-10-01 18:54_

So I realized that a simpler approach that doesn't require threading this context through would be to just use `symbol_ty_by_id` (which can always use `Never` and never `Unbound` or `Unknown` as the unbound-ty), and then explicitly use the use-def map here to check directly whether the name may be undeclared and may be unbound, and then if so, look up the builtin type and union that with the symbol public type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:105 on 2024-10-01 18:55_

I still think it might be fine to inline this into `TypeInferenceBuilder::lookup_name`, since I don't think it will have any other call sites? If we do keep it as a separate function, then I think it should include the builtins-fallback part too; that's part of a "global symbol lookup".

Also I don't think it needs to be `pub(crate)`.

---

_@carljm had review dismissed on 2024-10-01 18:57_

Nice work! You did what I asked for yesterday, but I now realize that in some cases I asked for the wrong thing 😆 Sorry about that! I've talked this over with the team (by which I mean @AlexWaygood in this case) and I think we've got a clearer plan in mind now; let me know if any of the comments are not clear.

Thanks for all your work on this! You definitely didn't pick an easy first task :)

---

_Converted to draft by @pilleye on 2024-10-02 00:18_

---

_Comment by @carljm on 2024-10-14 18:42_

Hi @pilleye! I'd love to get this landed soon since it affects other work we want to do. Really appreciate your work on it so far, and if you don't have time to wrap it up that's no problem, we can pick it up. Let me know if we should go ahead and do that!

---

_Comment by @pilleye on 2024-10-15 19:06_

Ah sorry, I've just been a bit busy the past week with other work, I'll try to get it in a ready-to-review state tonight.

---

_@pilleye reviewed on 2024-10-15 19:27_

---

_Review comment by @pilleye on `crates/red_knot_python_semantic/src/types/infer.rs`:3369 on 2024-10-15 19:27_

Just to double-check, `x` as viewed from `bar.py` should still be `Unknown`, not `Never`, right?

---

_@pilleye reviewed on 2024-10-15 19:45_

---

_Review comment by @pilleye on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-15 19:45_

~~Is there a way to add public type tests to the markdown tests introduced in #13719?~~

Never mind, figured it out with importing and revealing the type.

---

_@carljm reviewed on 2024-10-15 20:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-15 20:07_

You can import it (since tests can be multi-file), or refer to the name as a global from within a nested function. Import probably makes it clearer what's going on?

---

_@pilleye reviewed on 2024-10-15 20:31_

---

_Review comment by @pilleye on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-15 20:31_

How do I run the mdtests? `cargo test` inside `red_knot_python_semantic` and `red_knot_test` both don't seem to run it.

---

_@carljm reviewed on 2024-10-15 20:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3369 on 2024-10-15 20:33_

Right, I think we should handle that in the importing code, instead of the current code which replaces unbound with unknown we'll want to replace never with unknown (and emit a diagnostic, but that can be a TODO if you want, doesn't have to be in this PR.)

---

_@carljm reviewed on 2024-10-15 20:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-15 20:34_

`cargo test` inside `red_knot_python_semantic` (or `cargo test -p red_knot_python_semantic` from the workspace) should run the mdtests, and do for me. How are you concluding that they aren't running?

---

_@pilleye reviewed on 2024-10-15 20:40_

---

_Review comment by @pilleye on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-15 20:40_

Hmm, they only run when I run `cargo test mdtest` inside `red_knot_python_semantic`.

Without `mdtest`: https://gist.github.com/pilleye/7454e668bc19f9ca1969e0765a82a301
With `mdtest`: https://gist.github.com/pilleye/3e9842e37a911eb88f6ac26c281bb0dc

---

_@carljm reviewed on 2024-10-15 20:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-15 20:51_

That's weird -- I'm not enough of a `cargo test` expert to explain this discrepancy :) For me they definitely run with just `cargo test`... In general my experience has been that `cargo test` runs all tests it discovers, both unit tests and integration tests. Not sure what might cause that to not be happening for you. `mdtest` is just a normal Rust integration test.

---

_Comment by @pilleye on 2024-10-15 21:29_

I'm stuck on a couple tests.

I also have another question, why don't we use `symbol_ty(...)` in `infer.rs` for this line:

```
let member_ty = module_ty.member(...)
```

---

_Marked ready for review by @pilleye on 2024-10-15 21:29_

---

_Comment by @carljm on 2024-10-15 23:11_

> I'm stuck on a couple tests.

Taking a look! Thanks for all your work on this. If the changes to get everything passing look reasonably small, I might just go ahead and push them and land this, in the interest of efficiency, hope that's OK with you.

> I also have another question, why don't we use `symbol_ty(...)` in `infer.rs` for this line:
> 
> ```
> let member_ty = module_ty.member(...)
> ```

We kinda do use `global_symbol_ty`, because that's the entire implementation of `Type::member` for `Type::Module` types. It's just a layer of abstraction, since semantically what is happening here is an attribute access on a module object, and we model attribute accesses via `Type::member`.

---

_Comment by @pilleye on 2024-10-16 02:08_

> If the changes to get everything passing look reasonably small, I might just go ahead and push them and land this, in the interest of efficiency, hope that's OK with you.

Taking one more stab at it tonight, but feel free to finish it if it unblocks y'all.

---

_@pilleye reviewed on 2024-10-16 13:43_

---

_Review comment by @pilleye on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-16 13:43_

Oh interesting, I didn't realize that cargo doesn't run integration tests if unit tests fail, so you have to run it with `--no-fail-fast` to get both tests.

---

_@AlexWaygood reviewed on 2024-10-16 13:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-16 13:49_

huh, that _is_ counterintuitive!

---

_Comment by @pilleye on 2024-10-16 14:54_

I think I have it mostly working, there are just a few weird issues that feel orthogonal like:
```
---- types::infer::tests::exception_handler_with_invalid_syntax stdout ----
thread 'types::infer::tests::exception_handler_with_invalid_syntax' panicked at crates/red_knot_python_semantic/src/types/infer.rs:3519:9:
assertion `left == right` failed
  left: ["Object of type `Literal[reveal_type, reveal_type]` is not assignable to `Literal[reveal_type, reveal_type]`", "Revealed type is `Unknown`"]
 right: ["Revealed type is `Unknown`"]
```

I'm guessing this is because when we're looking at typeshed, we're replacing a union containing `Unknown` with a union containing `Unbound` and that is getting replaced with `Never` and removed from the union. So we were originally assigning something to `Any`/`Unknown`, which was fine, but now we're assigning literals to literals.

---

_@carljm reviewed on 2024-10-16 18:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4506 on 2024-10-16 18:07_

Ah, that makes sense! TIL

---

_Comment by @carljm on 2024-10-16 18:12_

@pilleye Ok, made some updates here and I think I got everything working -- didn't really change that much, but I split out the commits to help clarify what I changed and why, feel free to look at the commits if you're curious, lmk if anything doesn't make sense.

Now getting these weird errors about the Salsa crate from CI, need to figure out what's up with that but it doesn't seem related to this PR...

---

_Comment by @carljm on 2024-10-16 18:32_

Yeah ruff CI is currently broken for all PRs: https://github.com/astral-sh/ruff/pull/13780 

---

_Comment by @pilleye on 2024-10-16 19:35_

> @pilleye Ok, made some updates here and I think I got everything working -- didn't really change that much, but I split out the commits to help clarify what I changed and why, feel free to look at the commits if you're curious, lmk if anything doesn't make sense.

That makes so much more sense! Appreciate the breakdown of commits :)


---

_Comment by @carljm on 2024-10-16 20:45_

This ends up being kind of a hacky approach to the original problem, in that we still keep Unbound in all the base type queries (`global_symbol_ty`, etc) but replace it with Never in unions wherever a type "escapes" as a public type. But that's fine; it gets the tests reflecting the types we actually should be seeing, which is progress. The better implementation will ultimately involve totally removing `Type::Unbound` anyway, which is a bigger change (https://github.com/astral-sh/ruff/issues/13671).

Thanks again @pilleye for all your work on this (very confusing, as it turned out) issue!

---

_Merged by @carljm on 2024-10-16 20:46_

---

_Closed by @carljm on 2024-10-16 20:46_

---

_Branch deleted on 2024-10-18 04:09_

---
