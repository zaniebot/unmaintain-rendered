```yaml
number: 16048
title: RUF009 should behave similar to B008 and ignore attributes with immutable types
type: pull_request
state: merged
author: smokyabdulrahman
labels:
  - rule
assignees: []
merged: true
base: main
head: gh-issue-15772
created_at: 2025-02-08T23:38:49Z
updated_at: 2025-02-10T08:46:23Z
url: https://github.com/astral-sh/ruff/pull/16048
synced_at: 2026-01-10T19:57:22Z
```

# RUF009 should behave similar to B008 and ignore attributes with immutable types

---

_Pull request opened by @smokyabdulrahman on 2025-02-08 23:38_

This PR resolved #15772

Before PR:
```
def _(
    this_is_fine: int = f(),           # No error
    this_is_not: list[int] = f()       # B008: Do not perform function call `f` in argument defaults
): ...


@dataclass
class _:
    this_is_not_fine: list[int] = f()  # RUF009: Do not perform function call `f` in dataclass defaults
    this_is_also_not: int = f()        # RUF009: Do not perform function call `f` in dataclass defaults
```

After PR:
```
def _(
    this_is_fine: int = f(),           # No error
    this_is_not: list[int] = f()       # B008: Do not perform function call `f` in argument defaults
): ...


@dataclass
class _:
    this_is_not_fine: list[int] = f()  # RUF009: Do not perform function call `f` in dataclass defaults
    this_is_fine: int = f()
```

---

_Comment by @github-actions[bot] on 2025-02-08 23:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -12 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/airflow/models/dag.py#L432'>airflow/models/dag.py:432:25:</a> RUF009 Do not perform function call in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/airflow/models/dag.py#L433'>airflow/models/dag.py:433:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/common_build_params.py#L61'>dev/breeze/src/airflow_breeze/params/common_build_params.py:61:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/common_build_params.py#L62'>dev/breeze/src/airflow_breeze/params/common_build_params.py:62:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/common_build_params.py#L64'>dev/breeze/src/airflow_breeze/params/common_build_params.py:64:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/shell_params.py#L151'>dev/breeze/src/airflow_breeze/params/shell_params.py:151:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/shell_params.py#L164'>dev/breeze/src/airflow_breeze/params/shell_params.py:164:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/shell_params.py#L166'>dev/breeze/src/airflow_breeze/params/shell_params.py:166:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/handler.py#L23'>src/latch_cli/exceptions/handler.py:23:23:</a> RUF009 Do not perform function call `platform.version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/handler.py#L24'>src/latch_cli/exceptions/handler.py:24:26:</a> RUF009 Do not perform function call `get_local_package_version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/handler.py#L26'>src/latch_cli/exceptions/handler.py:26:21:</a> RUF009 Do not perform function call `platform.platform` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_sdk_config/latch.py#L86'>src/latch_sdk_config/latch.py:86:22:</a> RUF009 Do not perform function call `urljoin` in dataclass defaults
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
ℹ️ ecosystem check **detected linter changes**. (+0 -12 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/airflow/models/dag.py#L432'>airflow/models/dag.py:432:25:</a> RUF009 Do not perform function call in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/airflow/models/dag.py#L433'>airflow/models/dag.py:433:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/common_build_params.py#L61'>dev/breeze/src/airflow_breeze/params/common_build_params.py:61:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/common_build_params.py#L62'>dev/breeze/src/airflow_breeze/params/common_build_params.py:62:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/common_build_params.py#L64'>dev/breeze/src/airflow_breeze/params/common_build_params.py:64:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/shell_params.py#L151'>dev/breeze/src/airflow_breeze/params/shell_params.py:151:31:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/shell_params.py#L164'>dev/breeze/src/airflow_breeze/params/shell_params.py:164:27:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- <a href='https://github.com/apache/airflow/blob/1baa7520bea853b199430543fe85f0c352f9dd13/dev/breeze/src/airflow_breeze/params/shell_params.py#L166'>dev/breeze/src/airflow_breeze/params/shell_params.py:166:25:</a> RUF009 Do not perform function call `os.environ.get` in dataclass defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/handler.py#L23'>src/latch_cli/exceptions/handler.py:23:23:</a> RUF009 Do not perform function call `platform.version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/handler.py#L24'>src/latch_cli/exceptions/handler.py:24:26:</a> RUF009 Do not perform function call `get_local_package_version` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/handler.py#L26'>src/latch_cli/exceptions/handler.py:26:21:</a> RUF009 Do not perform function call `platform.platform` in dataclass defaults
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_sdk_config/latch.py#L86'>src/latch_sdk_config/latch.py:86:22:</a> RUF009 Do not perform function call `urljoin` in dataclass defaults
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

_Renamed from "#15772 RUF009 should behave similar to B008 and ignore attributes with immutable types" to "RUF009 should behave similar to B008 and ignore attributes with immutable types" by @MichaReiser on 2025-02-10 08:33_

---

_Label `rule` added by @MichaReiser on 2025-02-10 08:34_

---

_Comment by @MichaReiser on 2025-02-10 08:46_

Thank you

---

_Merged by @MichaReiser on 2025-02-10 08:46_

---

_Closed by @MichaReiser on 2025-02-10 08:46_

---
