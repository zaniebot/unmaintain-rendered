```yaml
number: 10298
title: Start tracking quoting style in the AST
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: quotestyle-ast
created_at: 2024-03-08T13:28:53Z
updated_at: 2024-03-08T19:11:50Z
url: https://github.com/astral-sh/ruff/pull/10298
synced_at: 2026-01-10T22:47:01Z
```

# Start tracking quoting style in the AST

---

_Pull request opened by @AlexWaygood on 2024-03-08 13:28_

## Summary

This PR modifies our AST so that nodes for string literals, bytes literals and f-strings all retain the following information:
- The quoting style used (double or single quotes)
- Whether the string is triple-quoted or not
- Whether the string is raw or not

This PR is a followup to #10256. Like with that PR, this PR does not, in itself, fix any bugs. However, it means that we will have the necessary information to preserve quoting style and rawness of strings in the `ExprGenerator` in a followup PR, which will allow us to provide a fix for https://github.com/astral-sh/ruff/issues/7799.

The PR should be easiest to review commit-by-commit:
1. The [first commit](https://github.com/astral-sh/ruff/pull/10298/commits/6349648f77b55530047ff76bdfc608f4ab8c58ac) makes the necessary modifications for tracking this information in `ast::StringLiteral` nodes
2. The [second commit](https://github.com/astral-sh/ruff/pull/10298/commits/89c6be270267b6ddc8ec2dfb110fc43c39563d6b) does the same for `ast::BytesLiteral` nodes
3. The [third commit](https://github.com/astral-sh/ruff/pull/10298/commits/3b31804a29b696a9075e3fa3be3a2abd94e9eef9) does the same for `ast::FString` nodes

The information is recorded on the AST nodes using a bitflag field on each node, similarly to how we recorded the information on `Tok::String`, `Tok::FStringStart` and `Tok::FStringMiddle` tokens in #10298. Rather than reusing the bitflag I used for the tokens, however, I decided to create a custom bitflag for each AST node.

Using different bitflags for each node allows us to make invalid states unrepresentable: it is valid to set a `u` prefix on a string literal, but not on a bytes literal or an f-string. It also allows us to have better debug representations for each AST node modified in this PR.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2024-03-08 13:28_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-08 13:28_

---

_Comment by @AlexWaygood on 2024-03-08 13:35_

Performance is [neutral on all benchmarks](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:quotestyle-ast) according to Codspeed.

---

_@AlexWaygood reviewed on 2024-03-08 13:38_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:50 on 2024-03-08 13:38_

I couldn't figure out a way to "unpack" the `FStringStart` node in `python.lalrpop` while keeping it a tuple variant rather than a struct variant. Not sure if it's possible to do that? If it is, I could make the diff slightly smaller for the third commit :)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1240 on 2024-03-08 13:40_

Nit. It might be worth exposing `quote_style` as a method on `FString`. I think that could be useful in other situations too. I see that even exists. 
```suggestion
        f.debug_struct("FStringFlags")
            .field("quote_style", &self.quote_style())
            .field("raw", &self.contains(Self::R_PREFIX))
            .field("triple_quoted", &self.contains(Self::TRIPLE_QUOTED))
            .finish()
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1211 on 2024-03-08 13:41_

What's the reason for implementing `Default` on `FString`. I'm not sure if `Default` on a node makes a lot of sense because ideally you set a `TextRange`. It's also something that no other node has.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1554 on 2024-03-08 13:42_


I recommend expsoing prefix and quote_style as methods and reusing the methods here.

```suggestion
        f.debug_struct("StringLiteralFlags")
            .field("quote_style", &self.quote_style())
            .field("prefix", &self.prefix())
            .field("triple_quoted", &self.contains(Self::TRIPLE_QUOTED))
            .finish()
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1597 on 2024-03-08 13:44_

Nit, 
```suggestion
    pub const fn is_unicode(&self) -> bool { 
```

Potentially without the `string` postfix because that's given by the enclosing type. E.g it's also `is_triple_quoted` and not `is_triple_quoted_string`. E.g let's compare some call-sites: `string_literal.is_unicode_string()` seems repetivie in comparison to `string_literal.is_unicode()`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:4433 on 2024-03-08 13:45_

I think it was very deliberate that the author used destructuring here. It ensures that whoever adds a new field thinks about whether it needs to be visited or not. This way, rust guides you when adding a new field

---

_Comment by @github-actions[bot] on 2024-03-08 13:47_

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

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/stylist.rs`:112 on 2024-03-08 13:47_

Can we replace the `Quote` type with `QuoteStyle`? I would prefer to minimize the number of `Quote`like types we have.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:50 on 2024-03-08 13:49_

This should work (when you change the definition at the end of the `python.lalrpop` file)

```
fstring_start => token::Tok::FStringStart(<StringKind>),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:224 on 2024-03-08 13:57_

Nit: I think I would prefer `Unicode`, it avoids the `string` repetition 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:226 on 2024-03-08 13:57_

Nit: Empty?

---

_@MichaReiser approved on 2024-03-08 14:00_

Nice. It would be nice to use the new string flags in the formatter to avoid parsing them from source. But that's nothing for this PR. 

https://github.com/astral-sh/ruff/blob/72bf1c28805787d0aa842fcf493b4406e0e97d2d/crates/ruff_python_formatter/src/string/mod.rs#L121-L144

I posted a solution that avoids the need to change `FStringStart`. I would prefer if we keep it as is to reduce the diff size. 



---

_@AlexWaygood reviewed on 2024-03-08 14:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:226 on 2024-03-08 14:05_

Sure!

---

_@AlexWaygood reviewed on 2024-03-08 14:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:224 on 2024-03-08 14:06_

I dislike `Unicode`, because _all_ strings in Python are unicode... the `u` prefix does nothing at runtime, it's purely there for Python 2 compatibility. (In fact, it was originally removed in Python 3.0, but they added it back in a later Python 3 version because it was so difficult to write code that worked on both Python 2 and Python 3 if the prefix was gone in Python 3.)

---

_@MichaReiser reviewed on 2024-03-08 14:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:226 on 2024-03-08 14:07_

Or maybe `None`, for the Rustys

---

_@AlexWaygood reviewed on 2024-03-08 14:08_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/node.rs`:4433 on 2024-03-08 14:08_

Thanks, that makes sense.

---

_@AlexWaygood reviewed on 2024-03-08 14:08_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1597 on 2024-03-08 14:08_

Same response as https://github.com/astral-sh/ruff/pull/10298#discussion_r1517761719

---

_@MichaReiser reviewed on 2024-03-08 14:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:224 on 2024-03-08 14:09_

That's fair. But it is the unicode flag ;) Can you do a quick check on what we call it in other places in the code base? It would be nice if it is consistent.

---

_@AlexWaygood reviewed on 2024-03-08 14:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:226 on 2024-03-08 14:26_

> Or maybe `None`, for the Rustys

I think I'm still adjusting to the fact that `None` isn't a keyword in Rust 😆

---

_@AlexWaygood reviewed on 2024-03-08 14:41_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/node.rs`:4433 on 2024-03-08 14:41_

Unfortunately it is hard to go back to the destructuring approach here, because the `flags` field I have added is private (and I'd prefer to keep it that way). It's possible to do this:

```rs
        let ast::FString {elements, range: _, ..} = self;

        for fstring_element in elements {
            visitor.visit_f_string_element(fstring_element);
        }
```

But that's no more type-safe than what I have currently

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:4433 on 2024-03-08 14:43_

I'm a huge fan of encapsulation and would probably have made all fields private if I started working the AST today. But I don't think the field should be private for consistency. All our AST fields are public; I would rather change this holistically or it becomes confusing when using the AST node because you never know if destructuring is possible or not.

---

_@MichaReiser reviewed on 2024-03-08 14:43_

---

_@AlexWaygood reviewed on 2024-03-08 14:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/node.rs`:4433 on 2024-03-08 14:46_

Hmm. Maybe I could use a newtype, like I did with the bitflag for the tokens in https://github.com/astral-sh/ruff/pull/10256, so that the `flag` field is public but its internals are private.

---

_@AlexWaygood reviewed on 2024-03-08 14:57_

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/stylist.rs`:112 on 2024-03-08 14:57_

We could do, but it would require making the `ruff_python_literal` crate have a dependency on the `ruff_python_ast` crate (or vice versa), due to this:

https://github.com/astral-sh/ruff/blob/965adbed4b56a76d6cce02534e47c4d7a2a9cd32/crates/ruff_python_codegen/src/stylist.rs#L123-L130

If we used `ruff_python_ast::str::QuoteStyle` there instead of `ruff_python_codegen::stylist::Quote`, we wouldn't be able to have the implementation of that trait in the `ruff_python_codegen` crate, since the `QuoteStyle` enum comes from the `ruff_python_literal` crate. `ruff_python_literal` does not currently depend on `ruff_python_ast` (nor vice versa).

---

_@AlexWaygood reviewed on 2024-03-08 15:00_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1211 on 2024-03-08 15:00_

It should be easy to remove the `Default` implementations for the nodes if I make the `flags` field public

---

_@AlexWaygood reviewed on 2024-03-08 18:02_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:50 on 2024-03-08 18:02_

That worked, thanks! Could've sworn I tried that, but oh well...

---

_@AlexWaygood reviewed on 2024-03-08 18:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:224 on 2024-03-08 18:58_

I did a grep -- there's a pyupgrade rule called `UnicodeKindPrefix`, but other than that, I can't see _much_ use of the term `Unicode` across the codebase to describe this prefix...

I'd still rather keep this as-is -- I personally would find it really confusing to refer to these strings as "unicode strings", given that _all_ strings are unicode strings, even without the prefix

---

_Merged by @AlexWaygood on 2024-03-08 19:11_

---

_Closed by @AlexWaygood on 2024-03-08 19:11_

---

_Branch deleted on 2024-03-08 19:11_

---
