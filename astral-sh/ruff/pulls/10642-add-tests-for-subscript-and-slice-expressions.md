```yaml
number: 10642
title: Add tests for subscript and slice expressions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-subscript-slice
created_at: 2024-03-28T04:14:40Z
updated_at: 2024-03-29T08:54:56Z
url: https://github.com/astral-sh/ruff/pull/10642
synced_at: 2026-01-10T22:47:02Z
```

# Add tests for subscript and slice expressions

---

_Pull request opened by @dhruvmanila on 2024-03-28 04:14_

## Summary

This PR adds test cases for subscript and slice expressions. Slices can only exists inside a subscript expressions.


---

_Label `parser` added by @dhruvmanila on 2024-03-28 04:14_

---

_Label `testing` added by @dhruvmanila on 2024-03-28 04:14_

---

_Marked ready for review by @dhruvmanila on 2024-03-28 07:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-28 07:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:736 on 2024-03-28 08:42_

I think it could make sense to copy in the slice rule here because it's unclear to me to which rule you're referring here.

It' something that I've found useful when reading TypeScript's parser code: 
* https://github.com/microsoft/TypeScript/blob/02bb3108ad625cddcec228846b9ff56cc5398976/src/compiler/parser.ts#L4977-L4986
* https://github.com/microsoft/TypeScript/blob/02bb3108ad625cddcec228846b9ff56cc5398976/src/compiler/parser.ts#L5078-L5082

They also often give examples:

* https://github.com/microsoft/TypeScript/blob/02bb3108ad625cddcec228846b9ff56cc5398976/src/compiler/parser.ts#L5186-L5191



---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:773 on 2024-03-28 08:42_

Longterm, I think we want to move this checks into `checker`. Once we have something like that ;)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__subscript__invalid_slice_element.py.snap`:258 on 2024-03-28 08:46_

Do we only parse out a named expression if we're at `:=` but not when we're at `:`? I assume that is to avoid ambiguity when parsing assignment parsing `a: str`, meaning it is unlikely that we want to use namede expression parsing for error recovery if we find a single `identifier:` in an expression (without the `=`)

---

_@MichaReiser reviewed on 2024-03-28 08:47_

Looks good. I would like to understand that starred expression as tuple parsing better before merging. 

---

_@dhruvmanila reviewed on 2024-03-28 09:18_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:736 on 2024-03-28 09:18_

I see. Thanks for the reference. My only worry with this is that the grammar might get out of sync but it's probably way down the line. I'll add them.

---

_@dhruvmanila reviewed on 2024-03-28 09:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__subscript__invalid_slice_element.py.snap`:258 on 2024-03-28 09:26_

> Do we only parse out a named expression if we're at `:=` but not when we're at `:`?

This is specific to slices. To understand this, we need to look at the `slice` grammar:

```
slice:
    | [expression] ':' [expression] [':' [expression] ] 
    | named_expression 
```

Here, `expression` corresponds to `parse_conditional_expression_or_higher` and `named_expression` corresponds to `parse_named_expression_or_higher` method in our parser.

This basically says that a named expression is only valid if it's a standalone and not followed by a `:` which then limits the precedence.

Does this help? If not, I can expand further

---

_Comment by @dhruvmanila on 2024-03-28 09:31_

> Looks good. I would like to understand that starred expression as tuple parsing better before merging.

Yes, for that let's look at the slices grammar:

```
slices:
    | slice !',' 
    | ','.(slice | starred_expression)+ [','] 

slice:
    | [expression] ':' [expression] [':' [expression] ] 
    | named_expression 
```

Here, an element can either be a single slice element which cannot contain starred expression, but the second part of the `slices` grammar says that it actually can be a starred expression. The grammar syntax is custom and the following helps in understanding that:

```py
# s.e+
#   Match one or more occurrences of e, separated by s. The generated parse tree
#   does not include the separator. This is otherwise identical to (e (s e)*).
```

And the CPython parser makes them a tuple if it matches the `starred_expression`: https://github.com/python/cpython/blob/6c8ac8a32fd6de1960526561c44bc5603fab0f3e/Grammar/python.gram#L831-L833

---

_Comment by @MichaReiser on 2024-03-28 10:10_

> The grammar syntax is custom and the following helps in understanding that:

That indeed is helpful. I misread it as it must be a sequence of a `,` followed by one or more `slice` or `starred_expression` elements, followed by an optional trailing comma. But what i understand now is that this is: It's a `slice` or `starred_expression`. It then also makes sense that the specification is explicit that the `slice` syntax has higher priority over any expression syntax because the grammar is ambiguous. 

---

_@MichaReiser approved on 2024-03-28 10:10_

---

_@dhruvmanila reviewed on 2024-03-29 08:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:736 on 2024-03-29 08:40_

I'm thinking of adding this in a future PR clubbed with other document / comment improvements. This way it can be reviewed better.

---

_Merged by @dhruvmanila on 2024-03-29 08:41_

---

_Closed by @dhruvmanila on 2024-03-29 08:41_

---

_Branch deleted on 2024-03-29 08:41_

---

_Comment by @github-actions[bot] on 2024-03-29 08:54_

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
