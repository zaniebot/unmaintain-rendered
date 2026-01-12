```yaml
number: 11381
title: "Reword `future-rewritable-type-annotation` (`FA100`) message"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: fa-reword
created_at: 2024-05-12T19:47:38Z
updated_at: 2024-05-13T01:45:40Z
url: https://github.com/astral-sh/ruff/pull/11381
synced_at: 2026-01-12T15:55:37Z
```

# Reword `future-rewritable-type-annotation` (`FA100`) message

---

_@tjkuson_

## Summary

Changes `future-rewritable-type-annotation` (`FA100`) message to be less confusing. Uses phrasing from the rule documentation to be consistent. For example,

```
from_typing_import.py:5:13: FA100 Add `from __future__ import annotations` to rewrite `typing.List` more succinctly
```

Closes #10573.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-05-12 20:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6447 -6447 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+27 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L51'>airflow/serialization/pydantic/dataset.py:51:12:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L51'>airflow/serialization/pydantic/dataset.py:51:12:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L56'>airflow/serialization/pydantic/dataset.py:56:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.List`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L56'>airflow/serialization/pydantic/dataset.py:56:21:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L57'>airflow/serialization/pydantic/dataset.py:57:22:</a> FA100 Add `from __future__ import annotations` to simplify `typing.List`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L57'>airflow/serialization/pydantic/dataset.py:57:22:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L66'>airflow/serialization/pydantic/dataset.py:66:17:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L66'>airflow/serialization/pydantic/dataset.py:66:17:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L68'>airflow/serialization/pydantic/dataset.py:68:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L68'>airflow/serialization/pydantic/dataset.py:68:21:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6420 -6420 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L105'>analytics/lib/counts.py:105:23:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L105'>analytics/lib/counts.py:105:23:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:68:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:68:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L119'>analytics/lib/counts.py:119:53:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L119'>analytics/lib/counts.py:119:53:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L183'>analytics/lib/counts.py:183:49:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L183'>analytics/lib/counts.py:183:49:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L212'>analytics/lib/counts.py:212:49:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L212'>analytics/lib/counts.py:212:49:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:15:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:15:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:14:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Dict`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:14:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L315'>analytics/lib/counts.py:315:20:</a> FA100 Add `from __future__ import annotations` to simplify `typing.List`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L315'>analytics/lib/counts.py:315:20:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:15:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:15:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Tuple`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:30:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:30:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L470'>analytics/lib/counts.py:470:19:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L470'>analytics/lib/counts.py:470:19:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:15:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:15:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Tuple`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:30:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:30:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L475'>analytics/lib/counts.py:475:73:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L475'>analytics/lib/counts.py:475:73:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L488'>analytics/lib/counts.py:488:51:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L488'>analytics/lib/counts.py:488:51:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L523'>analytics/lib/counts.py:523:69:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L523'>analytics/lib/counts.py:523:69:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Dict`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:21:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:26:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Tuple`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:26:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L558'>analytics/lib/counts.py:558:40:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L558'>analytics/lib/counts.py:558:40:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L58'>analytics/lib/counts.py:58:19:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
... 12791 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 12894 | 6447 | 6447 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6447 -6447 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+27 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L51'>airflow/serialization/pydantic/dataset.py:51:12:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L51'>airflow/serialization/pydantic/dataset.py:51:12:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L56'>airflow/serialization/pydantic/dataset.py:56:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.List`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L56'>airflow/serialization/pydantic/dataset.py:56:21:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L57'>airflow/serialization/pydantic/dataset.py:57:22:</a> FA100 Add `from __future__ import annotations` to simplify `typing.List`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L57'>airflow/serialization/pydantic/dataset.py:57:22:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L66'>airflow/serialization/pydantic/dataset.py:66:17:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L66'>airflow/serialization/pydantic/dataset.py:66:17:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L68'>airflow/serialization/pydantic/dataset.py:68:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/apache/airflow/blob/9e61daff7d271ddf3ad3e1e54f16d6af881acb67/airflow/serialization/pydantic/dataset.py#L68'>airflow/serialization/pydantic/dataset.py:68:21:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6420 -6420 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L105'>analytics/lib/counts.py:105:23:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L105'>analytics/lib/counts.py:105:23:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:68:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L106'>analytics/lib/counts.py:106:68:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L119'>analytics/lib/counts.py:119:53:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L119'>analytics/lib/counts.py:119:53:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L183'>analytics/lib/counts.py:183:49:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L183'>analytics/lib/counts.py:183:49:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L212'>analytics/lib/counts.py:212:49:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L212'>analytics/lib/counts.py:212:49:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:15:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:15:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L306'>analytics/lib/counts.py:306:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:14:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Dict`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:14:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Union`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L314'>analytics/lib/counts.py:314:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L315'>analytics/lib/counts.py:315:20:</a> FA100 Add `from __future__ import annotations` to simplify `typing.List`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L315'>analytics/lib/counts.py:315:20:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:15:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:15:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Tuple`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:30:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L436'>analytics/lib/counts.py:436:30:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L470'>analytics/lib/counts.py:470:19:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L470'>analytics/lib/counts.py:470:19:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:15:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:15:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:24:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Tuple`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:24:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:30:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Type`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L472'>analytics/lib/counts.py:472:30:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L475'>analytics/lib/counts.py:475:73:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L475'>analytics/lib/counts.py:475:73:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L488'>analytics/lib/counts.py:488:51:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L488'>analytics/lib/counts.py:488:51:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L523'>analytics/lib/counts.py:523:69:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L523'>analytics/lib/counts.py:523:69:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:21:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Dict`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:21:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:26:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Tuple`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L536'>analytics/lib/counts.py:536:26:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L558'>analytics/lib/counts.py:558:40:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L558'>analytics/lib/counts.py:558:40:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
+ <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/analytics/lib/counts.py#L58'>analytics/lib/counts.py:58:19:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Optional`
... 12791 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 12894 | 6447 | 6447 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-05-13 01:31_

---

_Label `rule` added by @charliermarsh on 2024-05-13 01:31_

---

_Merged by @charliermarsh on 2024-05-13 01:38_

---

_Closed by @charliermarsh on 2024-05-13 01:38_

---
