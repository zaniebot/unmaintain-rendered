```yaml
number: 11120
title: Add support for PEP 696 syntax
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - parser
  - python313
assignees: []
merged: true
base: main
head: pep696
created_at: 2024-04-24T01:16:28Z
updated_at: 2024-09-10T23:38:39Z
url: https://github.com/astral-sh/ruff/pull/11120
synced_at: 2026-01-12T15:55:34Z
```

# Add support for PEP 696 syntax

---

_@JelleZijlstra_

Fixes #11099.

My strategy here was:
- Add new fields to the AST nodes for TypeVar, ParamSpec, and TypeVarTuple
- Fix all the missing pattern matching cases that the compiler found
- Fix tests
- Add parser support for the new syntax and update tests
- Grep for existing code dealing with TypeVar bounds to find other places to edit

---

_Review requested from @MichaReiser by @JelleZijlstra on 2024-04-24 01:16_

---

_Review requested from @dhruvmanila by @JelleZijlstra on 2024-04-24 01:16_

---

_Comment by @JelleZijlstra on 2024-04-24 03:12_

The CI failure looks like a random timeout. (Speculation: Maybe this change triggered too many recompilations and therefore it timed out?)

---

_Comment by @github-actions[bot] on 2024-04-24 03:27_

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

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:3135 on 2024-04-24 05:53_

I think we could keep it `default` for consistency

https://github.com/astral-sh/ruff/blob/f5c7a62aa65decb9e286bd65ba17f1a3bd1f91e6/crates/ruff_python_ast/src/nodes.rs#L3246-L3250

```suggestion
    pub default: Option<Box<Expr>>,
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3185 on 2024-04-24 06:02_

Can you clarify what the grammar is? I'm a bit confused between the two:
1. The reference implementation uses `expression` https://github.com/Gobot1234/cpython/blob/a05c2f31f580bd001dcac34668bb3cbd8d3e0eb0/Grammar/python.gram#L664
2. The PEP specifies two alternatives https://peps.python.org/pep-0696/#grammar-changes:
	```
	type_param_default:
	    | '=' e=expression
	    | '=' e=starred_expression
	```

Although, looking at [Pyright's implementation](https://github.com/microsoft/pyright/blob/f4afe405cdf8d6a95abe67a459e8ea84af4162b5/packages/pyright-internal/src/parser/parser.ts#L574-L582), this does seem correct.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3175 on 2024-04-24 06:04_

Can you add one more test case to check the starred expression precedence (`bitwise_or`)? This could be:

```python
type X[*Ts = *int or str] = int
```



---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/type_param/type_param_type_var_tuple.rs`:19 on 2024-04-24 06:11_

I think this will require a space before the `=` token as well.

```suggestion
        if let Some(default_value) = default_value {
            write!(f, [space(), token("="), space(), default_value.format()])?;
        }
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/type_param/type_param_type_var.rs`:23 on 2024-04-24 06:11_

Similar to other suggestion.

```suggestion
        if let Some(default_value) = default_value {
            write!(f, [space(), token("="), space(), default_value.format()])?;
        }
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/type_param/type_param_param_spec.rs`:19 on 2024-04-24 06:11_

And here,

```suggestion
        if let Some(default_value) = default_value {
            write!(f, [space(), token("="), space(), default_value.format()])?;
        }
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor.rs`:679 on 2024-04-24 06:23_

I might've missed this in the PEP if it's mentioned but I just want to confirm if this is the correct order for this visitor. This should be in the evaluation order. I'm asking because for parameters, defaults are evaluated before the annotations.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor/transformer.rs`:665 on 2024-04-24 06:24_

Same question as for the `Visitor` trait w.r.t. the evaluation-order.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:1207 on 2024-04-24 06:25_

Same suggestion as in the AST node, we should rename this to `default` for consistency.

---

_@dhruvmanila reviewed on 2024-04-24 06:36_

Wow, that was blazingly fast! Thank you for working on this. It is an impressive PR as it touches all of the major areas of the codebase (AST, parser, linter, and formatter).

I've made some suggestions for consistency and have a few doubts regarding the grammar and the evaluation-order. But, otherwise I think this is pretty much good to go.

---

_Label `parser` added by @dhruvmanila on 2024-04-24 06:36_

---

_Label `python313` added by @dhruvmanila on 2024-04-24 06:36_

---

_@MichaReiser reviewed on 2024-04-24 07:19_

Wow nice! This is excellent. 

Would you mind adding a few formatter snapshot tests? For example, you can add them to https://github.com/astral-sh/ruff/blob/0bf0aa28ac05eb4157c8e1932abc91301aaedce7/crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/type_alias.py 

Ideally, with a few cases that involve comments and extra long lines. 

---

_@JelleZijlstra reviewed on 2024-04-24 13:21_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_ast/src/nodes.rs`:3135 on 2024-04-24 13:21_

My reasoning for this name was that it's the name being used in the CPython AST. I picked that name (well, Brandt Bucher suggested it) because `default` is a keyword in C, and AST field names get used as C identifiers in Python. I thought it'd be nice for consistency to reuse the name.

However, `default` isn't a keyword in Rust, so we could also use the shorter name here. Do you think consistency with the Python AST is useful here?

---

_@JelleZijlstra reviewed on 2024-04-24 13:24_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_parser/src/parser/statement.rs`:3185 on 2024-04-24 13:24_

In a sense there isn't a grammar yet, because my implementation hasn't been merged into CPython yet. I posted about it here: https://discuss.python.org/t/pep-696-type-defaults-for-typevarlikes/22569/36

Based on the test cases I added, I think I picked the right flavor of expression here to match what I did in CPython.

---

_@JelleZijlstra reviewed on 2024-04-24 13:30_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_parser/src/parser/statement.rs`:3175 on 2024-04-24 13:30_

Sure. For reference here's what CPython does in my implementation:

```
>>> type X[*Ts = *int or str] = int
  File "<stdin>", line 1
    type X[*Ts = *int or str] = int
                      ^^
SyntaxError: invalid syntax
```

And Ruff produces:
```
+2 | type X[*Ts = *int or str] = int
+  |               ^^^^^^^^^^ Syntax Error: Boolean expression cannot be used here
```


---

_@JelleZijlstra reviewed on 2024-04-24 13:32_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_formatter/src/type_param/type_param_type_var_tuple.rs`:19 on 2024-04-24 13:32_

Agree, done

---

_@JelleZijlstra reviewed on 2024-04-24 13:35_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_ast/src/visitor.rs`:679 on 2024-04-24 13:35_

They are evaluated lazily, similar to bounds, so there isn't really an evaluation order.

---

_@JelleZijlstra reviewed on 2024-04-24 13:35_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_ast/src/visitor/transformer.rs`:665 on 2024-04-24 13:35_

As above

---

_@JelleZijlstra reviewed on 2024-04-24 13:36_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_ast/src/comparable.rs`:1207 on 2024-04-24 13:36_

Mentioned this above, will rename it everywhere if you confirm that you prefer this name.

---

_Comment by @JelleZijlstra on 2024-04-24 13:52_

@MichaReiser thanks, I added a number of tests in https://github.com/astral-sh/ruff/pull/11120/commits/7e053f6c9ce0ca252f34f41eecbd92400b8d101f. A few comments get moved to the other side of a punctuation mark but that seems fine.

It looks like you don't have a similar test case file for type parameter definitions on functions, other than the one in `fixtures/black`.

---

_@dhruvmanila reviewed on 2024-04-25 09:01_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:3135 on 2024-04-25 09:01_

I don't think it's required for the AST structure to be consistent with the Python AST as we've already diverged from it as per the needs of a static analysis tool. So, `default` should be fine here.

---

_@dhruvmanila reviewed on 2024-04-25 09:03_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3185 on 2024-04-25 09:03_

I see, thanks for providing the context. The grammar then matches the implementation.

---

_@dhruvmanila reviewed on 2024-04-25 09:03_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3175 on 2024-04-25 09:03_

Thank you! This is what I expected

---

_@JelleZijlstra reviewed on 2024-04-25 13:42_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_ast/src/nodes.rs`:3135 on 2024-04-25 13:42_

Sure, moved to `default`.

---

_@dhruvmanila approved on 2024-04-25 15:19_

Thank you!

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-25 15:19_

---

_@MichaReiser approved on 2024-04-26 07:47_

Perfect! Thank you!

---

_Merged by @MichaReiser on 2024-04-26 07:47_

---

_Closed by @MichaReiser on 2024-04-26 07:47_

---

_Comment by @charliermarsh on 2024-04-26 13:23_

Thank you @JelleZijlstra, this is awesome.

---

_Branch deleted on 2024-04-26 13:54_

---

_Branch restored on 2024-09-10 23:38_

---
