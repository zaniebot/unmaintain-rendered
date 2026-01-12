```yaml
number: 10265
title: Avoid repeating function calls in f-string conversions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/up-function
created_at: 2024-03-07T04:14:42Z
updated_at: 2024-03-07T04:33:20Z
url: https://github.com/astral-sh/ruff/pull/10265
synced_at: 2026-01-12T15:55:31Z
```

# Avoid repeating function calls in f-string conversions

---

_@charliermarsh_

## Summary

Given a format string like `"{x} {x}".format(x=foo())`, we should avoid converting to an f-string, since doing so would require repeating the function call (`f"{foo()} {foo()}"`), which could introduce side effects.

Closes https://github.com/astral-sh/ruff/issues/10258.

---

_Label `bug` added by @charliermarsh on 2024-03-07 04:14_

---

_Comment by @charliermarsh on 2024-03-07 04:17_

We could arguably make this an unsafe fix, but I think it's unlikely to be a desirable change for users.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 04:17_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-03-07 04:17_

---

_Comment by @github-actions[bot] on 2024-03-07 04:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/tests/functional/test_install.py#L269'>tests/functional/test_install.py:269:9:</a> UP032 Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/corporate/lib/stripe.py#L2207'>corporate/lib/stripe.py:2207:28:</a> UP032 Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/3f824542e0f0459718151ffbde3a1db2db145b3f/indico/core/db/sqlalchemy/custom/greatest.py#L24'>indico/core/db/sqlalchemy/custom/greatest.py:24:12:</a> UP032 Use f-string instead of `format` call
- <a href='https://github.com/indico/indico/blob/3f824542e0f0459718151ffbde3a1db2db145b3f/indico/core/db/sqlalchemy/custom/least.py#L24'>indico/core/db/sqlalchemy/custom/least.py:24:12:</a> UP032 Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP032 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/tests/functional/test_install.py#L269'>tests/functional/test_install.py:269:9:</a> UP032 Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/corporate/lib/stripe.py#L2207'>corporate/lib/stripe.py:2207:28:</a> UP032 Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/3f824542e0f0459718151ffbde3a1db2db145b3f/indico/core/db/sqlalchemy/custom/greatest.py#L24'>indico/core/db/sqlalchemy/custom/greatest.py:24:12:</a> UP032 Use f-string instead of `format` call
- <a href='https://github.com/indico/indico/blob/3f824542e0f0459718151ffbde3a1db2db145b3f/indico/core/db/sqlalchemy/custom/least.py#L24'>indico/core/db/sqlalchemy/custom/least.py:24:12:</a> UP032 Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP032 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-03-07 04:33_

\cc @ThiefMaster who filed this

---

_Merged by @charliermarsh on 2024-03-07 04:33_

---

_Closed by @charliermarsh on 2024-03-07 04:33_

---

_Branch deleted on 2024-03-07 04:33_

---
