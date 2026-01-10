```yaml
number: 11400
title: "Create a newtype wrapper around `Vec<FStringElement>`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/f-string-elements
created_at: 2024-05-13T09:05:42Z
updated_at: 2024-05-27T15:47:26Z
url: https://github.com/astral-sh/ruff/pull/11400
synced_at: 2026-01-10T22:05:26Z
```

# Create a newtype wrapper around `Vec<FStringElement>`

---

_Pull request opened by @dhruvmanila on 2024-05-13 09:05_

## Summary

This PR adds a newtype wrapper around `Vec<FStringElement>` that derefs to a `&Vec<FStringElement>`.

Both f-string and format specifier are made up of `Vec<FStringElement>`. By creating a newtype wrapper around it, we can share the methods for both parent types.


---

_Label `internal` added by @dhruvmanila on 2024-05-13 09:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-13 09:05_

---

_Comment by @github-actions[bot] on 2024-05-13 10:36_

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

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/visitor.rs`:742 on 2024-05-13 13:35_

Did you consider adding an `IntoIterator` implementation for `&'a FStringElements`? I think you could avoid changes like this if you did.

```rs
impl<'a> IntoIterator for &'a FStringElements {
    type IntoIter = Iter<'a, FStringElement>;
    type Item = &'a FStringElement;
    fn into_iter(self) -> Self::IntoIter {
        self.iter()
    }
}

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1505 on 2024-05-13 13:38_

I would be tempted to keep these methods as simple wrappers, e.g.

```rs
impl FString {
    pub fn literals(&self) -> impl Iterator<Item = &FStringLiteralElement> {
        self.elements.literals()
    }

    pub fn expressions(&self) -> impl Iterator<Item = &FStringExpressionElement> {
        self.elements.expressions()
    }
}
```

As I think `for literal in f_string.literals()` feels like a slightly more natural API for downstream users than `for literal in f_string.elements.literals()`. But this is just a nit.

---

_@AlexWaygood approved on 2024-05-13 13:38_

---

_@dhruvmanila reviewed on 2024-05-13 14:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1505 on 2024-05-13 14:11_

Yeah, I thought about that but then I'm not sure if there's any more benefit to implement the newtype. Also, the reason I think `f_string.elements.literals()` API is better is because `elements` is a public field, so the user can either get all of the element or apply a filter on top of it (`elements.literals()`). And, this wrapper would need to be implemented for `FStringFormatSpec` as well.

---

_@AlexWaygood reviewed on 2024-05-13 14:14_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1505 on 2024-05-13 14:14_

Maybe part of the issue is the name of the method... what if `FStringElements.literals()` was renamed to `iter_literal_elements()`? And `expressions` renamed to `iter_expression_elements`? That might make it slightly clearer -- I think `for literal in f_string.elements.iter_literal_elements()` would read more naturally to me, although it's obviously slightly more verbose

But, as I say, this is just a nit :-)

---

_@dhruvmanila reviewed on 2024-05-13 15:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor.rs`:742 on 2024-05-13 15:50_

No I didn't consider that although it makes sense to implement it. Thanks!

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1505 on 2024-05-13 15:58_

That suggestion is valid although I'm fine with this and this naming pattern has been used in a lot of similar places ;)

Thanks for the suggestion!

---

_@dhruvmanila reviewed on 2024-05-13 15:58_

---

_Merged by @dhruvmanila on 2024-05-13 16:04_

---

_Closed by @dhruvmanila on 2024-05-13 16:04_

---

_Branch deleted on 2024-05-13 16:04_

---

_@AlexWaygood reviewed on 2024-05-13 16:05_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/visitor.rs`:742 on 2024-05-13 16:05_

Are you going to do this in a followup?

---

_@dhruvmanila reviewed on 2024-05-13 16:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor.rs`:742 on 2024-05-13 16:06_

I've implemented it in this PR.

---

_@dhruvmanila reviewed on 2024-05-13 16:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor.rs`:742 on 2024-05-13 16:07_

Oh my, how did I lost that change lol. I'll implement it in a follow-up then


---

_@MichaReiser reviewed on 2024-05-27 07:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1530 on 2024-05-27 07:31_

Nit: Should this be dereferencing to `[FStringElement]` or are there cases where we need a Vec?

---

_@dhruvmanila reviewed on 2024-05-27 15:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1530 on 2024-05-27 15:47_

I don't think so. I think I wanted it to be `[FStringElement]`. I'll change it.

---
