```yaml
number: 6171
title: "[`pylint`] Implement Pylint `bad-format-character` (`E1300`)"
type: pull_request
state: merged
author: silvanocerza
labels:
  - rule
assignees: []
merged: true
base: main
head: pylint-bad-format-character
created_at: 2023-07-29T12:34:38Z
updated_at: 2023-08-12T17:42:42Z
url: https://github.com/astral-sh/ruff/pull/6171
synced_at: 2026-01-12T02:52:03Z
```

# [`pylint`] Implement Pylint `bad-format-character` (`E1300`)

---

_Pull request opened by @silvanocerza on 2023-07-29 12:34_

## Summary

Relates to #970.

Add new `bad-format-character` Pylint rule.

I had to make a change in `crates/ruff_python_literal/src/format.rs` to get a more detailed error in case the format character is not correct. I chose to do this since most of the format spec parsing functions are private. It would have required me reimplementing most of the parsing logic just to know if the format char was correct.

This PR also doesn't reflect current Pylint functionality in two ways.

It supports new format strings correctly, Pylint as of now doesn't. See pylint-dev/pylint#6085.

In case there are multiple adjacent string literals delimited by whitespace the index of the wrong format char will relative to the single string. Pylint will instead reported it relative to the concatenated string.

Given this:
```
"%s" "%z" % ("hello", "world")
```

Ruff will report this:
```Unsupported format character 'z' (0x7a) at index 1```

Pylint instead:
```Unsupported format character 'z' (0x7a) at index 3```

I believe it's more sensible to report the index relative to the individual string.

## Test Plan

Added new snapshot and a small test in `crates/ruff_python_literal/src/format.rs`.


---

_Comment by @github-actions[bot] on 2023-07-29 13:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.24     13.6±0.50ms     3.0 MB/sec    1.00     11.0±0.38ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.16      2.5±0.09ms     6.8 MB/sec    1.00      2.1±0.12ms     7.8 MB/sec
formatter/numpy/globals.py                 1.06    256.8±8.46µs    11.5 MB/sec    1.00   241.3±17.87µs    12.2 MB/sec
formatter/pydantic/types.py                1.20      5.5±0.31ms     4.6 MB/sec    1.00      4.6±0.17ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.33ms     2.6 MB/sec    1.00     15.9±0.38ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.12ms     4.4 MB/sec    1.03      3.9±0.15ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   522.2±13.09µs     5.7 MB/sec    1.00   517.1±15.46µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.19ms     3.7 MB/sec    1.01      7.0±0.22ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.5±0.20ms     4.8 MB/sec    1.00      8.2±0.22ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1619.9±59.13µs    10.3 MB/sec    1.00  1604.7±50.26µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    186.8±5.19µs    15.8 MB/sec    1.00    185.5±5.20µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.6±0.08ms     7.2 MB/sec    1.00      3.4±0.16ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.08ms     4.1 MB/sec    1.03     10.1±0.20ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1905.6±27.34µs     8.7 MB/sec    1.01  1929.4±33.50µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    207.6±4.77µs    14.2 MB/sec    1.03    213.4±7.74µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.2 MB/sec    1.02      4.2±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.02     13.4±0.11ms     3.0 MB/sec    1.00     13.2±0.11ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    430.4±5.75µs     6.9 MB/sec    1.00    416.9±6.58µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.1±0.09ms     4.2 MB/sec    1.00      5.9±0.07ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.06ms     5.6 MB/sec    1.00      7.2±0.06ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1487.9±13.21µs    11.2 MB/sec    1.00  1453.7±16.30µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.04    165.6±3.11µs    17.8 MB/sec    1.00    159.8±2.68µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @silvanocerza on 2023-07-29 14:52_

No idea why the `cargo test (wasm)` check is failing. 
I tried running it locally using `wasm-pack` version `0.12.1` and it didn't fail.
Also I see no other PRs failing cause of that.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-01 04:22_

---

_Comment by @charliermarsh on 2023-08-01 04:22_

(I think the wasm test is just a little flaky, I've seen that failure before.)

---

_Label `rule` added by @charliermarsh on 2023-08-02 21:22_

---

_Comment by @charliermarsh on 2023-08-02 21:23_

Awesome, thank you! I made a few minor changes, including only running the `.format()` version if the it's part of a `.format()` call.

I also dropped the index reporting for now... I'm worried that we'll get it wrong in some cases due to the string preprocessing that happens in the parser (for the `.format()` case), which you hinted at above (with regards to the `%` case, where we use the token stream). I know it's not parity with pylint, but it seems ok to me for now... If you feel strongly, I'm obviously happy to revisit :)

---

_Comment by @charliermarsh on 2023-08-02 21:24_

There's also at least one false negative which I've documented in the tests: `print(("%" "z") % 1)`

---

_Merged by @charliermarsh on 2023-08-02 21:32_

---

_Closed by @charliermarsh on 2023-08-02 21:32_

---

_Comment by @silvanocerza on 2023-08-04 08:13_

No worries, I appreciate the feedback and the tweak. I'm still learning Rust looking at those certainly helps.

Though I'm wondering if the snapshot is correct. I see no new style formatting reports. Does the new logic actually catch both f-strings and `.format()`?

---

_Comment by @charliermarsh on 2023-08-04 13:19_

Thanks, I missed that! Added here: https://github.com/astral-sh/ruff/pull/6340. (It doesn't fire for f-strings, but it didn't before either IIRC.)

---

_Branch deleted on 2023-08-12 17:34_

---

_Comment by @silvanocerza on 2023-08-12 17:42_

No worries. 
In any case it was working for f-strings too though cause it was firing for string constant instead of expr calls.

---
