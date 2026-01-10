```yaml
number: 13981
title: "Add augmented assignment inference for `-=` operator"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/aug
created_at: 2024-10-29T17:16:03Z
updated_at: 2024-10-30T08:13:36Z
url: https://github.com/astral-sh/ruff/pull/13981
synced_at: 2026-01-10T20:59:37Z
```

# Add augmented assignment inference for `-=` operator

---

_Pull request opened by @charliermarsh on 2024-10-29 17:16_

## Summary

See: https://github.com/astral-sh/ruff/issues/12699


---

_Review requested from @carljm by @charliermarsh on 2024-10-29 17:16_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-29 17:16_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-29 17:16_

---

_@charliermarsh reviewed on 2024-10-29 17:16_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:962 on 2024-10-29 17:16_

Per Discord: the use shouldn't see the definition, so we had to change the order here. (This hasn't mattered up until augmented assignments.)

---

_@charliermarsh reviewed on 2024-10-29 17:16_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1466 on 2024-10-29 17:16_

The logic here is meant to mimic:
```python
if hasattr(a, "__isub__"):
    _value = a.__isub__(b)
    if _value is not NotImplemented:
        a = _value
    else:
        a = a - b
    del _value
 else:
     a = a - b
```

As per https://snarky.ca/unravelling-augmented-arithmetic-assignment/.

---

_@charliermarsh reviewed on 2024-10-29 17:17_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1445 on 2024-10-29 17:17_

Should this... fall back to `a - b`?

---

_@charliermarsh reviewed on 2024-10-29 17:17_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1466 on 2024-10-29 17:17_

However, I don't really know how to handle the `NotImplemented` case, if at all -- i.e., how we can determine whether to use `__isub__` or fall back to `a - b`. Can we even detect that from the type system?

---

_Label `red-knot` added by @charliermarsh on 2024-10-29 17:17_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2418 on 2024-10-29 17:18_

The diff here is messy, but this is just: taking the `ExprContext::Load` branch and moving it into a dedicated method (`infer_name_load`), then calling it from the aforementioned branch.

---

_@charliermarsh reviewed on 2024-10-29 17:18_

---

_@charliermarsh reviewed on 2024-10-29 17:18_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2535 on 2024-10-29 17:18_

I assume these still need to happen, even though we're not using the result, since they mutate the inference builder.

---

_@AlexWaygood reviewed on 2024-10-29 17:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1466 on 2024-10-29 17:28_

The convention for dunder methods is that if calling the dunder would return `NotImplemented`, it's annotated "as if it were an illegal call". So `int.__sub__` is annotated as "not accepting" `float` in typeshed, even though actually calling it with a `float` "works fine" (it just returns `NotImplemented`, allowing the operation to fallback to `float.__rsub__`):

https://github.com/astral-sh/ruff/blob/60a2dc53e7b4407c3c0c14093fd8542879997ca8/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L273

```pycon
>>> (42).__sub__(42.0)
NotImplemented
```

Currently, though, we don't do any checking of argument types passed into functions, so we can't implement the "fallback to the other method" logic fully right now. There's lots of TODO comments in our tests for binary arithmetic about this currently.

---

_@AlexWaygood reviewed on 2024-10-29 17:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1466 on 2024-10-29 17:30_

Some example TODOs for you: https://github.com/astral-sh/ruff/blob/60a2dc53e7b4407c3c0c14093fd8542879997ca8/crates/red_knot_python_semantic/resources/mdtest/binary/instances.md?plain=1#L284-L291

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1466 on 2024-10-29 17:39_

The way these dunder methods are typically annotated is that their parameter type should reflect the type of argument for which the method will return a value (not `Notimplemented`), and any other type will return `NotImplemented`. So we can detect this from the type system, but currently red-knot can't, because we don't yet have call signature checking (it's something I want to work on this week.)

I'll comment more details above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1445 on 2024-10-29 17:45_

This relates to your question below about `NotImplemented`. If there is an `__isub__` method, but it doesn't accept the `value_type` we try to pass it, then we should fall back to `__sub__`. For any other call error (e.g. `__isub__` is set to some non-callable type for some reason) we should just emit a diagnostic and be done here. This will require some more complex matching on the specific error kind we get back from the call -- but this is all a TODO until we have call signature checking.

I would like to add a test for this case (`__isub__` exists but doesn't accept the type we pass to it) with a TODO comment in the tests for the right result, to help us remember that this needs fixing.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1442 on 2024-10-29 17:48_

I think this diagnostic probably should clarify that it's specific to augmented assignment, and should include the `value_type` as well.
```suggestion
                            "Operator `{op}=` is unsupported for type `{}`",
                            target_type.display(self.db),
                            value_type.display(self.db),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1429 on 2024-10-29 17:51_

The test you added above is great, but it's not even testing this branch at all, since the type won't be `Type::Instance` but rather `Type::IntLiteral`, so you're getting the behavior from the fallback to regular binary ops below.

Let's add a test where we actually define `__isub__` on a type and show that we take its return type. And a test where we set `__isub__` to something not callable and show we get an error. (And the TODO test I mention below about `__isub__` not accepting `value_type` as argument.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2535 on 2024-10-29 17:52_

Yeah, we currently maintain an invariant that every expression gets a type inferred for it, so we have to infer types even if we won't use them. (The theory is that in future an LSP may want to query a type on hover for any expression.)

---

_@carljm approved on 2024-10-29 17:52_

Looks good! Mostly just would like to add a few more tests.

---

_Comment by @carljm on 2024-10-29 17:56_

Oh, just noticed the `corpus_no_panic` test failure. This is the test that verifies that all expressions get a type. I think the problem here is that when we use `infer_name_load` and `infer_attribute_load` to get the type of the LHS for a Name or Attribute, we never actually store the type on the expression node (`infer_expression` normally takes care of that.)

But we don't want to store the assume-Load type on the node. So I think we also need to call `infer_expression` on the LHS node regardless, even if we are also calling `infer_name_load` or `infer_attribute_load` on it, just to get the `None` type stored on it that we currently infer for Store expressions.

---

_@carljm reviewed on 2024-10-29 18:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1466 on 2024-10-29 18:05_

Oops, didn't see Alex's review before submitting mine.

---

_@charliermarsh reviewed on 2024-10-29 22:27_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1429 on 2024-10-29 22:27_

Hah -- thank you, that's embarrassing :)

---

_Comment by @charliermarsh on 2024-10-29 22:34_

Still getting that same failure after calling `self.infer_expression` (at least locally), will return to it later and see what I can figure out.

---

_Comment by @charliermarsh on 2024-10-29 22:36_

Oh I think it's because we end up visiting the attribute twice, maybe...? Something like that? Because `infer_attribute_load` calls `self.infer_expression`?

---

_Comment by @carljm on 2024-10-29 22:47_

Ah yeah -- if you look at the error you're getting, I'll bet before it was `no entry found for key` and now it's a failure of the assertion on line 1900? That is because we end up calling `infer_expression` twice for the same expression, which we try to maintain an invariant that we don't do.

---

_Comment by @carljm on 2024-10-29 22:55_

One option for how to handle this is to extract that code at the end of `self.infer_expression` (that stores a type into the expression map and asserts there wasn't one there already) into a method, like `fn store_expression_type(...)`, and then instead of calling `self.infer_expression(target)` along with `self.infer_attribute_load(target)`, instead you'd just manually store a `None` (or like I mentioned, `Never` is probably better but then you should also update it in `infer_attribute_expression` and `infer_name_expression` for consistency) for the target node. This means we end up duplicating the knowledge of what type to store for a `Store` context expression... there's probably a way to refactor away that duplication.

---

_Merged by @charliermarsh on 2024-10-30 02:14_

---

_Closed by @charliermarsh on 2024-10-30 02:14_

---

_Branch deleted on 2024-10-30 02:14_

---

_Comment by @github-actions[bot] on 2024-10-30 02:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1431 on 2024-10-30 08:09_

You need to explicitly pass in the `self` argument here because we've accessed the method on the class rather than the instance, so you're getting the original function object rather than a method object from the `class.class_member` call

```suggestion
            let call = class_member.call(self.db, &[target_type, value_type]);
```

This will matter when we start checking that function calls are being passed arguments of the correct types

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2521 on 2024-10-30 08:13_

nit

```suggestion
        value_ty.member(self.db, &Name::new(&attr.id))
```

---

_@AlexWaygood reviewed on 2024-10-30 08:13_

Nice

---
