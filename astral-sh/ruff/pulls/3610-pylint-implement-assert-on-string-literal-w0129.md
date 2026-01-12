```yaml
number: 3610
title: "[`pylint`]: Implement `assert-on-string-literal` (`W0129`)"
type: pull_request
state: merged
author: latonis
labels:
  - rule
assignees: []
merged: true
base: main
head: pylint-assert-on-string-literal
created_at: 2023-03-19T18:43:29Z
updated_at: 2023-03-20T04:03:29Z
url: https://github.com/astral-sh/ruff/pull/3610
synced_at: 2026-01-12T04:39:45Z
```

# [`pylint`]: Implement `assert-on-string-literal` (`W0129`)

---

_Pull request opened by @latonis on 2023-03-19 18:43_

Rule reference: https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/assert-on-string-literal.html

Implements as part of #970 

---

_Comment by @github-actions[bot] on 2023-03-19 18:53_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+18, -0, 0 error(s))

<details><summary>airflow (+18, -0)</summary>
<p>

```diff
+ tests/providers/cncf/kubernetes/operators/test_pod.py:1354:20: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/cncf/kubernetes/triggers/test_pod.py:156:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/cncf/kubernetes/triggers/test_pod.py:157:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/cncf/kubernetes/triggers/test_pod.py:172:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/cncf/kubernetes/triggers/test_pod.py:173:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/cncf/kubernetes/triggers/test_pod.py:211:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_bigquery_dts.py:165:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_bigquery_dts.py:166:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_dataflow.py:159:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_dataflow.py:160:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_dataproc.py:204:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_dataproc.py:205:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_dataproc.py:285:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_dataproc.py:286:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_kubernetes_engine.py:187:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_kubernetes_engine.py:188:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_kubernetes_engine.py:210:16: PLW0129 Asserting on a non-empty string literal will always pass
+ tests/providers/google/cloud/triggers/test_kubernetes_engine.py:211:16: PLW0129 Asserting on a non-empty string literal will always pass
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.00     14.0±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.3±1.25µs     6.8 MB/sec    1.00    431.8±1.37µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.02ms     5.3 MB/sec    1.01      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1699.6±2.64µs     9.8 MB/sec    1.00   1690.6±2.13µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.0±0.39µs    16.9 MB/sec    1.00    175.8±0.63µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.2±0.58ms     2.2 MB/sec    1.01     18.4±0.66ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.19ms     3.4 MB/sec    1.00      4.9±0.25ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   634.9±23.03µs     4.6 MB/sec    1.03   651.3±34.14µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.26ms     3.2 MB/sec    1.00      8.1±0.26ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.36ms     4.2 MB/sec    1.03     10.1±0.36ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.6 MB/sec    1.02      2.2±0.09ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.01   246.7±11.25µs    12.0 MB/sec    1.00    243.1±9.48µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.16ms     5.6 MB/sec    1.03      4.7±0.19ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-19 19:05_

I'm a little surprised that Pylint doesn't include byte strings or f-strings here. Maybe we should?

---

_@charliermarsh reviewed on 2023-03-19 19:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/assert_on_string_literal.rs`:26 on 2023-03-19 19:10_

It looks like we need to change the `never` if the string is empty -- try running `pylint` over this snippet:

```py
assert "foo"
assert ""
```

---

_@latonis reviewed on 2023-03-19 19:11_

---

_Review comment by @latonis on `crates/ruff/src/rules/pylint/rules/assert_on_string_literal.rs`:26 on 2023-03-19 19:11_

Ah right, `""` is falsey isn't it, so that would evaluate to `assert False` instead of `assert True`

---

_@charliermarsh reviewed on 2023-03-19 19:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/assert_on_string_literal.rs`:26 on 2023-03-19 19:12_

Yeah, that's right!

---

_@latonis reviewed on 2023-03-19 19:12_

---

_Review comment by @latonis on `crates/ruff/src/rules/pylint/rules/assert_on_string_literal.rs`:26 on 2023-03-19 19:12_

"Asserting on a string literal may result in unintended results" Something like that?

---

_Comment by @latonis on 2023-03-19 19:13_

> I'm a little surprised that Pylint doesn't include byte strings or f-strings here. Maybe we should?

I think this would be a good case to deviate from the exact logic of the original Pylint rule, will implement those 

---

_@charliermarsh reviewed on 2023-03-19 19:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/assert_on_string_literal.rs`:26 on 2023-03-19 19:14_

I think it can be `Asserting on a non-empty string literal will always pass` and `Asserting on an empty string literal will never pass`

---

_Comment by @charliermarsh on 2023-03-19 19:15_

We may not be able to support f-strings... We can't know ahead-of-time whether `f"{foo}"` evaluates to an empty or non-empty string.

---

_Comment by @charliermarsh on 2023-03-19 19:16_

Or, we could use a separate message for the case in which an f-string only consists of expressions, like what you had above: `Asserting on a string literal may have unintended results`.


---

_Comment by @latonis on 2023-03-19 20:49_

> Or, we could use a separate message for the case in which an f-string only consists of expressions, like what you had above: `Asserting on a string literal may have unintended results`.

I am in favor of this one :) 


---

_Review requested from @charliermarsh by @latonis on 2023-03-19 20:57_

---

_Merged by @charliermarsh on 2023-03-20 03:45_

---

_Closed by @charliermarsh on 2023-03-20 03:45_

---

_Label `rule` added by @charliermarsh on 2023-03-20 03:45_

---

_Branch deleted on 2023-03-20 04:03_

---
