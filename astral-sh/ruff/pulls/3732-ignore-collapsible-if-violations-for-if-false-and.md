```yaml
number: 3732
title: "Ignore `collapsible-if` violations for `if False:` and `if True:`"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
assignees: []
merged: true
base: main
head: exempt-attributes-from-sim223-auto-fix
created_at: 2023-03-25T15:19:13Z
updated_at: 2023-04-30T19:46:59Z
url: https://github.com/astral-sh/ruff/pull/3732
synced_at: 2026-01-12T15:55:13Z
```

# Ignore `collapsible-if` violations for `if False:` and `if True:`

---

_@JonathanPlasse_

- Close #2919

Instead of using this to disable code
```python
if False and self._requestid2response_queue:
	do_something()
```
Use this
```python
if False:
    if self._requestid2response_queue:
        do_something()
```
It will still flag the rule SIM102 but will not auto-fix.

---

_Comment by @github-actions[bot] on 2023-03-25 15:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.33ms     2.8 MB/sec    1.01     15.0±0.20ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.01      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.0±1.55µs     7.1 MB/sec    1.01    418.3±2.48µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.10ms     4.0 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.02ms     5.2 MB/sec    1.01      7.9±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1727.0±4.86µs     9.6 MB/sec    1.01   1752.8±3.23µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.3±1.60µs    16.1 MB/sec    1.01    185.1±1.39µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.01      3.7±0.09ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.8±0.35ms     2.1 MB/sec    1.01     20.0±0.38ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.13ms     3.3 MB/sec    1.01      5.1±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   614.8±19.46µs     4.8 MB/sec    1.01   620.9±24.76µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.28ms     3.0 MB/sec    1.00      8.4±0.21ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.22ms     3.9 MB/sec    1.00     10.3±0.32ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.5 MB/sec    1.01      2.3±0.07ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   243.9±22.56µs    12.1 MB/sec    1.00    240.0±8.96µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.10ms     5.5 MB/sec    1.01      4.7±0.14ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Exempt attribute expressions from SIM223 rule" to "Fix contain_effect()" by @JonathanPlasse on 2023-03-25 15:37_

---

_Comment by @charliermarsh on 2023-03-25 15:38_

I think the intent of #2919 was, perhaps, to avoid fixing these at all, since the author was suggesting that the use of `if False and X` was to temporarily deactivate the block while leaving the intended condition (`X`). We might consider just closing that issue, I don't know that we can really do much apart from removing that autofix entirely. If the code is intended to be temporarily disabled, then we should recommend:

```py
if False:
  if condition:
    ...
```

---

_Comment by @JonathanPlasse on 2023-03-25 15:43_

https://github.com/apache/airflow/blob/f7d5b165fcb8983bd82a852dcc5088b4b7d26a91/airflow/providers/databricks/operators/databricks.py#L73-L76
> ```python
> if "error" in run_output:
>     notebook_error = run_output["error"]
> else:
>     notebook_error = run_state.state_message
> ```
https://github.com/zulip/zulip/blob/0bcf03ea64ebc8ad6e621935933196e45dd26e1c/zerver/worker/queue_processors.py#L458-L461
> ```python
> if "email_language" in data:
>     email_language = data["email_language"]
> else:
>     email_language = referrer.realm.default_language

Both if fixed could cause behavior change as attribute access can have side effects.

---

_Comment by @charliermarsh on 2023-03-25 15:45_

Which rule are we talking about, for clarity?

---

_Comment by @JonathanPlasse on 2023-03-25 15:47_

I changed the scope of the issue, as it is bigger than #2919.
The function `contain_effect` did not consider that attribute access could have side effects.

> Which rule are we talking about, for clarity?

It concerns all rules that use `contains_effect`.

---

_Renamed from "Fix contain_effect()" to "Fix contains_effect()" by @JonathanPlasse on 2023-03-25 15:47_

---

_Comment by @JonathanPlasse on 2023-03-25 15:51_

This concerns these rules:
- `UselessExpression`
- `CompareWithTuple`
- `ExprAndNotExpr`
- `ExprOrTrue`
- `ExprAndFalse`
- `IfElseBlockInsteadOfDictLookup`
- `IfElseBlockInsteadOfDictGet`
- `UnusedVariable`

---

_Comment by @JonathanPlasse on 2023-03-25 15:53_

Should I add tests for each rule concerned?

---

_Comment by @charliermarsh on 2023-03-25 15:54_

Na that's not necessary. I'm just thinking about the tradeoffs here. It's true that it could be unsafe to change these, but it also hinders our ability to fix a lot of useful and valid cases, since the vast majority of attribute accesses are _not_ effectful.

---

_Comment by @charliermarsh on 2023-03-25 15:55_

(Separately: for `ExprOrTrue` and `ExprAndFalse`, we should only abort if the effect-ful expression comes _after_ the short-circuiting condition. E.g., `False and foo()` can be removed, since `foo()` will never execute regardless; but `foo() and False` can't be removed.)

---

_Renamed from "Fix contains_effect()" to "Disable SIM102 auto-fix for if False block " by @JonathanPlasse on 2023-03-26 10:38_

---

_Comment by @JonathanPlasse on 2023-03-26 10:41_

> (Separately: for `ExprOrTrue` and `ExprAndFalse`, we should only abort if the effect-ful expression comes _after_ the short-circuiting condition. E.g., `False and foo()` can be removed, since `foo()` will never execute regardless; but `foo() and False` can't be removed.)

I will make a following PR to fix this.

> Na that's not necessary. I'm just thinking about the tradeoffs here. It's true that it could be unsafe to change these, but it also hinders our ability to fix a lot of useful and valid cases, since the vast majority of attribute accesses are not effectful.

I reverted the changes, as you said it will cause too many false negatives for the added auto-fix safety.

---

_Renamed from "Disable SIM102 auto-fix for if False block " to "Ignore `collapsible-if` violations for `if False:` and `if True:` blcoks" by @charliermarsh on 2023-03-30 15:47_

---

_Renamed from "Ignore `collapsible-if` violations for `if False:` and `if True:` blcoks" to "Ignore `collapsible-if` violations for `if False:` and `if True:`" by @charliermarsh on 2023-03-30 15:47_

---

_Label `rule` added by @charliermarsh on 2023-03-30 15:47_

---

_Merged by @charliermarsh on 2023-03-30 15:52_

---

_Closed by @charliermarsh on 2023-03-30 15:52_

---

_Branch deleted on 2023-04-30 19:46_

---
