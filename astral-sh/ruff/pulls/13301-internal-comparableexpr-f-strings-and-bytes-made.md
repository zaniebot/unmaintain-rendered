```yaml
number: 13301
title: "[internal] `ComparableExpr` (f)strings and bytes made invariant under concatenation"
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: comparable
created_at: 2024-09-10T04:54:50Z
updated_at: 2024-09-25T15:07:37Z
url: https://github.com/astral-sh/ruff/pull/13301
synced_at: 2026-01-10T20:59:36Z
```

# [internal] `ComparableExpr` (f)strings and bytes made invariant under concatenation

---

_Pull request opened by @dylwil3 on 2024-09-10 04:54_

# Summary

This PR modifies the construction of `ComparableExpr` objects from `Expr` objects in order to ensure that the following equalities hold at the level of comparable AST nodes:

```python
"a" "b" == "ab"
b"a" b"b" == b"ab"
"text" f"{foo} more" "text" f"{bar}" == f"text{foo} moretext{bar}"
```

- [x] `ExprStringLiteral`
- [x] `ExprBytesLiteral`
- [x] `ExprFString`

Closes #13291

<details>
    <summary>Details</summary>

- `ExprStringLiteral`: The comparable variant simply stores the concatenated string value using the `to_str` method
- `ExprBytesLiteral`: The comparable variant stores a `Cow<'ast, [u8]>` which is owned for concatenated bytes expressions and borrowed otherwise.
- `ExprFString`: The comparable variant stores the vector of `FStringElement`'s obtained by joining together contiguous sequences of `StringLiteral` and `FString` elements. So, for example

```python
"text" f"{foo} more" "text" f"{bar}"
```
would be interpreted as the vector with components`"text"`, `f"{foo}"`, `" moretext"`, and `f"{bar}"`.
</details>

# Test Plan

- [x] Unit tests with hand-crafted AST nodes
- [x] Integration tests with parsed source code

---

_Renamed from "[internal] `ComparableExpr` (f)strings and bytes made invariant under concatenation" to "[internal] [WIP] `ComparableExpr` (f)strings and bytes made invariant under concatenation" by @dylwil3 on 2024-09-10 04:55_

---

_Comment by @github-actions[bot] on 2024-09-11 04:12_

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

_Renamed from "[internal] [WIP] `ComparableExpr` (f)strings and bytes made invariant under concatenation" to "[internal] `ComparableExpr` (f)strings and bytes made invariant under concatenation" by @dylwil3 on 2024-09-24 16:58_

---

_Marked ready for review by @dylwil3 on 2024-09-24 17:09_

---

_Review requested from @MichaReiser by @zanieb on 2024-09-24 19:57_

---

_Review requested from @dhruvmanila by @zanieb on 2024-09-24 19:57_

---

_Assigned to @MichaReiser by @zanieb on 2024-09-24 19:57_

---

_Label `internal` added by @zanieb on 2024-09-24 19:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:695 on 2024-09-25 07:44_

Nit: We can simplify this a bit
```suggestion
                ast::FStringPart::Literal(string_literal) => {
                    literal_accumulator = Some(match literal_accumulator.take() {
                        None => Cow::Borrowed(string_literal),
                        Some(existing_literal) => {
                            let mut s = existing_literal.into_owned();
                            s.push_str(string_literal);
                            s.into()
                        }
                    });
                },
                ast::FStringPart::FString(fstring) => {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:662 on 2024-09-25 07:49_

Nit: This can be simplified to
```suggestion
                        Some(existing_literal) => existing_literal.to_mut().push_str(&literal.value),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:602 on 2024-09-25 08:05_


```suggestion
    elements: Box<[ComparableFStringElement<'a>]>,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:637 on 2024-09-25 08:19_

Nit: there's some repetitive code in this implementation that we can avoid by introducing a `Collector` new-type wrapper that provides the necessary utility methods:

```rust
                #[derive(Default)]
        struct Collector<'a> {
            elements: Vec<ComparableFStringElement<'a>>,
        }

        impl<'a> Collector<'a> {
            fn push_literal(&mut self, literal: &'a str) {
                if let Some(ComparableFStringElement::Literal(existing_literal)) =
                    self.elements.last_mut()
                {
                    existing_literal.to_mut().push_str(literal);
                } else {
                    self.elements
                        .push(ComparableFStringElement::Literal(literal.into()));
                }
            }

            fn push_expression(&mut self, expression: &'a ast::FStringExpressionElement) {
                self.elements.push(expression.into());
            }
        }

        let mut collector = Collector::default();

        for part in value {
            match part {
                ast::FStringPart::Literal(string_literal) => {
                    collector.push_literal(&*string_literal.value);
                }
                ast::FStringPart::FString(fstring) => {
                    // Note: we do _not_ clear the literal buffer here
                    // since we may encounter literals amongst the
                    // f-string elements.
                    for element in &fstring.elements {
                        match element {
                            ast::FStringElement::Literal(literal) => {
                                collector.push_literal(&*literal.value);
                            }
                            ast::FStringElement::Expression(expression) => {
                                collector.push_expression(expression)
                            }
                        }
                    }
                }
            }
        }

        Self {
            elements: collector.elements.into_boxed_slice(),
        }

```

This does require splitting the `From` implementation for `FStringElement`

```rust
impl<'a> From<&'a ast::FStringElement> for ComparableFStringElement<'a> {
    fn from(fstring_element: &'a ast::FStringElement) -> Self {
        match fstring_element {
            ast::FStringElement::Literal(ast::FStringLiteralElement { value, .. }) => {
                Self::Literal(value.as_ref().into())
            }
            ast::FStringElement::Expression(formatted_value) => formatted_value.into(),
        }
    }
}

impl<'a> From<&'a ast::FStringExpressionElement> for ComparableFStringElement<'a> {
    fn from(fstring_expression_element: &'a ast::FStringExpressionElement) -> Self {
        let ast::FStringExpressionElement {
            expression,
            debug_text,
            conversion,
            format_spec,
            range: _,
        } = fstring_expression_element;

        Self::FStringExpressionElement(FStringExpressionElement {
            expression: (expression).into(),
            debug_text: debug_text.as_ref(),
            conversion: *conversion,
            format_spec: format_spec
                .as_ref()
                .map(|spec| spec.elements.iter().map(Into::into).collect()),
        })
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:629 on 2024-09-25 08:22_

The word `stack` here is a bit confusing because the implementation doesn't make use of a stack. It uses a single "slot" storing the last string literal. 

I think I'm good with the above comment because the most important informatin that I wasn't aware of is that adjacent string literals need to be concatenated.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:1710 on 2024-09-25 08:28_

Thanks for taking the time to write all those tests. 

I don't have a great suggestion but I wonder if we should make them integration tests instead, because they're very verbose and are difficult to read. E.g. there's so much boilerplate that I find it difficult to pick up the important bits. I also assume that they were very annoying to write. What do you think?

---

_@MichaReiser requested changes on 2024-09-25 08:28_

This is great and nice work on f-strings. I've a few suggestions on how we can reduce the code for the f-string transformation and I think it would be great if we can reduce the test-boilerplate to improve readability.

---

_@dylwil3 reviewed on 2024-09-25 14:28_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:1710 on 2024-09-25 14:28_

The integration tests have the same content so I elected to remove these verbose unit tests entirely. They were helpful for development (and for my own understanding of the internals of f-string nodes), but keeping them around is probably more trouble than they're worth!

---

_@dylwil3 reviewed on 2024-09-25 14:28_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:629 on 2024-09-25 14:28_

Reworded a bit.

---

_@dylwil3 reviewed on 2024-09-25 14:28_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:637 on 2024-09-25 14:28_

Very elegant! Done!

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:602 on 2024-09-25 14:28_

Done.

---

_@dylwil3 reviewed on 2024-09-25 14:28_

---

_@dylwil3 reviewed on 2024-09-25 14:29_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:662 on 2024-09-25 14:29_

Done (but now inside `Collector`)

---

_@dylwil3 reviewed on 2024-09-25 14:29_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:695 on 2024-09-25 14:29_

Done, but superseded by `Collector`.

---

_@MichaReiser approved on 2024-09-25 14:58_

This is great! Thank you!

---

_Merged by @MichaReiser on 2024-09-25 14:58_

---

_Closed by @MichaReiser on 2024-09-25 14:58_

---

_Branch deleted on 2024-09-25 15:07_

---
