```yaml
number: 6782
title: "Fix `WithItem` ranges for parenthesized, non-`as` items"
type: pull_request
state: merged
author: charliermarsh
labels:
  - parser
assignees: []
merged: true
base: main
head: charlie/parse
created_at: 2023-08-22T16:58:30Z
updated_at: 2023-08-31T15:21:31Z
url: https://github.com/astral-sh/ruff/pull/6782
synced_at: 2026-01-12T02:45:38Z
```

# Fix `WithItem` ranges for parenthesized, non-`as` items

---

_Pull request opened by @charliermarsh on 2023-08-22 16:58_

## Summary

This PR attempts to address a problem in the parser related to the range's of `WithItem` nodes in certain contexts -- specifically, `WithItem` nodes in parentheses that do not have an `as` token after them.

For example, [here](https://play.ruff.rs/71be2d0b-2a04-4c7e-9082-e72bff152679):

```python
with (a, b):
    pass
```

The range of the `WithItem` `a` is set to the range of `(a, b)`, as is the range of the `WithItem` `b`. In other words, when we have this kind of sequence, we use the range of the entire parenthesized context, rather than the ranges of the items themselves.

Note that this also applies to cases [like](https://play.ruff.rs/c551e8e9-c3db-4b74-8cc6-7c4e3bf3713a):

```python
with (a, b, c as d):
    pass
```

You can see the issue in the parser here:

```rust
#[inline]
WithItemsNoAs: Vec<ast::WithItem> = {
    <location:@L> <all:OneOrMore<Test<"all">>> <end_location:@R> => {
        all.into_iter().map(|context_expr| ast::WithItem { context_expr, optional_vars: None, range: (location..end_location).into() }).collect()
    },
}
```

Fixing this issue is... very tricky. The naive approach is to use the range of the `context_expr` as the range for the `WithItem`, but that range will be incorrect when the `context_expr` is itself parenthesized. For example, _that_ solution would fail here, since the range of the first `WithItem` would be that of `a`, rather than `(a)`:

```python
with ((a), b):
    pass
```

The `with` parsing in general is highly precarious due to ambiguities in the grammar. Changing it in _any_ way seems to lead to an ambiguous grammar that LALRPOP fails to translate. Consensus seems to be that we don't really understand _why_ the current grammar works (i.e., _how_ it avoids these ambiguities as-is).

The solution implemented here is to avoid changing the grammar itself, and instead change the shape of the nodes returned by various rules in the grammar. Specifically, everywhere that we return `Expr`, we instead return `ParenthesizedExpr`, which includes a parenthesized range and the underlying `Expr` itself. (If an `Expr` isn't parenthesized, the ranges will be equivalent.) In `WithItemsNoAs`, we can then use the parenthesized range as the range for the `WithItem`.


---

_@charliermarsh reviewed on 2023-08-22 17:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.lalrpop`:1603 on 2023-08-22 17:00_

This is where the parser deals with parenthesize expressions. We return the full range, as well as the underlying expression, rather than discarding the range here as we are today.

---

_@charliermarsh reviewed on 2023-08-22 17:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.lalrpop`:1563 on 2023-08-22 17:00_

I guess this requires another allocation unfortunately, since we have to translate `Vec<ParenthesizedExpr>` to `Vec<Expr>`?

---

_@charliermarsh reviewed on 2023-08-22 17:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.lalrpop`:1325 on 2023-08-22 17:01_

We mostly need to add these `From`/`Into` conversions everywhere to translate between `ParenthesizedExpr` and `Expr`.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-22 17:01_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-22 17:01_

---

_Comment by @charliermarsh on 2023-08-22 17:01_

I am looking for feedback on the approach prior to fixing the remaining ~150 compiler errors.

---

_Marked ready for review by @charliermarsh on 2023-08-22 22:27_

---

_Comment by @charliermarsh on 2023-08-22 22:27_

Okay, this now compiles and does the right thing. The ranges are fixed.

---

_@charliermarsh reviewed on 2023-08-22 22:28_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__parser__tests__with_statement.snap`:1188 on 2023-08-22 22:28_

This is key: the length of the `WithItem` is 3, while the length of the inner item is 1.

---

_Label `bug` added by @charliermarsh on 2023-08-22 22:28_

---

_Comment by @github-actions[bot] on 2023-08-22 23:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-08-23 13:45_

Note that, on `main`, given:

```python
with ((1), (2), 3 as x, (5)): pass
```

The last `WithItem` _does_ use the full `(5)` range.

---

_Comment by @charliermarsh on 2023-08-23 13:47_

AFAICT in the CPython AST the withitems don't have ranges at all so not sure it can guide us:

```
Module(
    body=[
        With(
            items=[
                withitem(
                    context_expr=Constant(
                        value=1,
                        lineno=1,
                        col_offset=7,
                        end_lineno=1,
                        end_col_offset=8)),
                withitem(
                    context_expr=Constant(
                        value=2,
                        lineno=1,
                        col_offset=12,
                        end_lineno=1,
                        end_col_offset=13)),
                withitem(
                    context_expr=Constant(
                        value=3,
                        lineno=1,
                        col_offset=16,
                        end_lineno=1,
                        end_col_offset=17),
                    optional_vars=Name(
                        id='x',
                        ctx=Store(),
                        lineno=1,
                        col_offset=21,
                        end_lineno=1,
                        end_col_offset=22)),
                withitem(
                    context_expr=Constant(
                        value=5,
                        lineno=1,
                        col_offset=25,
                        end_lineno=1,
                        end_col_offset=26))],
            body=[
                Pass(
                    lineno=1,
                    col_offset=30,
                    end_lineno=1,
                    end_col_offset=34)],
            lineno=1,
            col_offset=0,
            end_lineno=1,
            end_col_offset=34)],
    type_ignores=[])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3089 on 2023-08-24 06:37_

Can we make this type private (or pub(super) at most) and move it to `parser.rs` because it is mainly an internal workaround, but by no means public api

---

_@MichaReiser reviewed on 2023-08-24 06:37_

---

_@MichaReiser approved on 2023-08-24 06:41_

I'm torn on this, mainly because of 

>  Consensus seems to be that we don't really understand why the current grammar works (i.e., how it avoids these ambiguities as-is).

and because I believe that LR(1) or LALR(1) grammars should be able to support with statements without the need for hacks (It's LL(1) grammars that the Python PEP points out are unsupported). That's why I'm convinced that the issue is somewhere else in our grammar (but yeah, LALRPOP ambiguity messages are impossible to understand). 

I'm okayish with merging this when we open an issue to follow up, ideally as part of the formatter Beta. Maybe I can get some time to play around with this. 



---

_Comment by @charliermarsh on 2023-08-24 12:50_

@MichaReiser - do you have an opinion on whether the ranges should include or exclude the parentheses? I’m torn on that myself. (Either outcome requires a code change — the PR here includes the parentheses, although that’s inconsistent with some other AST nodes, like the parenthesized pattern match case.)

---

_Comment by @charliermarsh on 2023-08-24 13:32_

I'm honestly not sure what's correct here.

Given:

```python
with (a): pass
```

We currently omit the parentheses for the range (so the withitem has the range of `a`).

Given:

```python
with (a) as b: pass
```

We use the full range of `(a) as b` as the withitem range.

---

_Comment by @konstin on 2023-08-24 16:10_

Those ranges look inconvenient but correct to me. It's the same if you'd see `as` as a bin op:
```python
with (a):
    pass

with (a) + b:
    pass
```

---

_Comment by @MichaReiser on 2023-08-24 16:16_

> @MichaReiser - do you have an opinion on whether the ranges should include or exclude the parentheses? I’m torn on that myself. (Either outcome requires a code change — the PR here includes the parentheses, although that’s inconsistent with some other AST nodes, like the parenthesized pattern match case.)

I'm leaning towards including the range similar to tuples, where the parentheses are optional. With items are different from parenthesized expressions (and patterns) in that they don't allow nesting:

```python
with (((a) as b)):
    pass
```

Isn't valid syntax

However, this does raises the question of what the expected range should be for:

```python
with (a): pass
```

Is this a with item with a parenthesized expression or is the with item parenthesized, containing an expression? We would need to take a look at the grammar to understand the precedence.

Edit: 
I studied the [grammar](https://docs.python.org/3.12/reference/grammar.html) and the precedence rules described in [PEP617](https://peps.python.org/pep-0617/#background-on-peg-parsers). 

```
with_stmt:
    | 'with' '(' ','.with_item+ ','? ')' ':' block 
    | 'with' ','.with_item+ ':' [TYPE_COMMENT] block 
    | ASYNC 'with' '(' ','.with_item+ ','? ')' ':' block 
    | ASYNC 'with' ','.with_item+ ':' [TYPE_COMMENT] block 

with_item:
    | expression 'as' star_target &(',' | ')' | ':') 
    | expression 
```

> ...while a PEG parser will check if the first alternative succeeds and only if it fails, will it continue with the second or the third one in the order in which they are written. This makes the choice operator not commutative.

My understanding of this is that the `a` in `with (a)` is not a parenthesized expression, the parentheses belong to the `with_stmt` because the parentheses rule is the first and it succeeds parsing.



---

_Comment by @MichaReiser on 2023-08-24 16:31_

I guess the `with (a)` is more involved than I thought because the following is valid as well:

```python
with (
	a,
	b
): pass
```

Meaning, it's not the with items that are parenthesized, but all of them.

---

_Comment by @MichaReiser on 2023-08-24 16:46_

Oh, I did read the spec incorrectly. With items that cannot be parenthesized. It's either parenthesizing all with items, or the expression in the with item. 

I would still expect the `WithItem` range to span the full range for parenthesized expressions. This to align it with an expression statement

```
(a)
```

The range of the expression statement is 0..3, the range of the expression is `1...2`

---

_Comment by @charliermarsh on 2023-08-24 18:15_

I might be misunderstanding your comment but `with (a, b): pass` _is_ valid and it's two separate `WithItems`. Here are a few cases to consider:

```python
with (a, b): pass

with (a, b) as c: pass

with ((a, b) as c): pass

with (a as b): pass
```

In the first case, that's two `WithItem`'s (`a` and `b`), so both ranges should exclude the parentheses IIUC.

In the second case, it's a single `WithItem` that should include the parentheses.

In the third and fourth cases, that's a single `WithItem`. I guess it should include the outer parentheses?


---

_Comment by @MichaReiser on 2023-08-24 18:29_

> In the third and fourth cases, that's a single WithItem. I guess it should include the outer parentheses?

No, it should not include the ranges in my view because WithItems can't have parentheses. It's the `With` statement that allows parentheses or the expression, but not the with item. 

From ny comment aboce

I studied the [grammar](https://docs.python.org/3.12/reference/grammar.html) and the precedence rules described in [PEP617](https://peps.python.org/pep-0617/#background-on-peg-parsers). 
```
with_stmt:
    | 'with' '(' ','.with_item+ ','? ')' ':' block 
    | 'with' ','.with_item+ ':' [TYPE_COMMENT] block 
    | ASYNC 'with' '(' ','.with_item+ ','? ')' ':' block 
    | ASYNC 'with' ','.with_item+ ':' [TYPE_COMMENT] block 

with_item:
    | expression 'as' star_target &(',' | ')' | ':') 
    | expression 
```

> ...while a PEG parser will check if the first alternative succeeds and only if it fails, will it continue with the second or the third one in the order in which they are written. This makes the choice operator not commutative.

My understanding of this is that the `a` in `with (a)` is not a parenthesized expression, the parentheses belong to the `with_stmt` because the parentheses rule is the first and it succeeds parsing.

What I meant that isn't valid is `match (a as b), c: pass`, proving that with items can't have their own parentheses.


---

_Comment by @charliermarsh on 2023-08-24 18:38_

Ah, I see, I think I was just confused by the message you posted after that, saying that it _should_ include the parentheses. But I assume there, when you say `(a)`, you're referring to something like:

```python
with ((a), b):
  pass
```

Since in this case, the outer parentheses belong to the `with` statement, but the inner parentheses on `(a)` belong to the item?

---

_Comment by @MichaReiser on 2023-08-24 18:44_

> Since in this case, the outer parentheses belong to the with statement, but the inner parentheses on (a) belong to the item?

yes, but we need to be careful with terminology. The parentheses belong to the expression `a`. But the range of the enclosing with item includes the whole expression range (including parentheses).

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1059 on 2023-08-25 12:01_

Nit: We could probably use the range from the `context_expr` directly rather than collecting another range.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1601 on 2023-08-25 12:02_

`into`...`unwrap`...`into` what :exploding_head: 

---

_@MichaReiser approved on 2023-08-25 12:05_

It hurts, but I think that's the best we can do without rewriting our parser :sob: 

I also don't fully understand why this is working. It could be poor luck, or even a bug, but we take it...

---

_Label `parser` added by @MichaReiser on 2023-08-25 12:06_

---

_Label `bug` removed by @MichaReiser on 2023-08-25 12:06_

---

_@charliermarsh reviewed on 2023-08-25 23:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.lalrpop`:1059 on 2023-08-25 23:47_

This is leading me down some confusing path around ranges for parenthesized named expressions in with-items :sob:

---

_Comment by @charliermarsh on 2023-08-28 00:45_

This is still getting on case wrong (sigh):

```python
with (a := 1):
    pass
```

The expression rules _require_ that the named expression is parenthesized, so the `(` `withitem` `)` case _doesn't_ match, and it ends up being difficult to treat the parentheses as part of the `WithItem`.

---

_Comment by @charliermarsh on 2023-08-30 17:34_

Okay, this is now working in all (?) cases. I had to special-case parenthesized expressions and other expressions that "require" parentheses.

---

_Comment by @charliermarsh on 2023-08-30 17:34_

I now need to look into the formatter changes.

---

_@charliermarsh reviewed on 2023-08-30 17:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:3089 on 2023-08-30 17:37_

Unfortunately no -- it somehow becomes part of the public API in the generated LALRPOP code.

---

_Comment by @charliermarsh on 2023-08-30 17:37_

I also want to see the CodSpeed benchmarks.

---

_Comment by @MichaReiser on 2023-08-31 06:28_

> I also want to see the CodSpeed benchmarks.

[Parse time regresses by 1-2%](https://codspeed.io/astral-sh/ruff/branches/charlie/parse). Can you change the codespeed threshold to 1% to see how it goes? 

---

_Comment by @MichaReiser on 2023-08-31 06:28_

> I now need to look into the formatter changes.

Does this mean the PR is ready to review or not? If not, can you put it back in draft state?

---

_Comment by @charliermarsh on 2023-08-31 06:40_

No, the PR is ready to review. Or rather, to merge? I actually didn’t expect it to be re-reviewed given that it had already been reviewed and accepted.

---

_Comment by @charliermarsh on 2023-08-31 06:42_

(The formatter changes are downstream of this, for example, https://github.com/astral-sh/ruff/issues/3711.)

---

_Comment by @codspeed-hq[bot] on 2023-08-31 13:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/parse)

### Merging #6782 will **degrade performances by 1.73%**

<sub>Comparing <code>charlie/parse</code> (9e2aec4) with <code>main</code> (f4ba0ea)</sub>



### Summary

`🔥 1` improvements
`❌ 9` regressions
`✅ 10` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/parse)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/parse` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🔥 | `linter/default-rules[pydantic/types.py]` | 39.2 ms | 38.2 ms | +2.49% |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 18.2 ms | 18.5 ms | -1.41% |
| ❌ | `linter/default-rules[large/dataset.py]` | 91.5 ms | 92.9 ms | -1.47% |
| ❌ | `linter/all-rules[large/dataset.py]` | 156.9 ms | 159.1 ms | -1.37% |
| ❌ | `parser[numpy/globals.py]` | 1.4 ms | 1.4 ms | -1.23% |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.5 ms | 12.7 ms | -1.52% |
| ❌ | `parser[unicode/pypinyin.py]` | 4.3 ms | 4.3 ms | -1.47% |
| ❌ | `parser[large/dataset.py]` | 68.8 ms | 70 ms | -1.73% |
| ❌ | `parser[pydantic/types.py]` | 26.8 ms | 27.2 ms | -1.34% |
| ❌ | `linter/default-rules[unicode/pypinyin.py]` | 6.3 ms | 6.4 ms | -1% |


---

_Merged by @charliermarsh on 2023-08-31 15:21_

---

_Closed by @charliermarsh on 2023-08-31 15:21_

---

_Branch deleted on 2023-08-31 15:21_

---
