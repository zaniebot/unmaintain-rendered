```yaml
number: 14746
title: "[`ruff`] Unnecessary `re.escape()` call (`RUF047`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: RUF047
created_at: 2024-12-03T00:41:47Z
updated_at: 2024-12-11T13:04:52Z
url: https://github.com/astral-sh/ruff/pull/14746
synced_at: 2026-01-10T20:42:27Z
```

# [`ruff`] Unnecessary `re.escape()` call (`RUF047`)

---

_Pull request opened by @InSyncWithFoo on 2024-12-03 00:41_

## Summary

Resolves #13705.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-03 00:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+11 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/9a1f20ebfe5484c101a2e9b6b7753e3bb5bfff24/bin/maintenance/make-release.py#L98'>bin/maintenance/make-release.py:98:18:</a> RUF047 [*] Pattern does not need escaping
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9d4f36d87dae9a968fb527e2cb87e8a507b0beb3/testing/test_main.py#L215'>testing/test_main.py:215:31:</a> RUF047 [*] Pattern does not need escaping
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/cosmology/funcs/tests/test_comparison.py#L134'>astropy/cosmology/funcs/tests/test_comparison.py:134:31:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/cosmology/funcs/tests/test_comparison.py#L145'>astropy/cosmology/funcs/tests/test_comparison.py:145:45:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_core.py#L1609'>astropy/modeling/tests/test_core.py:1609:15:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_core.py#L1649'>astropy/modeling/tests/test_core.py:1649:15:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_fitting_parallel.py#L381'>astropy/modeling/tests/test_fitting_parallel.py:381:19:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_fitting_parallel.py#L686'>astropy/modeling/tests/test_fitting_parallel.py:686:15:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_fitting_parallel.py#L747'>astropy/modeling/tests/test_fitting_parallel.py:747:19:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_fitting_parallel.py#L762'>astropy/modeling/tests/test_fitting_parallel.py:762:19:</a> RUF047 [*] Pattern does not need escaping
+ <a href='https://github.com/astropy/astropy/blob/24d3dd96015839ecf87899b02d403a9002bd3ef6/astropy/modeling/tests/test_fitting_parallel.py#L778'>astropy/modeling/tests/test_fitting_parallel.py:778:19:</a> RUF047 [*] Pattern does not need escaping
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF047 | 11 | 11 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-12-03 08:28_

Looking through the ecosystem changes, using `re.escape` seems a common pattern when using `pytest` because it saves the developer the time to think about: Does my exception string need escaping? I'd probably do the same. 

Considering that there are zero other ecosystem findings, I'm not sure it's worth adding this rule when it only flags "arguably valid" usages. 



---

_Label `rule` added by @MichaReiser on 2024-12-03 08:28_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-03 08:28_

---

_Label `preview` added by @MichaReiser on 2024-12-03 08:28_

---

_Comment by @MichaReiser on 2024-12-10 12:31_

Thanks for your work on this rule @InSyncWithFoo It has been very valuable to look through the violations to make an informed decision on the rule. 

Seeing the violations, I don't think we should add the rule because it trains users not to use `re.escape`, and then they'll not use it in the few cases where they should use `re.escape. 

---

_Closed by @MichaReiser on 2024-12-10 12:31_

---

_Branch deleted on 2024-12-11 13:04_

---
