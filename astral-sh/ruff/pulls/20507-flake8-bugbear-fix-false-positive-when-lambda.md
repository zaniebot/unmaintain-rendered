```yaml
number: 20507
title: "[`flake8-bugbear`] Fix false positive when `lambda` parameters have same name as loop variables (`B023`)"
type: pull_request
state: open
author: danparizher
labels:
  - bug
assignees: []
base: main
head: fix-15716
created_at: 2025-09-22T05:06:51Z
updated_at: 2025-12-04T19:55:38Z
url: https://github.com/astral-sh/ruff/pull/20507
synced_at: 2026-01-10T16:48:01Z
```

# [`flake8-bugbear`] Fix false positive when `lambda` parameters have same name as loop variables (`B023`)

---

_Pull request opened by @danparizher on 2025-09-22 05:06_

## Summary

Fixes #15716


---

_Comment by @github-actions[bot] on 2025-10-01 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B023.py`:209 on 2025-10-01 16:36_

I actually think this _is_ a false positive that should be treated like the `min`, `max`, and `sorted` examples starting on line 124 of this file. Based on the issue comment, this works correctly, I assume because `apply` uses the lambda immediately rather than storing a reference to it like the problematic cases.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B023.py`:223 on 2025-10-01 16:39_

I also don't think this case should trigger the rule, as mentioned on the issue too.

---

_@ntBre requested changes on 2025-10-01 16:56_

Thanks for tackling such a long-standing issue! However, I don't think this resolves the problems reported in #15716. Two of the three false positives are still present, and I don't think explicitly filtering out lambda parameters is really the right solution to that false positive either.

I consider the problem in the initial report to be the "leaking" of the comprehension scope. `x` is not the `for` loop variable, and the `x` in the comprehension isn't actually in scope here:

```py
for _ in range(3):
    [x for x in []]
    def func():
        lambda x: x
```

```pycon
>>> [x for x in []]
[]
>>> x
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    x
NameError: name 'x' is not defined
```

I haven't really looked closely enough at the rule to have a good suggestion for an alternative, but the idea that comes to mind from reading the comments in `function_uses_rule_variable` is that we're missing a check that the variable is actually in scope. That may not require explicit scope tracking; maybe the `SuspiciousVariableVisitor` and/or `AssignedNamesVisitor` are too eager in some way. That's what I would look into instead of tracking a separate vec of lambda parameters. This also triggers B023 currently without being a lambda:

```py
for _ in range(3):
    [x for x in []]
    def func():
        def f(x): x
```

I didn't look as much into the other false positives, but we should try to see how we filter out the other cases (`min`, `max`, `sorted`, etc.) and see if we can apply that here too

---

_Label `bug` added by @ntBre on 2025-10-01 16:57_

---

_Review requested from @ntBre by @danparizher on 2025-10-01 21:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/function_uses_loop_variable.rs`:180 on 2025-10-08 15:48_

Ah I didn't realize we had a list of allowed functions. I'm not totally sure we should add a special case for pandas, but we do have other pandas-specific rules so it's not without precedent.

If we do add this, we should at least check that pandas has been imported. I think that's what we do in some other cases where we inspect pandas attributes. I believe as written this would allow any type with an `apply` attribute, which is a bit more permissive than necessary.

Ideally we would resolve the type of the value and see that it's a pandas object with a known `apply` method.



---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/snapshots/ruff_linter__rules__flake8_bugbear__tests__B023_B023.py.snap`:257 on 2025-10-08 15:51_

This is a new false positive compared to main on a pre-existing test.

---

_@ntBre requested changes on 2025-10-08 15:53_

I'm still skeptical that filtering out a list of lambda parameters is the right approach, and there's a new issue caused by the last round of changes.

---

_Review requested from @ntBre by @danparizher on 2025-10-09 02:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/function_uses_loop_variable.rs`:330 on 2025-10-30 16:19_

We definitely shouldn't run the visitor twice. The `seen_module` check is just a check of a bit flag, and the method could even be const from what I can tell (though it isn't marked as such currently), so it should be fine either to check it each time we hit `apply` or to save the result of calling it once. Either way should be much cheaper than two visits.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/function_uses_loop_variable.rs`:72 on 2025-10-30 16:36_

I think this check is also incorrect. Using a slight variation on one of the examples from the issue:

```py
lst = []
for value in range(2):
    def add_one():
        def _add_one_inner():
            return value + 1

        return _add_one_inner

    lst.append(add_one())

for l in lst:
    print(l())
```

This _should_ trigger B023 because the inner function captures the loop variable. Running this code produces:

```pycon
2
2
```

but no diagnostic is emitted on this branch.

---

_@ntBre requested changes on 2025-10-30 16:42_

I still don't think either of the function changes is the right fix. I still stand by my [first comment](https://github.com/astral-sh/ruff/pull/20507#pullrequestreview-3290071170) saying that the leaking scope is  the real problem. Have you looked into that at all instead of trying to filter out lambda parameters and avoiding visiting nested functions?

The  pandas issue seems a bit different and might be worth landing separately, at least once we avoid visiting twice as noted inline. But that will not close #15716, which is really about the scope problem above. I'm still not 100% sold on that either because the other functions we special-case here are from the standard library. It could be a slippery slope to start adding third-party functions.

---

_Review requested from @ntBre by @danparizher on 2025-10-31 20:54_

---

_Comment by @ntBre on 2025-12-04 19:55_

@amyreese and I took another look at this today, and I think the approach here makes a bit more sense to me now. However, we were wondering if there might be another solution, potentially with a larger refactor of the rule itself. Going back to the main case from the issue:

```py
for _ in range(3):
    [x for x in []]
    def func():
        lambda x: x
```

the `x` in the lambda body is currently flagged as suspicious by the first visitor in the rule, the `SuspiciousVariablesVisitor`, because it's a load reference to a variable without a store reference inside the body of `func`. Then, the second visitor, `AssignedNamesVisitor` finds a _different_ `x` within the loop, the target of the list comprehension.

These two are only equal, and thus trigger the `reassigned_in_loop.contains` check, because the rule tracks all of the names as simple strings instead of preserving any scope or even range information.

I guess this is similar to my first suggestion, but to me, the better fix will be something like confirming that the unmatched load reference actually loads a variable defined in the loop and accessible in the same scope as the load reference. The `x` in the comprehension here is not accessible to the lambda, and the same is true in the loop case from https://github.com/astral-sh/ruff/issues/15716#issuecomment-3304010685.

I think the test cases here are good and helpful, but I would still prefer a different, more robust implementation that fixes the underlying issue. We could also consider spinning off the pandas fix, which is pretty separate from the main issue in #15716 to me, if we want to land that earlier.

---
