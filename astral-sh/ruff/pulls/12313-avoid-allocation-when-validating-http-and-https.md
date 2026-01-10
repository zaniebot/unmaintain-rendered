```yaml
number: 12313
title: Avoid allocation when validating HTTP and HTTPS prefixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/alloc
created_at: 2024-07-13T21:00:39Z
updated_at: 2024-07-13T22:05:19Z
url: https://github.com/astral-sh/ruff/pull/12313
synced_at: 2026-01-10T21:47:02Z
```

# Avoid allocation when validating HTTP and HTTPS prefixes

---

_Pull request opened by @charliermarsh on 2024-07-13 21:00_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-07-13 21:00_

---

_Label `performance` added by @charliermarsh on 2024-07-13 21:00_

---

_Comment by @github-actions[bot] on 2024-07-13 21:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L111'>tests/support/plugins/file_server.py:111:13:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f23fdede6750f335af3c0e3049edab12faf56d12/scripts/lib/puppet_cache.py#L67'>scripts/lib/puppet_cache.py:67:10:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S310 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L111'>tests/support/plugins/file_server.py:111:13:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f23fdede6750f335af3c0e3049edab12faf56d12/scripts/lib/puppet_cache.py#L67'>scripts/lib/puppet_cache.py:67:10:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S310 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-07-13 21:25_

---

_Closed by @charliermarsh on 2024-07-13 21:25_

---

_Branch deleted on 2024-07-13 21:25_

---
