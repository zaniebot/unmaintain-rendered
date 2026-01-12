```yaml
number: 7852
title: "Show custom message for `Path.joinpath` with starred arguments"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/join
created_at: 2023-10-08T14:47:01Z
updated_at: 2023-10-09T12:12:17Z
url: https://github.com/astral-sh/ruff/pull/7852
synced_at: 2026-01-12T15:55:25Z
```

# Show custom message for `Path.joinpath` with starred arguments

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/7833.

---

_Comment by @github-actions[bot] on 2023-10-08 15:01_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+7, -7, 0 error(s))

<details><summary>airflow (+4, -4)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L492'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:492:27:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L492'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:492:27:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
+ <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/dev/provider_packages/prepare_provider_packages.py#L281'>dev/provider_packages/prepare_provider_packages.py:281:12:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/dev/provider_packages/prepare_provider_packages.py#L281'>dev/provider_packages/prepare_provider_packages.py:281:12:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
+ <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/dev/provider_packages/prepare_provider_packages.py#L833'>dev/provider_packages/prepare_provider_packages.py:833:12:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/dev/provider_packages/prepare_provider_packages.py#L833'>dev/provider_packages/prepare_provider_packages.py:833:12:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
+ <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/dev/provider_packages/prepare_provider_packages.py#L851'>dev/provider_packages/prepare_provider_packages.py:851:29:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/apache/airflow/blob/61af8d747d6176cec079cbb2ab330330fe554bb8/dev/provider_packages/prepare_provider_packages.py#L851'>dev/provider_packages/prepare_provider_packages.py:851:29:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/91bf7d8931363245c1f1d3fc66d92d321da03c1c/src/bokeh/util/compiler.py#L525'>src/bokeh/util/compiler.py:525:36:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/bokeh/bokeh/blob/91bf7d8931363245c1f1d3fc66d92d321da03c1c/src/bokeh/util/compiler.py#L525'>src/bokeh/util/compiler.py:525:36:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
</pre>

</p>
</details>
<details><summary>zulip (+2, -2)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e8ced3e74a6ed4d50feba1b529b5b57d66c01a99/tools/documentation_crawler/documentation_crawler/spiders/check_documentation.py#L12'>tools/documentation_crawler/documentation_crawler/spiders/check_documentation.py:12:19:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/zulip/zulip/blob/e8ced3e74a6ed4d50feba1b529b5b57d66c01a99/tools/documentation_crawler/documentation_crawler/spiders/check_documentation.py#L12'>tools/documentation_crawler/documentation_crawler/spiders/check_documentation.py:12:19:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
+ <a href='https://github.com/zulip/zulip/blob/e8ced3e74a6ed4d50feba1b529b5b57d66c01a99/tools/documentation_crawler/documentation_crawler/spiders/check_help_documentation.py#L12'>tools/documentation_crawler/documentation_crawler/spiders/check_help_documentation.py:12:42:</a> PTH118 `os.path.join()` should be replaced by `Path.joinpath()`
- <a href='https://github.com/zulip/zulip/blob/e8ced3e74a6ed4d50feba1b529b5b57d66c01a99/tools/documentation_crawler/documentation_crawler/spiders/check_help_documentation.py#L12'>tools/documentation_crawler/documentation_crawler/spiders/check_help_documentation.py:12:42:</a> PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH118 | 14 | 7 | 7 |



---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_use_pathlib/violations.rs`:844 on 2023-10-09 09:21_

```suggestion
                format!("`os.{module}.join()` should be replaced by `Path.joinpath()`")
```

---

_@konstin approved on 2023-10-09 09:22_

---

_Merged by @charliermarsh on 2023-10-09 12:04_

---

_Closed by @charliermarsh on 2023-10-09 12:04_

---

_Branch deleted on 2023-10-09 12:04_

---
