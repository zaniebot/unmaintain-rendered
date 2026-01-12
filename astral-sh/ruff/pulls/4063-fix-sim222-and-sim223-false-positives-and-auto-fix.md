```yaml
number: 4063
title: Fix SIM222 and SIM223 false positives and auto-fix
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-sim222-sim223-false-positive-with-truthy-expressions
created_at: 2023-04-22T14:30:24Z
updated_at: 2023-04-25T07:17:13Z
url: https://github.com/astral-sh/ruff/pull/4063
synced_at: 2026-01-12T15:55:14Z
```

# Fix SIM222 and SIM223 false positives and auto-fix

---

_@JonathanPlasse_

- Close #4058

Special case truthy expressions like side-effect expressions.
If a truthy expression is followed by the short-circuiting boolean, remove the short-circuiting boolean too as it is unnecessary.
The violation message should be changed in this case to something like "remove everything after the truth expression".
```python
assert ("foo" and False) == "foo"
assert ("foo" or True) == "foo"
```

I use `Truthiness` from `flake8-bandit`, but `flake8-pytest` also has `is_falsy_constant()`.
Should we make extract `Truthiness` from `flake8-bandit`, add the missing cases from `flake8-pytest`, and use it for `flake8-bandit`, `flake8-pytest`, and `flake8-simplify`?
I would be open making another PR for this.

---

_Comment by @github-actions[bot] on 2023-04-22 14:51_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- airflow/providers/apache/kafka/operators/consume.py:100:29: SIM222 [*] Use `True` instead of `... or True`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.04ms     2.6 MB/sec    1.00     15.4±0.02ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.01ms     4.3 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.03    431.4±8.32µs     6.8 MB/sec    1.00    420.7±1.34µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.01ms     3.9 MB/sec    1.00      6.6±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.02      8.2±0.02ms     4.9 MB/sec    1.00      8.1±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1791.0±3.17µs     9.3 MB/sec    1.00   1775.1±4.08µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.6±0.93µs    16.0 MB/sec    1.00    185.5±1.35µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.4 MB/sec    1.01      6.5±0.00ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1257.8±1.34µs    13.2 MB/sec    1.01   1272.6±1.50µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    126.4±0.35µs    23.4 MB/sec    1.01    127.5±0.30µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.00ms     9.3 MB/sec    1.01      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.6±0.33ms     2.3 MB/sec    1.00     17.4±0.29ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.07ms     3.8 MB/sec    1.02      4.5±0.08ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   546.1±11.96µs     5.4 MB/sec    1.00   548.7±10.70µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.10ms     3.5 MB/sec    1.01      7.4±0.11ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.06ms     4.6 MB/sec    1.03      9.1±0.14ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1925.1±18.13µs     8.6 MB/sec    1.03  1976.8±26.42µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    216.5±3.16µs    13.6 MB/sec    1.03   223.7±13.01µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.06ms     6.4 MB/sec    1.02      4.1±0.06ms     6.2 MB/sec
parser/large/dataset.py                    1.00      7.0±0.07ms     5.8 MB/sec    1.01      7.0±0.04ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1326.4±21.77µs    12.6 MB/sec    1.01  1335.3±15.68µs    12.5 MB/sec
parser/numpy/globals.py                    1.00    131.9±2.01µs    22.4 MB/sec    1.01    133.7±2.12µs    22.1 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.04ms     8.4 MB/sec    1.00      3.0±0.03ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-22 22:15_

Yeah, I'd vote to extract that concept into `ruff_python_semantic` and use it in more places!

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_bool_op.rs`:529 on 2023-04-22 22:15_

Nit: Can we move the `to_string` call to where we create the `Edit` to avoid allocating a string on the heap if there are no diagnostics. 

---

_@MichaReiser approved on 2023-04-22 22:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:185 on 2023-04-22 22:16_

I guess we should also include `JoinedStr` here, and maybe the `ITERABLE_INITIALIZERS` logic we see in `flake8-pytest`? Whatever's most comprehensive.

---

_@charliermarsh reviewed on 2023-04-22 22:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM222_SIM222.py.snap`:157 on 2023-04-23 05:11_

Can we update these diagnostic messages, since they no longer reflect the suggestion IIUC?

---

_@charliermarsh reviewed on 2023-04-23 05:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM223_SIM223.py.snap`:152 on 2023-04-23 05:12_

So in the summary, we mention:

```py
assert ("foo" and False) == "foo"
```

But I don't _think_ this holds, since the left-hand side evaluates to `False`. And in this example here, `a and "foo"` isn't equivalent to `a and "foo" and False and "bar"`, is it? If `a` is truth-y, then `a and "foo" and False and "bar"` gives you `False`; if `a` is false-y, it gives you `a`. But `a and "foo"` gives you either `a` or `"foo"`. I think the logic is different for the false case.

---

_@charliermarsh reviewed on 2023-04-23 05:12_

---

_@JonathanPlasse reviewed on 2023-04-23 07:27_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM223_SIM223.py.snap`:152 on 2023-04-23 07:27_

You are right it should check for falsey value not truthy value for `and` operands.
```python
assert ("" and False) == ""
```

---

_@JonathanPlasse reviewed on 2023-04-23 13:33_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:185 on 2023-04-23 13:33_

Done.

---

_@JonathanPlasse reviewed on 2023-04-23 13:38_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_simplify/rules/ast_bool_op.rs`:527 on 2023-04-23 13:38_

```python
a and ""
a or "a"
```
is replaced by
```python
""
"a"
```
This auto-fix is valid only if the result of the boolean operations is used by `bool(...)` or `if ...`.

We should only check for truthiness is unknown and not `contains_effect` in the other cases.

---

_@JonathanPlasse reviewed on 2023-04-23 14:54_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM222_SIM222.py.snap`:157 on 2023-04-23 14:54_

Done!

---

_@JonathanPlasse reviewed on 2023-04-23 14:56_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_simplify/rules/ast_bool_op.rs`:527 on 2023-04-23 14:56_

I am working on a visitor to know if we are in a boolean context (i.e. boolean operation result will be converted to a boolean).

---

_Comment by @JonathanPlasse on 2023-04-23 16:32_

I added `in_test` to `Context`.
It is considered to be in a test context when you are inside an `if` or an `assert` test, or inside the builtin `bool`.
Boolean operations inside other expressions than boolean operations are not in a test context.

For example:
```python
# Inside test `a` is simplified

bool(a or [1] or True or [2])

assert a or [1] or True or [2]

if (a or [1] or True or [2]) and (a or [1] or True or [2]):
    pass

# Outside test `a` is not simplified

a or [1] or True or [2]

if (a or [1] or True or [2]) == (a or [1]):
    pass

if f(a or [1] or True or [2]):
    pass
```

---

_Comment by @JonathanPlasse on 2023-04-23 17:10_

There is a false positive that has been fixed in `airflow`.
```python
self.max_messages = max_messages or True
```

---

_Renamed from "Fix SIM222 SIM223 false positive with truthy value" to "Fix SIM222 and SIM223 false positives and auto-fix" by @JonathanPlasse on 2023-04-23 18:46_

---

_Comment by @JonathanPlasse on 2023-04-23 19:21_

Should I split this PR?

---

_Comment by @charliermarsh on 2023-04-23 19:55_

I think it would be helpful. What do you think we can carve out into separate PRs?

---

_Comment by @JonathanPlasse on 2023-04-23 20:54_

Here is the list of possible PRs
1. #4071
2. #4072 
3. #4063 (This PR)

I will rebase this PR on `main` after the first two are merged.

---

_Comment by @JonathanPlasse on 2023-04-24 06:13_

The rebase is done.
It is ready for review.

---

_Comment by @JonathanPlasse on 2023-04-24 06:20_

I will document the rules as we diverge a lot from the behavior of `flake8-simplify`.

---

_Comment by @JonathanPlasse on 2023-04-24 10:42_

I documented the rules and detect when `... or True`, `True or ...`, or `... or True or ...` is used and adapted the violation message.

---

_Merged by @charliermarsh on 2023-04-25 04:44_

---

_Closed by @charliermarsh on 2023-04-25 04:44_

---

_Label `bug` added by @charliermarsh on 2023-04-25 04:46_

---

_Branch deleted on 2023-04-25 07:05_

---

_Comment by @JonathanPlasse on 2023-04-25 07:17_

 You for your edits, it is much cleaner and clearer now.

---
