```yaml
number: 4909
title: "[`flake8-slots`] Add plugin, add `SLOT000`, `SLOT001` and `SLOT002`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
  - plugin
assignees: []
merged: true
base: main
head: plugin/flake8-slots
created_at: 2023-06-06T21:35:24Z
updated_at: 2023-06-09T04:33:18Z
url: https://github.com/astral-sh/ruff/pull/4909
synced_at: 2026-01-12T15:55:17Z
```

# [`flake8-slots`] Add plugin, add `SLOT000`, `SLOT001` and `SLOT002`

---

_@qdegraaf_

## Summary

Adds all rules from the [flake8-slots](https://github.com/python-formate/flake8-slots) plugin.

No autofixes yet but maybe a `Fix::suggested` autofix with a default `__slots__` val could be useful? 

## Test Plan

Added fixtures for all rules. 

I added a check for "indirect" subclassing in situations like:
```python
from collections import namedtuple 

some_named_tuple = namedtuple("foo", ["bar", "bla"])

class Foo(some_named_tuple):
    pass
```

As I imagined this would happen regularly. For completeness' sake it is arguable we should do this for `str` and `tuple` too in scenarios like:

```python
str_alias = str

class Foo(str_alias):
    pass
```

This will be more rare, left it out for now. Unsure if we have a convention for `base` checks across Ruff (e.g. do we **always** check for type aliases or **never** check for type aliases because XYZ). Happy to add the check to SLOT000 and SLOT001 and/or remove the check from SLOT002 depending on what is preferred. 

NOTE: If adding the check for the other subclasses makes sense. Maybe a general helper which combines some of the logic in `no_slots_in_namedtuple_subclass` and `is_indirect_namedtuple_subclass` and making it more generic so that it becomes something like: `fn is_indirect_subclass_of(expr: &ExprName, subclass: [&str]) -> bool {}` might be useful. 

## Issue links

Closes: https://github.com/charliermarsh/ruff/issues/3873 

---

_Renamed from "[flake8-slots] Add `SLOT000`, `SLOT001` and `SLOT002`" to "`[flake8-slots]` Add plugin, add `SLOT000`, `SLOT001` and `SLOT002`" by @qdegraaf on 2023-06-06 21:38_

---

_Comment by @github-actions[bot] on 2023-06-06 21:56_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+13, -0, 0 error(s))

<details><summary>airflow (+13, -0)</summary>
<p>

```diff
+ airflow/models/dagwarning.py:94:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/providers/amazon/aws/hooks/dms.py:26:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/providers/cncf/kubernetes/triggers/pod.py:33:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/providers/google/cloud/hooks/dataprep.py:47:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/providers/google/cloud/utils/dataform.py:31:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/serialization/enums.py:26:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/serialization/enums.py:35:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/utils/log/file_task_handler.py:49:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/utils/state.py:23:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/utils/state.py:55:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/utils/trigger_rule.py:23:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/utils/types.py:47:7: SLOT000 Subclasses of `str` should define `__slots__`
+ airflow/utils/weight_rule.py:25:7: SLOT000 Subclasses of `str` should define `__slots__`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.05ms     5.9 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1203.2±3.32µs    13.8 MB/sec    1.00   1207.3±3.85µs    13.8 MB/sec
formatter/numpy/globals.py                 1.00    134.6±0.20µs    21.9 MB/sec    1.00    134.4±0.16µs    21.9 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.07ms     2.7 MB/sec    1.01     15.0±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.4±6.24µs     8.0 MB/sec    1.01    369.4±0.85µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.4±0.03ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1534.4±6.41µs    10.9 MB/sec    1.00   1536.3±4.75µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.7±0.25µs    18.0 MB/sec    1.01    165.8±0.20µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.05ms     5.3 MB/sec    1.02      7.8±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1343.8±11.66µs    12.4 MB/sec    1.02   1371.4±8.74µs    12.1 MB/sec
formatter/numpy/globals.py                 1.00    152.9±1.70µs    19.3 MB/sec    1.02    155.5±7.93µs    19.0 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.02ms     8.1 MB/sec    1.03      3.2±0.07ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.09ms     2.5 MB/sec    1.00     16.5±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.01      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.7±4.77µs     6.9 MB/sec    1.00    432.3±5.08µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.03ms     4.9 MB/sec    1.00      8.3±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1721.4±8.36µs     9.7 MB/sec    1.01  1740.2±15.46µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.3±1.26µs    15.9 MB/sec    1.00    186.1±1.32µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.01      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:827 on 2023-06-07 12:42_

Nit: What's your reasoning for passing a callback here that extracts the identifier range rather than calling `identifier_range` internally in `no_slots_in_str_subclass`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_slots/rules/no_slots_in_namedtuple_subclass.rs`:64 on 2023-06-07 12:43_

Nit
```suggestion
    if let [Expr::Name(ast::ExprName { id, .. })] = class.bases.as_slice() {
        let scope = checker.semantic_model().scope();
```

Or, since you want to match on multiple ones:

```
match class.bases.as_slice() {
	[Expr::Name(ast::ExprName { id, .. })] => {
		...
	},
	[Expr::Call(ast::ExprCall { func, .. })] => {
		...
	},
	_ => {
		return; // or noop
	}
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_slots/rules/no_slots_in_str_subclass.rs`:56 on 2023-06-07 12:45_

NIt
```suggestion
   let [Expr::Name(ast::ExprName { id, .. })] = &class.bases.as_slice() else {
        return;
    };

```

---

_@MichaReiser reviewed on 2023-06-07 12:45_

---

_@qdegraaf reviewed on 2023-06-07 12:55_

---

_Review comment by @qdegraaf on `crates/ruff/src/checkers/ast/mod.rs`:827 on 2023-06-07 12:55_

It was a coin toss between that and passing in the `stmt` as an additional argument. I was hoping `identifier_range()` would accept the `class_def` object but that leads to: `expected &Stmt, found &StmtClassDef`. I saw the callback set-up and figured it is potentially cleaner to have all rules go:

```rust
rule_func(self, the_ast_object, callback)
```
rather than
```rust
rule_func(self, the_ast_object, potentially_another_ast_object)
```

But definitely a weak opinion, weakly held. So can change to passing in `stmt` if preferred (it does have the advantage of less complexity).


---

_@MichaReiser reviewed on 2023-06-07 12:56_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:827 on 2023-06-07 12:56_

Hmm I see. Yeah it's annoying that there's no way to `&Stmt` from a specific Stmt.

---

_@qdegraaf reviewed on 2023-06-07 12:57_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_slots/rules/no_slots_in_namedtuple_subclass.rs`:64 on 2023-06-07 12:57_

Ah that's nice! Really loving the extent to which those `if let`'s can check things in an implicit-but-transparent manner. 

---

_@qdegraaf reviewed on 2023-06-07 13:04_

---

_Review comment by @qdegraaf on `crates/ruff/src/checkers/ast/mod.rs`:827 on 2023-06-07 13:04_

Yeah I was thinking some kind of helper/converter for that might be useful (especially if in the future as  more and more funcs will just take a reference to a whole specific Stmt object). Unsure how much work that is or whether it opens a bunch of doors we don't want to open, not too knowledgeable about internals yet. 

---

_@MichaReiser reviewed on 2023-06-07 20:09_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:827 on 2023-06-07 20:09_

We have `AnyNodeRef` and `AnyNode` that can represent arbitrary nodes and you can convert from `Stmt` or `StmtClass` to either of those. We should probably start using those more widely but I don't consider this part of this PR's scope. CC: @charliermarsh 

---

_Review requested from @charliermarsh by @MichaReiser on 2023-06-07 20:10_

---

_@charliermarsh reviewed on 2023-06-09 02:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:827 on 2023-06-09 02:23_

I think I prefer just passing in the `Stmt`, it's more consistent with how we solve this elsewhere (even if it's not ideal).

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_slots/rules/no_slots_in_tuple_subclass.rs`:51 on 2023-06-09 02:25_

I think the originating plugin checks all bases.

---

_@charliermarsh reviewed on 2023-06-09 02:25_

---

_@charliermarsh reviewed on 2023-06-09 02:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_slots/rules/no_slots_in_namedtuple_subclass.rs`:95 on 2023-06-09 02:25_

Does the originating plugin do this? As far as I can tell, it doesn't.

---

_@charliermarsh reviewed on 2023-06-09 02:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_slots/rules/no_slots_in_namedtuple_subclass.rs`:95 on 2023-06-09 02:41_

Gonna simplify for now to bias towards merging, but happy to revisit and we'll have it here in the Git history if we want to restore.

---

_Renamed from "`[flake8-slots]` Add plugin, add `SLOT000`, `SLOT001` and `SLOT002`" to "[`flake8-slots`] Add plugin, add `SLOT000`, `SLOT001` and `SLOT002`" by @charliermarsh on 2023-06-09 03:56_

---

_Label `rule` added by @charliermarsh on 2023-06-09 03:59_

---

_Label `plugin` added by @charliermarsh on 2023-06-09 03:59_

---

_Merged by @charliermarsh on 2023-06-09 04:14_

---

_Closed by @charliermarsh on 2023-06-09 04:14_

---
