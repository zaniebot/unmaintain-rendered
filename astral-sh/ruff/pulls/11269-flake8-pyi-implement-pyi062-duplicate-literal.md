```yaml
number: 11269
title: "[`flake8-pyi`] Implement `PYI062` (`duplicate-literal-member`)"
type: pull_request
state: merged
author: tusharsadhwani
labels: []
assignees: []
merged: true
base: main
head: pyi-062
created_at: 2024-05-03T19:27:49Z
updated_at: 2024-05-07T18:30:04Z
url: https://github.com/astral-sh/ruff/pull/11269
synced_at: 2026-01-12T15:55:37Z
```

# [`flake8-pyi`] Implement `PYI062` (`duplicate-literal-member`)

---

_@tusharsadhwani_

## Summary

Implements `Y062` from `flake8-pyi`, mostly adopted from `Rule::DuplicateUnionMember` (without the `|` BinOp parts).

Currently `DuplicateUnionMember` also doesn't have an autofix for `Union[]` cases, so implementing both together seems like a good idea.

## Test Plan

`cargo test` / `cargo insta review`


---

_Review requested from @AlexWaygood by @tusharsadhwani on 2024-05-03 19:27_

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:811 on 2024-05-03 19:46_

We add new rules in the `Preview` rule group

---

_@zanieb reviewed on 2024-05-03 19:46_

---

_@zanieb reviewed on 2024-05-03 19:48_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI062.py`:7 on 2024-05-03 19:48_

Can you add test cases where the inner literal is entirely redundant? e.g.

`Literal[1, Literal[1]]`
`Literal[1, 2, Literal[1, 2]]`
`Literal[1, Literal[1], Literal[1]]`
`Literal[1, Literal[2], Literal[2]]`
`Literal[1, Literal[2, Literal[1]]]`

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI062.py`:7 on 2024-05-03 19:50_

These will be more important for when we add a fix, I think. 

---

_@zanieb reviewed on 2024-05-03 19:50_

---

_@zanieb reviewed on 2024-05-03 19:50_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI062.py`:7 on 2024-05-03 19:50_

It's also good to have coverage for cases like `Literal[1, 1, 1]` where we should raise the diagnostic twice.

---

_Comment by @github-actions[bot] on 2024-05-03 20:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L569'>src/bokeh/events.py:569:36:</a> PYI062 Duplicate literal member `-1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:10239:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:10244:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:10249:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:10254:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:3424:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:5833:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:5838:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:7915:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:7920:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:8172:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1051'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1051:8177:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:918:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:923:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/3e3fd0dd86933214d903654c1e7057f25dba93fa/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:928:</a> PYI062 Duplicate literal member `...`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI062 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2024-05-03 21:08_

Hm the typeshed ecosystem checks look concerning, do you know what's going on there?

---

_Comment by @tusharsadhwani on 2024-05-03 21:17_

@zanieb Doesn't seem to reproduce locally at least with copypasting the concerned files :/

---

_Comment by @dhruvmanila on 2024-05-06 09:04_

@tusharsadhwani can you try rebasing your PR on the latest `main`? Maybe it's out of sync.

---

_Comment by @AlexWaygood on 2024-05-06 09:06_

> @tusharsadhwani can you try rebasing your PR on the latest `main`? Maybe it's out of sync.

I think something might be up with the ecosystem checks generally when it comes to typeshed. I've had several odd results on some of my other PRs that have touched `PYI` checks, e.g. https://github.com/astral-sh/ruff/pull/11128. I want to try to look into this today.

---

_Comment by @tusharsadhwani on 2024-05-06 09:09_

Can confirm, merging main doesn't fix the issue.

---

_Comment by @dhruvmanila on 2024-05-06 09:11_

> I think something might be up with the ecosystem checks generally when it comes to typeshed. I've had several odd results on some of my other PRs that have touched `PYI` checks, e.g. #11128. I want to try to look into this today.

Oh, I see. @AlexWaygood Can you open a new issue to keep track of it? No need to go into detail there but if you're unable to look into it we're still aware of it.


---

_Comment by @AlexWaygood on 2024-05-06 09:16_

> > I think something might be up with the ecosystem checks generally when it comes to typeshed. I've had several odd results on some of my other PRs that have touched `PYI` checks, e.g. #11128. I want to try to look into this today.
> 
> Oh, I see. @AlexWaygood Can you open a new issue to keep track of it? No need to go into detail there but if you're unable to look into it we're still aware of it.

Done:
-  #11305

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:61 on 2024-05-07 13:57_

```suggestion
            diagnostics.push(Diagnostic::new(
                DuplicateLiteralMember {
                    duplicate_name: checker.generator().expr(expr),
                },
                expr.range(),
            ));
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1210 on 2024-05-07 14:01_

Hmm, why are we checking `BinOp` expressions here? A `Union` can either be a `Subscript` expression (`Union[int, str]`, etc.) or a `BitOr` expression (`int | str`, etc.). But AFAIK a `Literal` can only be a `Subscript` expression, right?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:15 on 2024-05-07 14:02_

```suggestion
/// Checks for duplicate members in a `typing.Literal[]` slice.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_literal_member.rs`:28 on 2024-05-07 14:03_

Let's use an example that doesn't have booleans in the `Literal` slice, since strictly speaking `Literal[True, False]` could be written more simply using `bool` anyway:

```suggestion
/// ```python
/// foo: Literal["a", "b", "a"]
/// ```
///
/// Use instead:
/// ```python
/// foo: Literal["a", "b"]
/// ```
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/typing.rs`:440 on 2024-05-07 14:09_

nit: I know the existing `traverse_union` function in this file uses `.as_ref()` here, but it's best to avoid `AsRef<>` where possible in situations like this. A single struct in Ruff can have multiple implementations of the `AsRef` trait, so if you use `.as_ref()`, you're relying on type inference by the compiler to ensure that it gets converted to the correct type. That's potentially fragile -- it could unexpectedly break if the struct gets more `AsRef` implementations added in the future.

`&**slice` means:
- Take the `slice` variable, which currently has type `&Box<Expr>`
- Dereference it once: -> `Box<Expr>`
- Dereference it again: -> `Expr`
- And now borrow it: -> `&Expr`

```suggestion
                match &**slice {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI062.py`:1 on 2024-05-07 14:15_

There's no reason I can think of why they wouldn't work fine, but I would personally also add one or two examples to the fixtures that use fully-qualified `typing.Literal[]` and `typing_extensions.Literal[]` for the type (rather than just `Literal[]`)

---

_@AlexWaygood reviewed on 2024-05-07 14:15_

Nice!

---

_@tusharsadhwani reviewed on 2024-05-07 14:30_

---

_Review comment by @tusharsadhwani on `crates/ruff_python_semantic/src/analyze/typing.rs`:440 on 2024-05-07 14:30_

Clippy has suggested me &** before, but it seemed awkward, it's not used very much in the codebase so I refrained.

I'll take care of the nits, and also have a PR up for `PYI064` in a couple hours.

---

_@zanieb reviewed on 2024-05-07 14:31_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/analyze/typing.rs`:440 on 2024-05-07 14:31_

We're having this discussion internally now :)

---

_@AlexWaygood reviewed on 2024-05-07 14:35_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/typing.rs`:440 on 2024-05-07 14:35_

Yeah, feel free to leave this for now if you don't like it :-)

I personally like `&**` as I feel like it's more explicit about exactly what you want to happen to the type. But as you say, we're pretty inconsistent about this currently across the codebase, and it's not a big deal either way :-)

---

_@tusharsadhwani reviewed on 2024-05-07 17:41_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1210 on 2024-05-07 17:41_

I'm not sure what you mean by `BinOp` being checked here, this code only checks for `Subscript` exprs as far as I understand.

---

_@AlexWaygood reviewed on 2024-05-07 17:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1210 on 2024-05-07 17:44_

You've added a hook here in this file so that the visitor that traverses the AST calls the `duplicate_literal_member` function on every `BinOp` node that it encounters:

![image](https://github.com/astral-sh/ruff/assets/66076021/fecbaeb1-9019-4d73-b9b9-293d7935ad1a)

But it doesn't need to (you only need the `Subscript` hook that you also added elsewhere in this file) since, unlike `Union`s, a `Literal` can only be a `Subscript` node

---

_Review requested from @MichaReiser by @tusharsadhwani on 2024-05-07 17:44_

---

_Review requested from @dhruvmanila by @tusharsadhwani on 2024-05-07 17:44_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2024-05-07 17:47_

---

_Review request for @dhruvmanila removed by @AlexWaygood on 2024-05-07 17:47_

---

_@tusharsadhwani reviewed on 2024-05-07 17:47_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1210 on 2024-05-07 17:47_

~Oh right. Meant to place it 1 level above.~

---

_@tusharsadhwani reviewed on 2024-05-07 17:49_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1210 on 2024-05-07 17:49_

It was completely unnecessary, removed.

---

_@AlexWaygood reviewed on 2024-05-07 17:49_

---

_Review comment by @AlexWaygood on `ruff.schema.json`:1 on 2024-05-07 17:49_

did you run `prettier` on this file by mistake, by any chance? That's a pretty large diff for a generated file 👀

---

_@tusharsadhwani reviewed on 2024-05-07 17:53_

---

_Review comment by @tusharsadhwani on `ruff.schema.json`:1 on 2024-05-07 17:53_

Fixed. There was a merge conflict in the file and $EDITOR decided to format it on save.

---

_@AlexWaygood approved on 2024-05-07 18:08_

LGTM, thanks!

---

_Comment by @AlexWaygood on 2024-05-07 18:26_

I double-checked again, and I cannot reproduce the ecosystem results locally on my clone of typeshed. I checked out this PR branch locally in my ruff clone, and then ran this command from the directory of my typeshed clone, with the following results

```
% cargo run --manifest-path ../ruff/Cargo.toml -- check stubs stdlib --isolated --select=PYI062 --preview --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `/Users/alexw/dev/ruff/target/debug/ruff check stubs stdlib --isolated --select=PYI062 --preview --no-cache`
All checks passed!
```

I also confirmed that the same command did highlight duplicate literal members if I purposefully introduced some to a random `.pyi` file in typeshed.

---

_Renamed from "[flake8-pyi] Implement PYI062" to "[`flake8-pyi`] Implement `PYI062` (`duplicate-literal-member`)" by @AlexWaygood on 2024-05-07 18:28_

---

_Merged by @AlexWaygood on 2024-05-07 18:28_

---

_Closed by @AlexWaygood on 2024-05-07 18:28_

---

_Branch deleted on 2024-05-07 18:30_

---
