```yaml
number: 8499
title: Allow collapsed-ellipsis bodies in other statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/E701
created_at: 2023-11-06T00:25:25Z
updated_at: 2023-11-06T00:44:28Z
url: https://github.com/astral-sh/ruff/pull/8499
synced_at: 2026-01-10T23:40:55Z
```

# Allow collapsed-ellipsis bodies in other statements

---

_Pull request opened by @charliermarsh on 2023-11-06 00:25_

## Summary

Black and Ruff's preview styles now collapse statements like:

```python
from contextlib import nullcontext

ctx = nullcontext()
with ctx: ...
```

Historically, we made an exception here for classes (https://github.com/astral-sh/ruff/pull/2837). This PR extends it to other statement kinds for consistency with the formatter.

Closes https://github.com/astral-sh/ruff/issues/8496.


---

_Label `bug` added by @charliermarsh on 2023-11-06 00:25_

---

_Merged by @charliermarsh on 2023-11-06 00:42_

---

_Closed by @charliermarsh on 2023-11-06 00:42_

---

_Branch deleted on 2023-11-06 00:42_

---

_Comment by @github-actions[bot] on 2023-11-06 00:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -9 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/docker_utils/__init__.py#L251'>latch_cli/docker_utils/__init__.py:251:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/menus.py#L298'>latch_cli/menus.py:298:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L226'>latch_cli/services/get_executions.py:226:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L319'>latch_cli/services/get_executions.py:319:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L455'>latch_cli/services/get_executions.py:455:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L516'>latch_cli/services/get_executions.py:516:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/local_dev_old.py#L314'>latch_cli/services/local_dev_old.py:314:34:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/preview.py#L179'>latch_cli/services/preview.py:179:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/utils/__init__.py#L203'>latch_cli/utils/__init__.py:203:29:</a> E701 Multiple statements on one line (colon)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E701 | 9 | 0 | 9 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -9 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/docker_utils/__init__.py#L251'>latch_cli/docker_utils/__init__.py:251:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/menus.py#L298'>latch_cli/menus.py:298:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L226'>latch_cli/services/get_executions.py:226:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L319'>latch_cli/services/get_executions.py:319:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L455'>latch_cli/services/get_executions.py:455:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/get_executions.py#L516'>latch_cli/services/get_executions.py:516:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/local_dev_old.py#L314'>latch_cli/services/local_dev_old.py:314:34:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/services/preview.py#L179'>latch_cli/services/preview.py:179:29:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/latchbio/latch/blob/0502cebf68cb76c49df42ff1e546b48b33b2863f/latch_cli/utils/__init__.py#L203'>latch_cli/utils/__init__.py:203:29:</a> E701 Multiple statements on one line (colon)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E701 | 9 | 0 | 9 | 0 | 0 |

</p>
</details>




---
