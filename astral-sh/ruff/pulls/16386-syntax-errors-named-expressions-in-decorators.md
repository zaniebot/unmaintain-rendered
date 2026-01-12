```yaml
number: 16386
title: "[syntax-errors] Named expressions in decorators before Python 3.9"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syntax-decorators-39
created_at: 2025-02-26T00:46:02Z
updated_at: 2025-03-05T17:13:42Z
url: https://github.com/astral-sh/ruff/pull/16386
synced_at: 2026-01-12T15:55:54Z
```

# [syntax-errors] Named expressions in decorators before Python 3.9

---

_@ntBre_

Summary
--

This PR detects the relaxed grammar for decorators proposed in [PEP 614](https://peps.python.org/pep-0614/) on Python 3.8 and lower.

The 3.8 grammar for decorators is [here](https://docs.python.org/3.8/reference/compound_stmts.html#grammar-token-decorators):

```
decorators                ::=  decorator+
decorator                 ::=  "@" dotted_name ["(" [argument_list [","]] ")"] NEWLINE
dotted_name               ::=  identifier ("." identifier)*
```

in contrast to the current grammar [here](https://docs.python.org/3/reference/compound_stmts.html#grammar-token-python-grammar-decorators)

```
decorators                ::= decorator+
decorator                 ::= "@" assignment_expression NEWLINE
assignment_expression ::= [identifier ":="] expression
```

Test Plan
--

New inline parser tests.


---

_Comment by @github-actions[bot] on 2025-02-26 01:02_

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

_Comment by @codspeed-hq[bot] on 2025-02-26 17:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fsyntax-decorators-39)

### Merging #16386 will **not alter performance**

<sub>Comparing <code>brent/syntax-decorators-39</code> (0585337) with <code>main</code> (d062388)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Renamed from "Detect named expressions in decorators before Python 3.9" to "[syntax-errors] Named expressions in decorators before Python 3.9" by @ntBre on 2025-02-28 22:18_

---

_Label `preview` added by @ntBre on 2025-02-28 22:39_

---

_Marked ready for review by @ntBre on 2025-03-03 18:08_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 18:08_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 18:08_

---

_Label `parser` added by @ntBre on 2025-03-03 18:45_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-03 21:43_

Hmm, I think "named expression" generally refers specifically to walrus expressions (https://peps.python.org/pep-0572 uses the term several times), so I'm not sure it's the best term to use here

---

_@AlexWaygood reviewed on 2025-03-03 21:43_

---

_Comment by @AlexWaygood on 2025-03-03 21:46_

> This was the trickiest one of these to detect yet. It seemed like the best approach was to attempt to parse the old version and fall back on the new grammar if anything goes wrong, but I'm not as confident in this approach since it required adding a new method to the parser.

Is it possible that this one would be easier to detect using the "greeter" approach that you proposed for syntax errors emitted by the compiler?

---

_@ntBre reviewed on 2025-03-03 21:49_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-03 21:49_

Yeah that makes sense, I was just following [PEP 614](https://peps.python.org/pep-0614/#specification) and the AST [reference](https://docs.python.org/3.13/reference/compound_stmts.html#grammar-token-python-grammar-decorator) (which I see now actually calls them `assignment_expression`s, but I think that's another synonym for the walrus operator?).

Do you have any ideas for a replacement? I didn't think "relaxed decorator" was much more helpful, but that's closer to the PEP name and the "What's new" entry.

---

_Comment by @ntBre on 2025-03-03 22:15_

> Is it possible that this one would be easier to detect using the "greeter" approach that you proposed for syntax errors emitted by the compiler?

Hmm, maybe it would be slightly easier. We could store state indicating that we're in a decorator and then check if each visited component of the expression is an attribute access, with an optional top-level `ExprCall`.

Still, I think the approach here makes sense (at least to me :laughing:) and lets us keep the detection in the parser. It's just certainly trickier than some of the other errors like "is this a walrus operator." And I wasn't sure if I wrote idiomatic parser code or if this is exactly what the checkpoint system is meant for.

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-03 22:19_

Maybe something like this?

```rs
impl Display for UnsupportedSyntaxError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        let kind = match self.kind {
            UnsupportedSyntaxErrorKind::Match => "`match` statement",
            UnsupportedSyntaxErrorKind::Walrus => "named assignment expression (`:=`)",
            UnsupportedSyntaxErrorKind::ExceptStar => "`except*`",
            UnsupportedSyntaxErrorKind::RelaxedDecorator => {
                let expression_kind = ...;
                return write!(
                    "Cannot use {expression_kind} expressions in decorators on Python {} (syntax was added in Python 3.9)",
                    self.target_version,
               );   
            }      
        };
        write!(
            f,
            "Cannot use {kind} on Python {} (syntax was added in Python {})",
            self.target_version,
            self.kind.minimum_version(),
        )
    }
}
```

Then if it was e.g. a subscript expression, the error message would be

```
Cannot use subscript expressions in decorators on Python 3.8 (syntax was added in Python 3.9)
```

---

_@AlexWaygood reviewed on 2025-03-03 22:19_

---

_@MichaReiser reviewed on 2025-03-04 08:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2629 on 2025-03-04 08:15_

I'd prefer not to use checkpoints here. Checkpoints are expensive. 

I think I'd prefer to visit the parsed AST instead and check if any name isn't a named expression. One thing that might be challenging is that the outer-most expression can't be parenthesized, but that's something that we should be able to detect at the parse decorator function

---

_Comment by @AlexWaygood on 2025-03-04 08:37_

It might be interesting to add a test that uses a walrus expression in a decorator with the target version set to py37

---

_@dhruvmanila reviewed on 2025-03-04 10:49_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2629 on 2025-03-04 10:49_

I don't think it's just named expression. As per the grammar and the tests show this as well, one other example is a subscript expression like `@foo[0]` (top-level expression is subscript) or `@foo[0].bar` (top-level expression is an attribute) as both are not allowed.

I agree that it might be simplest to do this using a visitor instead of using speculative parsing.

I quickly looked at what Pyright does and it seems to be doing a basic check on the parsed expression if the target version is < 3.9: https://github.com/microsoft/pyright/blob/1d1b43c17a83927d422c9ff52f98fb595419e329/packages/pyright-internal/src/parser/parser.ts#L2401-L2417. I'm not sure if that covers the entire extent of the grammar but it might be worth exploring.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2629 on 2025-03-04 13:47_

It looks like pyright's [`_isNameOrMemberAccessExpression`](https://github.com/microsoft/pyright/blob/1d1b43c17a83927d422c9ff52f98fb595419e329/packages/pyright-internal/src/parser/parser.ts#L2429) function called there is recursively checking the decorator expression. I guess that *is* a minimal visitor. When Alex mentioned the greeter approach, I was picturing holding off on this error until I start working on the separate greeter for compile-time/semantic errors. Should I just have a mini visitor directly in the parser like pyright instead?

---

_@ntBre reviewed on 2025-03-04 13:47_

---

_@MichaReiser reviewed on 2025-03-04 13:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2629 on 2025-03-04 13:48_

I don't mind a mini visitor in the parser, as long as it isn't a full `Visitor` based visitor (and only runs for < 3.9)

---

_@ntBre reviewed on 2025-03-04 13:50_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2629 on 2025-03-04 13:50_

Of course, I think I can pretty much follow pyright directly here.

---

_@dhruvmanila reviewed on 2025-03-04 16:00_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2629 on 2025-03-04 16:00_

Thanks for looking into it. I also think that's fine.

---

_@MichaReiser approved on 2025-03-05 16:07_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-05 17:05_

I think I'll leave this nicer error reporting for a possible follow-up. It would complicate the code a fair amount to track the expression kind through the whole process just for the error message. Micha also pointed out on Discord that 3.9 is the oldest version receiving security updates, so this message will probably have a fairly limited audience.

I did remove the confusing "named expression" part and went for something closer to pyright's "Expression form not supported for decorator prior to Python 3.9":

```
Syntax Error: Unsupported expression in decorators on Python 3.8 (syntax was added in Python 3.9)
```

---

_@ntBre reviewed on 2025-03-05 17:05_

---

_@AlexWaygood reviewed on 2025-03-05 17:06_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-05 17:06_

Nice, thanks. I still think there's room for improvement but this is definitely good enough for now!

---

_@ntBre reviewed on 2025-03-05 17:08_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-05 17:08_

Absolutely, I meant to say I think your suggestion would make for the ideal error message. Thanks!

---

_Merged by @ntBre on 2025-03-05 17:08_

---

_Closed by @ntBre on 2025-03-05 17:08_

---

_Branch deleted on 2025-03-05 17:08_

---
