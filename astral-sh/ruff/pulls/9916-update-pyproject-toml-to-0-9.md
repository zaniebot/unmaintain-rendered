```yaml
number: 9916
title: Update pyproject-toml to 0.9
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: upgrade-pyproject-toml
created_at: 2024-02-09T20:32:26Z
updated_at: 2024-02-09T20:50:28Z
url: https://github.com/astral-sh/ruff/pull/9916
synced_at: 2026-01-12T15:55:30Z
```

# Update pyproject-toml to 0.9

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/9913

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-02-09 20:32_

---

_@charliermarsh approved on 2024-02-09 20:33_

---

_Comment by @charliermarsh on 2024-02-09 20:35_

It looks like the yank was due to a breaking change in the API. They released 0.8.2, which bumped the PEP 508 crate version, which was actually a non-semver-compatible change for pyproject.toml -- so they yanked 0.8.2 and re-released as 0.9.0.

---

_Comment by @MichaReiser on 2024-02-09 20:36_

> It looks like the yank was due to a breaking change in the API. They released 0.8.2, which bumped the PEP 508 crate version, which was actually a non-semver-compatible change for pyproject.toml -- so they yanked 0.8.2 and re-released as 0.9.0.

Thanks for summarising the yank reason. I should have done so in my summary

---

_Comment by @charliermarsh on 2024-02-09 20:37_

Oh no prob. I was just curious myself.

---

_Merged by @MichaReiser on 2024-02-09 20:38_

---

_Closed by @MichaReiser on 2024-02-09 20:38_

---

_Branch deleted on 2024-02-09 20:38_

---

_Comment by @codspeed-hq[bot] on 2024-02-09 20:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/upgrade-pyproject-toml)

### Merging #9916 will **degrade performances by 7.2%**

<sub>Comparing <code>upgrade-pyproject-toml</code> (e995f7d) with <code>main</code> (52ebfc9)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/upgrade-pyproject-toml)._

### Benchmarks breakdown

|     | Benchmark | `main` | `upgrade-pyproject-toml` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 2.7 ms | 2.9 ms | -7.2% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 8.5 ms | 8 ms | +6.21% |


---

_Comment by @github-actions[bot] on 2024-02-09 20:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+33 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L122'>tests/integration/testdata/start_api/lambda_authorizers/app.py:122:17:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L123'>tests/integration/testdata/start_api/lambda_authorizers/app.py:123:15:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L131'>tests/integration/testdata/start_api/lambda_authorizers/app.py:131:8:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L132'>tests/integration/testdata/start_api/lambda_authorizers/app.py:132:9:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L133'>tests/integration/testdata/start_api/lambda_authorizers/app.py:133:8:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L134'>tests/integration/testdata/start_api/lambda_authorizers/app.py:134:10:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L135'>tests/integration/testdata/start_api/lambda_authorizers/app.py:135:9:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L136'>tests/integration/testdata/start_api/lambda_authorizers/app.py:136:11:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L138'>tests/integration/testdata/start_api/lambda_authorizers/app.py:138:8:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L140'>tests/integration/testdata/start_api/lambda_authorizers/app.py:140:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L14'>tests/integration/testdata/start_api/lambda_authorizers/app.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L159'>tests/integration/testdata/start_api/lambda_authorizers/app.py:159:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L17'>tests/integration/testdata/start_api/lambda_authorizers/app.py:17:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L203'>tests/integration/testdata/start_api/lambda_authorizers/app.py:203:30:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L204'>tests/integration/testdata/start_api/lambda_authorizers/app.py:204:29:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L208'>tests/integration/testdata/start_api/lambda_authorizers/app.py:208:30:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L209'>tests/integration/testdata/start_api/lambda_authorizers/app.py:209:29:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L212'>tests/integration/testdata/start_api/lambda_authorizers/app.py:212:9:</a> PLR6301 Method `_getEmptyStatement` could be a function, class method, or static method
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L284'>tests/integration/testdata/start_api/lambda_authorizers/app.py:284:26:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L285'>tests/integration/testdata/start_api/lambda_authorizers/app.py:285:29:</a> E203 [*] Whitespace before ':'
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L286'>tests/integration/testdata/start_api/lambda_authorizers/app.py:286:26:</a> E203 [*] Whitespace before ':'
... 8 additional changes omitted for rule E203
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L34'>tests/integration/testdata/start_api/lambda_authorizers/app.py:34:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L44'>tests/integration/testdata/start_api/lambda_authorizers/app.py:44:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L47'>tests/integration/testdata/start_api/lambda_authorizers/app.py:47:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L59'>tests/integration/testdata/start_api/lambda_authorizers/app.py:59:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/06502930837677227f7975c93402039821d1f0ea/tests/integration/testdata/start_api/lambda_authorizers/app.py#L68'>tests/integration/testdata/start_api/lambda_authorizers/app.py:68:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary>Changes by rule (6 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E203 | 16 | 16 | 0 | 0 | 0 |
| E221 | 7 | 7 | 0 | 0 | 0 |
| E302 | 7 | 7 | 0 | 0 | 0 |
| I001 | 1 | 1 | 0 | 0 | 0 |
| E303 | 1 | 1 | 0 | 0 | 0 |
| PLR6301 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
