```yaml
number: 12415
title: "Avoid shadowing diagnostics for `@override` methods"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/o
created_at: 2024-07-20T01:19:08Z
updated_at: 2024-08-22T17:40:02Z
url: https://github.com/astral-sh/ruff/pull/12415
synced_at: 2026-01-12T15:55:41Z
```

# Avoid shadowing diagnostics for `@override` methods

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/12412.

---

_Marked ready for review by @charliermarsh on 2024-07-20 01:19_

---

_Label `bug` added by @charliermarsh on 2024-07-20 01:19_

---

_Comment by @github-actions[bot] on 2024-07-20 01:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -6 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L57'>src/bokeh/core/property/enum.py:57:78:</a> A002 Argument `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L59'>src/bokeh/core/property/enum.py:59:75:</a> A002 Argument `help` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/lib/db.py#L38'>zerver/lib/db.py:38:37:</a> A002 Argument `vars` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/tests/test_auth_backends.py#L1954'>zerver/tests/test_auth_backends.py:1954:9:</a> A002 Argument `next` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/tests/test_auth_backends.py#L3475'>zerver/tests/test_auth_backends.py:3475:9:</a> A002 Argument `next` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/tests/test_auth_backends.py#L3521'>zerver/tests/test_auth_backends.py:3521:9:</a> A002 Argument `next` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A002 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -6 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L57'>src/bokeh/core/property/enum.py:57:78:</a> A002 Argument `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L59'>src/bokeh/core/property/enum.py:59:75:</a> A002 Argument `help` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/lib/db.py#L38'>zerver/lib/db.py:38:37:</a> A002 Argument `vars` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/tests/test_auth_backends.py#L1954'>zerver/tests/test_auth_backends.py:1954:9:</a> A002 Argument `next` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/tests/test_auth_backends.py#L3475'>zerver/tests/test_auth_backends.py:3475:9:</a> A002 Argument `next` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/d5883cfbb21dd1001ea87fc3840015e745a6b52a/zerver/tests/test_auth_backends.py#L3521'>zerver/tests/test_auth_backends.py:3521:9:</a> A002 Argument `next` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A002 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-07-20 01:32_

---

_Closed by @charliermarsh on 2024-07-20 01:32_

---

_Branch deleted on 2024-07-20 01:32_

---

_Comment by @cbornet on 2024-08-22 17:38_

I'm not sure it's good to always ignore the rule on overrides.
I have some classes that look like this:
```python
class A:
    def foo(**kwargs):
        pass

class B(A):
    @override
    def foo(*, filter, **kwargs):
        pass
 ```
 Previously, it was triggering `A002` because `filter` is a builtin which I had to silence (couldn't rename because of backward compatibility).
 I think that was fair.
 But now the rule is not triggered anymore.

---
