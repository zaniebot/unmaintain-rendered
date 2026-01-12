```yaml
number: 5835
title: "[`flake8-use-pathlib`] Implement `os-path-getsize` and `os-path-get(a|m|c)-time` (`PTH202-205`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: pathlib-getsize-time
created_at: 2023-07-17T15:54:24Z
updated_at: 2023-07-20T08:41:49Z
url: https://github.com/astral-sh/ruff/pull/5835
synced_at: 2026-01-12T15:55:19Z
```

# [`flake8-use-pathlib`] Implement `os-path-getsize` and `os-path-get(a|m|c)-time` (`PTH202-205`)

---

_@sbrugman_

Reviving https://github.com/astral-sh/ruff/pull/2348 step by step

Pt 3. implement detection for:
- `os.path.getsize`
- `os.path.getmtime`
- `os.path.getctime`
- `os.path.getatime`

---

_Renamed from "[`flake8-use-path`] Implement `os-path-getsize` and `os-path-get(a|m|c)-time` (`PTH202-205`)" to "[`flake8-use-pathlib`] Implement `os-path-getsize` and `os-path-get(a|m|c)-time` (`PTH202-205`)" by @sbrugman on 2023-07-17 15:54_

---

_Comment by @github-actions[bot] on 2023-07-17 16:07_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+19, -0, 0 error(s))

<details><summary>airflow (+8, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/dag_processing/manager.py#L1134'>airflow/dag_processing/manager.py:1134:51:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/models/dagbag.py#L293'>airflow/models/dagbag.py:293:64:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/models/dagcode.py#L117'>airflow/models/dagcode.py:117:17:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L200'>airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:200:16:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/sensors/filesystem.py#L68'>airflow/sensors/filesystem.py:68:60:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/triggers/file.py#L66'>airflow/triggers/file.py:66:34:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/dev/breeze/src/airflow_breeze/utils/parallel.py#L308'>dev/breeze/src/airflow_breeze/utils/parallel.py:308:24:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/docs/exts/docs_build/fetch_inventories.py#L93'>docs/exts/docs_build/fetch_inventories.py:93:71:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
</pre>

</p>
</details>
<details><summary>bokeh (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/sphinxext/bokeh_gallery.py#L122'>src/bokeh/sphinxext/bokeh_gallery.py:122:26:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/sphinxext/bokeh_gallery.py#L143'>src/bokeh/sphinxext/bokeh_gallery.py:143:41:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
</pre>

</p>
</details>
<details><summary>zulip (+9, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/scripts/lib/zulip_tools.py#L356'>scripts/lib/zulip_tools.py:356:12:</a> PTH205 `os.path.getctime` should be replaced by `Path.stat().st_ctime`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/scripts/setup/generate_secrets.py#L66'>scripts/setup/generate_secrets.py:66:47:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/tools/lib/test_server.py#L64'>tools/lib/test_server.py:64:45:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/data_import/mattermost.py#L356'>zerver/data_import/mattermost.py:356:21:</a> PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/data_import/mattermost.py#L357'>zerver/data_import/mattermost.py:357:24:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/compatibility.py#L32'>zerver/lib/compatibility.py:32:9:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/test_fixtures.py#L355'>zerver/lib/test_fixtures.py:355:33:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/test_fixtures.py#L384'>zerver/lib/test_fixtures.py:384:33:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/8743602648d8618ff22b5a2686407f531078af5a/zerver/lib/upload/local.py#L114'>zerver/lib/upload/local.py:114:43:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
</pre>

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH204 | 13 | 13 | 0 |
| PTH202 | 5 | 5 | 0 |
| PTH205 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.26ms     4.0 MB/sec    1.02     10.3±0.29ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.07ms     8.2 MB/sec    1.04      2.1±0.08ms     7.9 MB/sec
formatter/numpy/globals.py                 1.01   235.9±11.80µs    12.5 MB/sec    1.00    233.5±8.24µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.12ms     5.7 MB/sec    1.02      4.6±0.20ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     14.5±0.38ms     2.8 MB/sec    1.02     14.9±0.42ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.10ms     4.5 MB/sec    1.00      3.7±0.08ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   489.4±11.78µs     6.0 MB/sec    1.01   494.0±15.31µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.18ms     3.9 MB/sec    1.03      6.8±0.15ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.03      7.6±0.17ms     5.4 MB/sec    1.00      7.4±0.19ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1622.9±45.46µs    10.3 MB/sec    1.00  1580.2±45.19µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    177.2±5.46µs    16.7 MB/sec    1.00    175.4±6.87µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.09ms     7.6 MB/sec    1.00      3.4±0.08ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.31ms     3.4 MB/sec    1.00     12.1±0.42ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.09ms     7.4 MB/sec    1.01      2.3±0.06ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    251.3±6.37µs    11.7 MB/sec    1.04   262.4±17.92µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.17ms     5.1 MB/sec    1.02      5.1±0.20ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.52ms     2.4 MB/sec    1.00     16.9±0.33ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.21ms     3.8 MB/sec    1.01      4.4±0.30ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   528.7±25.59µs     5.6 MB/sec    1.00   530.2±28.58µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.31ms     3.3 MB/sec    1.00      7.7±0.24ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      8.9±0.28ms     4.6 MB/sec    1.00      8.8±0.25ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1802.2±35.79µs     9.2 MB/sec    1.00  1773.4±38.57µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   202.1±11.76µs    14.6 MB/sec    1.00    199.0±6.63µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.10ms     6.5 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-17 16:17_

Ecosystem changes are correct

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_path_getatime.rs`:7 on 2023-07-18 17:15_

```suggestion
/// Checks for the use of `os.path.getatime`.
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_path_getctime.rs`:7 on 2023-07-18 17:16_

```suggestion
/// Checks for the use of `os.path.getctime`.
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:48 on 2023-07-18 17:19_

[Ref1]

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_path_getatime.rs`:4 on 2023-07-18 17:20_

I think i'd rather place this (and the other two respectively) at [Ref1]. Currently, it's a doc comment (`///` vs `//`), so it would be the first line of the documentation ahead of "#What it does"

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_path_getmtime.rs`:7 on 2023-07-18 17:20_

```suggestion
/// Checks for the use of `os.path.getmtime`.
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_path_getatime.rs`:10 on 2023-07-18 17:22_

nit (and kinda  annoying since it's not automated, sorry): The rule description doc comment are generally wrapped at 80 characters

---

_@konstin approved on 2023-07-18 17:23_

---

_Label `rule` added by @charliermarsh on 2023-07-20 01:57_

---

_Merged by @charliermarsh on 2023-07-20 02:05_

---

_Closed by @charliermarsh on 2023-07-20 02:05_

---

_Branch deleted on 2023-07-20 08:41_

---
