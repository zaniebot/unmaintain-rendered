```yaml
number: 15845
title: "[`ruff_linter`] ignore function call as default value for immutable annotation (RUF009)"
type: pull_request
state: closed
author: BKCF
labels: []
assignees: []
draft: true
base: main
head: RUF009-immutable-annotation
created_at: 2025-01-31T06:00:58Z
updated_at: 2025-02-02T21:39:15Z
url: https://github.com/astral-sh/ruff/pull/15845
synced_at: 2026-01-12T15:55:52Z
```

# [`ruff_linter`] ignore function call as default value for immutable annotation (RUF009)

---

_@BKCF_

## Summary
Resolves #15772 to sync the behavior of RUF009 and B008. When defining




## Test Plan
explicit test for the minimal example provided in the issue
cargo run -p ruff -- check .\crates\ruff_linter\resources\test\fixtures\ruff\RUF009*.py --preview --no-cache --select RUF009
cargo nextest run
cargo insta test


---

_Comment by @github-actions[bot] on 2025-01-31 06:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -12 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/airflow/models/dag.py#L430'>airflow/models/dag.py:430:25:</a> RUF009 Do not perform function call in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/airflow/models/dag.py#L431'>airflow/models/dag.py:431:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/common_build_params.py#L61'>dev/breeze/src/airflow_breeze/params/common_build_params.py:61:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/common_build_params.py#L62'>dev/breeze/src/airflow_breeze/params/common_build_params.py:62:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/common_build_params.py#L64'>dev/breeze/src/airflow_breeze/params/common_build_params.py:64:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/shell_params.py#L151'>dev/breeze/src/airflow_breeze/params/shell_params.py:151:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/shell_params.py#L164'>dev/breeze/src/airflow_breeze/params/shell_params.py:164:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/shell_params.py#L166'>dev/breeze/src/airflow_breeze/params/shell_params.py:166:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/handler.py#L23'>src/latch_cli/exceptions/handler.py:23:23:</a> RUF009 Do not perform function call `platform.version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/handler.py#L24'>src/latch_cli/exceptions/handler.py:24:26:</a> RUF009 Do not perform function call `get_local_package_version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/handler.py#L26'>src/latch_cli/exceptions/handler.py:26:21:</a> RUF009 Do not perform function call `platform.platform` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_sdk_config/latch.py#L86'>src/latch_sdk_config/latch.py:86:22:</a> RUF009 Do not perform function call `urljoin` in dataclass defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF009 | 12 | 0 | 12 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -12 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/airflow/models/dag.py#L430'>airflow/models/dag.py:430:25:</a> RUF009 Do not perform function call in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/airflow/models/dag.py#L431'>airflow/models/dag.py:431:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/common_build_params.py#L61'>dev/breeze/src/airflow_breeze/params/common_build_params.py:61:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/common_build_params.py#L62'>dev/breeze/src/airflow_breeze/params/common_build_params.py:62:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/common_build_params.py#L64'>dev/breeze/src/airflow_breeze/params/common_build_params.py:64:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/shell_params.py#L151'>dev/breeze/src/airflow_breeze/params/shell_params.py:151:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/shell_params.py#L164'>dev/breeze/src/airflow_breeze/params/shell_params.py:164:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/826fcc9832fd715d14a980b6567ffd556b08b35c/dev/breeze/src/airflow_breeze/params/shell_params.py#L166'>dev/breeze/src/airflow_breeze/params/shell_params.py:166:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/handler.py#L23'>src/latch_cli/exceptions/handler.py:23:23:</a> RUF009 Do not perform function call `platform.version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/handler.py#L24'>src/latch_cli/exceptions/handler.py:24:26:</a> RUF009 Do not perform function call `get_local_package_version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/handler.py#L26'>src/latch_cli/exceptions/handler.py:26:21:</a> RUF009 Do not perform function call `platform.platform` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_sdk_config/latch.py#L86'>src/latch_sdk_config/latch.py:86:22:</a> RUF009 Do not perform function call `urljoin` in dataclass defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF009 | 12 | 0 | 12 | 0 | 0 |

</p>
</details>




---

_Closed by @BKCF on 2025-02-02 21:39_

---
