```yaml
number: 19108
title: "[ty] First cut at semantic token provider"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: semantic_tokens
created_at: 2025-07-02T22:45:03Z
updated_at: 2025-07-07T22:34:52Z
url: https://github.com/astral-sh/ruff/pull/19108
synced_at: 2026-01-12T15:56:32Z
```

# [ty] First cut at semantic token provider

---

_@UnboundVariable_

This PR implements a basic semantic token provider for ty's language server. This allows for more accurate semantic highlighting / coloring within editors that support this LSP functionality.

Here are screen shots that show how code appears in VS Code using the "rainbow" theme both before and after this change.

![461737617-15630625-d4a9-4ec5-9886-77b00eb7a41a](https://github.com/user-attachments/assets/f963b55b-3195-41d1-ba38-ac2e7508d5f5)

![461737624-d6dcf5f0-7b9b-47de-a410-e202c63e2058](https://github.com/user-attachments/assets/111ca2c5-bb4f-4c8a-a0b5-6c1b2b6f246b)

The token types and modifier tags in this implementation largely mirror those used in Microsoft's default language server for Python.

The implementation supports two LSP interfaces. The first provides semantic tokens for an entire document, and the second returns semantic tokens for a requested range within a document.

The PR includes unit tests. It also includes comments that document known limitations and areas for future improvements.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-02 22:45_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-02 22:45_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-02 22:45_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-02 22:45_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-02 22:45_

---

_Comment by @github-actions[bot] on 2025-07-02 22:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-02 22:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/UnboundVariable%3Asemantic_tokens?runnerMode=WallTime)

### Merging #19108 will **not alter performance**

<sub>Comparing <code>UnboundVariable:semantic_tokens</code> (3be32e1) with <code>main</code> (4dd2c03)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Renamed from "First cut at semantic token provider." to "[ty] First cut at semantic token provider" by @UnboundVariable on 2025-07-02 23:08_

---

_Review comment by @carljm on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:18 on 2025-07-03 01:37_

We would often make a function like this a Salsa query, by annotating it with `#[salsa::tracked]`. This means its results are automatically cached and don't have to be re-generated unless the underlying file contents or type inference changes.

(Though I suppose it's possible this is sufficiently fast to generate that it's not worth the memory to cache it? Not sure.)

---

_@carljm reviewed on 2025-07-03 01:37_

Thanks for the PR! I'd prefer for Micha or Dhruv (who know the LSP and our plans in that area better) to review this; just one initial comment.

---

_@UnboundVariable reviewed on 2025-07-03 02:56_

---

_Review comment by @UnboundVariable on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:18 on 2025-07-03 02:56_

Interesting idea. However, I think it's unlikely that an editor would request semantic tokens in a redundant manner, so it's not clear that the caching would result in any savings in practice.

Also, it's worth noting that this function's output can change based on the requested text range. Is salsa smart enough to memoize results based on function inputs, or does it assume that the function will be called with the same arguments every time?

---

_@carljm reviewed on 2025-07-03 03:23_

---

_Review comment by @carljm on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:18 on 2025-07-03 03:23_

It is smart enough to memoize results based on all function arguments, as well as on all other Salsa queries called while the function executes (so e.g. in this case, all queried types for nodes), and only re-execute if any of those "inputs" (function arguments and otherwise) change.

But if it is the case that clients are unlikely to query this multiple times on an unchanged file, then I agree it may not make sense to memoize it.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:56 on 2025-07-03 06:02_

Any specific reason for manually setting these discriminant values? The default values are going to be the same as the ones mentioned here.

I see that the client will look up the token types by index to this vector which makes it important that these are in the correct order. I think we can rely on the default values but I leave this up to you.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:61 on 2025-07-03 06:05_

We can use a `[&'static str; 15]` as the return type to avoid allocating here. The caller can then do the allocation if it's required. This will also allow us to make this function a `const`.

```suggestion
    pub const fn all() -> [&'static str; 15] {
        [
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:88 on 2025-07-03 06:06_

Same as above, we can remove the discriminant values and use the default which should be the same.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:93 on 2025-07-03 06:06_

Same as above
```suggestion
    pub const fn all() -> [&'static str; 3] {
        ["definition", "readonly", "async"]
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:113 on 2025-07-03 06:21_

Should we use `Option<TextRange>` instead where `None` indicates the full file range? This would allow us to avoid doing any range operations which for now is only intersection. The operation can then be changed to the following which skips the intersection checks if it's `None` and defaults to `false`

```rs
if self.range_filter.is_some_and(|range_filter| range.intersect(range_filter)) { ... }
```

This would also simplify the tests where we can directly pass in `None`.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:396 on 2025-07-03 06:25_

Curious to know the reason to allow this lint? I think we can remove the `'db` lifetime here?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:113 on 2025-07-03 06:33_

Is it intentional that the return type is `Option<SemanticTokens>`? I'm asking because the `None` variant is not being returned in any control flow path.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:109 on 2025-07-03 06:36_

Do you expect that there could be other fields that would be required in the future?

I think we could implement the [`Deref`](https://doc.rust-lang.org/stable/std/ops/trait.Deref.html) trait here to directly reference the inner `Vec` and avoid making the field public and thus the vector cannot be mutated outside this module.

```rs
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct SemanticTokens {
    tokens: Vec<SemanticToken>,
}

impl Deref for SemanticTokens {
    type Target = [SemanticToken];

    fn deref(&self) -> &Self::Target {
        &self.tokens
    }
}
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:88 on 2025-07-03 06:47_

The `modifiers` field could be represented using [`bitflags`](https://docs.rs/bitflags/2.9.1/bitflags/) as multiple modifiers can be present for a single token and then it can directly be used in the server response as `u32`.

```rs
use bitflags::bitflags;

bitflags! {
    /// Bitset representing semantic token modifiers.
    #[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
    pub struct SemanticTokenModifiers: u32 {
        const DEFINITION = 1 << 0;
        const READONLY = 1 << 1;
        const ASYNC = 1 << 2;
    }
}
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:438 on 2025-07-03 06:48_

Can we move this logic in `SourceOrderVisitor::visit_parameters` and use that instead? This will benefit the parameters defined in a `lamdba` expression as well as that's the other node which will use `visit_parameters`.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:446 on 2025-07-03 06:56_

Should this be visited first as decorators appear before the function definition in source order? This would avoid doing the sort at a later stage. Refer to my other comment where the tokens are being sorted.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:35 on 2025-07-03 06:56_

Should we instead make sure that the tokens are being pushed in sorted order as we're already using `SourceOrderVisitor`? That would be a good optimization.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:162 on 2025-07-03 07:02_

If we decide to maintain the sort order during insertion, we should add a `debug_assert` here that makes sure that the start position of the new token is after the previous semantic token. We could do this by storing the start value of the previous token and using that only under `#[cfg(debug_assertions)]`. We've this invariant in our lexer:

https://github.com/astral-sh/ruff/blob/28ab61d8850af9abe7f5b3f0406008b8abde4f8c/crates/ruff_python_parser/src/lexer/cursor.rs#L19-L21

This doesn't affect the release build.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:481 on 2025-07-03 07:03_

Same comment as the one for function definition, we should move this at the first of this match branch to maintain the sort order.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:512 on 2025-07-03 07:10_

This `walk_stmt` call seems redundant as visiting an import statement in source order is only about visiting the aliases which is already happening above.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:533 on 2025-07-03 07:10_

Same here, this `walk_stmt` call seems redundant.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:553 on 2025-07-03 07:11_

The `walk_expr` here seems redundant

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:563 on 2025-07-03 07:13_

Should we explicitly walk the base expression here instead of the attribute expression as we've already handled the `attr` field?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:571 on 2025-07-03 07:14_

Would this not add duplicate tokens? The `call.func` as a name expression is being added here and then inside the `walk_expr` it will call `visit_expr(call.func)` again which will add this token using the above `ast::Expr::Name` branch.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:576 on 2025-07-03 07:17_

The `walk_expr` call seems redundant here as it'll just visit the individual string literal parts.

Is it correct to send a single token for implicitly concatenated strings or should it be separate tokens for each string literals that are present in the concatenation? For example, in `"foo" "bar"` should it be a single token for both the strings or two tokens?

If we need to have separate tokens for individual string literals, then we could use `SourceOrderVisitor::visit_string_literal` instead?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:588 on 2025-07-03 07:18_

These `walk_expr` calls seems redundant as well

---

_@dhruvmanila reviewed on 2025-07-03 07:19_

This is a great start, thank you for doing this!

I've been reviewing this for an hour and I need to take a lunch break but here are my initial comments. I'll look at the remaining parts after the lunch break

---

_Label `server` added by @AlexWaygood on 2025-07-03 07:28_

---

_Label `ty` added by @AlexWaygood on 2025-07-03 07:28_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:217 on 2025-07-03 08:13_

Can we spell out the type similar to other match arms?

```suggestion
            Type::FunctionLiteral(_) => {
                // Check if this is a method based on current scope
                if self.in_class_scope {
                    (SemanticTokenType::Method, modifiers)
                } else {
                    (SemanticTokenType::Function, modifiers)
                }
            }
            Type::BoundMethod(_) => (SemanticTokenType::Method, modifiers),
            Type::ModuleLiteral(_) => (SemanticTokenType::Namespace, modifiers),
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:251 on 2025-07-03 08:14_

Same here, can we use `match` expression instead?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:414 on 2025-07-03 08:17_

Is this pattern to support `builtins.classmethod`?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:416 on 2025-07-03 08:20_

I think this is good enough for the initial implementation but we could make use of the `FunctionType` which contains the information about which decorator exists on the function. The way that would work is that we would need to get the `Definition` from the `SemanticIndex` that's corresponding to the function this parameter belongs to and use `infer_definition_types`. The `infer_definition_types` is a salsa query which means it will be cached or should be already cached.

And, similarly for `@staticmethod`

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:307 on 2025-07-03 08:23_

`Identifier` implements `Ranged` :)

```suggestion
        let name_start = name.start();
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:322 on 2025-07-03 08:29_

I think we can simplify this by using operations available on `TextSize` and `TextRange` like so:
```rs
        let mut current_offset = TextSize::default();
        for part in name_str.split('.') {
            if !part.is_empty() {
                self.add_token(
                    TextRange::at(name_start + current_offset, part.text_len()),
                    token_type,
                    vec![],
                );
            }
            // Move past this part and the dot
            current_offset += part.text_len() + '.'.text_len();
		}
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:379 on 2025-07-03 08:31_

Can we use `SourceOrderVisitor::visit_decorator` instead i.e., implement the `visit_decorator` on `SourceOrderVisitor` and use that instead of `visit_decorators`

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:346 on 2025-07-03 08:32_

Same as the above comment for these two methods, we could implement it via `SourceOrderVisitor`

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:653 on 2025-07-03 08:36_

Should we instead assert the number of tokens because we know how many it's going to be for each test cases? Using the count also has the benefit that when it fails we would know what was the expected value and how many did we get when using `assert_eq!(class_tokens.len(), expected)`

This applies to other test cases as well.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:657 on 2025-07-03 08:37_

Can we add a test case for implicitly concatenated string expression?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:694 on 2025-07-03 08:38_

Let's add a couple of test cases where the first parameter isn't `self` / `cls` when `@classmethod` / `@staticmethod` / no decorators are involved. This will make sure that the implementation doesn't special case the word "self" and "cls" (which it doesn't).

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:725 on 2025-07-03 08:41_

We can use multi-line string here and similarly for other test cases:

```suggestion
        let test =
            cursor_test(
            	"\
class MyClass:
    @classmethod
    def method(cls, x): 
    	pass<CURSOR>
");
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:898 on 2025-07-03 08:43_

nit: `r` isn't required because the test case doesn't use any escape character and `#` isn't required because it doesn't use any nested quotes

```suggestion
            "
import sys
class MyClass:
    pass

def my_function():
    return 42

x = MyClass()
y = my_function()
z = sys.version<CURSOR>
",
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:940 on 2025-07-03 08:44_

I think it might be useful to assert the exact number of these tokens for which we can use `assert_eq` macro. Refer to my other comment around this.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:971 on 2025-07-03 08:46_

I think we can extract these assertions in a separate function that would take in a parameter that says how many of the semantic tokens are expected and the function would then assert that only those tokens are present with the exact same quantity. For example, for this test case the call could look like:

```rs
assert_token_count([
    (SemanticTokenType::BuiltinConstant, 3),
    (SemanticTokenType::Variable, 3),
]);
```

And, same for other test cases.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:1052 on 2025-07-03 08:48_

I'd probably just use the playground (https://play.ty.dev/4aeb6053-197e-49a7-9117-fd9589ea062e) and hard code these values :)

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:1570 on 2025-07-03 08:52_

Do you see any benefit of using `>=` instead of `assert_eq`? It might be useful to assert for equality to avoid any future change to go unnoticed.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:18 on 2025-07-03 08:57_

The convention that we've established is to keep each handler corresponding to either a request or notification to be in a single file. Do you mind moving either the `SemanticTokensRequestHandler` or `SemanticTokensRangeRequestHandler` in a separate file? We can use a common `crates/ty_server/src/server/api/semantic_tokens.rs` file to contain the `generate_semantic_tokens` function.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:18 on 2025-07-03 09:02_

It might be useful to do a quick check on how does different clients implement this in addition to VS Code. I usually go for Zed and Neovim (my main editor) in addition to VS Code. I can check them out as well.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:103 on 2025-07-03 09:05_

We should implement the `Ranged` trait for this so that it's easier to access the start and end values.

```rs
impl Ranged for SemanticToken { ... }
```

And, later we can directly use:
```rs
token.start();
token.end();
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:154 on 2025-07-03 09:10_

We can directly convert the range here using https://github.com/astral-sh/ruff/blob/28ab61d8850af9abe7f5b3f0406008b8abde4f8c/crates/ty_server/src/document/range.rs#L89-L101:

```suggestion
        let requested_range = params.range.to_text_range(...);
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:45 on 2025-07-03 09:16_

The spec says:

> The `deltaStart` and the `length` values must be encoded using the encoding the client and server agrees on during the `initialize` request (see also [TextDocuments](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocuments)).

So, I think we would need to convert them to the LSP values and use them instead.

For that, we've 

https://github.com/astral-sh/ruff/blob/28ab61d8850af9abe7f5b3f0406008b8abde4f8c/crates/ty_server/src/document/range.rs#L42-L52

So, I think this should be:

```suggestion
        let start_position = token.start().to_position(...);
```

And, then you can access the `line` and `character` values directly like `start_position.line` and `start_position.character`

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:51 on 2025-07-03 09:20_

This could be removed if we use `bitflags` :)

---

_@dhruvmanila requested changes on 2025-07-03 09:23_

Ok, my review is finally done, this is an awesome PR!

The test cases are extremely thorough, thank you for writing them!

I think the only required change would be to make sure that we convert the ty location values back to the LSP values in the common function for the semantic tokens handler. And, around the visitor implementation where some of the calls that might lead to duplicate tokens could be removed.

A lot of my review comments could be considered as suggestions so feel free to either apply them or discard them. It's totally fine if those are done as follow-up. I can also own them as follow-up to this PR if you prefer.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-03 10:54_

---

_Comment by @UnboundVariable on 2025-07-04 22:08_

@dhruvmanila, thanks for the thorough and insightful code review comments! I think I've addressed them all.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:111 on 2025-07-07 05:42_

I don't really understand why do we need this method. Can we not just use the contained `u32` directly which is what the LSP response is expecting? The usage of `to_lsp_indices` is constructing the same `u32` as the one that's contained in `SemanticTokenModifier`:

```rs
        let token_modifiers = token
            .modifiers
            .to_lsp_indices()
            .into_iter()
            .fold(0u32, |acc, modifier_index| acc | (1 << modifier_index));
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:431 on 2025-07-07 05:53_

This isn't required because the `SourceOrderVisitor` already has the `visit_body` method with the same logic as this.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:644 on 2025-07-07 06:07_

I think we should be implementing the `visit_string_literal` and `visit_bytes_literal` method which are part of the `SourceOrderVisitor` trait here instead because otherwise this is going to miss adding the string literal tokens which are inside an f-string. For example, in `f"foo {'nested'}"`, the `'nested'` is a `StringLiteral` but this logic won't emit the string token as it's not part of the `Expr::StringLiteral` but `Expr::FString`

```rs
    fn visit_string_literal(&mut self, string_literal: &ast::StringLiteral) {
        self.add_token(
            string_literal.range(),
            SemanticTokenType::String,
            SemanticTokenModifier::empty(),
        );
    }

    fn visit_bytes_literal(&mut self, bytes_literal: &ast::BytesLiteral) {
        self.add_token(
            bytes_literal.range(),
            SemanticTokenType::String,
            SemanticTokenModifier::empty(),
        );
    }
```

Can we also add a test case using f-strings if it's not already present?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:668 on 2025-07-07 06:13_

These `walk_expr` calls looks redundant as there are no expressions that are nested inside any of these expression variants.

```suggestion
            ast::Expr::NumberLiteral(_) => {
                self.add_token(
                    expr.range(),
                    SemanticTokenType::Number,
                    SemanticTokenModifier::empty(),
                );
            }
            ast::Expr::BooleanLiteral(_) => {
                self.add_token(
                    expr.range(),
                    SemanticTokenType::BuiltinConstant,
                    SemanticTokenModifier::empty(),
                );
            }
            ast::Expr::NoneLiteral(_) => {
                self.add_token(
                    expr.range(),
                    SemanticTokenType::BuiltinConstant,
                    SemanticTokenModifier::empty(),
                );
            }
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:900 on 2025-07-07 06:18_

Sorry, I totally missed this in my first review and it would've probably saved you some time. We've been using snapshot based testing for individual IDE capabilities like the hover example below:

https://github.com/astral-sh/ruff/blob/28ab61d8850af9abe7f5b3f0406008b8abde4f8c/crates/ty_ide/src/hover.rs#L168-L202

The `assert_snapshot!` macro is important here. What happens is that we create an output format for these requests which are human readable and use `assert_snapshot!(test.hover(), "")`. Then, running the tests will automatically replace the `""` (second argument) with the generated content from `test.hover()` call. And, finally we'd verify that those are correct. The output format is decided by us and is based on what information is required to verify the implementation.

I don't think we need to change anything in this PR but it'd be useful to convert these tests into snapshots instead. I'll open a new issue to keep track of it and it would be a good "help wanted" issue a contributor could pick up.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/semantic_tokens.rs`:220 on 2025-07-07 06:20_

Should we instead have a strict `<` check instead of `<=`? I'm asking because adding duplicate tokens (same range) wouldn't raise an assertion here.

---

_@dhruvmanila approved on 2025-07-07 06:22_

This is great! Thank you for taking this on and addressing all the review comments!

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:62 on 2025-07-07 10:49_

While it's mentioned in the comment, it wasn't immediately clear to me that the names here need to match the names from the LSP specification. I think I would split this method into two, to make this more clear:

1. Change the `all` method to return a slice over all known `SemanticTokenType`s
2. Add a `as_lsp_concept` method (and link to https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/) that takes `&self` and returns a `&'static str`

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:40 on 2025-07-07 10:50_

It seems we don't rely on the `u32` representation and can let Rust pick the best representation instead.
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:85 on 2025-07-07 10:51_

We use the [bitflags](https://docs.rs/bitflags/latest/bitflags/) crate for bitflag datastructures. Is there a reason why we can't use it here?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:83 on 2025-07-07 10:52_

I'd find a link to the LSP specification useful https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#semanticTokenModifiers

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:93 on 2025-07-07 10:53_

I also recommend changing `all` to return a ` &'static [SemanticTokenModifier]` and instead have a `to_lsp_key` (or similar) method instead.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:176 on 2025-07-07 10:56_

You can get the `db` using `semantic_model.db()` if it turns out to be necessary
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:177 on 2025-07-07 10:56_


```suggestion
    #[expect(dead_code)]
```

or remove the field for now (and add it when it becomes necessary). 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:447 on 2025-07-07 11:02_

I suggest overriding `enter_node` instead and return `TraverseSignal::Skip` if a node shouldn't be visisted. This has the benefit that it also works for non-statements and avoids that nodes get (deeply) visited for which you don't override the `visit_` method.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:203 on 2025-07-07 11:04_

Nit: You could change this to take an `impl Ranged` so that you don't need to call `.range` when e.g. adding a token for `Name` node
```suggestion
        ranged: impl Ranged,
```


---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:216 on 2025-07-07 11:05_

Nit

```suggestion
            self.tokens.last().is_none_or(|last| last.start() <= range.start()),
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:754 on 2025-07-07 11:08_

This will ensure that Rust shows the callsite of `assert_token_counts` if the assertion fails instead of this function in a stacktrace, which is probably more useful (because it helps narrow down which test failed)

```suggestion
    #[track_caller]
    fn assert_token_counts(
        tokens: &SemanticTokens,
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:900 on 2025-07-07 11:10_

Agree. I think it would make the tests more readable and also assert that all computed ranges are correct. It shouldn't be too hard to change. All that is needed is to transform each token into a `Diagnostic` and then render all diagnostics (the annotation could be the kind and maybe modifier?)

Here's an example on how this is done in the go to type definition tests: 

https://github.com/astral-sh/ruff/blob/29927f2b594c4af5c470419754342ff33cc65d8b/crates/ty_ide/src/goto.rs#L843-L848

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/semantic_tokens.rs`:21 on 2025-07-07 11:14_

I would prefer passing `None` for the `requested_range` instead of a range covering the full document. It's more what I would expect, given that the `semantic_tokens` API already takes an `Option`

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/semantic_tokens.rs`:32 on 2025-07-07 11:16_

I think the `length` here must respect the `encoding`. The value you get here is the byte length of the token, but that's only correct if the negogiated encoding is UTF8. 

Is there a reason why you can't use `text_range.to_lsp_range` instead?

---

_@MichaReiser approved on 2025-07-07 11:18_

---

_Comment by @MichaReiser on 2025-07-07 12:11_

This can be done in a sepreate PR, but it would be great to add semantic highlighting to the playground too. It should be "as easy as" implementing the necessary provider and registering them here:

https://github.com/astral-sh/ruff/blob/a6637964d200cc0f1a7a0f89454e4976212d2693/playground/ty/src/Editor/Editor.tsx#L143-L180

---

_Comment by @MichaReiser on 2025-07-07 16:11_

I'll leave it up to you if this PR should close https://github.com/astral-sh/ty/issues/260



---

_@UnboundVariable reviewed on 2025-07-07 17:40_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/semantic_tokens.rs`:111 on 2025-07-07 17:40_

Good catch. That was left over from my earlier implementation. As you point out, it's no longer needed.

---

_@UnboundVariable reviewed on 2025-07-07 18:00_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/semantic_tokens.rs`:644 on 2025-07-07 18:00_

Nice. I didn't realize there were visitor methods specifically for string and bytes literals.

---

_@UnboundVariable reviewed on 2025-07-07 21:10_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/semantic_tokens.rs`:900 on 2025-07-07 21:10_

Good suggestion! I wasn't previously familiar with insta, but I really like the snapshot test framework it provides. This makes the tests much more readable and maintainable.

---

_Comment by @github-actions[bot] on 2025-07-07 22:31_

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

_Merged by @UnboundVariable on 2025-07-07 22:34_

---

_Closed by @UnboundVariable on 2025-07-07 22:34_

---

_Branch deleted on 2025-07-07 22:34_

---
