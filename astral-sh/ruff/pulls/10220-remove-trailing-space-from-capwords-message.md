```yaml
number: 10220
title: "Remove trailing space from `CapWords` message"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/space
created_at: 2024-03-04T01:40:12Z
updated_at: 2024-03-04T02:00:49Z
url: https://github.com/astral-sh/ruff/pull/10220
synced_at: 2026-01-12T15:55:31Z
```

# Remove trailing space from `CapWords` message

---

_@charliermarsh_

_No description provided._

---

_Label `bug` added by @charliermarsh on 2024-03-04 01:40_

---

_Merged by @charliermarsh on 2024-03-04 01:54_

---

_Closed by @charliermarsh on 2024-03-04 01:54_

---

_Branch deleted on 2024-03-04 01:54_

---

_Comment by @github-actions[bot] on 2024-03-04 02:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+338 -338 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+18 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/io/utils/stat.py#L22'>airflow/io/utils/stat.py:22:7:</a> N801 Class name `stat_result` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/io/utils/stat.py#L22'>airflow/io/utils/stat.py:22:7:</a> N801 Class name `stat_result` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L39'>airflow/providers/amazon/aws/transfers/sql_to_s3.py:39:7:</a> N801 Class name `FILE_FORMAT` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L39'>airflow/providers/amazon/aws/transfers/sql_to_s3.py:39:7:</a> N801 Class name `FILE_FORMAT` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/cncf/kubernetes/operators/pod.py#L1083'>airflow/providers/cncf/kubernetes/operators/pod.py:1083:7:</a> N801 Class name `_optionally_suppress` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/cncf/kubernetes/operators/pod.py#L1083'>airflow/providers/cncf/kubernetes/operators/pod.py:1083:7:</a> N801 Class name `_optionally_suppress` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L116'>airflow/providers/google/common/hooks/base_google.py:116:7:</a> N801 Class name `retry_if_temporary_quota` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L116'>airflow/providers/google/common/hooks/base_google.py:116:7:</a> N801 Class name `retry_if_temporary_quota` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L123'>airflow/providers/google/common/hooks/base_google.py:123:7:</a> N801 Class name `retry_if_operation_in_progress` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L123'>airflow/providers/google/common/hooks/base_google.py:123:7:</a> N801 Class name `retry_if_operation_in_progress` should use CapWords convention 
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+320 -320 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L134'>src/bokeh/client/states.py:134:7:</a> N801 Class name `WAITING_FOR_REPLY` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L134'>src/bokeh/client/states.py:134:7:</a> N801 Class name `WAITING_FOR_REPLY` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L68'>src/bokeh/client/states.py:68:7:</a> N801 Class name `NOT_YET_CONNECTED` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L68'>src/bokeh/client/states.py:68:7:</a> N801 Class name `NOT_YET_CONNECTED` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L76'>src/bokeh/client/states.py:76:7:</a> N801 Class name `CONNECTED_BEFORE_ACK` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L76'>src/bokeh/client/states.py:76:7:</a> N801 Class name `CONNECTED_BEFORE_ACK` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L85'>src/bokeh/client/states.py:85:7:</a> N801 Class name `CONNECTED_AFTER_ACK` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L85'>src/bokeh/client/states.py:85:7:</a> N801 Class name `CONNECTED_AFTER_ACK` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L112'>src/bokeh/colors/groups.py:112:7:</a> N801 Class name `cyan` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L112'>src/bokeh/colors/groups.py:112:7:</a> N801 Class name `cyan` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L132'>src/bokeh/colors/groups.py:132:7:</a> N801 Class name `green` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L132'>src/bokeh/colors/groups.py:132:7:</a> N801 Class name `green` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L159'>src/bokeh/colors/groups.py:159:7:</a> N801 Class name `orange` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L159'>src/bokeh/colors/groups.py:159:7:</a> N801 Class name `orange` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L170'>src/bokeh/colors/groups.py:170:7:</a> N801 Class name `pink` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L170'>src/bokeh/colors/groups.py:170:7:</a> N801 Class name `pink` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L182'>src/bokeh/colors/groups.py:182:7:</a> N801 Class name `purple` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L182'>src/bokeh/colors/groups.py:182:7:</a> N801 Class name `purple` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L208'>src/bokeh/colors/groups.py:208:7:</a> N801 Class name `red` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L208'>src/bokeh/colors/groups.py:208:7:</a> N801 Class name `red` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L223'>src/bokeh/colors/groups.py:223:7:</a> N801 Class name `white` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L223'>src/bokeh/colors/groups.py:223:7:</a> N801 Class name `white` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L247'>src/bokeh/colors/groups.py:247:7:</a> N801 Class name `yellow` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L247'>src/bokeh/colors/groups.py:247:7:</a> N801 Class name `yellow` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L48'>src/bokeh/colors/groups.py:48:7:</a> N801 Class name `black` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L48'>src/bokeh/colors/groups.py:48:7:</a> N801 Class name `black` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L65'>src/bokeh/colors/groups.py:65:7:</a> N801 Class name `blue` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L65'>src/bokeh/colors/groups.py:65:7:</a> N801 Class name `blue` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L87'>src/bokeh/colors/groups.py:87:7:</a> N801 Class name `brown` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L87'>src/bokeh/colors/groups.py:87:7:</a> N801 Class name `brown` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L45'>src/bokeh/core/property/validation.py:45:7:</a> N801 Class name `validate` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L45'>src/bokeh/core/property/validation.py:45:7:</a> N801 Class name `validate` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L425'>src/bokeh/layouts.py:425:11:</a> N801 Class name `row` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L425'>src/bokeh/layouts.py:425:11:</a> N801 Class name `row` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L428'>src/bokeh/layouts.py:428:11:</a> N801 Class name `col` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L428'>src/bokeh/layouts.py:428:11:</a> N801 Class name `col` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L903'>src/bokeh/models/plots.py:903:7:</a> N801 Class name `_list_attr_splat` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L903'>src/bokeh/models/plots.py:903:7:</a> N801 Class name `_list_attr_splat` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L931'>src/bokeh/models/plots.py:931:7:</a> N801 Class name `_legend_attr_splat` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L931'>src/bokeh/models/plots.py:931:7:</a> N801 Class name `_legend_attr_splat` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L93'>src/bokeh/plotting/_figure.py:93:7:</a> N801 Class name `figure` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L93'>src/bokeh/plotting/_figure.py:93:7:</a> N801 Class name `figure` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/ack.py#L42'>src/bokeh/protocol/messages/ack.py:42:7:</a> N801 Class name `ack` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/ack.py#L42'>src/bokeh/protocol/messages/ack.py:42:7:</a> N801 Class name `ack` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/error.py#L49'>src/bokeh/protocol/messages/error.py:49:7:</a> N801 Class name `error` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/error.py#L49'>src/bokeh/protocol/messages/error.py:49:7:</a> N801 Class name `error` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/ok.py#L43'>src/bokeh/protocol/messages/ok.py:43:7:</a> N801 Class name `ok` should use CapWords convention
... 593 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N801 | 676 | 338 | 338 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+338 -338 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+18 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/io/utils/stat.py#L22'>airflow/io/utils/stat.py:22:7:</a> N801 Class name `stat_result` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/io/utils/stat.py#L22'>airflow/io/utils/stat.py:22:7:</a> N801 Class name `stat_result` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L39'>airflow/providers/amazon/aws/transfers/sql_to_s3.py:39:7:</a> N801 Class name `FILE_FORMAT` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L39'>airflow/providers/amazon/aws/transfers/sql_to_s3.py:39:7:</a> N801 Class name `FILE_FORMAT` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/cncf/kubernetes/operators/pod.py#L1083'>airflow/providers/cncf/kubernetes/operators/pod.py:1083:7:</a> N801 Class name `_optionally_suppress` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/cncf/kubernetes/operators/pod.py#L1083'>airflow/providers/cncf/kubernetes/operators/pod.py:1083:7:</a> N801 Class name `_optionally_suppress` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L116'>airflow/providers/google/common/hooks/base_google.py:116:7:</a> N801 Class name `retry_if_temporary_quota` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L116'>airflow/providers/google/common/hooks/base_google.py:116:7:</a> N801 Class name `retry_if_temporary_quota` should use CapWords convention 
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L123'>airflow/providers/google/common/hooks/base_google.py:123:7:</a> N801 Class name `retry_if_operation_in_progress` should use CapWords convention
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/providers/google/common/hooks/base_google.py#L123'>airflow/providers/google/common/hooks/base_google.py:123:7:</a> N801 Class name `retry_if_operation_in_progress` should use CapWords convention 
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+320 -320 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L134'>src/bokeh/client/states.py:134:7:</a> N801 Class name `WAITING_FOR_REPLY` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L134'>src/bokeh/client/states.py:134:7:</a> N801 Class name `WAITING_FOR_REPLY` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L68'>src/bokeh/client/states.py:68:7:</a> N801 Class name `NOT_YET_CONNECTED` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L68'>src/bokeh/client/states.py:68:7:</a> N801 Class name `NOT_YET_CONNECTED` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L76'>src/bokeh/client/states.py:76:7:</a> N801 Class name `CONNECTED_BEFORE_ACK` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L76'>src/bokeh/client/states.py:76:7:</a> N801 Class name `CONNECTED_BEFORE_ACK` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L85'>src/bokeh/client/states.py:85:7:</a> N801 Class name `CONNECTED_AFTER_ACK` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L85'>src/bokeh/client/states.py:85:7:</a> N801 Class name `CONNECTED_AFTER_ACK` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L112'>src/bokeh/colors/groups.py:112:7:</a> N801 Class name `cyan` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L112'>src/bokeh/colors/groups.py:112:7:</a> N801 Class name `cyan` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L132'>src/bokeh/colors/groups.py:132:7:</a> N801 Class name `green` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L132'>src/bokeh/colors/groups.py:132:7:</a> N801 Class name `green` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L159'>src/bokeh/colors/groups.py:159:7:</a> N801 Class name `orange` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L159'>src/bokeh/colors/groups.py:159:7:</a> N801 Class name `orange` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L170'>src/bokeh/colors/groups.py:170:7:</a> N801 Class name `pink` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L170'>src/bokeh/colors/groups.py:170:7:</a> N801 Class name `pink` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L182'>src/bokeh/colors/groups.py:182:7:</a> N801 Class name `purple` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L182'>src/bokeh/colors/groups.py:182:7:</a> N801 Class name `purple` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L208'>src/bokeh/colors/groups.py:208:7:</a> N801 Class name `red` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L208'>src/bokeh/colors/groups.py:208:7:</a> N801 Class name `red` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L223'>src/bokeh/colors/groups.py:223:7:</a> N801 Class name `white` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L223'>src/bokeh/colors/groups.py:223:7:</a> N801 Class name `white` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L247'>src/bokeh/colors/groups.py:247:7:</a> N801 Class name `yellow` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L247'>src/bokeh/colors/groups.py:247:7:</a> N801 Class name `yellow` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L48'>src/bokeh/colors/groups.py:48:7:</a> N801 Class name `black` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L48'>src/bokeh/colors/groups.py:48:7:</a> N801 Class name `black` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L65'>src/bokeh/colors/groups.py:65:7:</a> N801 Class name `blue` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L65'>src/bokeh/colors/groups.py:65:7:</a> N801 Class name `blue` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L87'>src/bokeh/colors/groups.py:87:7:</a> N801 Class name `brown` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L87'>src/bokeh/colors/groups.py:87:7:</a> N801 Class name `brown` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L45'>src/bokeh/core/property/validation.py:45:7:</a> N801 Class name `validate` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L45'>src/bokeh/core/property/validation.py:45:7:</a> N801 Class name `validate` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L425'>src/bokeh/layouts.py:425:11:</a> N801 Class name `row` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L425'>src/bokeh/layouts.py:425:11:</a> N801 Class name `row` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L428'>src/bokeh/layouts.py:428:11:</a> N801 Class name `col` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L428'>src/bokeh/layouts.py:428:11:</a> N801 Class name `col` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L903'>src/bokeh/models/plots.py:903:7:</a> N801 Class name `_list_attr_splat` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L903'>src/bokeh/models/plots.py:903:7:</a> N801 Class name `_list_attr_splat` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L931'>src/bokeh/models/plots.py:931:7:</a> N801 Class name `_legend_attr_splat` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L931'>src/bokeh/models/plots.py:931:7:</a> N801 Class name `_legend_attr_splat` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L93'>src/bokeh/plotting/_figure.py:93:7:</a> N801 Class name `figure` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L93'>src/bokeh/plotting/_figure.py:93:7:</a> N801 Class name `figure` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/ack.py#L42'>src/bokeh/protocol/messages/ack.py:42:7:</a> N801 Class name `ack` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/ack.py#L42'>src/bokeh/protocol/messages/ack.py:42:7:</a> N801 Class name `ack` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/error.py#L49'>src/bokeh/protocol/messages/error.py:49:7:</a> N801 Class name `error` should use CapWords convention
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/error.py#L49'>src/bokeh/protocol/messages/error.py:49:7:</a> N801 Class name `error` should use CapWords convention 
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/messages/ok.py#L43'>src/bokeh/protocol/messages/ok.py:43:7:</a> N801 Class name `ok` should use CapWords convention
... 593 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N801 | 676 | 338 | 338 | 0 | 0 |

</p>
</details>




---
