```yaml
number: 6152
title: Preserve backslash in raw string literal
type: pull_request
state: merged
author: harupy
labels:
  - formatter
assignees: []
merged: true
base: main
head: preserve-backslash
created_at: 2023-07-28T14:05:13Z
updated_at: 2023-07-31T16:29:44Z
url: https://github.com/astral-sh/ruff/pull/6152
synced_at: 2026-01-12T02:58:30Z
```

# Preserve backslash in raw string literal

---

_Pull request opened by @harupy on 2023-07-28 14:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #5941

## Test Plan

<!-- How was it tested? -->

Existing tests


---

_Comment by @github-actions[bot] on 2023-07-28 14:39_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.05ms     4.5 MB/sec    1.00      9.0±0.06ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1730.8±7.25µs     9.6 MB/sec    1.00  1726.8±15.26µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    181.1±0.53µs    16.3 MB/sec    1.00    180.2±0.88µs    16.4 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.02     12.0±0.12ms     3.4 MB/sec    1.00     11.8±0.12ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.0±0.02ms     5.5 MB/sec    1.00      3.0±0.01ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    325.1±1.64µs     9.1 MB/sec    1.00    326.4±0.92µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      5.5±0.05ms     4.7 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.04      6.7±0.03ms     6.0 MB/sec    1.00      6.5±0.03ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1314.0±2.36µs    12.7 MB/sec    1.00   1278.9±2.27µs    13.0 MB/sec
linter/default-rules/numpy/globals.py      1.04    129.8±0.63µs    22.7 MB/sec    1.00    125.2±0.35µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.9±0.01ms     8.8 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.0±0.77ms     2.9 MB/sec    1.02     14.3±0.71ms     2.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.14ms     6.3 MB/sec    1.02      2.7±0.11ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   298.1±16.71µs     9.9 MB/sec    1.00   297.3±17.43µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.6±0.24ms     4.5 MB/sec    1.04      5.9±0.28ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.04     20.1±0.90ms     2.0 MB/sec    1.00     19.2±0.97ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.9±0.29ms     3.4 MB/sec    1.00      4.9±0.26ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   577.0±41.43µs     5.1 MB/sec    1.05   606.8±35.50µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.55ms     3.0 MB/sec    1.01      8.6±0.47ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.04     11.0±0.46ms     3.7 MB/sec    1.00     10.6±0.53ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.2±0.11ms     7.6 MB/sec    1.00      2.1±0.09ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   238.4±11.41µs    12.4 MB/sec    1.04   247.2±13.92µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.17ms     5.6 MB/sec    1.00      4.5±0.26ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:213 on 2023-07-28 16:31_

Nit: I would implement this as a method on `StringPrefix` to reduce the complexity here
```suggestion
			prefix.is_raw_string()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@miscellaneous__string_quotes.py.snap`:90 on 2023-07-28 16:33_

There seems to be a related issue that we don't count the `"` in `preferred_quotes` for raw strings. Do you want to take a look at this as well, and if so, as part of this PR or do you prefer a second pr?

---

_@MichaReiser reviewed on 2023-07-28 16:33_

---

_@harupy reviewed on 2023-07-28 16:37_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/expression/string.rs`:213 on 2023-07-28 16:37_

Sounds good. I'll make the change.

---

_Review comment by @harupy on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@miscellaneous__string_quotes.py.snap`:90 on 2023-07-28 16:45_

@MichaReiser 

> There seems to be a related issue that we don't count the " in preferred_quotes for raw strings.

Can you elaborate on this?

---

_@harupy reviewed on 2023-07-28 16:45_

---

_Review comment by @harupy on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@miscellaneous__string_quotes.py.snap`:90 on 2023-07-28 17:21_

Anyway, I prefer a second PR.

---

_@harupy reviewed on 2023-07-28 17:21_

---

_@MichaReiser reviewed on 2023-07-28 17:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:479 on 2023-07-28 17:30_

Do we need to move this check to line 490 instead because we need to make sure that quotes are properly escaped. Can you add the following test

```python
r'It\'s normalizing \' and " quotes'
```

This should be formatted as:

```python
r"It's normalizing ' and \" quotes"r
```

---

_@harupy reviewed on 2023-07-29 00:45_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/expression/string.rs`:479 on 2023-07-29 00:45_

@MichaReiser `r'It\'s normalizing \' and " quotes'` is not equivalent to `r"It's normalizing ' and \" quotes"`:

```python
>>> r'It\'s normalizing \' and " quotes'
'It\\\'s normalizing \\\' and " quotes'
>>> r"It's normalizing ' and \" quotes"
'It\'s normalizing \' and \\" quotes'
>>> r'It\'s normalizing \' and " quotes' == r"It's normalizing ' and \" quotes"
False
```

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/expression/string.rs`:532 on 2023-07-29 02:36_

Is this the right place to insert `is_raw`? We can't insert or remove `/` if we're normalizing a raw string.

---

_@harupy reviewed on 2023-07-29 02:36_

---

_@MichaReiser reviewed on 2023-07-29 07:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:479 on 2023-07-29 07:52_

Okay, I now played with your PR and found an example that produces invalid syntax:

```python
# Input
r'Not-so-tricky "quote \'\''

# Ruff
r"Not-so-tricky "quote \'\'"

# Black
r'Not-so-tricky "quote \'\''
```

Note how Ruff changes the quotes from `'` to `"` but fails to escape the `"`.  

We need to play a bit more with black to understand how black determines the preferred quotes for raw strings, and how the normalization has to work. Maybe @konstin knows more, because I'm not that familiar with Python and I must say, the escaping logic behind raw strings is confusing to me (you have to escape quotes)

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/expression/string.rs`:479 on 2023-07-29 08:16_

Thanks for the investigation. I'm reading black's source code. It looks like black returns the original string if it contains unescaped opposite quotes:

https://github.com/psf/black/blob/1a972e3e11b144912155babdf48ff23d68059d57/src/black/strings.py#L201

---

_@harupy reviewed on 2023-07-29 08:16_

---

_@konstin reviewed on 2023-07-31 09:06_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/string.rs`:479 on 2023-07-31 09:06_

tbh i find python's raw string escaping rules confusing and i think there are cases that are just not properly representable (as the case above where black returns)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:303 on 2023-07-31 10:42_

Would you mind extending the documentation by explaining when normalizing is possible for raw strings?

---

_@MichaReiser approved on 2023-07-31 10:43_

---

_Label `formatter` added by @konstin on 2023-07-31 12:43_

---

_Comment by @MichaReiser on 2023-07-31 12:44_

Thanks for implementing Raw-strings. Something I entirely overlooked. 

---

_Comment by @konstin on 2023-07-31 12:47_

The formatter ecosystem checks are currently failing in CI. You can run the script locally (`scripts/formatter_progress.sh`). You can also merge main where to get the fix from https://github.com/astral-sh/ruff/pull/6187 that will print the ecosystem check results in CI also.

---

_Merged by @MichaReiser on 2023-07-31 12:48_

---

_Closed by @MichaReiser on 2023-07-31 12:48_

---

_Branch deleted on 2023-07-31 13:02_

---

_Comment by @harupy on 2023-07-31 13:02_

@MichaReiser @konstin Thanks for reviewing this PR :)

---

_Comment by @konstin on 2023-07-31 16:29_

Fixed the end-of-string unescaped quote in https://github.com/astral-sh/ruff/pull/6202

---
