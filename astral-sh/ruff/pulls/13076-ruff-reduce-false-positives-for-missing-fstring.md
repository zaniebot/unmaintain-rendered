```yaml
number: 13076
title: "[`ruff`] Reduce false positives for `missing-fstring-syntax` (`RUF027`) by analyzing references to the binding when the string is assigned to a variable"
type: pull_request
state: open
author: AlexWaygood
labels:
  - rule
  - preview
assignees: []
draft: true
base: main
head: alex/ruf027-3
created_at: 2024-08-23T12:28:11Z
updated_at: 2025-10-29T15:05:27Z
url: https://github.com/astral-sh/ruff/pull/13076
synced_at: 2026-01-10T16:59:49Z
```

# [`ruff`] Reduce false positives for `missing-fstring-syntax` (`RUF027`) by analyzing references to the binding when the string is assigned to a variable

---

_Pull request opened by @AlexWaygood on 2024-08-23 12:28_

## Summary

When we tried to stabilise RUF027 for the 0.6 release earlier this month, the ecosystem report revealed lots of false positives along the lines of [this](https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/main.py#L1055-L1060):

```py
        msg = (
            "module or package not found: {arg} (missing __init__.py?)"
            if as_pypath
            else "file or directory not found: {arg}"
        )
        raise UsageError(msg.format(arg=arg))
```

A human can see quite clearly that this isn't meant to be an f-string, since the variable it's assigned to immediately has `.format()` called on it. Because of the indirection in the code, however, the logic for RUF027 can't see that right now, since it only checks for methods being _directly_ called on strings with braces; it doesn't check for methods being called on symbols that have strings with braces as their assigned value.

This PR tackles these false positives by splitting the check into two functions. If we detect that a string literal is part of a "simple assignment" (`x: str = "foo"` or `x = "foo"`), we defer checking whether it's an "f-string with the `f`" until the entire AST has been visited. This enables us to iterate through all the references to the binding and check whether any of them are used in ways that make it clear the string is intended to be a template string rather than an f-string. (For string literals that are _not_ inside simple assignments, we analyze them as we did before, while we're doing our initial traversal of the AST.)

## Test Plan

I added some new fixtures, but I won't consider this PR a success unless it results in some false positives going away from the ecosystem report. Otherwise, I don't think it's worth the added complexity.


---

_Label `rule` added by @AlexWaygood on 2024-08-23 12:28_

---

_Label `preview` added by @AlexWaygood on 2024-08-23 12:34_

---

_Comment by @github-actions[bot] on 2024-08-23 12:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -20 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/56a3987c3fca3a574b8f1c7faa8f08de2becee60/tests/providers/docker/operators/test_docker.py#L568'>tests/providers/docker/operators/test_docker.py:568:28:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/07985e2f5aa165f6868abbf88594e6d75300caae/superset/db_engine_specs/base.py#L835'>superset/db_engine_specs/base.py:835:25:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/13cdd115ec964f396a4c9cbf7296a0cae344c274/pandas/io/formats/format.py#L1247'>pandas/io/formats/format.py:1247:27:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/333eedb351e195a85932c935f739a629e6dfc3d8/zerver/webhooks/librato/view.py#L129'>zerver/webhooks/librato/view.py:129:38:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/333eedb351e195a85932c935f739a629e6dfc3d8/zerver/webhooks/librato/view.py#L148'>zerver/webhooks/librato/view.py:148:13:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/ab259a36e3359659e4568358fb0e9d43b10be5de/src/_pytest/main.py#L1056'>src/_pytest/main.py:1056:13:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pytest-dev/pytest/blob/ab259a36e3359659e4568358fb0e9d43b10be5de/src/_pytest/main.py#L1058'>src/_pytest/main.py:1058:18:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pytest-dev/pytest/blob/ab259a36e3359659e4568358fb0e9d43b10be5de/src/_pytest/main.py#L1063'>src/_pytest/main.py:1063:13:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pytest-dev/pytest/blob/ab259a36e3359659e4568358fb0e9d43b10be5de/src/_pytest/main.py#L1065'>src/_pytest/main.py:1065:18:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L80'>src/pdm/pep582/sitecustomize.py:80:19:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L81'>src/pdm/pep582/sitecustomize.py:81:23:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L82'>src/pdm/pep582/sitecustomize.py:82:20:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L83'>src/pdm/pep582/sitecustomize.py:83:20:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L84'>src/pdm/pep582/sitecustomize.py:84:20:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L86'>src/pdm/pep582/sitecustomize.py:86:17:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L87'>src/pdm/pep582/sitecustomize.py:87:19:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/src/pdm/pep582/sitecustomize.py#L88'>src/pdm/pep582/sitecustomize.py:88:20:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/tests/cli/test_run.py#L172'>tests/cli/test_run.py:172:22:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/tests/cli/test_run.py#L314'>tests/cli/test_run.py:314:22:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pdm-project/pdm/blob/7d52cfd1cc1d133f9fe777909db242d31daa78a3/tests/cli/test_run.py#L776'>tests/cli/test_run.py:776:41:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF027 | 20 | 0 | 20 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-08-23 13:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fruf027-3?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #13076 will **not alter performance**

<sub>Comparing <code>alex/ruf027-3</code> (09edde8) with <code>main</code> (a998320)</sub>



### Summary

`✅ 32` untouched  





---

_Comment by @AlexWaygood on 2024-08-23 13:33_

> ℹ️ ecosystem check **detected linter changes**. (+0 -5 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

That's 5 false positives going away, so the PR is "working". I was hoping for the impact to be a little bigger, though. I suppose cases like [this](https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/main.py#L1055-L1060) aren't counted as "simple assignments" according to the logic I added because the r.h.s. is a conditional expression rather than just a string literal.

This is slightly underwhelming, but I think it's still an improvement. I think it's probably worth the extra complexity, but not sure.

---

_Converted to draft by @AlexWaygood on 2024-08-29 11:21_

---
