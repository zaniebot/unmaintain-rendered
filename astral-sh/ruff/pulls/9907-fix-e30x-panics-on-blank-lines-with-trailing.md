```yaml
number: 9907
title: "Fix `E30X` panics on blank lines with trailing white spaces"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - bug
assignees: []
merged: true
base: main
head: hoel/fix_E30X_panics
created_at: 2024-02-09T09:25:44Z
updated_at: 2024-02-09T14:07:02Z
url: https://github.com/astral-sh/ruff/pull/9907
synced_at: 2026-01-10T22:57:09Z
```

# Fix `E30X` panics on blank lines with trailing white spaces

---

_Pull request opened by @hoel-bagard on 2024-02-09 09:25_

## Summary

closes #9906
The #9906 issue is triggered by blank lines with trailing white spaces. The trailing spaces cause the  `assert_eq!` that was added in [this discussion](
https://github.com/astral-sh/ruff/pull/9266#discussion_r1452383949) to fail.

This PR removes the  `assert_eq!`.

## Test Plan

I tested the change on the python files provided in #9906 (on top of cargo test).
I also added a few fixtures with blank lines with trailing white spaces.

## Issue example/explaination
The issue is triggered by blank lines with trailing white spaces. 
For example, by creating a blank file with `echo "\n     \n" >> temp.py`, we get the snippet given in the issue:

```

     
```
Running ruff on this, we get the following tokens:
```
(NonLogicalNewline, 0..1)
(NonLogicalNewline, 6..7)
(NonLogicalNewline, 7..8)
```

We can see that the end of the first token range's end and the second token range's start do not match because of the 5 spaces.

---

_Comment by @github-actions[bot] on 2024-02-09 09:41_

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




---

_Marked ready for review by @hoel-bagard on 2024-02-09 09:46_

---

_Comment by @MichaReiser on 2024-02-09 12:44_

Thanks for looking into the panic. I'm reluctant to just remove the assertion without better understanding the reason why it is incorrect (I added it to guarantee it's only called on two directly following lines). Would you mind adding an example explaining why the two ranges don't end and start at the same position to the PR summary? 

---

_Label `bug` added by @MichaReiser on 2024-02-09 12:44_

---

_Comment by @hoel-bagard on 2024-02-09 13:45_

@MichaReiser  This happens because of the whitespaces, I added an explanation to the PR summary.

---

_@MichaReiser approved on 2024-02-09 14:00_

---

_Merged by @MichaReiser on 2024-02-09 14:00_

---

_Closed by @MichaReiser on 2024-02-09 14:00_

---

_Comment by @MichaReiser on 2024-02-09 14:00_

Thanks for the explanation. That makes sense 

---
