```yaml
number: 7655
title: "[`pycodestyle`] Add E122 rule (\"continuation line missing indentation or outdented\")"
type: pull_request
state: closed
author: hoel-bagard
labels: []
assignees: []
base: main
head: add_E122
created_at: 2023-09-25T13:31:22Z
updated_at: 2023-11-08T13:10:05Z
url: https://github.com/astral-sh/ruff/pull/7655
synced_at: 2026-01-10T23:40:55Z
```

# [`pycodestyle`] Add E122 rule ("continuation line missing indentation or outdented")

---

_Pull request opened by @hoel-bagard on 2023-09-25 13:31_

## Summary

This PR is part of https://github.com/charliermarsh/ruff/issues/2402, it aims to add the `E12` rules, starting with `E122`, along with their fixes.

## Test Plan

The test fixture uses [the one from pycodestyle](https://github.com/PyCQA/pycodestyle/blob/main/testing/data/E12.py
).

## Discussion

I am opening a draft PR while implementation is not done yet in order to be able to receive advice before proceeding.

Implementing the rule using logical lines seems natural (and that is what pycodestyle does). It seems like it would work for continuation lines made using parentheses since a `NonLogicalNewline` is emitted for the new line.
However, when using a backslash (case covered by pycodestyle), no `NonLogicalNewline` token is created, rendering it impossible to know if there is a newline within the logical line.

Could a `NonLogicalNewline` be emitted when there is a backslash, or should the rule be implemented using physical lines (or another method) ?

Example with parentheses:
```python
print("E122", (
"dent"))
```

Example using a backslash:
```python
if some_very_very_very_long_variable_name or var \
or another_very_long_variable_name:
    raise Exception()
```

---

_Comment by @charliermarsh on 2023-09-29 01:23_

Have you taken a look at how pycodestyle solves this problem, given that the `\` doesn't emit a token?

You could also take advantage of the fact that if you see a newline character _without_ a `Tok::Newline` or `Tok::NonLogicalNewline`, then it must be a continuation. We take advantage of this in the `Indexer` struct.


---

_Comment by @hoel-bagard on 2023-10-05 01:31_

The pycodestyle solution rely on the fact that each token contains the physical line number it is on, therefore I can't use their approach.
However I can look for newline characters like in the `Indexer`.

Thank you for the help.

---

_Comment by @codspeed-hq[bot] on 2023-10-28 12:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:add_E122)

### Merging #7655 will **degrade performances by 9.56%**

<sub>Comparing <code>hoel-bagard:add_E122</code> (0bc5d29) with <code>main</code> (f64c389)</sub>



### Summary

`❌ 5` regressions
`✅ 20` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:add_E122)._

### Benchmarks breakdown

|     | Benchmark | `main` | `hoel-bagard:add_E122` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 3.9 ms | 4.2 ms | -5.93% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 74.4 ms | 80.9 ms | -8.07% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 16.2 ms | 17.2 ms | -5.86% |
| ❌ | `linter/all-rules[large/dataset.py]` | 168.6 ms | 186.4 ms | -9.56% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.8 ms | 38 ms | -8.55% |


---

_Comment by @github-actions[bot] on 2023-10-28 20:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/5e830b3592a994635cae11b27ef7a20e929eddd8/rotkehlchen/rotkehlchen.py#L963'>rotkehlchen/rotkehlchen.py:963:29:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/5e830b3592a994635cae11b27ef7a20e929eddd8/rotkehlchen/rotkehlchen.py#L964'>rotkehlchen/rotkehlchen.py:964:29:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/5e830b3592a994635cae11b27ef7a20e929eddd8/rotkehlchen/rotkehlchen.py#L965'>rotkehlchen/rotkehlchen.py:965:25:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/5e830b3592a994635cae11b27ef7a20e929eddd8/rotkehlchen/tests/unit/test_ethereum_inquirer.py#L121'>rotkehlchen/tests/unit/test_ethereum_inquirer.py:121:13:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E122 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @github-actions[bot] on 2023-11-03 03:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/rotkehlchen.py#L963'>rotkehlchen/rotkehlchen.py:963:29:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/rotkehlchen.py#L964'>rotkehlchen/rotkehlchen.py:964:29:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/rotkehlchen.py#L965'>rotkehlchen/rotkehlchen.py:965:25:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/tests/unit/test_ethereum_inquirer.py#L121'>rotkehlchen/tests/unit/test_ethereum_inquirer.py:121:13:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E122 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+65 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/models/ui/ui_element.py#L81'>src/bokeh/models/ui/ui_element.py:81:5:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/util/compiler.py#L329'>src/bokeh/util/compiler.py:329:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/util/compiler.py#L340'>src/bokeh/util/compiler.py:340:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/util/compiler.py#L351'>src/bokeh/util/compiler.py:351:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/util/compiler.py#L366'>src/bokeh/util/compiler.py:366:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/util/compiler.py#L382'>src/bokeh/util/compiler.py:382:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/util/compiler.py#L385'>src/bokeh/util/compiler.py:385:1:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+52 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L103'>selfdrive/car/chrysler/values.py:103:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L113'>selfdrive/car/chrysler/values.py:113:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L120'>selfdrive/car/chrysler/values.py:120:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L124'>selfdrive/car/chrysler/values.py:124:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L128'>selfdrive/car/chrysler/values.py:128:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L131'>selfdrive/car/chrysler/values.py:131:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/chrysler/values.py#L138'>selfdrive/car/chrysler/values.py:138:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L149'>selfdrive/car/gm/values.py:149:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L150'>selfdrive/car/gm/values.py:150:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L154'>selfdrive/car/gm/values.py:154:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L155'>selfdrive/car/gm/values.py:155:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L158'>selfdrive/car/gm/values.py:158:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L159'>selfdrive/car/gm/values.py:159:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L162'>selfdrive/car/gm/values.py:162:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L163'>selfdrive/car/gm/values.py:163:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L167'>selfdrive/car/gm/values.py:167:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L168'>selfdrive/car/gm/values.py:168:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L172'>selfdrive/car/gm/values.py:172:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L173'>selfdrive/car/gm/values.py:173:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L177'>selfdrive/car/gm/values.py:177:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L178'>selfdrive/car/gm/values.py:178:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L182'>selfdrive/car/gm/values.py:182:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L183'>selfdrive/car/gm/values.py:183:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L187'>selfdrive/car/gm/values.py:187:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L188'>selfdrive/car/gm/values.py:188:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L191'>selfdrive/car/gm/values.py:191:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L192'>selfdrive/car/gm/values.py:192:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L196'>selfdrive/car/gm/values.py:196:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L200'>selfdrive/car/gm/values.py:200:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L204'>selfdrive/car/gm/values.py:204:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L208'>selfdrive/car/gm/values.py:208:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L212'>selfdrive/car/gm/values.py:212:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/gm/values.py#L216'>selfdrive/car/gm/values.py:216:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L309'>selfdrive/car/hyundai/values.py:309:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L312'>selfdrive/car/hyundai/values.py:312:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L315'>selfdrive/car/hyundai/values.py:315:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L318'>selfdrive/car/hyundai/values.py:318:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L324'>selfdrive/car/hyundai/values.py:324:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L327'>selfdrive/car/hyundai/values.py:327:3:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/commaai/openpilot/blob/fb3c0934d85d6a056643b2792852884b43fbdd3c/selfdrive/car/hyundai/values.py#L345'>selfdrive/car/hyundai/values.py:345:3:</a> E122 Continuation line missing indentation or outdented.
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/32b8cd2c81108dc51d59d2d09287df29d40d255b/scripts/validate_rst_title_capitalization.py#L269'>scripts/validate_rst_title_capitalization.py:269:21:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/rotkehlchen.py#L963'>rotkehlchen/rotkehlchen.py:963:29:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/rotkehlchen.py#L964'>rotkehlchen/rotkehlchen.py:964:29:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/rotkehlchen.py#L965'>rotkehlchen/rotkehlchen.py:965:25:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/rotki/rotki/blob/6e25d892bba823d873aad92c747f716f2ff9bad4/rotkehlchen/tests/unit/test_ethereum_inquirer.py#L121'>rotkehlchen/tests/unit/test_ethereum_inquirer.py:121:13:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/writers/texinfo.py#L250'>sphinx/writers/texinfo.py:250:17:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E122 | 65 | 65 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @hoel-bagard on 2023-11-03 03:39_

The changes from the PR seem to be slow, I'm guessing that the `get_token_infos` is the slow part, but it was needed to implement rule `E122` the same way as pycodestyle. It will also be useful when implementing the other `E12` rules. Is there a way to implement it that would be faster ?

Part of the code is meant to be used for the other `E12` rules, since at first I thought about also adding them in this PR. However, the pycodestyle implementation does not detect anything for `E121`, `E123` and `E126`, so I will add them in a separate PR (if this PR is accepted). I tried to remove most of the code directly linked to the other rules, but there are still traces of it in some complex and empty ifs.

---

_Marked ready for review by @hoel-bagard on 2023-11-03 03:39_

---

_Comment by @hoel-bagard on 2023-11-08 13:10_

Closed in favor of #8557

---

_Closed by @hoel-bagard on 2023-11-08 13:10_

---
