```yaml
number: 20756
title: "[ty] Disambiguate classes that live in different modules but have the same fully qualified names"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/even-more-class-disambiguation
created_at: 2025-10-07T23:31:55Z
updated_at: 2025-10-08T17:27:42Z
url: https://github.com/astral-sh/ruff/pull/20756
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Disambiguate classes that live in different modules but have the same fully qualified names

---

_@AlexWaygood_

## Summary

Even disambiguating classes using their fully qualified names is not enough for some diagnostics. We've seen real-world examples in the ecosystem (and https://github.com/astral-sh/ruff/pull/20368 introduces some more!) where two types can be different, but can still have the same fully qualified name. In these cases, our disambiguation machinery needs to print the file path and line number of the class in order to disambiguate classes with similar names in our diagnostics.

Helps with https://github.com/astral-sh/ty/issues/1306

## Test Plan

Mdtests

---

_Label `ty` added by @AlexWaygood on 2025-10-07 23:31_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-07 23:31_

---

_Comment by @github-actions[bot] on 2025-10-07 23:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 23:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model]`, found `set[bokeh.model.model.Model]`
+ src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.pyi:40]`

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-10-07 23:36_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-07 23:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-07 23:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-07 23:36_

---

_Comment by @github-actions[bot] on 2025-10-08 00:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:290 on 2025-10-08 04:41_

Why can't we use `self.db` here anymore?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:333 on 2025-10-08 04:42_

I don't think we should do this. We intentionally use platform specific path representations anywhere where we display a path. I don't see why we should deviate from that here and it results in an inconsistent experience (I don't think easier testing is a good enough reason)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:122 on 2025-10-08 04:45_

I'm not sure I fully understand this code but what if you have something like this

```
class B:
	...

class A(B):
	...
```

both are defined in a pyi and in the py file and there's a diagnostic involving `A`. Would it mean that we don't show the file and line number for `A` because it has different parents?

---

_@MichaReiser reviewed on 2025-10-08 04:45_

Lovely :)

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-08 06:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:290 on 2025-10-08 11:50_

Because it's now a method on `ClassLiteral` (it used to be a method on `ClassDisplay`), and `ClassLiteral` doesn't have a `self.db` field. It's now a method on `ClassLiteral` rather than `ClassDisplay` because it makes it easier to reuse the method from other places in this module.

---

_@AlexWaygood reviewed on 2025-10-08 11:50_

---

_@AlexWaygood reviewed on 2025-10-08 11:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:333 on 2025-10-08 11:51_

Yeah, fair enough. I guess I have to go write some complicated regexes to get mdtest to work with these paths that are rendered differently on differnt platforms. So be it :)

---

_@AlexWaygood reviewed on 2025-10-08 11:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:122 on 2025-10-08 11:56_

"class parents" is possibly a slightly confusing term here. (I didn't add or name that method, in my defence ;)

For a class `A` in a module `foo.bar`, the "parents" of `A` will be `["foo", "bar"]`. For a class `B` defined in the scope of a method `m` of a class `D` in a module `foo.bar`, the "parents" of `B` will be `["foo", "bar", "D", "<locals of function 'm'>"]`.

On `main`, we currently only disambiguate classes in our diagnostics if there are two classes with the same unqualified name appearing in the same diagnostic (e.g. "expected `A`, got `A`" is a terrible diagnostic message -- "expected `foo.bar.A`, got `baz.A`" is much better). This PR goes a step further by also comparing the _fully_ qualified names of the classes: if using the fully qualified names would also not be sufficient to disambiguate the classes (because they have the same fully qualified names, as well as the same unqualified names!), we also display the file and line number when displaying the class.

---

_@AlexWaygood reviewed on 2025-10-08 12:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:122 on 2025-10-08 12:35_

I renamed the method and added some docs

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-08 12:36_

---

_@MichaReiser approved on 2025-10-08 17:26_

---

_Merged by @AlexWaygood on 2025-10-08 17:27_

---

_Closed by @AlexWaygood on 2025-10-08 17:27_

---

_Branch deleted on 2025-10-08 17:27_

---
