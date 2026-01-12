```yaml
number: 6365
title: FStringPart for f-string formatting
type: pull_request
state: closed
author: davidszotten
labels:
  - parser
assignees: []
base: main
head: joined-string-part
created_at: 2023-08-05T13:34:34Z
updated_at: 2023-11-24T23:46:45Z
url: https://github.com/astral-sh/ruff/pull/6365
synced_at: 2026-01-10T23:40:55Z
```

# FStringPart for f-string formatting

---

_Pull request opened by @davidszotten on 2023-08-05 13:34_

Step one of ["proper f-string formatting" ](https://github.com/astral-sh/ruff/pull/5932#issuecomment-1659647476)

Implements `JoinedStringPart` suggested in #5970

The previous implementation uses `Constant::Str` to model the constant parts of formatted strings (including eg `format_spec`s) but the formatter expects `Constant::Str` to include the surrounding quotes which these string parts don't have

I think i've got far enough into my [f-string formatting poc](https://github.com/davidszotten/ruff/pull/1) to say this is good

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/nodes.rs`:880 on 2023-08-05 13:37_

Maybe `String(StringTodoName)` -> `Constant(StringPart)` or `Constant(ConstantStringPart)`

---

_@davidszotten reviewed on 2023-08-05 13:37_

---

_Comment by @github-actions[bot] on 2023-08-06 17:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @davidszotten on 2023-08-07 18:44_

@MichaReiser are we still interested in something like this?

---

_Comment by @MichaReiser on 2023-08-08 08:37_

> @MichaReiser are we still interested in something like this?

Sorry. I have yet to look at it since it is a draft pull request. Generally, yes, but I'm a bit underwater right now. It's almost a full-day job to keep up with @charliermarsh's PRs ;P 

Is there specific feedback you're looking for?

---

_Comment by @davidszotten on 2023-08-08 09:02_

> Is there specific feedback you're looking for?

https://github.com/astral-sh/ruff/discussions/6183 mentions larger structural changes around handling of implicit concatenation (and how it would be nice to unify this with `JoinedStringPart` (i guess i need to rename to `FStringPart` now). 

You've previously (sorry can't find reference now) mentioned that someone might be making larger changes in order to support 3.12 style formatted-values (though that might be orthogonal until we drop support for 3.11 in 2029 or so)

i suppose for a quick review the interesting changes are in 
https://github.com/astral-sh/ruff/pull/6365/files#diff-3af3e83bb70765ce947636ab537ecd13676c5dc1929edf6e2a1e3d1d46191191

`ast/nodes.rs` 

perhaps also a glance at the snapshots


considering changing `format_spec` from `Option<Box<Expr>>` to `Vec<JoinedStringPart>`
(same reason as above, these "snippets" don't have quotes so can't really stand alone as expressions. but does it need to be `Option<Vec<_>>` to avoid memory overhead? afict an empty vec takes space for metadata but doesn't allocate a buffer for elements so maybe ok?

https://github.com/astral-sh/ruff/pull/6365/files#diff-3af3e83bb70765ce947636ab537ecd13676c5dc1929edf6e2a1e3d1d46191191R871

and open q about naming
https://github.com/astral-sh/ruff/pull/6365/files#r1285057553


lastly i hadn't quite appreciated how much changing the ast node leaks into rules. i think i managed most but will require thorough review, and i'll need help with https://github.com/astral-sh/ruff/pull/6365/files#diff-c2c8422959405016e1fb54ca23edee9716bd06ad64e610ceea9612a8291122fcR1292  and probably https://github.com/astral-sh/ruff/pull/6365/files#diff-b6e87960d75a3034eafafa5ba307dbce96935905deada7841a56f80e7927acbbR63

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:818 on 2023-08-10 12:51_

Using a `Vec` here instead of `Option<Vec<_>>` is fine in my view and has the benefit that it requires less space. 

What's the reason for `format_spec` to require a `Vec`? Is it possible to have implicit string concatenation in the middle of a format spec?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:868 on 2023-08-10 12:53_

`parts` or `elements` (changing the postfix of the type too) makes sense to me.




---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:2568 on 2023-08-10 13:02_

We'll still need the implementations for `FormattedValue` and `StringTodo`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/helpers.rs`:64 on 2023-08-10 13:05_

WHat part do we not know? How to handle FStrings?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:880 on 2023-08-10 13:09_

My preference (in the long term) is to rename `Constant` (it's a constant at runtime but it is literal in the source code) to `Literal`. How about `FStringLiteralElement` and `FStringExpressionElement` (or `FormattedValueElement`). 



---

_@MichaReiser reviewed on 2023-08-10 13:13_

Sorry for the late reply and thanks for working on this refactor.

What's your overall sense for this change? Does it simplify the AST handling or does it add unnecessary complexity?

@dhruvmanila is working on the python 312 f-string parsing. My understanding is that this is mainly limited to how we lex f-strings, and shouldn't affect the AST representation. 

> https://github.com/astral-sh/ruff/discussions/6183 mentions larger structural changes around handling of implicit concatenation (and how it would be nice to unify this with JoinedStringPart (i guess i need to rename to FStringPart now).

Yeah, I'm not yet sure how implicit concatenation should work. Especially because you can mix regular strings an FString (what a mess...). I'm thinking about removing `FStringExpr` and instead have a single `StringLiteral` that may consist of FString and regular string elements. I would ignore this for now but I would appreciate any suggestions that you have.



---

_@davidszotten reviewed on 2023-08-10 19:14_

---

_Review comment by @davidszotten on `crates/ruff/src/rules/flynt/helpers.rs`:64 on 2023-08-10 19:14_

sorry for being unclear, this is a reference to an existing comment next to the only caller of this function, https://github.com/astral-sh/ruff/pull/6365/files/96513114ba006216c2601882692d9e96b96b5844#diff-18474f15dbc4220d441b5f36f7065939d7fa0acc0609b1b3a84d253db8211ae6L91-R92

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/nodes.rs`:818 on 2023-08-10 19:20_

format_spec can made up from literals and nested formatted values, e.g. `f'{a:{b}d}'`

---

_@davidszotten reviewed on 2023-08-10 19:20_

---

_Comment by @davidszotten on 2023-08-10 19:25_

> What's your overall sense for this change? 

as i mention in the summary it feels more correct to use a different representation for string "snippets" than `Constant` since they don't include the quotes (and often aren't the entire string)

it's also nice to remove a few `unreachable`s

in general i'd say slightly better 

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/node.rs`:2568 on 2023-08-11 08:04_

ok. what's the rule for when the "helpers" in ast::nodes should be considered nodes?

related: which of these should have methods in the visitor traits?

---

_@davidszotten reviewed on 2023-08-11 08:04_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-08-14 12:20_

---

_@davidszotten reviewed on 2023-08-15 13:25_

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/node.rs`:2568 on 2023-08-15 13:25_

bump @MichaReiser ?

---

_@MichaReiser reviewed on 2023-08-16 06:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:2568 on 2023-08-16 06:16_

As a rule of thumb. They are nodes if
* they contain other nodes (is the case for `FormattedValue`)
* they are in a union with other nodes. The way I think about this is that there are nodes and the rest is mainly tokens/literals. We don't want unions where one variant is a terminal literal and another variant is anode. We either want unions over literals, or nodes. That's why `StringTodo` should be a node 
* If they have a range (`Identifier` is the exception to this)

We may not need dedicated visitor methods for `StringTodo` and `FormattedValue`, instead, we can create a single `visit_fstring_part` method.

---

_Renamed from "JoinedStringPart for f-string formatting" to "FStringPart for f-string formatting" by @davidszotten on 2023-08-20 15:24_

---

_Marked ready for review by @davidszotten on 2023-08-20 15:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:534 on 2023-08-21 03:08_

Can you provide the reason for keeping these (`conversion`, `format_spec`) 2 fields public?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/node.rs`:2651 on 2023-08-21 03:13_

Yes, I think it's correct. (You can inline this method)
```suggestion
	#[inline]
    fn visit_preorder<'a, V>(&'a self, _visitor: &mut V)
    where
        V: PreorderVisitor<'a> + ?Sized,
    {
    }
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/checkers/ast/mod.rs`:1286 on 2023-08-21 03:27_

I'm assuming this visit is done elsewhere and that's why it's removed here?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:420 on 2023-08-21 03:35_

I think you moved this because it's only useful for formatted values i.e., the expression between `{` .. `}` or is it any other reason? Isn't `parse_fstring` the entrypoint so does that make any difference?

---

_@dhruvmanila approved on 2023-08-21 03:38_

This is great work, thanks for doing this! I have mainly looked at the AST/parser related changes and briefly the linter related changes but if you want I can give a closer look at other changes as well.

If I understand correctly, the introduction of `Literal` is (in the future) to move away from `Constant` as suggested by Micha. I'm a bit unsure of the difference between `PartialString` and `Constant::Str`. Is `PartialString` will only be used for f-string?

---

_Label `formatter` added by @konstin on 2023-08-21 08:32_

---

_@davidszotten reviewed on 2023-08-21 10:30_

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/comparable.rs`:534 on 2023-08-21 10:30_

copy/paste fail i think

---

_Comment by @davidszotten on 2023-08-21 10:32_

> I'm a bit unsure of the difference between `PartialString` and `Constant::Str`

`PartialString` doesn't include prefixes and quotes, and sometimes is only part of a string, e.g. 

```
f"before{formatted}after"
  ^^^^^^
```

---

_@davidszotten reviewed on 2023-08-21 11:56_

---

_Review comment by @davidszotten on `crates/ruff/src/checkers/ast/mod.rs`:1286 on 2023-08-21 11:56_

yes, this is now handled as part of walking f-strings (which walks the parts, some of which are formatted-values which has the format spec

see e.g. https://github.com/davidszotten/ruff/blob/0eafbe4afee2f43cfc1af3b6ccdec8479133d38b/crates/ruff_python_ast/src/visitor.rs#L705

---

_@davidszotten reviewed on 2023-08-21 11:58_

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/node.rs`:2651 on 2023-08-21 11:58_

interestingly for `ExprConstant` we have a `visit_constant` that is empty instead

---

_@davidszotten reviewed on 2023-08-21 12:00_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/string.rs`:420 on 2023-08-21 12:00_

if i recall i think this moved because this function no longer recurses, `parse_formatted_value` does

---

_Label `formatter` removed by @MichaReiser on 2023-08-22 06:54_

---

_Label `parser` added by @MichaReiser on 2023-08-22 06:55_

---

_Comment by @MichaReiser on 2023-08-22 06:56_

I'm still not feeling a 100%, and this change requires more brainpower than I have right now. I'll get back to you when I feel better.

---

_Comment by @davidszotten on 2023-08-22 07:58_

> I'm still not feeling a 100%, and this change requires more brainpower than I have right now. I'll get back to you when I feel better.

sorry to hear, hope you feel better soon! no rush on this

---

_Comment by @davidszotten on 2023-08-22 08:01_

might need to rethink the naming (and possibly more). i thought i had my poc for f-strings mostly working but i had forgotten about implicit concatenation which complicates things. i don't full understand how that works atm but need to figure out if/how it can be adapted for f-strings.  one thing i immediately noticed is that we already use the term "string part" there to mean "one of multiple implicitly concatenated strings" so we might want to use a different name than `FStringPart` here to avoid confusion

---

_Comment by @codspeed-hq[bot] on 2023-08-27 07:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/davidszotten:joined-string-part)

### Merging #6365 will **improve performances by 8.46%**

<sub>Comparing <code>davidszotten:joined-string-part</code> (1c35588) with <code>main</code> (15813a6)</sub>



### Summary

`⚡ 8` improvements
`✅ 17` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `davidszotten:joined-string-part` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 94.1 ms | 89.8 ms | +4.79% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 38 ms | 36.9 ms | +2.91% |
| ⚡ | `lexer[large/dataset.py]` | 9.8 ms | 9 ms | +8.46% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 620.1 µs | 591.9 µs | +4.75% |
| ⚡ | `parser[large/dataset.py]` | 68.4 ms | 66.7 ms | +2.54% |
| ⚡ | `parser[unicode/pypinyin.py]` | 4.3 ms | 4.2 ms | +2.53% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 2 ms | 1.9 ms | +2.55% |
| ⚡ | `lexer[pydantic/types.py]` | 4.1 ms | 4 ms | +3.69% |


---

_Review requested from @MichaReiser by @davidszotten on 2023-08-27 08:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:946 on 2023-08-28 08:14_

My preferred naming is

```rust
pub enum FStringElement {
	Literal(FStringLiteralElement)
	Expression(FStringExpressionElement)
}
```

---

_@MichaReiser approved on 2023-08-28 08:21_

This looks good to me and thanks for your patience. My preference is to rename parts to `elements` and rename the nodes to `FStringExpressionElement` and `FStringLiteralElement`. The names are somewhat long but I don't mind that. Wdyt?

@charliermarsh would you mind taking a look as well?

---

_@davidszotten reviewed on 2023-08-28 10:32_

---

_Review comment by @davidszotten on `crates/ruff_python_ast/src/nodes.rs`:946 on 2023-08-28 10:32_

i don't mind. i picked "part" because it was shorter but don't feel strongly

---

_Comment by @MichaReiser on 2023-08-31 08:08_

@charliermarsh could you take a look at this PR

---

_Comment by @MichaReiser on 2023-09-14 09:23_

@charliermarsh Could you have a look at this, now that you're back from PTO ;) 

---

_@charliermarsh reviewed on 2023-09-18 16:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:811 on 2023-09-18 16:53_

I think the part after the hash here is an incorrect search-replace error.

---

_Comment by @charliermarsh on 2023-09-18 17:02_

Sorry that it took me so long to look at this. I think the refinement of the AST structure generally makes sense, although it looks like we now have some false negatives in the linter, since we're not treating the f-string constant parts as string nodes. So, e.g., none of these rules seem to be run on f-string constant parts:

```rust
Expr::Constant(ast::ExprConstant {
    value: Constant::Str(value),
    range: _,
}) => {
    if checker.enabled(Rule::HardcodedBindAllInterfaces) {
        if let Some(diagnostic) =
            flake8_bandit::rules::hardcoded_bind_all_interfaces(value, expr.range())
        {
            checker.diagnostics.push(diagnostic);
        }
    }
    if checker.enabled(Rule::HardcodedTempFile) {
        flake8_bandit::rules::hardcoded_tmp_directory(checker, expr, value);
    }
    if checker.enabled(Rule::UnicodeKindPrefix) {
        pyupgrade::rules::unicode_kind_prefix(checker, expr, value.unicode);
    }
    if checker.source_type.is_stub() {
        if checker.enabled(Rule::StringOrBytesTooLong) {
            flake8_pyi::rules::string_or_bytes_too_long(checker, expr);
        }
    }
}
```

How could we resolve? Should we add these to the `Expr::JoinedStr` case in the match?

---

_Comment by @davidszotten on 2023-09-24 07:18_

good catch. 

`hardcoded_bind_all_interfaces` and `hardcoded_tmp_directory` are straightforward to add. `unicode_kind_prefix` doesn't apply but `string_or_bytes_too_long` is a bit tricker since (unlike the first 2 which only need the range) it actually unpacks the `expr`. let me know what you think of the approach (cast to astnodes)

i guess the main issue is remembering to add new checks in both places. could maybe factor out a helper function 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S104_S104.py.snap`:30 on 2023-09-25 07:07_

I think the range should include the quotes and prefix as is the case for normal strings:

```
11 | f"0.0.0.0"
   | ^^^^^^^^^^ S104
```

Same issue exists for other rules as well wherever the `FStringElement::Literal` is being used.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:978 on 2023-09-25 07:12_

I would prefer to not pass in the range here as otherwise it becomes difficult to know which range is this within the rule function.

Regarding my other comment, I think you would want to pass in the f-string expression node by capturing it in the match case arm above on line 961:

```rust
        fstring @ Expr::FString(ast::ExprFString { elements, .. }) => {
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flynt/helpers.rs`:17 on 2023-09-25 07:18_

The previous implementation would create a constant string node but it's changed to creating a f-string literal element. Is that an expected behavior? If so, could you change the function name and documentation to reflect that?

---

_@dhruvmanila reviewed on 2023-09-25 07:33_

---

_@davidszotten reviewed on 2023-09-25 19:34_

---

_Review comment by @davidszotten on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S104_S104.py.snap`:30 on 2023-09-25 19:34_

that behaviour doesn't change with this pr (i'm just adding tests since i briefly regressed).

---

_@davidszotten reviewed on 2023-09-25 19:36_

---

_Review comment by @davidszotten on `crates/ruff_linter/src/rules/flynt/helpers.rs`:17 on 2023-09-25 19:36_

yes it's expected, this is (and was) only used to build an f-string. makes sense to update the name

---

_@davidszotten reviewed on 2023-09-25 19:39_

---

_Review comment by @davidszotten on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:978 on 2023-09-25 19:39_

see my reply above. a more interesting example is an f-string with expressions as well. isn't it nice that we can be more specific than marking up the entire string, e.g.

```
f"0.0.0.0{port}/something-else"
  ^^^^^^^
```

vs

```
f"0.0.0.0{port}/something-else"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```


---

_@dhruvmanila reviewed on 2023-09-26 02:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S104_S104.py.snap`:30 on 2023-09-26 02:17_

Oh sorry, I didn't see that. Thanks for clarifying :)

---

_Review requested from @charliermarsh by @davidszotten on 2023-09-28 08:51_

---

_Comment by @charliermarsh on 2023-09-29 01:59_

(I will re-review this tonight; if not, then tomorrow, sorry!)

---

_Comment by @dhruvmanila on 2023-09-29 02:56_

(I'll take ownership of rebasing this PR as #7376 has been merged on `main`)

---

_Comment by @dhruvmanila on 2023-10-03 15:40_

If I understand this correctly, then we're replacing the `Constant::Str` with a dedicated `FStringElement::Literal`, right? I think we'll need to preserve the information across both nodes which includes the `unicode` and `implicit_concatenated` fields otherwise for something like `u"foo" f"{bar}"` we won't be able to detect whether it's a unicode string or not.

There are some changes involved in the parser definition as well mainly with the fact that the `format_spec` isn't a node anymore but rather a vector of f-string elements. The range of format spec was being used to extract the debug text.

---

_Comment by @davidszotten on 2023-10-04 07:40_

> If I understand this correctly, then we're replacing the `Constant::Str` with a dedicated `FStringElement::Literal`, right? I think we'll need to preserve the information across both nodes which includes the `unicode` and `implicit_concatenated` fields otherwise for something like `u"foo" f"{bar}"` we won't be able to detect whether it's a unicode string or not.


The original idea was focused on cases like `f"foo{bar}"` where the `foo` part was modelled the same as a single string `"foo"` even though those are different things (eg the latter includes quotes and can have a prefix)

i don't think i'd fully considered the case in your example but it does make me wonder if we should consider modelling implicit concatenation differently too

> There are some changes involved in the parser definition as well mainly with the fact that the `format_spec` isn't a node anymore but rather a vector of f-string elements. The range of format spec was being used to extract the debug text.

the f-string elements have ranges so could we find the range using the start of the first and the end of the last?

---

_Comment by @dhruvmanila on 2023-11-24 23:46_

This is replaced by #8835 with correct author attribution. Thanks @davidszotten for taking this up! Happy to take any additional feedback in the new PR.

---

_Closed by @dhruvmanila on 2023-11-24 23:46_

---
