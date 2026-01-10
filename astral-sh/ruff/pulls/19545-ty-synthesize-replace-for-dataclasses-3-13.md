```yaml
number: 19545
title: "[ty] synthesize `__replace__` for dataclasses (>=3.13)"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
assignees: []
merged: true
base: main
head: thejchap/more-dataclasses
created_at: 2025-07-25T01:21:22Z
updated_at: 2025-07-29T15:32:01Z
url: https://github.com/astral-sh/ruff/pull/19545
synced_at: 2026-01-10T17:58:13Z
```

# [ty] synthesize `__replace__` for dataclasses (>=3.13)

---

_Pull request opened by @thejchap on 2025-07-25 01:21_

## Summary
https://github.com/astral-sh/ty/issues/111

adds support for the new `copy.replace` and `__replace__` protocol [added in 3.13](https://docs.python.org/3/whatsnew/3.13.html#copy)

- docs: https://docs.python.org/3/library/copy.html#object.__replace__
- some discussion on pyright/mypy implementations: https://discuss.python.org/t/dataclass-transform-and-replace/69067



### Burndown
- [x] add tests
- [x] implement `__replace__`
  - [ ] [collections.namedtuple()](https://docs.python.org/3/library/collections.html#collections.namedtuple)
  - [x] [dataclasses.dataclass](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass)

## Test Plan
new mdtests

---

_Comment by @github-actions[bot] on 2025-07-25 01:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] synthesize __replace__ for >=3.13" to "[ty] synthesize `__replace__` for dataclasses (>=3.13)" by @thejchap on 2025-07-25 01:54_

---

_Label `ty` added by @AlexWaygood on 2025-07-25 07:12_

---

_Comment by @AlexWaygood on 2025-07-25 11:00_

Thank you for working on this!!

> * [datetime.datetime](https://docs.python.org/3/library/datetime.html#datetime.datetime), [datetime.date](https://docs.python.org/3/library/datetime.html#datetime.date), [datetime.time](https://docs.python.org/3/library/datetime.html#datetime.time)
> 
> * [ ]  [inspect.Signature](https://docs.python.org/3/library/inspect.html#inspect.Signature), [inspect.Parameter](https://docs.python.org/3/library/inspect.html#inspect.Parameter)
> 
> * [ ]  [types.SimpleNamespace](https://docs.python.org/3/library/types.html#types.SimpleNamespace)
> 
> * [ ]  [code objects](https://docs.python.org/3/reference/datamodel.html#code-objects)

Hmm, I'm not sure we need to implement special casing for any of these. Typeshed should already include `__replace__` methods for all of these in the stdlib stubs that we vendor, so no additional implementation from us _should_ be required -- it should hopefully "just work"! I _think_ the only cases where we need to add some special casing are named tuples and dataclasses, because for these very special types the methods are synthesized at runtime rather than being present in the source code.

---

_@thejchap reviewed on 2025-07-26 03:40_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/class.rs`:1675 on 2025-07-26 03:40_

not sure this is the right way to model this...

```py
reveal_type(a.__replace__)

# mypy: def (*, a: builtins.int =) -> test.Test
# ty: (*, a: int = int) -> Test
```

---

_@thejchap reviewed on 2025-07-26 03:42_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/class.rs`:1597 on 2025-07-26 03:42_

didn't feel like the cleanest way to toggle some of this behavior, open to suggestions...

---

_Marked ready for review by @thejchap on 2025-07-26 03:46_

---

_Review requested from @carljm by @thejchap on 2025-07-26 03:46_

---

_Review requested from @AlexWaygood by @thejchap on 2025-07-26 03:46_

---

_Review requested from @sharkdp by @thejchap on 2025-07-26 03:46_

---

_Review requested from @dcreager by @thejchap on 2025-07-26 03:46_

---

_Comment by @thejchap on 2025-07-26 03:46_

@AlexWaygood ready for some feedback on this one!

---

_@sharkdp reviewed on 2025-07-28 07:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1597 on 2025-07-28 07:50_

I think you could maybe just check for `name == "__replace__"` instead of adding this `for_replace` flag.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/replace.md`:9 on 2025-07-28 07:53_

If you move this block up to the top level `# Replace` section, it will be active for all sub-sections. I think it makes sense to do this here, as every test will otherwise need to have one of these.

---

_@sharkdp reviewed on 2025-07-28 07:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/replace.md`:45 on 2025-07-28 07:57_

The benefit of modeling the signature of `__replace__` more precisely is that we would catch errors if you try to update a field with an incorrect value. So I think we should add a test for this. Something like

```suggestion
reveal_type(d)  # revealed: Point

e = replace(a, x="wrong")  # error: [invalid-argument-type]
```

---

_@sharkdp reviewed on 2025-07-28 07:57_

---

_@sharkdp reviewed on 2025-07-28 07:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/replace.md`:49 on 2025-07-28 07:57_

I think it's fine to drop this.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1675 on 2025-07-28 07:59_

The type of the default value is not that important, and we're even thinking about removing this from the displayed signature (showing an actual value instead — if available. If none is available, like here, we could still display `= ...`). So what you have here seems fine. I would move the comment inside the `if` branch, though.

---

_@sharkdp reviewed on 2025-07-28 07:59_

---

_@sharkdp reviewed on 2025-07-28 08:00_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1749 on 2025-07-28 08:00_

```suggestion
            (CodeGeneratorKind::DataclassLike, "__replace__") if Program::get(db).python_version(db) < PythonVersion::PY313 => {
```

---

_@sharkdp approved on 2025-07-28 08:00_

Very nice — thank you. I added a couple of minor inline comments.

---

_Comment by @github-actions[bot] on 2025-07-28 12:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @thejchap on 2025-07-28 13:22_

@sharkdp thanks! addressed most of your comments, and added a wip implementation of special handling of `copy.replace` [here](https://github.com/astral-sh/ruff/pull/19545/files#diff-89ab607f2738327350b89c4bf98de6e4af15eb4cfdf55d3afc1cb71499928f88R952). please let me know if that's on the right track

if so, i'm fine to either merge as-is and complete that in a follow-on pr, or finish as part of this one

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/replace.md`:24 on 2025-07-29 06:54_

Remove?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:953 on 2025-07-29 06:55_

I don't think the version check is necessary here. If we have a `KnownFunction::Replace`, that should be enough evidence?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:955 on 2025-07-29 06:56_

Should this be
```suggestion
                            let [Some(instance_ty), ..] = overload.parameter_types() else {
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:980 on 2025-07-29 07:04_

As an upfront comment: I don't think it's particularly important that we support this feature (issuing `invalid-argument-type` for invalid `replace(…)` calls). So if this turns out to be complicated to get right, I think we should just drop the feature.

Instead of querying the signature of `__replace__`, I think we should try to actually call the `__replace__` dunder method via `try_call_dunder`, and then bubble up any call errors as diagnostics via `overload.errors.push`, similar to what we do in the `Type::MethodWrapper(MethodWrapperKind::PropertyDunderGet(property))` branch, for example.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1762 on 2025-07-29 07:05_

Minor: extract `Type::instance(db, self.apply_optional_specialization(db, specialization)` into a variable since it's used twice?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:1141 on 2025-07-29 07:06_

Similar here. Is the version check really required? Prior to 3.13, the function will just not be available in that module, so I think it's fine to just check the module

---

_@sharkdp reviewed on 2025-07-29 07:06_

---

_Comment by @thejchap on 2025-07-29 12:13_

@sharkdp thanks - i pulled out the `invalid-argument-type` logic for invalid `replace(…)` calls for now. i might work on this more in another pr

---

_@sharkdp approved on 2025-07-29 15:22_

Thank you very much. I made a few minor adjustments.

---

_Merged by @sharkdp on 2025-07-29 15:32_

---

_Closed by @sharkdp on 2025-07-29 15:32_

---
