```yaml
number: 6325
title: "Include comparisons to builtin types in `type-comparison` rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/types
created_at: 2023-08-04T02:11:45Z
updated_at: 2023-08-04T05:45:49Z
url: https://github.com/astral-sh/ruff/pull/6325
synced_at: 2026-01-12T15:55:21Z
```

# Include comparisons to builtin types in `type-comparison` rule

---

_@charliermarsh_

## Summary

Extends `type-comparison` to flag:

```python
if type(obj) is int:
    pass
```

In addition to the existing cases, like:

```python
if type(obj) is type(1):
    pass
```

Closes https://github.com/astral-sh/ruff/issues/6260.


---

_Label `rule` added by @charliermarsh on 2023-08-04 02:11_

---

_Merged by @charliermarsh on 2023-08-04 02:25_

---

_Closed by @charliermarsh on 2023-08-04 02:25_

---

_Branch deleted on 2023-08-04 02:25_

---

_Comment by @github-actions[bot] on 2023-08-04 02:29_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+36, -0, 0 error(s))

<details><summary>disnake (+7, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/disnake/ext/commands/converter.py#L1203'>disnake/ext/commands/converter.py:1203:8:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/disnake/ext/commands/params.py#L404'>disnake/ext/commands/params.py:404:20:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/disnake/ext/commands/params.py#L749'>disnake/ext/commands/params.py:749:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/disnake/ext/commands/params.py#L819'>disnake/ext/commands/params.py:819:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/tests/ext/commands/test_params.py#L143'>tests/ext/commands/test_params.py:143:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/tests/ext/commands/test_params.py#L146'>tests/ext/commands/test_params.py:146:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/tests/ext/commands/test_params.py#L215'>tests/ext/commands/test_params.py:215:16:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>airflow (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/airflow/providers/docker/operators/docker.py#L228'>airflow/providers/docker/operators/docker.py:228:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/tests/core/test_stats.py#L75'>tests/core/test_stats.py:75:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/tests/system/providers/amazon/aws/utils/__init__.py#L157'>tests/system/providers/amazon/aws/utils/__init__.py:157:16:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>bokeh (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/38d05d037223234aedeb25e35502abf3332bf0b4/tests/unit/bokeh/core/property/test_primitive.py#L249'>tests/unit/bokeh/core/property/test_primitive.py:249:17:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>setuptools (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/77c14a8e345a615c749fbac6345b07172cfc1e11/setuptools/_vendor/tomli/_parser.py#L682'>setuptools/_vendor/tomli/_parser.py:682:8:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>mypy (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/0d708cb9c9d5291c1c988ef90a1b77307ed5315c/mypy/config_parser.py#L464'>mypy/config_parser.py:464:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/python/mypy/blob/0d708cb9c9d5291c1c988ef90a1b77307ed5315c/mypy/plugins/common.py#L153'>mypy/plugins/common.py:153:60:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>scikit-build (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c2025f24823599fc0abe0e7376ed5a10ad1e7b88/tests/test_setup.py#L223'>tests/test_setup.py:223:12:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>scikit-build-core (+8, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/json_schema.py#L39'>src/scikit_build_core/settings/json_schema.py:39:31:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/json_schema.py#L41'>src/scikit_build_core/settings/json_schema.py:41:33:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/json_schema.py#L41'>src/scikit_build_core/settings/json_schema.py:41:52:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/json_schema.py#L46'>src/scikit_build_core/settings/json_schema.py:46:33:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/json_schema.py#L51'>src/scikit_build_core/settings/json_schema.py:51:14:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/json_schema.py#L53'>src/scikit_build_core/settings/json_schema.py:53:14:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/sources.py#L215'>src/scikit_build_core/settings/sources.py:215:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/52ad4df831490941b8dbbd499833479b9459fb70/src/scikit_build_core/settings/sources.py#L324'>src/scikit_build_core/settings/sources.py:324:12:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary>zulip (+13, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/lib/upload/__init__.py#L188'>zerver/lib/upload/__init__.py:188:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/lib/upload/__init__.py#L195'>zerver/lib/upload/__init__.py:195:16:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_audit_log.py#L767'>zerver/tests/test_audit_log.py:767:18:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_events.py#L2816'>zerver/tests/test_events.py:2816:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_events.py#L2896'>zerver/tests/test_events.py:2896:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_events.py#L2983'>zerver/tests/test_events.py:2983:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_openapi.py#L464'>zerver/tests/test_openapi.py:464:24:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_realm.py#L1190'>zerver/tests/test_realm.py:1190:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_realm.py#L1260'>zerver/tests/test_realm.py:1260:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_realm.py#L1386'>zerver/tests/test_realm.py:1386:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_realm.py#L624'>zerver/tests/test_realm.py:624:81:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_settings.py#L225'>zerver/tests/test_settings.py:225:54:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/zulip/zulip/blob/73fbca7ae9f5bcd4d07c18a780e28285caf97868/zerver/tests/test_settings.py#L390'>zerver/tests/test_settings.py:390:54:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| E721 | 36 | 36 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.10ms     5.2 MB/sec    1.07      8.5±0.04ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1576.7±14.83µs    10.6 MB/sec    1.06  1675.7±27.62µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    179.0±2.13µs    16.5 MB/sec    1.04    186.8±1.81µs    15.8 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.02ms     7.6 MB/sec    1.07      3.6±0.12ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.8±0.07ms     3.8 MB/sec    1.03     11.2±0.07ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.01      2.8±0.00ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    382.1±3.31µs     7.7 MB/sec    1.00    383.8±0.60µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.03ms     5.1 MB/sec    1.00      5.0±0.04ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.02ms     7.7 MB/sec    1.03      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1123.9±10.35µs    14.8 MB/sec    1.02   1150.2±4.18µs    14.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    126.1±2.16µs    23.4 MB/sec    1.02    128.4±1.65µs    23.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.04ms    10.7 MB/sec    1.02      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     11.7±0.36ms     3.5 MB/sec     1.00     11.7±0.32ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.10ms     7.3 MB/sec     1.00      2.3±0.09ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   252.2±12.71µs    11.7 MB/sec     1.05   264.8±20.94µs    11.1 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.27ms     5.1 MB/sec     1.00      5.0±0.20ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.01     16.5±0.42ms     2.5 MB/sec     1.00     16.3±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.17ms     3.9 MB/sec     1.00      4.3±0.20ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   530.6±24.70µs     5.6 MB/sec     1.00   524.4±21.21µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.5±0.17ms     3.4 MB/sec     1.00      7.3±0.17ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.24ms     4.9 MB/sec     1.00      8.2±0.17ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1736.6±118.79µs     9.6 MB/sec    1.00  1728.9±69.49µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.8±9.54µs    15.2 MB/sec     1.01    195.2±8.41µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.11ms     6.9 MB/sec     1.00      3.7±0.08ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-04 02:35_

Oh, hmm... we should probably require that the expression on the left is of the form `type(x)`.

---

_Comment by @charliermarsh on 2023-08-04 02:37_

I will follow-up with another PR.

---

_@MichaReiser reviewed on 2023-08-04 05:45_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/type_comparison.rs`:67 on 2023-08-04 05:45_

Nit: I don't know how expensive `is_builtin` is, but we could potentially move this check out (or avoid nested ifs ;)) 

---
