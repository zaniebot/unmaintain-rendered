```yaml
number: 9267
title: "[`pylint`] Implement `self-cls-assignment` (`W0642`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - accepted
  - preview
assignees: []
merged: true
base: main
head: add-W0642
created_at: 2023-12-24T22:05:01Z
updated_at: 2024-04-15T09:15:56Z
url: https://github.com/astral-sh/ruff/pull/9267
synced_at: 2026-01-12T15:55:28Z
```

# [`pylint`] Implement `self-cls-assignment` (`W0642`)

---

_@diceroll123_

## Summary

Implements [`W0642`/`self-cls-assignment`](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/self-cls-assignment.html)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-12-24 22:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+52 -0 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Invalid assignment to `cls` argument in class method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+51 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/categorical.py#L568'>pandas/core/arrays/categorical.py:568:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1083'>pandas/core/arrays/datetimelike.py:1083:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1098'>pandas/core/arrays/datetimelike.py:1098:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1099'>pandas/core/arrays/datetimelike.py:1099:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1127'>pandas/core/arrays/datetimelike.py:1127:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1147'>pandas/core/arrays/datetimelike.py:1147:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1149'>pandas/core/arrays/datetimelike.py:1149:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1154'>pandas/core/arrays/datetimelike.py:1154:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1205'>pandas/core/arrays/datetimelike.py:1205:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1221'>pandas/core/arrays/datetimelike.py:1221:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1278'>pandas/core/arrays/datetimelike.py:1278:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1292'>pandas/core/arrays/datetimelike.py:1292:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1498'>pandas/core/arrays/datetimelike.py:1498:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L1717'>pandas/core/arrays/datetimelike.py:1717:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L2144'>pandas/core/arrays/datetimelike.py:2144:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L2166'>pandas/core/arrays/datetimelike.py:2166:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L455'>pandas/core/arrays/datetimelike.py:455:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L806'>pandas/core/arrays/datetimelike.py:806:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/arrays/datetimelike.py#L997'>pandas/core/arrays/datetimelike.py:997:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/frame.py#L7715'>pandas/core/frame.py:7715:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/frame.py#L7728'>pandas/core/frame.py:7728:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/frame.py#L8071'>pandas/core/frame.py:8071:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/frame.py#L8083'>pandas/core/frame.py:8083:21:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/frame.py#L8123'>pandas/core/frame.py:8123:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L3407'>pandas/core/generic.py:3407:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L3568'>pandas/core/generic.py:3568:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L7905'>pandas/core/generic.py:7905:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L7908'>pandas/core/generic.py:7908:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L7911'>pandas/core/generic.py:7911:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L9168'>pandas/core/generic.py:9168:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L9175'>pandas/core/generic.py:9175:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/generic.py#L9178'>pandas/core/generic.py:9178:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/groupby/grouper.py#L249'>pandas/core/groupby/grouper.py:249:13:</a> PLW0642 Invalid assignment to `cls` argument in class method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L1329'>pandas/core/indexes/base.py:1329:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L2130'>pandas/core/indexes/base.py:2130:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L2913'>pandas/core/indexes/base.py:2913:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L3061'>pandas/core/indexes/base.py:3061:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L3293'>pandas/core/indexes/base.py:3293:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L4384'>pandas/core/indexes/base.py:4384:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L5958'>pandas/core/indexes/base.py:5958:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/indexes/base.py#L5962'>pandas/core/indexes/base.py:5962:20:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/internals/blocks.py#L1122'>pandas/core/internals/blocks.py:1122:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/internals/blocks.py#L1170'>pandas/core/internals/blocks.py:1170:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/internals/blocks.py#L1733'>pandas/core/internals/blocks.py:1733:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/internals/managers.py#L578'>pandas/core/internals/managers.py:578:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/series.py#L5835'>pandas/core/series.py:5835:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d8c7e85a1b260f2296ad4b921555b699b3ed3f27/pandas/core/series.py#L5844'>pandas/core/series.py:5844:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0642 | 52 | 52 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[pylint] - implement `W0642`/`self-cls-assignment`" to "[`pylint`] - implement `W0642`/`self-cls-assignment`" by @diceroll123 on 2024-01-20 22:56_

---

_Renamed from "[`pylint`] - implement `W0642`/`self-cls-assignment`" to "[`pylint`] - implement `self-cls-assignment` (`W0642`)" by @diceroll123 on 2024-01-21 04:34_

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:25_

---

_Comment by @MichaReiser on 2024-04-05 10:26_

The rule fits into the suspicious category. We should consider renaming the rule to `self-or-cls-assignments` to make it grammatically correct `allow(self-or-cls-assignments)`

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-10 09:46_

---

_Label `rule` added by @dhruvmanila on 2024-04-10 09:46_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:27 on 2024-04-10 10:13_

I think we can improve the message here depending on whether `self` or `cls` was used. We can frame it in a similar way as Pylint does:

```
Invalid assignment to (self|cls) in (instance|class) method
```

We can use the `MethodType` enum as suggested below.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:16 on 2024-04-10 10:13_

Can you expand the documentation to include an "Example" section and optionally a "References" section? For example,

https://github.com/astral-sh/ruff/blob/de46a36bbcdc1df4a56fc486ae5c7bcf2c4bbba0/crates/ruff_linter/src/rules/flake8_2020/rules/name_or_attribute.rs#L20-L37

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:18 on 2024-04-10 10:17_

As suggested by Micha, can you rename this to `self_or_cls_assignment`?

```suggestion
pub struct SelfOrClsAssignment {
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:31 on 2024-04-10 10:18_

As suggested by Micha, can you rename this to `self_or_cls_assignment`?
```suggestion
pub(crate) fn self_or_cls_assignment(checker: &mut Checker, target: &Expr) {
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:63 on 2024-04-10 10:21_

We'd need to first check in `posonlyargs` and then in `args` as in:

```rs
    let Some(ParameterWithDefault {
        parameter: self_or_cls,
        ..
    }) = parameters
        .posonlyargs
        .first()
        .or_else(|| parameters.args.first())
    else {
        return;
    };
```

This is in case there's a `/` in the parameter list `def foo(self, foo, /): ...`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:73 on 2024-04-10 10:26_

nit: I'd move these checks before the function classification above and possibly use an enum. We can then use this enum in `SelfOrClsAssignment` and accordingly format the error message as suggested earlier:

```rs
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
enum MethodType {
	Instance,
	Class,
}

impl MethodType {
	fn arg_name(self) -> &'static str {
		match self {
			MethodType::Instance => "self",
			MethodType::Class => "cls",
		}
	}
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:97 on 2024-04-10 10:28_

We'd also need to check in `Expr::List` in a similar manner as that's a valid assignment target.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/self_cls_assignment.py`:18 on 2024-04-10 10:28_

Can you add a similar test case for `self` used in a class method?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/self_cls_assignment.py`:8 on 2024-04-10 10:29_

We should also add test cases when the assignment target is a list. Refer to my comment below.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/self_cls_assignment.py`:3 on 2024-04-10 10:30_

Can we add some test cases which uses other type of functions? For example:

Using a normal function:
```py
def list_fruits(self, cls) -> None:
    self = "apple"  # Ok
    cls = "banana"  # Ok
```

Using a static method:
```py
class Fruit:
	@staticmethod
	def list_fruits(self, cls) -> None:
	    self = "apple"  # Ok
	    cls = "banana"  # Ok
```

---

_@dhruvmanila requested changes on 2024-04-10 10:32_

Thank you for the PR!

This looks like a great start. I've a few recommendations around testing and logic but otherwise looks good.

---

_Comment by @diceroll123 on 2024-04-13 17:45_

Couldn't find any official Python documentation on why this is not a good thing to do in code, but I've added an example, and addressed other changes as well. 😄 

---

_Review requested from @dhruvmanila by @diceroll123 on 2024-04-13 17:50_

---

_@dhruvmanila reviewed on 2024-04-15 08:54_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/self_cls_assignment.rs`:63 on 2024-04-15 08:54_

I went ahead and added test cases for this scenario

---

_@dhruvmanila approved on 2024-04-15 09:01_

Thank you! I made some additional changes which I've listed down:
* Check starred expressions (`*self = "foo"`)
* Update example case and add "Use instead:" section

Looking at the ecosystem changes, a follow-up would probably be to allow type casting like the following:

```py
self = cast(Fruits, self)
cls = cast(Colors, cls)
```

---

_Renamed from "[`pylint`] - implement `self-cls-assignment` (`W0642`)" to "[`pylint`] Implement `self-cls-assignment` (`W0642`)" by @dhruvmanila on 2024-04-15 09:01_

---

_Label `preview` added by @dhruvmanila on 2024-04-15 09:01_

---

_Merged by @dhruvmanila on 2024-04-15 09:06_

---

_Closed by @dhruvmanila on 2024-04-15 09:06_

---
