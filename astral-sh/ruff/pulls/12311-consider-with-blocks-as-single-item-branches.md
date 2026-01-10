```yaml
number: 12311
title: "Consider `with` blocks as single-item branches"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/with
created_at: 2024-07-13T18:24:45Z
updated_at: 2024-07-13T19:22:33Z
url: https://github.com/astral-sh/ruff/pull/12311
synced_at: 2026-01-10T21:47:02Z
```

# Consider `with` blocks as single-item branches

---

_Pull request opened by @charliermarsh on 2024-07-13 18:24_

## Summary

Ensures that, e.g., the following is not considered a redefinition-without-use:

```python
import contextlib

foo = None
with contextlib.suppress(ImportError):
    from some_module import foo
```

Closes https://github.com/astral-sh/ruff/issues/12309.


---

_Label `bug` added by @charliermarsh on 2024-07-13 18:24_

---

_Comment by @github-actions[bot] on 2024-07-13 19:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -4 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/7fad038290ed67327528785812c7b3b6afaa7524/blinksync/blinksync.py#L58'>blinksync/blinksync.py:58:53:</a> F811 Redefinition of unused `working` from line 55
- <a href='https://github.com/fronzbot/blinkpy/blob/7fad038290ed67327528785812c7b3b6afaa7524/blinksync/blinksync.py#L81'>blinksync/blinksync.py:81:53:</a> F811 Redefinition of unused `working` from line 78
</pre>

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/prefecthq/prefect/blob/66974e398882308bb3524155f915b765f656c50b/src/prefect/cli/cloud/__init__.py#L266'>src/prefect/cli/cloud/__init__.py:266:42:</a> F811 Redefinition of unused `timeout_scope` from line 241
</pre>

</p>
</details>
<details><summary><a href="https://github.com/encode/httpx">encode/httpx</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/encode/httpx/blob/db9072f998b53ff66d50778bf5edee8e2cc8ede1/tests/client/test_auth.py#L371'>tests/client/test_auth.py:371:73:</a> F811 Redefinition of unused `client` from line 366
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mesonbuild/meson-python/blob/063fe8bc8ad6984c29b24a0f72a14e1ced091a9c/tests/test_editable.py#L287'>tests/test_editable.py:287:30:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 4 | 0 | 4 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -4 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/7fad038290ed67327528785812c7b3b6afaa7524/blinksync/blinksync.py#L58'>blinksync/blinksync.py:58:53:</a> F811 Redefinition of unused `working` from line 55
- <a href='https://github.com/fronzbot/blinkpy/blob/7fad038290ed67327528785812c7b3b6afaa7524/blinksync/blinksync.py#L81'>blinksync/blinksync.py:81:53:</a> F811 Redefinition of unused `working` from line 78
</pre>

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/prefecthq/prefect/blob/66974e398882308bb3524155f915b765f656c50b/src/prefect/cli/cloud/__init__.py#L266'>src/prefect/cli/cloud/__init__.py:266:42:</a> F811 Redefinition of unused `timeout_scope` from line 241
</pre>

</p>
</details>
<details><summary><a href="https://github.com/encode/httpx">encode/httpx</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/encode/httpx/blob/db9072f998b53ff66d50778bf5edee8e2cc8ede1/tests/client/test_auth.py#L371'>tests/client/test_auth.py:371:73:</a> F811 Redefinition of unused `client` from line 366
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mesonbuild/meson-python/blob/063fe8bc8ad6984c29b24a0f72a14e1ced091a9c/tests/test_editable.py#L287'>tests/test_editable.py:287:30:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 4 | 0 | 4 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-07-13 19:22_

---

_Closed by @charliermarsh on 2024-07-13 19:22_

---

_Branch deleted on 2024-07-13 19:22_

---

_Comment by @charliermarsh on 2024-07-13 19:22_

Ecosystem checks are generally improvements. [This one](https://github.com/encode/httpx/blob/db9072f998b53ff66d50778bf5edee8e2cc8ede1/tests/client/test_auth.py#L371) is debatable though.

---
