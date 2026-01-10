```yaml
number: 10566
title: "[`Pylint`] Fixed false-positive on the rule `PLW1641` (`eq-without-hash`)"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - bug
assignees: []
merged: true
base: main
head: false-positive-eq-without-hash
created_at: 2024-03-25T12:03:38Z
updated_at: 2024-03-25T14:50:26Z
url: https://github.com/astral-sh/ruff/pull/10566
synced_at: 2026-01-10T22:47:02Z
```

# [`Pylint`] Fixed false-positive on the rule `PLW1641` (`eq-without-hash`)

---

_Pull request opened by @hikaru-kajita on 2024-03-25 12:03_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixed false-positive on the rule `PLW1641`, where the explicit assignment on the `__hash__` method is not counted as an definition of `__hash__`. (Discussed in #10557).

Also, added one new testcase.

## Test Plan

<!-- How was it tested? -->

Checked on `cargo test` in `eq_without_hash.py`.



Before the change, for the assignment into `__hash__`, only `__hash__ = None` was counted as an explicit definition of `__hash__` method.
Probably any assignment into `__hash__` property could be counted as an explicit definition of hash, so I removed `value.is_none_literal_expr()` check.

---

_Comment by @github-actions[bot] on 2024-03-25 12:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -11 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/c900dc8c09e178b7662cb643d2fd0d651e57c016/pandas/core/dtypes/dtypes.py#L947'>pandas/core/dtypes/dtypes.py:947:7:</a> PLW1641 Object does not implement `__hash__` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L17'>stdlib/io.pyi:17:5:</a> F822 Undefined name `BufferedRWPair` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L18'>stdlib/io.pyi:18:5:</a> F822 Undefined name `BufferedRandom` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L19'>stdlib/io.pyi:19:5:</a> F822 Undefined name `BufferedReader` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L20'>stdlib/io.pyi:20:5:</a> F822 Undefined name `BufferedWriter` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L25'>stdlib/io.pyi:25:5:</a> F822 Undefined name `StringIO` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L26'>stdlib/io.pyi:26:5:</a> F822 Undefined name `TextIOBase` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L27'>stdlib/io.pyi:27:5:</a> F822 Undefined name `TextIOWrapper` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L34'>stdlib/io.pyi:34:40:</a> F822 Undefined name `IncrementalNewlineDecoder` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L34'>stdlib/io.pyi:34:69:</a> F822 Undefined name `text_encoding` in `__all__`
- <a href='https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/stdlib/io.pyi#L36'>stdlib/io.pyi:36:1:</a> PYI018 Private TypeVar `_T` is never used
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/ce1e2d9eb2fa27bd0cc505725602ec4e190bece4/rotkehlchen/history/events/structures/evm_event.py#L63'>rotkehlchen/history/events/structures/evm_event.py:63:36:</a> RUF100 [*] Unused `noqa` directive (unused: `PLW1641`)
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F822 | 9 | 0 | 9 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |
| PLW1641 | 1 | 0 | 1 | 0 | 0 |
| PYI018 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-03-25 14:25_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-03-25 14:31_

---

_Merged by @charliermarsh on 2024-03-25 14:40_

---

_Closed by @charliermarsh on 2024-03-25 14:40_

---
