```yaml
number: 12582
title: Make setting and retrieving pydocstyle settings less tedious
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/is-property-2
created_at: 2024-07-30T14:50:29Z
updated_at: 2024-07-31T09:39:35Z
url: https://github.com/astral-sh/ruff/pull/12582
synced_at: 2026-01-12T15:55:41Z
```

# Make setting and retrieving pydocstyle settings less tedious

---

_@AlexWaygood_

## Summary

This PR is stacked on top of #12581.

The `pydocstyle` rules have settings that allow you to specify a list of decorators that should be considered property-like, and a list of decorators that should be ignored. The list of property-like decorators isn't just read by the pydocstyle rules; it's also used by many other rules so that they can pass in "extra" property-like decorators to `ruff_python_semantic::analyze::visibility::is_property()`.

Currently the pydocstyle `Settings` struct exposes these two settings as `BTreeSet<String>`, however, which is somewhat useless. To do any analysis with these values, rules first have to convert these lists into lists of `QualifiedName`s, which is tedious and verbose. This PR therefore makes the `ignore_decorators` and `property_decorators` fields on `ruff_linter::rules::pydocstyle::settings::Settings` private. Instead, it exposes public methods for setting the fields, and public methods for lazily iterating over the property-decorators or ignored-decoratos where the iterators map each decorator to a `QualifiedName`. The PR also adjusts `ruff_python_semantic::analyze::visibility::is_property()` so that it is able to receive a lazy iterator of `QualifiedName`s for the `extra_property` field, rather than a slice of `QualifiedName`s.

## Test Plan

No new tests added since this is a pure refactor that shouldn't change semantics at all.


---

_Label `internal` added by @AlexWaygood on 2024-07-30 14:50_

---

_Comment by @github-actions[bot] on 2024-07-30 15:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/visibility.rs`:68 on 2024-07-30 15:25_

```suggestion
    mut extra_properties: impl IntoIterator<Item = QualifiedName<'a>>,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:156 on 2024-07-30 15:25_

```suggestion
        extra_properties: impl IntoIterator<Item = QualifiedName<'a>>,
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2773 on 2024-07-30 15:26_

I personally prefer the old code here because it guarantees that all properties get initialized. The fact that it leaks the `BTreeSet` internals is unfortunate but I rather have compile time guarantees (and guidance) on how new settings have to be added.

---

_@MichaReiser approved on 2024-07-30 15:27_

---

_@AlexWaygood reviewed on 2024-07-30 15:37_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:2773 on 2024-07-30 15:37_

Yeah. I wondered about just creating a `Settings::new()` method, but went for this as I thought `Settings::default().with_property_decorators(&[...])` was more readable than `Settings::new(None, &[...], &[])`. It's hard to tell with the `new()` call which decorator list is empty and which isn't, and having to pass in `None` explicitly for the `convention` argument feels awkward.

But it's less code and it guarantees that all properties get initialized, so I guess I'll go for that instead.

---

_@AlexWaygood reviewed on 2024-07-30 15:56_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/visibility.rs`:68 on 2024-07-30 15:56_

I can't get this to work:

```
error[E0507]: cannot move out of `extra_properties`, a captured variable in an `FnMut` closure
  --> crates/ruff_python_semantic/src/analyze/visibility.rs:75:26
   |
71 |     let mut extra_properties = extra_properties.into_iter();
   |         -------------------- captured outer variable
72 |     decorator_list.iter().any(|decorator| {
   |                               ----------- captured by this `FnMut` closure
...
75 |             .is_some_and(|qualified_name| {
   |                          ^^^^^^^^^^^^^^^^ `extra_properties` is moved here
76 |                 let extra_properties = extra_properties.into_iter();
   |                                        ----------------
   |                                        |
   |                                        variable moved due to use in closure
   |                                        move occurs because `extra_properties` has type `<impl IntoIterator<Item = QualifiedName<'a>> as std::iter::IntoIterator>::IntoIter`, which does not implement the `Copy` trait

```

But it occurs to me anyway that this optimisation (to use a lazy iterator rather than eagerly collecting everything as a slice) is incorrect as I currently have it. I'm using the same iterator on each iteration of the outer loop currently. The iterator could have been exhausted after the first iteration of the outer loop, which would lead to incorrect results.

---

_Merged by @AlexWaygood on 2024-07-31 09:39_

---

_Closed by @AlexWaygood on 2024-07-31 09:39_

---

_Branch deleted on 2024-07-31 09:39_

---
