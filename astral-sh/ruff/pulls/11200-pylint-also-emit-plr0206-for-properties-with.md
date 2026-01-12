```yaml
number: 11200
title: "[`pylint`] Also emit `PLR0206` for properties with variadic parameters"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: pylint-property
created_at: 2024-04-29T11:31:02Z
updated_at: 2024-04-30T10:59:38Z
url: https://github.com/astral-sh/ruff/pull/11200
synced_at: 2026-01-12T15:55:37Z
```

# [`pylint`] Also emit `PLR0206` for properties with variadic parameters

---

_@AlexWaygood_

## Summary

Currently this rule checks the number of nonvariadic parameters a property has, but doesn't check how many variadic parameters the property has. That makes little sense to me: if a property has variadic non-self parameters, that seems just as invalid as if a property has nonvariadic non-self parameters.

This PR changes the rule so that it checks the total number of parameters the property has, rather than just the number of nonvariadic parameters. Following the new APIs added in https://github.com/astral-sh/ruff/commit/87929ad5f1306cb889cbc85ddfdb9404d9b5c6ad, this has the nice side effect of also simplifying the code slightly.

This means that we diverge from pylint's behaviour a little more in how we implement this rule, but we were already doing something slightly different. It looks like pylint for some reason only checks the number of positional-or-keyword nonvariadic parameters in a property function, and ignores the number of positional-only or keyword-only nonvariadic parameters: <https://github.com/pylint-dev/pylint/blob/524f2561eda21b5593ab1c69963442031817883d/pylint/checkers/classes/class_checker.py#L1414>. I don't understand why they would do that; I think that's buggy behaviour from pylint there.

## Test Plan

`cargo test`. I added some new fixtures.


---

_Label `bug` added by @AlexWaygood on 2024-04-29 11:31_

---

_Label `rule` added by @AlexWaygood on 2024-04-29 11:31_

---

_Comment by @github-actions[bot] on 2024-04-29 11:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L1114'>pymilvus/orm/collection.py:1114:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L1255'>pymilvus/orm/collection.py:1255:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L237'>pymilvus/orm/collection.py:237:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L259'>pymilvus/orm/collection.py:259:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L266'>pymilvus/orm/collection.py:266:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/partition.py#L112'>pymilvus/orm/partition.py:112:9:</a> PLR0206 Cannot have defined parameters for properties
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0206 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L1114'>pymilvus/orm/collection.py:1114:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L1255'>pymilvus/orm/collection.py:1255:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L237'>pymilvus/orm/collection.py:237:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L259'>pymilvus/orm/collection.py:259:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/collection.py#L266'>pymilvus/orm/collection.py:266:9:</a> PLR0206 Cannot have defined parameters for properties
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2fac2be3922126c7163936757b31153e7773358/pymilvus/orm/partition.py#L112'>pymilvus/orm/partition.py:112:9:</a> PLR0206 Cannot have defined parameters for properties
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0206 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-04-29 11:59_

> ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

These all look like true positives to me. The affected project defines a bunch of properties with `**kwargs` variadic parameters, but no arguments are ever passed to those parameters as far as I can see. The property definitions would make more sense without the `**kwargs` parameters, even if the code works fine today without any exception being raised.

---

_Comment by @dhruvmanila on 2024-04-30 08:17_

I think these changes are correct but it would be useful to get a second opinion. I'd also look at why Pylint doesn't trigger on them. Is there an issue in Pylint which might give us an insight here?

(Edit: Pyright -> Pylint)

---

_Comment by @AlexWaygood on 2024-04-30 08:22_

> I'd also look at why Pylint doesn't trigger on them. Is there an issue in Pylint which might give us an insight here?

Yeah, I'll do some digging and check to see if there's a reason why pylint only checks for the number of positional-or-keyword parameters.

---

_Comment by @AlexWaygood on 2024-04-30 08:42_

The results of digging:
- The pylint check was originally added in https://github.com/pylint-dev/pylint/commit/a636569b9cc53279b3b5f4d24b58d9bc739b05d3, a commit that was pushed straight to pylint's `main` branch without a pull request.
- The commit closed the pylint issue https://github.com/pylint-dev/pylint/issues/3006. The issue suggests that the check _was_ intended to catch properties with variadic non-`self` parameters as well as those with nonvariadic non-`self` parameters, but there's no discussion on the issue.
- The only issue regarding pylint's `property-with-parameters` check in the years since it was added that I can find is https://github.com/pylint-dev/pylint/issues/3600. That issue was fixed by the pylint PR https://github.com/pylint-dev/pylint/pull/3621.

I confirmed locally that running pylint on the original motivating example for the pylint check given in https://github.com/pylint-dev/pylint/issues/3006#issue-468017491 does not actually trigger a violation of the pylint rule (because the example has variadic non-`self` parameters rather than nonvariadic non-`self` parameters). I checked using `pylint==3.1.0`.

---

_Review requested from @carljm by @AlexWaygood on 2024-04-30 08:42_

---

_Comment by @AlexWaygood on 2024-04-30 08:43_

> I think these changes are correct but it would be useful to get a second opinion.

@carljm, could you possibly take a look?

---

_Comment by @dhruvmanila on 2024-04-30 08:55_

Thanks for the details. It's good to go from my side 👍 

---

_Comment by @AlexWaygood on 2024-04-30 09:01_

I opened a pylint issue here to let them know about the bug on their side: https://github.com/pylint-dev/pylint/issues/9584

---

_Comment by @AlexWaygood on 2024-04-30 10:59_

The pylint maintainers have added labels to https://github.com/pylint-dev/pylint/issues/9584 which indicate that they agree there's a bug on their side, so I'll merge this now 👍

Thanks for the review @dhruvmanila!

---

_Merged by @AlexWaygood on 2024-04-30 10:59_

---

_Closed by @AlexWaygood on 2024-04-30 10:59_

---

_Branch deleted on 2024-04-30 10:59_

---
