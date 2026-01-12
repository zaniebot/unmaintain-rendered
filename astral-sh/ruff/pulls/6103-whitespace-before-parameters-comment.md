```yaml
number: 6103
title: whitespace-before-parameters comment
type: pull_request
state: merged
author: arembridge
labels:
  - documentation
assignees: []
merged: true
base: main
head: whitespace-before-parentheses
created_at: 2023-07-26T19:28:40Z
updated_at: 2023-07-27T17:12:49Z
url: https://github.com/astral-sh/ruff/pull/6103
synced_at: 2026-01-12T03:30:22Z
```

# whitespace-before-parameters comment

---

_Pull request opened by @arembridge on 2023-07-26 19:28_

**Summary**

Updated doc comment for `whitespace_before_parameters.rs`. Online docs also benefit from this update.

**Test Plan**

Checked docs via [mkdocs](https://github.com/astral-sh/ruff/blob/389fe13c934fe73679474006412b1eded4a2cad0/CONTRIBUTING.md?plain=1#L267-L296)

---

_Comment by @github-actions[bot] on 2023-07-26 19:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.1±0.02ms     4.5 MB/sec    1.00      9.0±0.02ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.02   1747.2±5.55µs     9.5 MB/sec    1.00   1720.3±5.47µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    180.8±0.88µs    16.3 MB/sec    1.01    182.2±4.68µs    16.2 MB/sec
formatter/pydantic/types.py                1.02      3.9±0.01ms     6.6 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.01ms     3.2 MB/sec    1.01     12.8±0.02ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.02ms     5.2 MB/sec    1.00      3.2±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    341.0±1.27µs     8.7 MB/sec    1.01    345.8±2.07µs     8.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.01ms     4.5 MB/sec    1.01      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.01ms     6.4 MB/sec    1.03      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1271.3±4.44µs    13.1 MB/sec    1.03   1304.6±1.58µs    12.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.5±0.33µs    23.7 MB/sec    1.03    128.1±0.25µs    23.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.01ms     9.2 MB/sec    1.03      2.8±0.00ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.4±0.15ms     3.9 MB/sec    1.00     10.3±0.19ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1977.7±30.86µs     8.4 MB/sec    1.00  1969.6±20.04µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    215.1±5.66µs    13.7 MB/sec    1.02    218.4±6.82µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.06ms     5.9 MB/sec    1.01      4.3±0.11ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.01     14.7±0.16ms     2.8 MB/sec    1.00     14.6±0.14ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.04ms     4.4 MB/sec    1.00      3.8±0.04ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    448.0±5.91µs     6.6 MB/sec    1.00    445.5±6.27µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.09ms     3.9 MB/sec    1.01      6.6±0.11ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.05ms     5.4 MB/sec    1.01      7.5±0.05ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1513.5±36.32µs    11.0 MB/sec    1.00  1508.8±15.24µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.5±2.88µs    17.7 MB/sec    1.00    166.3±2.92µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.00      3.3±0.03ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-26 22:55_

---

_Merged by @charliermarsh on 2023-07-26 23:01_

---

_Closed by @charliermarsh on 2023-07-26 23:01_

---

_@arembridge reviewed on 2023-07-27 08:45_

---

_Review comment by @arembridge on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:11 on 2023-07-27 08:45_

hi @charliermarsh.  

**TLDR:** i've checked and double checked this, but i'm pretty sure that this should be `before` (not `after`) based on the behaviour of the rule.  arguably the rule name is wrong

I created two test files and executed `cargo run -p ruff_cli -- check <file> --no-cache --select=E211` on each
```python
print ("<- whitespace before parenthesis")
```
produces 
```bash
<path-to-folder>/whitespace_before_parenthesis.py:1:6: E211 [*] Whitespace before '('
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```
while
```python
print( "<- whitespace after parenthesis")
```
produces no output.

Looking at the test file also confirms the `before` behaviour: [not-okay](https://github.com/astral-sh/ruff/blob/86539c1fc5afa2516099bf20cdb4fdd85827274a/crates/ruff/resources/test/fixtures/pycodestyle/E21.py#L2) and [okay](https://github.com/astral-sh/ruff/blob/86539c1fc5afa2516099bf20cdb4fdd85827274a/crates/ruff/resources/test/fixtures/pycodestyle/E21.py#L9C1-L9C1) (note there is a space after the `]`)

Arguably, this rule should be called `whitespace_before_parentheses` similar to its documentation on flake8 [here](https://www.flake8rules.com/rules/E211.html)

Let me know if i've missed something! thanks 😊 

---

_@charliermarsh reviewed on 2023-07-27 13:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:11 on 2023-07-27 13:29_

Gah sorry, I will double-check this today. It's possible that any of the rule name, the docs, or the implementation are wrong. Thank you for following up!

---

_@arembridge reviewed on 2023-07-27 14:11_

---

_Review comment by @arembridge on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:11 on 2023-07-27 14:11_

i think the answer is to rename the rule.  i can raise a PR for this if you like 😊 

---

_@charliermarsh reviewed on 2023-07-27 17:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:11 on 2023-07-27 17:12_

Thanks again, fixed here: https://github.com/astral-sh/ruff/pull/6133.

---
