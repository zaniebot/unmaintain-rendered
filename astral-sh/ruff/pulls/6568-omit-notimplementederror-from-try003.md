```yaml
number: 6568
title: "Omit `NotImplementedError` from `TRY003`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/TRY003
created_at: 2023-08-14T18:14:48Z
updated_at: 2023-08-14T18:46:12Z
url: https://github.com/astral-sh/ruff/pull/6568
synced_at: 2026-01-12T02:52:04Z
```

# Omit `NotImplementedError` from `TRY003`

---

_Pull request opened by @charliermarsh on 2023-08-14 18:14_

Closes https://github.com/astral-sh/ruff/issues/6528.

---

_Merged by @charliermarsh on 2023-08-14 18:24_

---

_Closed by @charliermarsh on 2023-08-14 18:24_

---

_Branch deleted on 2023-08-14 18:24_

---

_Comment by @github-actions[bot] on 2023-08-14 18:29_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -37, 0 error(s))

<details><summary>airflow (+0, -29)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/models/mappedoperator.py#L311'>airflow/models/mappedoperator.py:311:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/models/taskinstance.py#L1449'>airflow/models/taskinstance.py:1449:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/hooks/base_aws.py#L1011'>airflow/providers/amazon/aws/hooks/base_aws.py:1011:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/hooks/base_aws.py#L1028'>airflow/providers/amazon/aws/hooks/base_aws.py:1028:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/hooks/base_aws.py#L246'>airflow/providers/amazon/aws/hooks/base_aws.py:246:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/hooks/base_aws.py#L286'>airflow/providers/amazon/aws/hooks/base_aws.py:286:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/hooks/base_aws.py#L345'>airflow/providers/amazon/aws/hooks/base_aws.py:345:23:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/hooks/s3.py#L1117'>airflow/providers/amazon/aws/hooks/s3.py:1117:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/operators/ecs.py#L71'>airflow/providers/amazon/aws/operators/ecs.py:71:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/operators/sagemaker.py#L157'>airflow/providers/amazon/aws/operators/sagemaker.py:157:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/emr.py#L105'>airflow/providers/amazon/aws/sensors/emr.py:105:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/emr.py#L115'>airflow/providers/amazon/aws/sensors/emr.py:115:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/emr.py#L95'>airflow/providers/amazon/aws/sensors/emr.py:95:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/sagemaker.py#L75'>airflow/providers/amazon/aws/sensors/sagemaker.py:75:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/sagemaker.py#L79'>airflow/providers/amazon/aws/sensors/sagemaker.py:79:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/sagemaker.py#L83'>airflow/providers/amazon/aws/sensors/sagemaker.py:83:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/sensors/sagemaker.py#L91'>airflow/providers/amazon/aws/sensors/sagemaker.py:91:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/utils/connection_wrapper.py#L407'>airflow/providers/amazon/aws/utils/connection_wrapper.py:407:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/amazon/aws/utils/sqs.py#L66'>airflow/providers/amazon/aws/utils/sqs.py:66:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/apache/drill/hooks/drill.py#L86'>airflow/providers/apache/drill/hooks/drill.py:86:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/apache/drill/hooks/drill.py#L99'>airflow/providers/apache/drill/hooks/drill.py:99:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/google/cloud/hooks/bigquery.py#L2349'>airflow/providers/google/cloud/hooks/bigquery.py:2349:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/slack/operators/slack.py#L77'>airflow/providers/slack/operators/slack.py:77:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/providers/tableau/hooks/tableau.py#L114'>airflow/providers/tableau/hooks/tableau.py:114:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/secrets/base_secrets.py#L109'>airflow/secrets/base_secrets.py:109:23:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/triggers/base.py#L57'>airflow/triggers/base.py:57:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/triggers/base.py#L76'>airflow/triggers/base.py:76:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/airflow/utils/compression.py#L29'>airflow/utils/compression.py:29:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/apache/airflow/blob/b8cdd284dd6e7197c8045cbce614b83b235d23a9/docs/exts/operators_and_hooks_ref.py#L249'>docs/exts/operators_and_hooks_ref.py:249:15:</a> TRY003 Avoid specifying long messages outside the exception class
</pre>

</p>
</details>
<details><summary>bokeh (+0, -3)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/88109a2966ed0ab146fe46c0cf0dde025fa37099/src/bokeh/application/handlers/handler.py#L141'>src/bokeh/application/handlers/handler.py:141:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/bokeh/bokeh/blob/88109a2966ed0ab146fe46c0cf0dde025fa37099/src/bokeh/command/subcommand.py#L174'>src/bokeh/command/subcommand.py:174:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/bokeh/bokeh/blob/88109a2966ed0ab146fe46c0cf0dde025fa37099/src/bokeh/core/property/descriptor_factory.py#L135'>src/bokeh/core/property/descriptor_factory.py:135:15:</a> TRY003 Avoid specifying long messages outside the exception class
</pre>

</p>
</details>
<details><summary>zulip (+0, -5)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/lib/stripe.py#L447'>corporate/lib/stripe.py:447:23:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/lib/stripe.py#L848'>corporate/lib/stripe.py:848:15:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/data_import/rocketchat.py#L953'>zerver/data_import/rocketchat.py:953:27:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/lib/markdown/__init__.py#L440'>zerver/lib/markdown/__init__.py:440:11:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/openapi/markdown_extension.py#L468'>zerver/openapi/markdown_extension.py:468:15:</a> TRY003 Avoid specifying long messages outside the exception class
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TRY003 | 37 | 0 | 37 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.02ms    10.4 MB/sec    1.00      3.9±0.02ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.00    777.5±4.03µs    21.4 MB/sec    1.00    778.4±8.80µs    21.4 MB/sec
formatter/numpy/globals.py                 1.00     78.7±0.35µs    37.5 MB/sec    1.00     79.0±0.57µs    37.4 MB/sec
formatter/pydantic/types.py                1.00   1566.8±4.16µs    16.3 MB/sec    1.00  1562.2±27.55µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.01     10.6±0.07ms     3.8 MB/sec    1.00     10.5±0.10ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.7±1.96µs     7.6 MB/sec    1.00    391.2±1.15µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.04ms     4.7 MB/sec    1.00      5.5±0.06ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.03ms     7.4 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1200.2±1.63µs    13.9 MB/sec    1.00   1205.8±2.52µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.2±1.72µs    20.9 MB/sec    1.00    141.9±1.68µs    20.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.06ms    10.2 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.0±0.26ms     8.2 MB/sec    1.05      5.2±0.23ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   946.3±35.21µs    17.6 MB/sec    1.06  1001.9±54.48µs    16.6 MB/sec
formatter/numpy/globals.py                 1.00     95.9±6.73µs    30.8 MB/sec    1.03    98.6±11.58µs    29.9 MB/sec
formatter/pydantic/types.py                1.00  1892.4±96.95µs    13.5 MB/sec    1.09      2.1±0.12ms    12.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.82ms     2.6 MB/sec    1.03     16.2±0.67ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.17ms     3.9 MB/sec    1.03      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   539.1±23.15µs     5.5 MB/sec    1.07   579.0±25.16µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.38ms     3.1 MB/sec    1.05      8.7±0.43ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.33ms     4.7 MB/sec    1.05      9.1±0.39ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1823.5±75.45µs     9.1 MB/sec    1.05  1921.9±84.63µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   230.0±10.38µs    12.8 MB/sec    1.00   228.8±12.22µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.21ms     6.3 MB/sec    1.00      4.0±0.41ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
