```yaml
number: 11053
title: "[`pygrep_hooks`] Move `blanket-noqa` to noqa checker (`PGH004`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - internal
assignees: []
merged: true
base: main
head: pgh004
created_at: 2024-04-20T03:09:47Z
updated_at: 2024-04-22T17:37:59Z
url: https://github.com/astral-sh/ruff/pull/11053
synced_at: 2026-01-10T22:37:01Z
```

# [`pygrep_hooks`] Move `blanket-noqa` to noqa checker (`PGH004`)

---

_Pull request opened by @augustelalande on 2024-04-20 03:09_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Move `blanket-noqa` rule from the token checker to the noqa checker. This allows us to make use of the line directives already computed in the noqa checker.

## Test Plan

Verified test results are unchanged.


---

_Comment by @github-actions[bot] on 2024-04-20 03:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L146'>src/bokeh/embed/util.py:146:32:</a> RUF100 [*] Unused blanket `noqa` directive
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L146'>src/bokeh/embed/util.py:146:32:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L1'>pytestgeventwrapper.py:1:48:</a> RUF100 [*] Unused blanket `noqa` directive
- <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L1'>pytestgeventwrapper.py:1:48:</a> RUF100 [*] Unused blanket `noqa` directive
+ <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L2'>pytestgeventwrapper.py:2:34:</a> RUF100 [*] Unused blanket `noqa` directive
- <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L2'>pytestgeventwrapper.py:2:34:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 6 | 3 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L146'>src/bokeh/embed/util.py:146:32:</a> RUF100 [*] Unused blanket `noqa` directive
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L146'>src/bokeh/embed/util.py:146:32:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L1'>pytestgeventwrapper.py:1:48:</a> RUF100 [*] Unused blanket `noqa` directive
- <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L1'>pytestgeventwrapper.py:1:48:</a> RUF100 [*] Unused blanket `noqa` directive
+ <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L2'>pytestgeventwrapper.py:2:34:</a> RUF100 [*] Unused blanket `noqa` directive
- <a href='https://github.com/rotki/rotki/blob/a82f29b86874ee2dc36a6e134d117b80347d40eb/pytestgeventwrapper.py#L2'>pytestgeventwrapper.py:2:34:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 6 | 3 | 3 | 0 | 0 |

</p>
</details>




---

_@MichaReiser approved on 2024-04-22 07:16_

---

_Label `internal` added by @MichaReiser on 2024-04-22 07:16_

---

_Comment by @MichaReiser on 2024-04-22 07:17_

Uhh, what are the differences in the ecosystem check. Are the ranges different? 

---

_Comment by @augustelalande on 2024-04-22 17:34_

It's because the reporting order gets switched.
Before
```
➜  ruff check ..\test\ruff_test\main.py --no-cache --exit-zero --output-format concise --preview --select RUF100,PGH004
C:\Users\Auguste\workspace\test\ruff_test\main.py:158:32: PGH004 Use specific rule codes when using `noqa`
C:\Users\Auguste\workspace\test\ruff_test\main.py:158:32: RUF100 [*] Unused blanket `noqa` directive
Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

After
```
➜  cargo run -p ruff -- check ..\test\ruff_test\main.py --no-cache --preview --exit-zero --output-format concise --select RUF100,PGH004
    Finished dev [unoptimized + debuginfo] target(s) in 0.44s
     Running `target\debug\ruff.exe check ..\test\ruff_test\main.py --no-cache --preview --exit-zero --output-format concise --select RUF100,PGH004`
C:\Users\Auguste\workspace\test\ruff_test\main.py:158:32: RUF100 [*] Unused blanket `noqa` directive
C:\Users\Auguste\workspace\test\ruff_test\main.py:158:32: PGH004 Use specific rule codes when using `noqa`
Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @charliermarsh on 2024-04-22 17:36_

Oh hah that's funny. Thanks for looking into it.

---

_Merged by @charliermarsh on 2024-04-22 17:36_

---

_Closed by @charliermarsh on 2024-04-22 17:36_

---

_Branch deleted on 2024-04-22 17:37_

---
