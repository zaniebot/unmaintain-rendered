```yaml
number: 17851
title: Implement template strings
type: pull_request
state: merged
author: dylwil3
labels:
  - python314
assignees: []
merged: true
base: main
head: template-strings
created_at: 2025-05-05T11:04:36Z
updated_at: 2025-05-30T21:33:02Z
url: https://github.com/astral-sh/ruff/pull/17851
synced_at: 2026-01-12T15:56:06Z
```

# Implement template strings

---

_@dylwil3_

This PR implements template strings (t-strings) in the parser and formatter for Ruff.

I have attempted to arrange the commits for easy review commit-by-commit. They are labeled by what piece of the code they touch, and I have written extended commit messages for some of them to indicate the important bits.

Please let me know if there's anything that can be made clearer or if I can help make the review process smoother!

---

_Label `python314` added by @dylwil3 on 2025-05-05 11:04_

---

_Comment by @github-actions[bot] on 2025-05-05 11:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-05-06 17:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4492 on 2025-05-06 17:50_

To make the corpus test happy, you need to recurse inside all subnodes of the t-string and store a `todo_type!("Template")` for each sub-node. This is to support a future type-aware linter integration where the linter can query the type of any arbitrary node in the AST and ty will have an answer for it.

---

_@AlexWaygood reviewed on 2025-05-08 19:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:7886 on 2025-05-08 19:36_

```suggestion
                    format_args!("T-strings are not allowed in type expressions"),
```

---

_Comment by @github-actions[bot] on 2025-05-08 20:54_

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

_Marked ready for review by @dylwil3 on 2025-05-14 22:45_

---

_Review requested from @carljm by @dylwil3 on 2025-05-14 22:45_

---

_Review requested from @sharkdp by @dylwil3 on 2025-05-14 22:45_

---

_Review requested from @dcreager by @dylwil3 on 2025-05-14 22:45_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-05-14 22:45_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-05-14 22:45_

---

_Comment by @carljm on 2025-05-15 01:34_

So cool!

I'm assuming the ty bits here are just the minimum to not panic, and we can handle the ty comments I'd have (e.g. some tests, maybe some code sharing between `infer_fstring_expression` and the f-string portion of `infer_tstring_expression`) in a separate ty-focused PR later on. Going to leave review on this one to people who know Ruff.

---

_Review request for @carljm removed by @carljm on 2025-05-15 01:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/ftstring.rs`:9 on 2025-05-15 07:04_

I find this name a bit awkward. Maybe `InterpolatedStringContext`. We should also update the file name and the documentation of this struct

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/ftstring.rs`:27 on 2025-05-15 07:06_

Unrelated to your changes but I think I'd prefer to change the constructor to

```rust
new(flags: TokenFlags, nesting: u32) -> Option<Self>
```

https://github.com/astral-sh/ruff/blob/fa628018b28fa018d7a0e109223186172ef91275/crates/ruff_python_parser/src/lexer.rs#L634-L636

can then call `new` and only call into `lex_fstring` if the return value is `Some`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/ftstring.rs`:44 on 2025-05-15 07:07_

This seems redundant given that we also have `TokenFlags`. I haven't seen the usage bits yet but I'd suggest to:

* Remove `FTStringKind` entirely
* Keep the method but remove the field. The method returns `Kind::FString` if `flags.is_f_string` is true and t-string otherwise

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:515 on 2025-05-15 07:10_

Does CPython distinguish between f-strings and t-strings here. Could we use a single error instead?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:933 on 2025-05-15 07:12_

What's the reason for duplicating all this logic? I understand that there's no real difference between how f-strings and t-strings are lexed. Could we just pass a `InterpolatedStringKind` value instead to parametrize the few places where they need to differ?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:93 on 2025-05-15 07:15_

We should update the documentation here to clarify that this includes t-strings and review all usages because t-string tokens feel like a string, they're not at runtime.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:563 on 2025-05-15 07:20_

Could we reuse the format spec between t and f-strings or what's the motivation for having different nodes. What does Python do? The same question applies to other nodes like `TStringLiteralElement`, `TStringInterpolationElement` etc.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1063 on 2025-05-15 07:20_

Same here. Could we use a single `InterpolatedStringFlagsInner` instead of duplicating all this code

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/expression.rs`:89 on 2025-05-15 07:21_

Update documentation

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/expression.rs`:93 on 2025-05-15 07:22_

I've a strong preference towards `interpolated_string`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:732 on 2025-05-15 07:25_

I think it could be desired to have an `InterpolatedString` enum that abstracts over t and f-strings so that code like this could be reused between. This goes back to my question in the AST crate: What's the motivation for having different nodes (not questioning whether we should have different `FString` and `TString` root nodes but do all nodes have to be different?)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1736 on 2025-05-15 07:28_

What's the reason that we need to buplicate the entire logic here? Can't we share some logic with parsing f-strings, given that they're, as far as I understand, identical?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:832 on 2025-05-15 07:28_

Can we unify the `ElementsKind` instead of duplicating all the code?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:3015 on 2025-05-15 07:29_

Why this change?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:89 on 2025-05-15 07:30_

Same as elsewhere. I don't think we have to duplicate the enum here and instead can use it for both f and t-string (the outer variant is enough to distinguish the two cases and even then, it's not clear to me if that's even required)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:323 on 2025-05-15 07:32_

Same here, What's the need for duplicating the entire parse logic? Can't we share most logic with the f-string handling?

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1447 on 2025-05-15 07:33_

Can't we share some logic with f-string interpolation elemetns?

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1508 on 2025-05-15 07:33_

Also here and probably other methods

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:180 on 2025-05-15 07:35_

What about f-strings inside t-strings?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_tmp_directory.rs`:78 on 2025-05-15 07:36_

We should add a test for all the lint rules that you changed in this PR

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:119 on 2025-05-15 07:37_

Consider adding a `count_fstring_chars` (and rename the existing one to `_expr`) to share the f-string logic instead of repeating it here

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:232 on 2025-05-15 07:38_

I don't think this is necessary for t-strings

The examples also need updating and it would be nice if we could share some of the logic with f-strings

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:300 on 2025-05-15 07:38_


```suggestion
/// Checks for unnecessary escaped quotes in an t-string.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:442 on 2025-05-15 07:39_

more f-string references

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:305 on 2025-05-15 07:39_

Same as in other places. Do we need to copy all the code?

---

_@MichaReiser requested changes on 2025-05-15 07:40_

Thanks for working on this. Exciting to seeing this come together.

I've only reviewed the parser, ast and parts of the linter at this point. Overall, my impression is that we went too far with copy pasting. We should consider more carefully where it is important to have different representations for t- and f-strings and where it is fine to share the same structs or parametrizing functions if they're identical except for e.g. some token kinds. Sharing the same structures should help with reusing more code as well.



---

_Comment by @dylwil3 on 2025-05-15 11:07_

> Overall, my impression is that we went too far with copy pasting

That's fair - I will see what can be done about merging more logic! Thank you!

---

_Comment by @AlexWaygood on 2025-05-15 12:38_

(I'll also leave this mostly to Micha/Dhruv, but please feel free to ping me if there's something specific you want my opinion on!)

---

_Comment by @dhruvmanila on 2025-05-15 15:47_

Looking at Micha's review, I'll wait for the follow-up changes to de-duplicate and then review but I agree that we should parameterize the methods and do minimal changes where f-strings and t-strings are different.

---

_@dylwil3 reviewed on 2025-05-16 19:15_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer/ftstring.rs`:9 on 2025-05-16 19:15_

I don't wanna bikeshed too much on this, so I will likely just apply your suggestion, but two points in favor of the current name:

1. It is pretty unambiguous, even if it's awkward - I don't think I could misinterpret it. On the other hand `InterpolatedString` is not related to any standard Python terminology, except maybe to `Interpolation` which is specific to t-strings and _not_ f-strings
2. It agrees with the terminology used in various places in the CPython parser/lexer (e.g. https://github.com/python/cpython/blob/ac8df4b5892d2e4bd99731e7d87223a35c238f81/Parser/lexer/lexer.c#L41) - but this is not a strong argument since it doesn't appear in, say, the actual grammar reference

anyway - if you still want the name to change I'll do it!

---

_@dylwil3 reviewed on 2025-05-16 19:18_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer.rs`:515 on 2025-05-16 19:18_

We can probably merge these, but the user-facing message in CPython does distinguish between f-strings and t-strings, e.g.:

```pycon
>>> t"{name"
  File "<python-input-0>", line 1
    t"{name"
           ^
SyntaxError: t-string: expecting '}'
>>> f"{name"
  File "<python-input-1>", line 1
    f"{name"
           ^
SyntaxError: f-string: expecting '}'
```

so I'll just have to add a bit of logic to recover the context.


---

_@dylwil3 reviewed on 2025-05-16 19:25_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/nodes.rs`:563 on 2025-05-16 19:25_

CPython has separate nodes for the format specs and only shares the conversion fields between them (see below). It's possible we could share the `?StringLiteralElement` pieces, but I would hesitate to merge interpolation elements with expression elements.

```
fstring_middle:
    | fstring_replacement_field
    | FSTRING_MIDDLE 
fstring_replacement_field:
    | '{' annotated_rhs '='? [fstring_conversion] [fstring_full_format_spec] '}' 
fstring_conversion:
    | "!" NAME 
fstring_full_format_spec:
    | ':' fstring_format_spec* 
fstring_format_spec:
    | FSTRING_MIDDLE 
    | fstring_replacement_field
fstring:
    | FSTRING_START fstring_middle* FSTRING_END 

tstring_format_spec_replacement_field:
    | '{' annotated_rhs '='? [fstring_conversion] [tstring_full_format_spec] '}' 
tstring_format_spec:
    | TSTRING_MIDDLE 
    | tstring_format_spec_replacement_field
tstring_full_format_spec:
    | ':' tstring_format_spec* 
tstring_replacement_field:
    | '{' annotated_rhs '='? [fstring_conversion] [tstring_full_format_spec] '}' 
tstring_middle:
    | tstring_replacement_field
    | TSTRING_MIDDLE 
tstring:
    | TSTRING_START tstring_middle* TSTRING_END 
```

---

_Comment by @dylwil3 on 2025-05-16 19:30_

> What's the motivation for having different nodes (not questioning whether we should have different FString and TString root nodes but do all nodes have to be different?)

Just to respond in general to the code duplication (I will still work on merging logic):

My main motivation for keeping so many things separate (e.g. the AST nodes) was to mimic to some extent what is being done in the CPython implementation. Granted, much of their parser is generated, so it's possible they are more comfortable with repetition than we ought to be, but it seemed like, down the line, if changes are introduced such that the behavior of t-strings and f-strings diverges further, this would be easier to maintain. However, I understand if this is too cautious/"YAGNI"-thinking. Anyway, just wanted to put on the record that I wasn't _just_ being lazy for laziness sake 😄 

---

_Comment by @dylwil3 on 2025-05-16 21:35_

Two quick questions as I continue to merge various things:

1. Is there a standard pattern in Rust to get around the fact that we don't have dependent types? There are several places where merging functions would be possible except that the return types are different. So like, we have a function of signature `(data) -> TStringThing` and a function of signature `(data) -> FStringThing`. I want instead to do `(data, enum variant?) -> ?? // FStringThing/TStringThing depending on enum variant`? I guess am I supposed to do `(data) -> T` and parameterize by `T` instead where `T` is allowed to either be `FStringThing` or `TStringThing`? Hopefully this question makes sense...
2. When I made both `FStringElement` and `TStringElement` share a node type (`FTStringLiteralElement`) (I know you hate the name!), this broke the code generation. Is there some other way I'm supposed to do this in `ast.toml` or should I modify the script? (Maybe a question for @dcreager ?)

---

_Comment by @MichaReiser on 2025-05-17 14:29_

For 1. Hard to say without seeing a concrete example. The alternative is to use a trait with an associated type:

```
trait InterpolatedString {
	type Element;
}

fn with_string<S>(string: S) -> S::Element where S: InterpolatedString {}
```

> My main motivation for keeping so many things separate (e.g. the AST nodes) was to mimic to some extent what is being done in the CPython implementation. 

I'm fine not sharing some AST nodes, e.g., if they're handled differently by most visitors anyway. I mainly wanted to make sure it was an intentional decision (maybe it was?).


---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:170 on 2025-05-20 17:45_

I think we can avoid the match expression by either adding a `TokenKind::is_ft_string_end` or implementing `.as_token` method on `FTStringKind`. I'd probably just do the former.

```rs
        if let Some(ftstring) = self.ftstrings.current() {
            if !ftstring.is_in_expression(self.nesting) {
                if let Some(token) = self.lex_ftstring_middle_or_end() {
                    if token.is_ft_string_end() {
                        self.ftstrings.pop();
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:515 on 2025-05-20 17:48_

I think we could add a `LexicalErrorType::from_ft_string_error` similar to `ParseErrorType::from_ft_string_error` for this

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:647 on 2025-05-20 17:49_

nit: should we have a `is_ft_string` method instead? Or, is it that the separation is required somewhere else?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:807 on 2025-05-20 17:52_

We could add methods like `.end_token`, `.start_token`, etc. to convert `FTStringKind` to `TokenKind`

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:846 on 2025-05-20 17:53_

`LexicalErrorType::from_ft_string_error`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:868 on 2025-05-20 17:53_

`LexicalErrorType::from_ft_string_error`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:947 on 2025-05-20 17:54_

`FTStringKind::as_middle_token` ?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1458 on 2025-05-20 17:54_

nit: can we combine these two methods?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:2475 on 2025-05-20 17:56_

Do t-strings have any behavior that's distinct from an f-string for which we should add tests here?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer/ftstring.rs`:24 on 2025-05-20 17:58_

I think we should keep this assertion and expand the method to either be `is_ft_string` or add a new `is_t_string` method. This would mean the `panic` in `FTStringContext::kind` can be `unreachable` instead.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:669 on 2025-05-20 18:10_

Same question as before regarding whether we need to add tests specific to t-string if there's any exclusive behavior that's not present for f-string.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1431 on 2025-05-20 18:11_

"f/t-string"

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1468 on 2025-05-20 18:12_

These aren't "pep701" ;)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:480 on 2025-05-20 18:13_

nit: `start_token`, `middle_token`, `end_token`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1609 on 2025-05-20 18:13_

"f/t-string"

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:1278 on 2025-05-20 18:17_

The standalone "f" looks a bit odd, maybe "f/t-string" or "f-string or t-string"?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:1318 on 2025-05-20 18:18_

nit: what about "FT_STRING_ELEMENTS" similar to other places?

---

_@dhruvmanila reviewed on 2025-05-20 18:22_

Thank you for doing this! This is very large change, taking a break from reviewing but wanted to post the comments that I had so far which are mainly around the parser. Taking a lunch break :)

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:732 on 2025-05-20 21:36_

Turns out that, in this case, I had to do something pretty different than in the f-string case.

---

_@dylwil3 reviewed on 2025-05-20 21:36_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer.rs`:647 on 2025-05-20 21:47_

Yes I think we need these two flags to be separate once we get to the parser (but we could have a method that gives the union). That's currently the only way we have of telling we're in an f-string vs a t-string when we descend into the node.

---

_@dylwil3 reviewed on 2025-05-20 21:47_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer.rs`:2475 on 2025-05-20 21:52_

Not during lexing, I don't think!

---

_@dylwil3 reviewed on 2025-05-20 21:52_

---

_@dylwil3 reviewed on 2025-05-20 22:01_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/string.rs`:669 on 2025-05-20 22:01_

Yep! These are the innocuous looking new tests `test_parse_f_t_string_concat_{i}`

---

_@dylwil3 reviewed on 2025-05-20 22:42_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/expression.rs`:1468 on 2025-05-20 22:42_

It's sort of "retroactively" pep701 in the sense that t-strings are meant to have the same flexibility as f-strings post-pep701. not sure what else to call it!

---

_@dylwil3 reviewed on 2025-05-20 22:45_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer/ftstring.rs`:24 on 2025-05-20 22:45_

I went with Micha's suggestion of having the constructor return an `Option` - but that still let me change the `panic` to an `unreachable`

---

_@dylwil3 reviewed on 2025-05-20 22:50_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer.rs`:515 on 2025-05-20 22:50_

merged these, thanks!

---

_@dylwil3 reviewed on 2025-05-20 22:51_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer.rs`:933 on 2025-05-20 22:51_

merged

---

_@dylwil3 reviewed on 2025-05-20 22:51_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/nodes.rs`:563 on 2025-05-20 22:51_

merged now, thank you!

---

_@dylwil3 reviewed on 2025-05-20 22:51_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/expression.rs`:1736 on 2025-05-20 22:51_

merged!

---

_@MichaReiser reviewed on 2025-05-22 06:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/ftstring.rs`:9 on 2025-05-22 06:21_

I agree that there's not much precedence for the term interpolated string. The terms interpolated or string interpolation are used in both the f-string and t-string PEP:

> Many languages that allow similar syntactic constructs (normally called “string interpolation”) allow quote reuse and arbitrary nesting. These languages include JavaScript, Ruby, C#, Bash, Swift and many others. The fact that many languages allow quote reuse can be a compelling argument in favour of allowing it in Python. This is because it will make the language more familiar to users coming from other languages.
> https://peps.python.org/pep-0701/#considerations-regarding-quote-reuse

> Python f-strings are easy to use and very popular. Over time, however, developers have encountered limitations that make them [unsuitable for certain use cases](https://docs.djangoproject.com/en/5.1/ref/utils/#django.utils.html.format_html). In particular, f-strings provide no way to intercept and transform interpolated values before they are combined into a final string.
> https://peps.python.org/pep-0750/#motivation

An other alternative would be `FormatString` where both `f` and `t` strings are format strings. I think it could be somewhat confusing because it is so close to template strings. 

A third option is `FOrTStringContext`. It also looks very ugly but I find it more accurate because there's no such thing as a `f` and `t` string. 

I think I still prefer `InterpolatedString` the most because it hast he advantage that it neither implies `f` or `t` string -- allowing us to define the term. But I'm fine with any of the other variation (I'm also okay with `FT` but it would hurt me a little ;))

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/generated.rs`:2810 on 2025-05-22 06:23_

Should we rename this to `InterpolationElement` (with or without the `FT` prefix) because that's the terminology used throughout PEP750

---

_@MichaReiser reviewed on 2025-05-22 06:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:560 on 2025-05-22 06:24_

Do we still need the `TStringFormatSpec` or could we use the `FTStringFormatSpec` here?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:934 on 2025-05-22 06:25_

Let's document this type. Specifically, why it is needed given that both `FStringFlags` and `TStringFlags` wrap a `FTStringFlagsInner`.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast_integration_tests/tests/comparable.rs`:5 on 2025-05-22 06:27_


```suggestion
#[track_caller]
fn assert_comparable(left: &str, right: &str) -> Result<(), ParseError> {
```

Adding `track_caller` has the benefit that an assertion failure will show the call site in the panic message instead of the line in `assert_comparble` (which is the same for all tests)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast_integration_tests/tests/comparable.rs`:17 on 2025-05-22 06:27_


```suggestion
#[track_caller]
fn assert_noncomparable(left: &str, right: &str) -> Result<(), ParseError> {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1443 on 2025-05-22 06:32_

What you have here is probably fine. An alternative would have been that `parse_ftstring` returns a struct with `elements`, `flags`, and `range` and the call site either wraps it in a `FString` or `TString`. That would allow you to pass a simple `enum` as argument that parametrizes the token kinds.

Looking more at the `InterpolatedString`. I think what we have here is complicating things unnecessarily. I looked at the call sites and all call-sites immediately wrap the return value in a `StringType::FString` or `StringType::TString`. So I think we can simply change the return type to `StringType` and then use an enum to parametrize whether its parsing an f or t-string.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:823 on 2025-05-22 06:34_

Do we allow both here because we know/assume that the lexer never emits an `FStringEnd` in a `TString` or could recover on both lead to confusion in the parser? 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:634 on 2025-05-22 06:37_

Let's update the comment on line 614

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:768 on 2025-05-22 06:42_

Maybe (to avoid the expect call)

```patch
Subject: [PATCH] nit
---
Index: crates/ruff_python_parser/src/lexer.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_parser/src/lexer.rs b/crates/ruff_python_parser/src/lexer.rs
--- a/crates/ruff_python_parser/src/lexer.rs	(revision 0ccfe03d9a0c0802908f31915aaa049b538f6fd3)
+++ b/crates/ruff_python_parser/src/lexer.rs	(date 1747896114842)
@@ -628,8 +628,8 @@
         };
 
         if let Some(quote) = quote {
-            if self.current_flags.is_ft_string() {
-                return self.lex_ftstring_start(quote);
+            if let Some(kind) = self.try_lex_ftstring_start(quote) {
+                return kind;
             }
 
             return self.lex_string(quote);
@@ -749,8 +749,8 @@
         true
     }
 
-    /// Lex a f-string or t-string start token.
-    fn lex_ftstring_start(&mut self, quote: char) -> TokenKind {
+    /// Lex a f-string or t-string start token if positioned at the start of an f or t-string.
+    fn try_lex_ftstring_start(&mut self, quote: char) -> Option<TokenKind> {
         #[cfg(debug_assertions)]
         debug_assert_eq!(self.cursor.previous(), quote);
 
@@ -762,14 +762,13 @@
             self.current_flags |= TokenFlags::TRIPLE_QUOTED_STRING;
         }
 
-        let ftcontext = FTStringContext::new(self.current_flags, self.nesting)
-            .expect("Expect to have f or t-string flag set");
+        let ftcontext = FTStringContext::new(self.current_flags, self.nesting)?;
 
         let kind = ftcontext.kind();
 
         self.ftstrings.push(ftcontext);
 
-        kind.start_token()
+        Some(kind.start_token())
     }
 
     /// Lex a f-string middle or end token.

```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:517 on 2025-05-22 06:47_

Nit: I would move this closer to where it is used to limit its visibility further. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/fixtures.rs`:43 on 2025-05-22 06:49_

Maybe add a `latest_preview` or similar or we'll forget updating it

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:346 on 2025-05-22 06:51_

Nit: Can we move this next to the `AnyNodeRef::FString` branch above. It makes it easier to remember that we should update both if we're touching one.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_t_string.rs`:31 on 2025-05-22 06:53_

TODO: Update comments

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_t_string.rs`:60 on 2025-05-22 06:53_

Can we reuse this logic between f- and t-strings by having a method that takes `t_string.elements.expressions` as argument?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_t_string_format_spec.rs`:9 on 2025-05-22 06:56_

Is this used anywhere? The `verbatim_text` looks like something's missing here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_t_string_interpolated_element.rs`:9 on 2025-05-22 06:57_

Same here. Is this used anywhere? I think this file (along with the other new `FTString` formating files shouldn't exist because they're listed here https://github.com/astral-sh/ruff/blob/27560b116ac11cdd52f3820b26f73a50f83b3399/crates/ruff_python_formatter/generate.py#L41-L43)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_t_string_literal_element.rs`:9 on 2025-05-22 06:57_

Delete

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/t_string.rs`:8 on 2025-05-22 06:57_


```suggestion
/// Formats a t-string which is part of a larger t-string expression.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:868 on 2025-05-22 07:01_

This is one of the most complicated formatting code. We shoudl avoid introducing unnecessary complexity by duplicating code at all cost. 

I think a very easy solution here is to introduce an `FTString` enum that has a `FString` and ` TString` variant. Then use that in all places where we used `FString` before.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/implicit.rs`:176 on 2025-05-22 07:03_

Can't we just early return here instead of having a mutable state variable?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/implicit.rs`:353 on 2025-05-22 07:04_

The code duplication here seems unnecessary (and it uses `f-string` instead of `t-string`). Maybe another place where an `FTString` enum could help.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:139 on 2025-05-22 07:05_

Same here. I rather not duplicate this code.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:55 on 2025-05-22 07:07_

Is this just a rename or did it change the logic? I think I prefered the way it was written before (less nesting)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:434 on 2025-05-22 07:08_

Can we remove this duplication?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:958 on 2025-05-22 07:09_

Not as important, but we may also be able to remove the duplication here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:1103 on 2025-05-22 07:10_

Do we need to duplicate this method, if yes, move it next to `is_fstring_with_quoted_format_spec_and_debug`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:335 on 2025-05-22 07:13_

This seems like a great way to verify your implementation but I've concerns about maintaining this long-term. Maybe we should just remove it before landing your PR.

I don't think we should run this on all test files. It unnecessarily increases the test wall time where most tests don't even use f-strings. 

I'm open to keeping this for a handful of tests but I'm more inclined to simply duplicate some f-string tests and update them to use t-strings. 

This will also be hard to maintain if we ever decide to diverge formatting between f- and t-strings. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:180 on 2025-05-22 07:16_

Should we resolve this by adding a TODO for now?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_tmp_directory.rs`:79 on 2025-05-22 07:16_

Same here, what about string literals inside t-strings? Maybe add a todo for now

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_bind_all_interfaces.rs`:74 on 2025-05-22 07:17_

Let's add a TODO for now. I think this rule should maybe trigger on template strings as well.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:97 on 2025-05-22 07:19_

Unrelated to your PR: Lol, wtf is this? Why the byte length of an expression. This isn't visible.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:106 on 2025-05-22 07:20_

Let's maybe add a TODO and remove t-string hadnling for now. We need to be careful here if the templates inferred type is different based on the interpolations. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/unnecessary_escaped_quote.rs`:151 on 2025-05-22 07:22_

Similar to the formatter: It seems annoying that this is necessary. I think it would be valuable to have an enum over `FString` and `TString` that gives access to the common elements. This could then be simplified to converting the expression into this `InterpolationStringRef` enum, and passing it as an argument

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:106 on 2025-05-22 07:23_

Is the code duplication here necessary?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:236 on 2025-05-22 07:23_

Can we avoid the code duplication here?

---

_@MichaReiser requested changes on 2025-05-22 07:24_

Thanks for removing a lot of the code duplication. The parser and AST changes look great. 

There's still a lot of duplication in the formatter and dead code that I like to remove before landing this PR because they are in areas that are already extremelly complicated and I don't want to further increase the complexity.

---

_@dylwil3 reviewed on 2025-05-22 17:11_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/nodes.rs`:560 on 2025-05-22 17:11_

removed

---

_@dylwil3 reviewed on 2025-05-22 17:14_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/mod.rs`:823 on 2025-05-22 17:14_

>  because we know/assume that the lexer never emits an `FStringEnd` in a `TString`

That was my assumption, yes

---

_@dylwil3 reviewed on 2025-05-22 17:14_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/string.rs`:517 on 2025-05-22 17:14_

this is now removed completely

---

_@dylwil3 reviewed on 2025-05-22 17:25_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/other/f_t_string.rs`:60 on 2025-05-22 17:25_

replaced methods with single `from_ft_string_elements`

---

_@dylwil3 reviewed on 2025-05-22 17:35_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:868 on 2025-05-22 17:35_

This was what I originally attempted to do, but did not find it to be a "very easy solution" - probably I was doing something wrong. In order to replace instances of `FString` and `TString` with this enum, I would have to implement the various traits used throughout this function (e.g. `.format`). I started doing that and the amount of boilerplate sort of exploded... moreover, some of the traits were actually not possible to duplicate because certain types were required to be like `AnyNodeRef`'s etc, so I couldn't have them as my "virtual" AST node enum.

In the end I extracted the duplicated code into the two ugly helper methods you see in these branches (`format_ft_string_right_left_helper` and `format_ft_string_left_right_helper`). In particular, there is not really much duplicated code (or at least a lot less than before).

Let me know if you have ideas for how to further reduce the complexity or duplication!

---

_@dylwil3 reviewed on 2025-05-22 17:37_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/string/implicit.rs`:176 on 2025-05-22 17:37_

🤦 

---

_@dylwil3 reviewed on 2025-05-22 17:38_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/string/normalize.rs`:55 on 2025-05-22 17:38_

holdover from when I temporarily had 3 variants in that enum - changed back

---

_@dylwil3 reviewed on 2025-05-22 17:42_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/fixtures.rs`:335 on 2025-05-22 17:42_

yep I agree - I'll delete it right before landing!

---

_@MichaReiser reviewed on 2025-05-22 17:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:868 on 2025-05-22 17:58_

Something like this

```patch
Index: crates/ruff_python_formatter/src/statement/stmt_assign.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/statement/stmt_assign.rs b/crates/ruff_python_formatter/src/statement/stmt_assign.rs
--- a/crates/ruff_python_formatter/src/statement/stmt_assign.rs	(revision 0ccfe03d9a0c0802908f31915aaa049b538f6fd3)
+++ b/crates/ruff_python_formatter/src/statement/stmt_assign.rs	(date 1747936612596)
@@ -1,7 +1,7 @@
 use ruff_formatter::{FormatError, GroupId, RemoveSoftLinesBuffer, format_args, write};
 use ruff_python_ast::{
-    AnyNodeRef, Expr, ExprAttribute, ExprCall, FString, Operator, StmtAssign, StringLike, TString,
-    TypeParams,
+    AnyNodeRef, Expr, ExprAttribute, ExprCall, ExprFString, ExprTString, FString, Operator,
+    StmtAssign, StringLike, TString, TypeParams,
 };
 
 use crate::builders::parenthesize_if_expands;
@@ -18,6 +18,7 @@
     maybe_parenthesize_expression,
 };
 use crate::other::f_t_string::FTStringLayout;
+use crate::other::f_t_string_element::FormatFTStringElement;
 use crate::statement::trailing_semicolon;
 use crate::string::StringLikeExtensions;
 use crate::string::implicit::{
@@ -291,18 +292,18 @@
                 let can_inline_comment = should_inline_comments(value, *statement, f.context());
 
                 let string_like = StringLike::try_from(*value).ok();
-                let format_f_string =
-                    string_like.and_then(|string| format_f_string_assignment(string, f.context()));
-                let format_t_string =
-                    string_like.and_then(|string| format_t_string_assignment(string, f.context()));
+                let format_ft_string = string_like.and_then(|string| {
+                    format_f_string_assignment(string, f.context())
+                        .or_else(|| format_t_string_assignment(string, f.context()))
+                });
+
                 let format_implicit_flat = string_like.and_then(|string| {
                     FormatImplicitConcatenatedStringFlat::new(string, f.context())
                 });
 
                 if !can_inline_comment
                     && format_implicit_flat.is_none()
-                    && format_f_string.is_none()
-                    && format_t_string.is_none()
+                    && format_ft_string.is_none()
                 {
                     return maybe_parenthesize_expression(
                         value,
@@ -449,20 +450,11 @@
                         best_fitting![single_line, joined_parenthesized, implicit_expanded]
                             .with_mode(BestFittingMode::AllLines)
                             .fmt(f)?;
-                    } else if let Some(format_t_string) = format_t_string {
-                        format_ft_string_left_right_helper(
-                            value,
-                            *statement,
-                            format_t_string,
-                            &inline_comments,
-                            f,
-                            group_id,
-                        )?;
-                    } else if let Some(format_f_string) = format_f_string {
+                    } else if let Some(format_ft_string) = format_ft_string {
                         format_ft_string_left_right_helper(
                             value,
                             *statement,
-                            format_f_string,
+                            format_ft_string,
                             &inline_comments,
                             f,
                             group_id,
@@ -507,10 +499,10 @@
                 let should_inline_comments = should_inline_comments(value, *statement, f.context());
 
                 let string_like = StringLike::try_from(*value).ok();
-                let format_f_string =
-                    string_like.and_then(|string| format_f_string_assignment(string, f.context()));
-                let format_t_string =
-                    string_like.and_then(|string| format_t_string_assignment(string, f.context()));
+                let format_ft_string = string_like.and_then(|string| {
+                    format_f_string_assignment(string, f.context())
+                        .or_else(|| format_t_string_assignment(string, f.context()))
+                });
                 let format_implicit_flat = string_like.and_then(|string| {
                     FormatImplicitConcatenatedStringFlat::new(string, f.context())
                 });
@@ -519,8 +511,7 @@
                 if !should_inline_comments
                     && !should_non_inlineable_use_best_fit(value, *statement, f.context())
                     && format_implicit_flat.is_none()
-                    && format_f_string.is_none()
-                    && format_t_string.is_none()
+                    && format_ft_string.is_none()
                 {
                     return write!(
                         f,
@@ -544,8 +535,7 @@
                 // Don't inline comments for attribute and call expressions for black compatibility
                 let inline_comments = if should_inline_comments
                     || format_implicit_flat.is_some()
-                    || format_f_string.is_some()
-                    || format_t_string.is_some()
+                    || format_ft_string.is_some()
                 {
                     OptionalParenthesesInlinedComments::new(
                         &expression_comments,
@@ -586,8 +576,7 @@
                 // and using the costly `BestFitting` layout if it is already known that only the last variant
                 // can ever fit because the left breaks.
                 if format_implicit_flat.is_none()
-                    && format_f_string.is_none()
-                    && format_t_string.is_none()
+                    && format_ft_string.is_none()
                     && last_target_breaks
                 {
                     return write!(
@@ -615,16 +604,11 @@
                         } else {
                             format_implicit_flat.fmt(f)
                         }
-                    } else if let Some(format_t_string) = format_t_string.as_ref() {
+                    } else if let Some(format_ft_string) = format_ft_string.as_ref() {
                         // Similar to above, remove any soft line breaks emitted by the f-string
                         // formatting.
                         let mut buffer = RemoveSoftLinesBuffer::new(&mut *f);
-                        write!(buffer, [format_t_string.format()])
-                    } else if let Some(format_f_string) = format_f_string.as_ref() {
-                        // Similar to above, remove any soft line breaks emitted by the f-string
-                        // formatting.
-                        let mut buffer = RemoveSoftLinesBuffer::new(&mut *f);
-                        write!(buffer, [format_f_string.format()])
+                        write!(buffer, [format_ft_string])
                     } else {
                         value.format().with_options(Parentheses::Never).fmt(f)
                     }
@@ -865,29 +849,13 @@
                         .with_mode(BestFittingMode::AllLines)
                         .fmt(f)
                     }
-                } else if let Some(format_t_string) = &format_t_string {
+                } else if let Some(format_ft_string) = &format_ft_string {
                     format_ft_string_right_left_helper(
                         value,
                         *statement,
                         *before_operator,
                         *operator,
-                        format_t_string,
-                        &inline_comments,
-                        f,
-                        &format_value,
-                        &last_target,
-                        last_target_breaks,
-                        &single_line,
-                        &split_target_flat_value,
-                        &flat_target_parenthesize_value,
-                    )
-                } else if let Some(format_f_string) = &format_f_string {
-                    format_ft_string_right_left_helper(
-                        value,
-                        *statement,
-                        *before_operator,
-                        *operator,
-                        format_f_string,
+                        *format_ft_string,
                         &inline_comments,
                         f,
                         &format_value,
@@ -910,6 +878,21 @@
     }
 }
 
+#[derive(Debug, Copy, Clone)]
+enum InterpolationString<'a> {
+    FString(&'a FString),
+    TString(&'a TString),
+}
+
+impl Format<PyFormatContext<'_>> for InterpolationString<'_> {
+    fn fmt(&self, f: &mut PyFormatter) -> FormatResult<()> {
+        match self {
+            InterpolationString::FString(string) => string.format().fmt(f),
+            InterpolationString::TString(string) => string.format().fmt(f),
+        }
+    }
+}
+
 /// Formats an f-string that is at the value position of an assignment statement.
 ///
 /// This is just a wrapper around [`FormatFString`] while considering a special case when the
@@ -967,7 +950,7 @@
 fn format_f_string_assignment<'a>(
     string: StringLike<'a>,
     context: &PyFormatContext,
-) -> Option<&'a FString> {
+) -> Option<InterpolationString<'a>> {
     let StringLike::FString(expr) = string else {
         return None;
     };
@@ -987,7 +970,7 @@
         return None;
     }
 
-    Some(f_string)
+    Some(InterpolationString::FString(f_string))
 }
 
 /// Formats a t-string that is at the value position of an assignment statement.
@@ -1047,7 +1030,7 @@
 fn format_t_string_assignment<'a>(
     string: StringLike<'a>,
     context: &PyFormatContext,
-) -> Option<&'a TString> {
+) -> Option<InterpolationString<'a>> {
     let StringLike::TString(expr) = string else {
         return None;
     };
@@ -1067,13 +1050,13 @@
         return None;
     }
 
-    Some(t_string)
+    Some(InterpolationString::TString(t_string))
 }
 
-fn format_ft_string_left_right_helper<'a, T: AsFormat<PyFormatContext<'a>>>(
+fn format_ft_string_left_right_helper<'a>(
     value: &Expr,
     statement: AnyNodeRef<'_>,
-    format_ft_string: &T,
+    format_ft_string: InterpolationString,
     inline_comments: &OptionalParenthesesInlinedComments,
     f: &mut Formatter<PyFormatContext<'a>>,
     group_id: GroupId,
@@ -1083,7 +1066,7 @@
     let ft_string_flat = format_with(|f| {
         let mut buffer = RemoveSoftLinesBuffer::new(&mut *f);
 
-        write!(buffer, [format_ft_string.format()])
+        write!(buffer, [format_ft_string])
     })
     .memoized();
 
@@ -1143,7 +1126,7 @@
     //     expression
     // }moreeeeeeeeeeeeeeeee"
     // ```
-    let format_ft_string = format_with(|f| write!(f, [format_ft_string.format(), inline_comments]));
+    let format_ft_string = format_with(|f| write!(f, [format_ft_string, inline_comments]));
 
     best_fitting![single_line, joined_parenthesized, format_ft_string]
         .with_mode(BestFittingMode::AllLines)
@@ -1152,12 +1135,12 @@
 }
 
 #[expect(clippy::too_many_arguments)]
-fn format_ft_string_right_left_helper<'a, T: AsFormat<PyFormatContext<'a>>>(
+fn format_ft_string_right_left_helper<'a>(
     value: &Expr,
     statement: AnyNodeRef<'_>,
     before_operator: AnyBeforeOperator<'_>,
     operator: AnyAssignmentOperator,
-    format_ft_string: &T,
+    format_ft_string: InterpolationString,
     inline_comments: &OptionalParenthesesInlinedComments,
     f: &mut Formatter<PyFormatContext<'a>>,
     format_value: &Memoized<
@@ -1206,7 +1189,7 @@
     }
 
     let format_ft_string =
-        format_with(|f| write!(f, [format_ft_string.format(), inline_comments])).memoized();
+        format_with(|f| write!(f, [format_ft_string, inline_comments])).memoized();
 
     // Considering the following initial source:
     //
```

`AsFormat` is mostly a workaround for the orphan rules. You can implement `Format` (similar to `Display` and `Debug`) directly on types defined in the same crate and pass them to `write!`. 

I strongly suggest inlining the formatting code again. The types look horrible and it's very unidiomatic that `f` isn't the first argument 😆 

---

_@dylwil3 reviewed on 2025-05-22 18:35_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/string/implicit.rs`:353 on 2025-05-22 18:35_

Here I just matched further down to `TString { elements, ..}` and merged the arms.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/string/mod.rs`:139 on 2025-05-22 18:43_

couldn't quite merge the match arms here because `TStringFlags` and `FStringFlags` differ - but pulled out the helper function above the match so it wasn't repeated.

---

_@dylwil3 reviewed on 2025-05-22 18:43_

---

_@dylwil3 reviewed on 2025-05-22 18:51_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/string/normalize.rs`:1103 on 2025-05-22 18:51_

merged

---

_@dylwil3 reviewed on 2025-05-22 18:53_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/string/normalize.rs`:958 on 2025-05-22 18:53_

couldn't quite merge the match arms because `TStringFlags` and `FStringFlags` are different so I left this one as is

---

_@dhruvmanila reviewed on 2025-05-28 05:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1468 on 2025-05-28 05:32_

We can just call it "pep750" unless I'm missing something obvious here?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_tmp_directory.rs`:79 on 2025-05-29 20:35_

(I'm making all of these TODOs as you suggest, but I still think it's correct - t-strings would trigger a `TypeError` when used inside `open` in the same way that bytes literals would.)

---

_@dylwil3 reviewed on 2025-05-29 20:35_

---

_@dylwil3 reviewed on 2025-05-29 20:58_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/token.rs`:93 on 2025-05-29 20:58_

Traced through - it seems to be called in places where we also want t-strings, as far as I can tell. Docs updated.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_quotes/rules/unnecessary_escaped_quote.rs`:151 on 2025-05-29 21:02_

In this case we can just inline `check_f_string` and `check_t_string` for a similar effect

---

_@dylwil3 reviewed on 2025-05-29 21:02_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:119 on 2025-05-29 21:03_

(the TString arm is not a todo in any event)

---

_@dylwil3 reviewed on 2025-05-29 21:03_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_tmp_directory.rs`:78 on 2025-05-29 21:04_

these are all "todo"s now

---

_@dylwil3 reviewed on 2025-05-29 21:04_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-05-29 21:19_

---

_@dylwil3 reviewed on 2025-05-29 21:20_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/statement.rs`:3015 on 2025-05-29 21:20_

I think it had to do with the change in Python version used for the parser tests? I will investigate...

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:2228 on 2025-05-30 05:59_

This change seems unrelated? Isn't it converting to an f-string?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:1078 on 2025-05-30 06:04_

nit: having both "FTString" and "Interpolated" seems a bit confusing, I'd probably just go with "ExprStringInterpolatedElement" (no "FT" prefix)

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:934 on 2025-05-30 06:16_

I'm not sure whether this TODO belongs here, should this be moved to be near the `Checker`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast_integration_tests/tests/snapshots/visitor__t_strings.snap`:5 on 2025-05-30 06:21_

Unrelated to this PR but it would be useful to include the source text in these snapshots otherwise it's difficult to know which source this snapshot belongs to.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:466 on 2025-05-30 06:27_

I'm not sure why do we need to change the code examples here and in other places where f-strings are used, it might be useful to just keep them as f-strings and just highlight that the logic applies to t-strings as well

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1053 on 2025-05-30 06:29_

I think in the AST we use "Interpolated". I don't have any strong opinions but I'd prefer to be consistent, it makes it easier to grep for related data structure across the code base.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1069 on 2025-05-30 06:29_

Newline is missing before the comment.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1145 on 2025-05-30 06:30_

Assuming that this is the same comment as in the f-string case, let's just provide a reference in the docstring instead of copy-pasting as otherwise it could get outdated.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1201 on 2025-05-30 06:32_

I'd be inclined to just combine this with the f-string function as `format_ft_string_assignment` which would be another way to avoid duplicating docstrings and comments.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:223 on 2025-05-30 06:46_

```suggestion
    // Whether we suggest converting to a raw string or
    // adding backslashes depends on the presence of valid
    // escape characters in the entire f/t-string. Therefore,
    // we must analyze escape characters in each f/t-string
    // element before pushing a diagnostic and fix.
    for element in elements {
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:97 on 2025-05-30 06:50_

What do you mean by "this isn't visible" ? I think there was some discussion around this and we settled on just using the expression range for now.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer/ftstring.rs`:9 on 2025-05-30 06:54_

I don't have a strong preference between the two names, so I'll leave this to you and Micha to decide but if it were upto me I'd use "InterpolatedString" :)

Another data point is that libCST uses "FormattedString" for f-strings

---

_@dhruvmanila approved on 2025-05-30 07:15_

Thank you again for working on this!

This seems pretty close to landing! I've a few comments but they're mostly around avoiding duplication so I'll leave it up to you.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:97 on 2025-05-30 15:36_

It just feels off. I think I'd prefer to use `chars` which seems more accurate.

---

_@MichaReiser reviewed on 2025-05-30 15:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:76 on 2025-05-30 15:38_

I'm a bit concerned about the wild mix of terminology used in this PR. I'd prefer if we sticked to one, even if it is `FT` but we should avoid using different terms for the same thing.

---

_@MichaReiser approved on 2025-05-30 15:43_

Awesome. Thank you for addressign all my feedback. 

My only concern left is that we use different terminology for the same thing in different places. Sometimes we use FT, sometimes we use InterpolatedString. We should make sure to only use one of the two.

---

_@dhruvmanila reviewed on 2025-05-30 16:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:97 on 2025-05-30 16:13_

Ah ok, I see. Yeah, that makes more sense

---

_@dylwil3 reviewed on 2025-05-30 19:36_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/lexer/ftstring.rs`:9 on 2025-05-30 19:36_

Done!

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:76 on 2025-05-30 19:37_

should all be aligned now - interpolated string everywhere

---

_@dylwil3 reviewed on 2025-05-30 19:37_

---

_@dylwil3 reviewed on 2025-05-30 19:41_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:97 on 2025-05-30 19:41_

now at #18395 

---

_@dylwil3 reviewed on 2025-05-30 19:43_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/fixtures.rs`:335 on 2025-05-30 19:43_

deleted - goodbye fun test!

---

_Review comment by @dylwil3 on `crates/ruff_python_ast_integration_tests/tests/snapshots/visitor__t_strings.snap`:5 on 2025-05-30 19:44_

#18394 

---

_@dylwil3 reviewed on 2025-05-30 19:44_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/statement.rs`:3015 on 2025-05-30 19:59_

confirmed! this became a semantic syntax error in python 3.14, see #17283

---

_@dylwil3 reviewed on 2025-05-30 19:59_

---

_Merged by @dylwil3 on 2025-05-30 20:00_

---

_Closed by @dylwil3 on 2025-05-30 20:00_

---

_Comment by @dylwil3 on 2025-05-30 20:01_

Thank you so much @MichaReiser and @dhruvmanila !!

---

_Branch deleted on 2025-05-30 20:01_

---

_Comment by @AlexWaygood on 2025-05-30 21:33_

🥳🥳 Congrats @dylwil3!!

---
