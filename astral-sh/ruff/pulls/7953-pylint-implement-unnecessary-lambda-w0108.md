```yaml
number: 7953
title: "[pylint] Implement unnecessary-lambda (W0108)"
type: pull_request
state: merged
author: clemux
labels:
  - rule
assignees: []
merged: true
base: main
head: plw0108_unnecessary_lambda
created_at: 2023-10-13T19:19:48Z
updated_at: 2023-10-20T17:34:19Z
url: https://github.com/astral-sh/ruff/pull/7953
synced_at: 2026-01-12T15:55:25Z
```

# [pylint] Implement unnecessary-lambda (W0108)

---

_@clemux_

This is my first PR and I'm new at rust, so feel free to ask me to rewrite everything if needed ;)

The rule must be called after deferred lambas have been visited because of the last check (whether the lambda parameters are used in the body of the function that's being called). I didn't know where to do it, so I did what I could to be able to work on the rule itself:

 - added a `ruff_linter::checkers::ast::analyze::lambda` module
 - build a vec of visited lambdas in `visit_deferred_lambdas`
 - call `analyze::lambda` on the vec after they all have been visited
 
Building that vec of visited lambdas was necessary so that bindings could be properly resolved in the case of nested lambdas.

Note that there is an open issue in pylint for some false positives, do we need to fix that before merging the rule? https://github.com/pylint-dev/pylint/issues/8192

Also, I did not provide any fixes (yet), maybe we want do avoid merging new rules without fixes?

## Summary

Checks for lambdas whose body is a function call on the same arguments as the lambda itself.

### Bad

```python
df.apply(lambda x: str(x))
```

### Good

```python
df.apply(str)
```

## Test Plan

Added unit test and snapshot.
Manually compared pylint and ruff output on pylint's test cases.

## References

 - [pylint documentation](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/unnecessary-lambda.html)
 - [pylint implementation](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/base/basic_checker.py#L521-L587)
 - https://github.com/astral-sh/ruff/issues/970


---

_Comment by @github-actions[bot] on 2023-10-13 19:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-14 18:54_

Before I get deep into the review, do you mind skimming through some of the ecosystem checks and confirming that the implemented behavior matches Pylint's?

---

_Comment by @clemux on 2023-10-14 23:23_

There are a few less errors with ruff than with pylint in pandas. For example, [this](https://github.com/pandas-dev/pandas/blob/b5b8be04b2a63fc02e89650a740779e718d7a99b/pandas/tests/window/test_base_indexer.py#L178):

```python
    expected2 = frame_or_series(rolling.apply(lambda x: np_func(x, **np_kwargs)))
```

I might be missing something but I'd say it's a bug in pylint. I'll look into it.


---

_Comment by @clemux on 2023-10-15 01:31_

Issue submitted on pylint side: https://github.com/pylint-dev/pylint/issues/9148

---

_@clemux reviewed on 2023-10-15 16:45_

---

_Review comment by @clemux on `crates/ruff_linter/src/checkers/ast/mod.rs`:1864 on 2023-10-15 16:45_

I don't expect you to accept that, but I couldn't figure out how to make it work otherwise :)

---

_@zanieb reviewed on 2023-10-16 18:41_

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:257 on 2023-10-16 18:41_

New rules should be added in `RuleGroup::Preview` then stabilized in a later release. We can improve the naming of the stable rule group :)

---

_@clemux reviewed on 2023-10-16 18:46_

---

_Review comment by @clemux on `crates/ruff_linter/src/codes.rs`:257 on 2023-10-16 18:46_

Done.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-10-20 16:22_

---

_Comment by @charliermarsh on 2023-10-20 16:22_

Gonna review this today, sorry for the delay.

---

_@charliermarsh approved on 2023-10-20 17:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_lambda.rs`:179 on 2023-10-20 17:04_

So the reason that `lookup_symbol` didn't work here is a bit hard to explain, but consider:

```python
_ = lambda x: f(lambda x: x)(x)
```

Here, we're trying to see if `f(lambda x: x)` contains any references to the `x` in `_ = lambda x`. If we use `lookup_symbol`, it will try to resolve the inner `x` relative to the outer scope, that of `_ = lambda x`, so it thinks it's a reference to the outer `x`. Using `resolve_name` ensures that we use the correct binding which was already computed in the binding phase.

---

_@charliermarsh reviewed on 2023-10-20 17:04_

---

_Label `rule` added by @charliermarsh on 2023-10-20 17:04_

---

_@charliermarsh reviewed on 2023-10-20 17:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1864 on 2023-10-20 17:11_

Going to tweak this a bit, but the idea makes sense :)

---

_Merged by @charliermarsh on 2023-10-20 17:25_

---

_Closed by @charliermarsh on 2023-10-20 17:25_

---
